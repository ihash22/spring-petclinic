pipeline {
    agent any
    
    environment {
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${MAVEN_HOME}/bin:${PATH}"
        SONAR_HOST_URL = 'http://sonarqube:9000'
        ZAP_HOST = 'http://owasp-zap:8080'
        PROD_SERVER = '192.168.1.100' // Replace with your production server IP
        DEPLOY_USER = 'deploy'
    }
    
    tools {
        maven 'Maven-3.8.1'
        jdk 'JDK-11'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    env.GIT_COMMIT_SHORT = sh(
                        script: "git rev-parse --short HEAD",
                        returnStdout: true
                    ).trim()
                }
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                    echo "Building Spring Pet Clinic..."
                    ./mvnw clean compile
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    echo "Running unit tests..."
                    ./mvnw test
                '''
            }
            post {
                always {
                    publishTestResults testResultsPattern: 'target/surefire-reports/*.xml'
                    publishCoverage adapters: [jacocoAdapter('target/site/jacoco/jacoco.xml')]
                }
            }
        }
        
        stage('Package') {
            steps {
                sh '''
                    echo "Packaging application..."
                    ./mvnw package -DskipTests
                '''
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: false
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        ./mvnw sonar:sonar \
                            -Dsonar.projectKey=spring-petclinic \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_AUTH_TOKEN}
                    '''
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("spring-petclinic:${env.GIT_COMMIT_SHORT}")
                    env.DOCKER_IMAGE = image.id
                }
            }
        }
        
        stage('Security Scan - OWASP ZAP') {
            steps {
                script {
                    // Start the application container for testing
                    sh '''
                        docker run -d --name petclinic-test \
                            --network devsecops-network \
                            -p 8081:8080 \
                            spring-petclinic:${GIT_COMMIT_SHORT}
                        
                        # Wait for application to start
                        sleep 30
                        
                        # Run ZAP baseline scan
                        docker exec owasp-zap zap-baseline.py \
                            -t http://petclinic-test:8080 \
                            -r zap-report.html \
                            -x zap-report.xml \
                            -I
                    '''
                }
            }
            post {
                always {
                    // Stop and remove test container
                    sh '''
                        docker stop petclinic-test || true
                        docker rm petclinic-test || true
                    '''
                    
                    // Publish ZAP reports
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'zap-reports',
                        reportFiles: 'zap-report.html',
                        reportName: 'OWASP ZAP Security Report'
                    ])
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                script {
                    // Deploy using Ansible
                    ansiblePlaybook(
                        playbook: 'ansible-playbooks/deploy-petclinic.yml',
                        inventory: 'ansible-playbooks/inventory/production',
                        extras: "-e docker_image=spring-petclinic:${env.GIT_COMMIT_SHORT}"
                    )
                }
            }
        }
        
        stage('Smoke Test') {
            steps {
                sh '''
                    echo "Running smoke tests on production..."
                    curl -f http://${PROD_SERVER}:8080 || exit 1
                    echo "Smoke tests passed!"
                '''
            }
        }
    }
    
    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            emailext (
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                    Good news! The build succeeded.
                    
                    Job: ${env.JOB_NAME}
                    Build Number: ${env.BUILD_NUMBER}
                    Git Commit: ${env.GIT_COMMIT_SHORT}
                    
                    Check console output at ${env.BUILD_URL}
                """,
                to: "${env.CHANGE_AUTHOR_EMAIL}"
            )
        }
        failure {
            emailext (
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """
                    Build failed!
                    
                    Job: ${env.JOB_NAME}
                    Build Number: ${env.BUILD_NUMBER}
                    Git Commit: ${env.GIT_COMMIT_SHORT}
                    
                    Check console output at ${env.BUILD_URL}
                """,
                to: "${env.CHANGE_AUTHOR_EMAIL}"
            )
        }
    }
}