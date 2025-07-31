# DevSecOps Final Project 
# DevSecOps Pipeline for the Spring-Petclinic Project

DevSecOps 17636 Summer 2025

Team Members:

Ching-Yen Ho - chingyen@andrew.cmu.edu

Ian Hash - ihash@andrew.cmu.edu

Kavi Sarna - ksarna@andrew.cmu.edu

Marco Bragado - mbragado@andrew.cmu.edu

Swastik Samaddar Chowdhury - ssamadda@andrew.cmu.edu

## Project Overview

This project implements a complete DevSecOps pipeline using Docker containers for all services, with Jenkins as the CI/CD orchestrator. The pipeline integrates security scanning, code quality analysis, monitoring, and automated deployment to a production environment.

## Architecture

The project consists of the following components:

### Core Services
- **Jenkins**: CI/CD pipeline orchestration and automation
- **SonarQube**: Static code analysis and quality gates
- **OWASP ZAP**: Dynamic security scanning and vulnerability assessment
- **Prometheus**: Metrics collection and monitoring
- **Grafana**: Visualization and dashboards
- **Production VM**: Target deployment environment

## Team Structure & Responsibilities

### Individual Work Phase

| Team Member | Role | Deadline | Dependencies |
|-------------|------|----------|-------------|
| **Person A (Marco)** | Docker Infrastructure & Core Setup | July 18 | None |
| **Person B (Swastik/Ian)** | Repository & Jenkins Pipeline | July 19 | Person A |
| **Person C (Swastik/Ian)** | Security Analysis, VM & Grafana Setup | July 19 | Person A |
| **Person D (Cynthia)** | Monitoring & Visualization | July 22 | Person A, C |
| **Person E (Kavi)** | Deployment & Integration | July 26 | Person B, C, D |

### Documentation Phase
- **Infrastructure & Security Documentation**: Person A + C (July 27)
- **Pipeline & Monitoring Documentation**: Person B + D (July 27)
- **Integration Documentation**: Person E (July 27)

### Final Phase
- **Demo Video**: All team members (July 27)
- **Bonus Automation**: Person A + E (July 28)
- **Final Review & Submission**: All team members (July 29)

## Key Features

### 🔧 Infrastructure
- Containerized microservices architecture
- Custom Docker network for service isolation
- Persistent volume management for data retention
- Environment-specific configurations

### 🚀 CI/CD Pipeline
- Automated builds triggered by SCM polling
- Multi-stage pipeline with quality gates
- Blue Ocean visualization
- Automated testing and deployment

### 🔐 Security Integration
- Static Application Security Testing (SAST) with SonarQube
- Dynamic Application Security Testing (DAST) with OWASP ZAP
- Automated security report generation
- Security-first deployment practices

### 📊 Monitoring & Observability
- Real-time metrics collection with Prometheus
- Custom Grafana dashboards
- Jenkins performance monitoring
- Infrastructure health tracking

### 🎯 Deployment Strategy
- Ansible-based configuration management
- VM-based production environment
- Automated application deployment
- Environment consistency validation

## Service Configuration

### Jenkins (Port 8080)
- CI/CD pipeline orchestration
- SCM integration and build triggers
- Plugin ecosystem for extended functionality
- Blue Ocean for visual pipeline management

### SonarQube (Port 9000)
- Code quality analysis
- Security vulnerability detection
- Technical debt tracking
- Quality gate enforcement

### Grafana (Port 3000)
- Monitoring dashboards
- Prometheus data visualization
- Custom metric displays
- Alert management

### OWASP ZAP (Port 8090)
- Security vulnerability scanning
- Automated penetration testing
- Security report generation
- Integration with CI/CD pipeline

## Getting Started

### Prerequisites
- Docker and Docker Compose
- Git for repository management
- Virtual Machine for production deployment
- Ansible for configuration management

### Quick Start
```bash
# Clone the repository
git clone <repository-url>
cd spring-petclinic

# Start the DevOps environment
cd devops
docker-compose up -d

# Verify services are running
docker-compose ps
```

### Service Access
- Jenkins: http://localhost:8080
- SonarQube: http://localhost:9000
- Grafana: http://localhost:3000
- OWASP ZAP: http://localhost:8090

## Project Deliverables

### 📋 Documentation (30 points)
- Comprehensive setup instructions
- Step-by-step configuration guides
- Troubleshooting documentation

### ⚙️ Configuration Files (30 points)
- Docker Compose configurations
- Jenkinsfile and pipeline scripts
- Ansible playbooks
- Service configuration files

### 📸 Screenshots (20 points)
- Service dashboards and interfaces
- Pipeline execution evidence
- Deployment verification
- Before/after code change comparisons

### 🎥 Demo Video (20 points)
- End-to-end pipeline demonstration
- Automated deployment showcase
- Monitoring and alerting features

### 🏆 Bonus Features (15 points)
- Advanced automation scripting
- One-command deployment
- Enhanced monitoring capabilities

## Technology Stack

- **Containerization**: Docker, Docker Compose
- **CI/CD**: Jenkins, Blue Ocean Plugin
- **Code Quality**: SonarQube
- **Security**: OWASP ZAP
- **Monitoring**: Prometheus, Grafana
- **Configuration Management**: Ansible
- **Source Control**: Git/GitHub
- **Application**: Spring Boot (PetClinic)

## Security Considerations

- Network isolation using custom Docker networks
- Secure service-to-service communication
- Automated security scanning in CI/CD pipeline
- Configuration management best practices
- Secret management and environment separation

## Monitoring Strategy

- **Application Metrics**: Performance, availability, errors
- **Infrastructure Metrics**: Resource utilization, container health
- **Pipeline Metrics**: Build success rates, deployment frequency
- **Security Metrics**: Vulnerability trends, scan results

## Future Enhancements

- Container orchestration with Kubernetes
- Advanced security scanning tools
- Multi-environment deployment strategies
- Enhanced monitoring and alerting
- Performance optimization strategies

---

**Project Deadline**: July 31, 2025  
**Course**: 17-636 DevOps  
**Institution**: Carnegie Mellon University

# Text Dump Containing The Following

* Implementation Steps For:
    * Jenkins Base Setup
    * Installed UTM VM
    * Ansible Setup
    * SonarQube Setup
    * Prometheus Setup
    * Grafana Setup
    * Owasp Zap
* References to screenshots of steps
___


## Jenkins Base Setup

* Create docker-compose file to include jenkins
 
 ```
services: 
    jenkins:
        image: jenkins/jenkins:lts 
        container_name: jenkins 
        ports:
            - "8080: 8080"
            - "50000:50000"
        volumes:
            - jenkins_devops_final:/var/jenkins_home
    volumes:
        jenkins_devops_final:
```
screenshots/Screenshot 2025-07-30 at 8.46.12 PM.png

* Create and configure Jenkins Pipeline (We are utilizing the freestyle template and not the pipeline template as freestyle is catching changes) to be listening for changes in  spring-petclinic forked repo

screenshots/Screenshot 2025-07-31 at 9.09.47 AM.png

Password: --YOUR PASSWORD--

 ```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

* Add polling listener

screenshots/Screenshot 2025-07-31 at 9.11.50 AM.png

* Make change on the pet clinic home page

screenshots/Screenshot 2025-07-31 at 9.13.07 AM.png

* Push changes to main to test jenkins auto polling feature

```
spring-petclinic on ! main [+] is & v3.5.0 via
G v8.14.3 vi
v22.0.2 on • (us-east-1)
› git commit -m 'add new homepage text and image change to t est jekins auto polling feature'
[main e8b98e8] add new homepage text and image change to tes t jekins auto polling feature
1 file changed, 4 insertions (+), 2 deletions (-) spring-petclinic on ! main [+] is v3.5.0 via
G v8.14.3 vi
a .
v22.0.2 on • (us-east-1)
› git push origin main
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to
12 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 811 bytes | 811.00 KiB/s, done.
Total 7 (delta 4), reused 0 (delta 0), pack-reused 0 (from 0
)
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
To https://github. com/MarcoSB-Dev/spring-petclinic.git
55b7328. e8b988 main →> main
spring-petclinic on l main is P
v3.5.0 via
v22.0.2 on (us-east-1)
```
screenshots/Screenshot 2025-07-31 at 9.14.00 AM.png

* Changes were able to be captured without manually building it.

screenshots/Screenshot 2025-07-31 at 9.15.58 AM.png

* The Base Jenkins pipeline is now completed.


## Installed UTM VM

* Install UTM VM for Macbook as it supports Apple Silicon (M3)

screenshots/Screenshot 2025-07-31 at 9.17.32 AM.png

* Create VM with Ubuntu 22.0  Server (ARM 64)

screenshots/Screenshot 2025-07-31 at 9.18.48 AM.png

screenshots/Screenshot 2025-07-31 at 9.19.29 AM.png

screenshots/Screenshot 2025-07-31 at 9.20.11 AM.png

* Start the VM

screenshots/Screenshot 2025-07-31 at 9.21.10 AM.png

* Enable OpenSSH in VM

```
sudo systemctl status ssh
```
screenshots/Screenshot 2025-07-31 at 9.22.29 AM.png

* SSH to Devops VM on Host Machine - Host can now securely connect to Devops VM

```
ssh devops-admin@192.168.64.2
```

screenshots/Screenshot 2025-07-31 at 9.24.54 AM.png

* Jenkins Container Successfully able to communicate with with Devops VM
```
ping 192.168.64.2
```
screenshots/Screenshot 2025-07-31 at 9.26.15 AM.png


## Ansible Setup


* First enter Jenkins container terminal via the following command
```
docker exec -u 0 -it jenkins /bin/bash
```
screenshots/Screenshot 2025-07-31 at 9.28.17 AM.png

* Then install ansible so that the Jenkins container can run the ansible playbook commands.

```
apt update && apt install -y iputils-ping
```
screenshots/Screenshot 2025-07-31 at 9.29.29 AM.png

* Create SSH Keys for Jenkins so that the VM won't need to enter a password every time.

```
ssh-keygen -t rsa -b 4096 -f /var/jenkins_home/.ssh/id_rsa -N"'
```
screenshots/Screenshot 2025-07-31 at 9.30.47 AM.png

```
chmod 700 /var/jenkins_home/.ssh 
touch /var/jenkins_home/.ssh/known_hosts 
chmod 644 /var/jenkins_home/.ssh/known_hosts 
chown -R jenkins:jenkins /var/jenkins_home/.ssh
```
screenshots/Screenshot 2025-07-31 at 9.31.54 AM.png

* Check to make sure the key was generated via the following command

```
cat /var/jenkins_home/.ssh/i d_rsa.pub
```
screenshots/Screenshot 2025-07-31 at 9.40.39 AM.png

* SSH into the devops VM environment
```
ssh devops-admin@192.168.64.2
```
screenshots/Screenshot 2025-07-31 at 9.41.53 AM.png

* Then add the public key and set the file permissions to the Devops VM


```
mkdir -p ~/.ssh
chmod 700 ~/ .ssh
nano ~/.ssh/authorized_keys devops-admin@devops-vm:~$ chmod 600 ~/.ssh/authorized_keys
```
screenshots/Screenshot 2025-07-31 at 9.43.16 AM.png

* Jenkins can now SSH into VM without needed to enter a password

```
docker exec -it jenkins ssh devops-admin@192.168.64.2
```
screenshots/Screenshot 2025-07-31 at 9.44.51 AM.png

* Creat an Ansible directory within the Spring-Petclinic directory and creat a deploy.yml and inventory file

screenshots/Screenshot 2025-07-31 at 10.04.07 AM.png

* Inventory file

screenshots/Screenshot 2025-07-31 at 10.04.26 AM.png

* deploy.yml

screenshots/Screenshot 2025-07-31 at 10.04.35 AM.png

* Commit these and push to main and check to see if Jenkins was able to capture the new changes

screenshots/Screenshot 2025-07-31 at 10.05.29 AM.png

* In the Jenkins container run the ansible playbook. Jenkins can now remotely control the Devops VM

```
/var/jenkins_home/workspace/dev ops-petclinic-pipeline/ansible# ansible-playbook
-i inventory deploy- yml
```
screenshots/Screenshot 2025-07-31 at 10.06.33 AM.png

* Added ansible as part of the build step of the pipeline

screenshots/Screenshot 2025-07-31 at 10.07.48 AM.png

* Then make a simple change to test if ansible ran on Jenkins

screenshots/Screenshot 2025-07-31 at 10.08.28 AM.png

* Check the Jenkins Logs to make sure it runs successfully (in this case it did - see screenshot)

screenshots/Screenshot 2025-07-31 at 10.09.38 AM.png

* Now to check to make sure that code is deployed correctly, change the homepage picture again and made sure it runs on the Devops Server’s Address. The image was successfully changed and reflected via the Devops Server Address.

```
http://192.168.64.2:8080/
```

screenshots/Screenshot 2025-07-31 at 10.10.58 AM.png


## SonarQube Setup

* Add SonarQube to the docker-compose.yml file and create a shared network called devops-petclinic-network so that the two containers can communicate.

```
docker-compose.yml


services:
    jenkins:
        image: jenkins/jenkins:lts
        container_name: jenkins 
        ports:
            - "8090:8080"
            - "50000:50000"
        volumes:
            - jenkins_devops_final:/var/jenkins_home
        networks:
        - devops-petclinic-network

    sonarqube:
        image: sonarqube: latest
        container_name: sonarqube 
        ports:
            - "9000:9000"
        networks:
            - devops-petclinic-network 
        environment:
            - SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true 
        volumes:
            - sonarqube_data:/opt/sonarqube/data
            - sonarqube_logs:/opt/sonarqube/logs
            - sonarqube_extensions:/opt/sonarqube/extensions
    volumes:
        jenkins_devops_final:
        sonarqube_data:
        sonarqube_logs:
        sonarqube_extensions:
    networks:
        devops-petclinic-network: {}
```

screenshots/Screenshot 2025-07-31 at 10.13.42 AM.png

* Re-run docker-compose to make sure it is visible in the docker container

screenshots/Screenshot 2025-07-31 at 10.17.20 AM.png

* Create Auth Token for Jenkins

screenshots/Screenshot 2025-07-31 at 10.17.40 AM.png

* Sonarqube login password 

```
<AskForPassword>
```

Sonarqube-auth-token

```
<AskForAuthenticationToken>
```

* Add sonarqube-auth-token credentials to Jenkins

screenshots/Screenshot 2025-07-31 at 10.19.40 AM.png

* Install the SonarQube Scanner Plugin

screenshots/Screenshot 2025-07-31 at 10.20.03 AM.png

* Add SonarQube configurations on Jenkins

screenshots/Screenshot 2025-07-31 at 10.20.23 AM.png

* Add SonarQube Scanner tool to the Jenkins Global Tools

screenshots/Screenshot 2025-07-31 at 10.20.38 AM.png

* Add SonarQube Scanner as part of build step on Jenkins

screenshots/Screenshot 2025-07-31 at 10.21.00 AM.png

* Make a quick change to the spring pet clinic to trigger the jenkins pipeline and see if sonarqube runs successfully.

screenshots/Screenshot 2025-07-31 at 10.21.21 AM.png

* Check the SonarQube Dashboard to make sure it passed (in this case it passed! - see screenshot)

screenshots/Screenshot 2025-07-31 at 10.21.42 AM.png

* Finally, check the Logs and SonarQube runs successfully and is now fully integrated into the Jenkins pipeline

screenshots/Screenshot 2025-07-31 at 10.22.19 AM.png


## Prometheus Setup

* Add Prometheus to docker-compose.yml file

```
docker-compose.yml 


services: 
    jenkins:
        image: jenkins/jenkins:lts 
        container_name: petclinic-jenkins
        ports:
            - "8090:8080"
            - "50000:50000"
        volumes:
            - jenkins_devops_final:/var/jenkins_home

        networks:
            - devops-petclinic-network

    sonarqube: 
        image: sonarqube: latest
        container_name: petclinic-sonarqube 
        ports:
            - "9000:9000"
        networks:
            - devops-petclinic-network environment:
            - SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true 
        volumes:
            - sonarqube_devops_data:/opt/sonarqube/data
            - sonarqube_devops_logs:/opt/sonarqube/logs
            - sonarqube_devops_extensions:/opt/sonarqube/extensions

    prometheus:
        image: prom/prometheus: latest 
        container_name: petclinic-prometheus 
        ports:
            - "9090:9090"
        volumes:
        - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
        networks:
            - devops-petclinic-network
```
screenshots/Screenshot 2025-07-31 at 10.24.46 AM.png

* Create a prometheus.yml file with configurations

```
prometheus › ! prometheus.yml



global:
    scrape_interval: 15s

scrape_configs:
    - job_name: 'spring-petclinic'
        metrics_path: '/actuator/prometheus' 
        static_configs:
                - targets: ['192.168.64.2:8080']
```

screenshots/Screenshot 2025-07-31 at 10.28.45 AM.png

* Restart docker to make sure prometheus was visible

```
docker-compose up -d
```
screenshots/Screenshot 2025-07-31 at 11.09.18 AM.png

* Add the prometheus dependency to the pom.xml file in the spring-petclinic repo so the app can generate metrics in the right format


```
<dependency>
    <groupId>io.micrometer</groupId>
    ‹artifactId»micrometer-registry-prometheus</artifactId>
</dependency>
```
screenshots/Screenshot 2025-07-31 at 11.10.49 AM.png

* Add prometheus configuration to application.properties file in spring-petclinic to expose metrics endpoints for scraping


```
spring-petclinic › src › main › resources › E application.properties



# database init, supports mysql too
database=h2
spring.sql. init.schema-locations=classpath*:db/$(database}/schema.sql
spring.sql.init.data-locations=classpath*:db/${database)/data.sql

# Web
spring.thymeleaf.mode=HTML

# JPA|
spring.jpa.hibernate.ddl-auto=none
spring-jpa.open-in-view=false
* Internationalization
spring.messages.basename=messages/messages

# Actuator
management.endpoints.web.exposure.include=*

# Logging
logging. level.org.springframework=INFO
#logging.level.org-springframework-web=DEBUG
# logging.level.org.springframework.context.annotation=TRACE

# Maximum time static resources should be cached
spring-web. resources.cache.cachecontrol.max-age=12h

# Prometheus
management.endpoints.web.exposure.include=*
management.endpoint.prometheus.enabled=true
management.metrics.export.prometheus.enabled=true
```

screenshots/Screenshot 2025-07-31 at 11.13.39 AM.png

* Push changes to repo

screenshots/Screenshot 2025-07-31 at 11.16.41 AM.png

* Check Jenkins Logs to make sure pipeline runs successfully

screenshots/Screenshot 2025-07-31 at 11.16.49 AM.png

* Check Prometheus Dashboard. Prometheus now tracking metrics for Spring Pet clinic

screenshots/Screenshot 2025-07-31 at 11.17.06 AM.png

* Add job to track metrics from Jenkins

```
prometheus > !prometheus.yml




global:
    scrape_interval: 15s

scrape_configs:
    - job_name: 'spring-petclinic'
    metrics_path: '/actuator/prometheus' 
    static_configs:
        - targets: ['192.168.64.2:8080']

- job_name: 'jenkins'
    metrics_path: '/prometheus' 
    static_configs:
        - targets: I'petclinic-jenkins: 8080']
```

screenshots/Screenshot 2025-07-31 at 11.18.13 AM.png

* Checked prometheus dashboard and it is now tracking back jenkins pipeline and pet-spring clinic metrics

screenshots/Screenshot 2025-07-31 at 11.20.15 AM.png

## Grafana Setup

* Add grafana to docker-compose.yml file

```
docker-compose.yml 




services:
    grafana:
        image: grafana/grafana: latest
        container_name: petclinic-grafana 
        ports:
            - "3000:3000"
        volumes:
            - grafana_devops_data:/var/lib/grafana 
        networks:
            - devops-petclinic-network

volumes:
    jenkins_devops_final:
    sonarqube_devops_data:
    sonarqube_devops_logs:
    sonarqube_devops_extensions:
    grafana_devops_data:

networks:
    devops-petclinic-network: {}
```
screenshots/Screenshot 2025-07-31 at 11.21.31 AM.png

* Re-run docker to make sure it got added to the devops docker container

screenshots/Screenshot 2025-07-31 at 11.25.27 AM.png

* UserName and Password

admin/admin

* Add prometheus as a data source within the grafana dashboard

screenshots/Screenshot 2025-07-31 at 11.29.31 AM.png

* Add basic queries to track spring-petclinic metrics

screenshots/Screenshot 2025-07-31 at 11.29.46 AM.png

* Add base queries to track Jenkin Build metrics.

screenshots/Screenshot 2025-07-31 at 11.29.55 AM.png

* Grafana is now visualizing metrics collected by Prometheus for both Jenkin Builds and the PetClinic application.

screenshots/Screenshot 2025-07-31 at 11.30.04 AM.png

* Make change to trigger pipeline and see if grafana dashboard captured new metrics

screenshots/Screenshot 2025-07-31 at 11.30.12 AM.png

* Grafana successfully capturing metrics for both petclinic app and jenkins dashboard

screenshots/Screenshot 2025-07-31 at 11.30.21 AM.png

## Owasp Zap


* Add owasp zap to the docker-compose file


```
docker-compose.yml





services:

    zap:
        image: zaproxy/zap-stable 
        container_name: petclinic-zap 
        networks:
            - devops-petclinic-network 
        entrypoint:
            - zap.sh
            - -daemon
            -host
            0.0.0.0
            -port
            - "8885"
            - -config
            - api.addrs.addr.name=.*
            -config
            - api.addrs.addr.regex=true
        ports:
            - "8885:8885"
        Volumes:
            - /zap-reports:/zap/wrk
        
volumes:
    jenkins_devops_final:
    sonarqube_devops_data:
    sonarqube_devops_logs:
    sonarqube_devops_extensions:
    grafana_devops data:

networks:
    devops-petclinic-network:{}
```
screenshots/Screenshot 2025-07-31 at 11.34.19 AM.png

* Re-run docker to make sure it got pulled successfully

screenshots/Screenshot 2025-07-31 at 11.34.55 AM.png

* Add build step for removing any old zap report artifact and re-adding it. This is to avoid zap report errors

screenshots/Screenshot 2025-07-31 at 11.35.04 AM.png

* Add the build step to run the zap container to generate the reports for the jenkins build

screenshots/Screenshot 2025-07-31 at 11.35.12 AM.png

* Finally, add post build action to generate the new zap html report

screenshots/Screenshot 2025-07-31 at 11.35.20 AM.png


