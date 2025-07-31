# DevSecOps Final Project 
# DevSecOps Pipeline for the Spring-Petclinic Project

Ian Hash
ihash
DevSecOps 17636 Summer 2025

## Text Dump Containing The Following

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

```
```


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

* Create and configure Jenkins Pipeline (Am using the freestyle template and not the pipeline template as freestyle is catching changes) to be listening for changes in  spring-petclinic forked repo

screenshots/Screenshot 2025-07-31 at 9.09.47 AM.png

Password: --YOUR PASSWORD--

 ```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

* Add polling listener

screenshots/Screenshot 2025-07-31 at 9.11.50 AM.png

* Made change on the pet clinic home page

screenshots/Screenshot 2025-07-31 at 9.13.07 AM.png

* Pushed changes to main to test jenkins auto polling feature

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

* Installed UTM VM for Macbook as it supports Apple Silicon (M3)

screenshots/Screenshot 2025-07-31 at 9.17.32 AM.png

* Created VM with Ubuntu 22.0  Server (ARM 64)

screenshots/Screenshot 2025-07-31 at 9.18.48 AM.png

screenshots/Screenshot 2025-07-31 at 9.19.29 AM.png

screenshots/Screenshot 2025-07-31 at 9.20.11 AM.png

* Started the VM

screenshots/Screenshot 2025-07-31 at 9.21.10 AM.png

* Enabled OpenSSH in VM

```
sudo systemctl status ssh
```
screenshots/Screenshot 2025-07-31 at 9.22.29 AM.png

* SSH’d to Devops VM on Host Machine - Host can now securely connect to Devops VM

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


* First entered Jenkins container terminal via the following command
```
docker exec -u 0 -it jenkins /bin/bash
```
screenshots/Screenshot 2025-07-31 at 9.28.17 AM.png

* Then installed ansible so that the Jenkins container could run the ansible playbook commands.

```
apt update && apt install -y iputils-ping
```
screenshots/Screenshot 2025-07-31 at 9.29.29 AM.png

* Created SSH Keys for Jenkins so that the VM won't need to enter a password every time.

```
ssh-keygen -t rsa -b 4096 -f /var/jenkins_home/.ssh/id_rsa -N"'
```
screenshots/Screenshot 2025-07-31 at 9.30.47 AM.png

root@c13ef980a06d:/# chmod 700 /var/jenkins_home/.ssh root@c13ef980a06d:/# touch /var/jenkins_home/.ssh/known_hosts root@c13ef980a06d:/# chmod 644 /var/jenkins_home/.ssh/known_hosts root@c13ef980a06d:/# chown -R jenkins:jenkins /var/jenkins_home/.ssh

```
chmod 700 /var/jenkins_home/.ssh root@c13ef980a06d:/# touch /var/jenkins_home/.ssh/known_hosts 
chmod 644 /var/jenkins_home/.ssh/known_hosts 
chown -R jenkins:jenkins /var/jenkins_home/.ssh
```
screenshots/Screenshot 2025-07-31 at 9.31.54 AM.png

* Checked to make sure the key was generated via the following command

```
cat /var/jenkins_home/.ssh/i d_rsa.pub
```
screenshots/Screenshot 2025-07-31 at 9.40.39 AM.png

* Then I SSH’d into the devops VM environment
```
ssh devops-admin@192.168.64.2
```
screenshots/Screenshot 2025-07-31 at 9.41.53 AM.png

* Then added the public key and set the file permissions to the Devops VM


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

* Created an Ansible directory within the Spring-Petclinic directory and created a deploy.yml and inventory file

screenshots/Screenshot 2025-07-31 at 10.04.07 AM.png

* Inventory file

screenshots/Screenshot 2025-07-31 at 10.04.26 AM.png

* deploy.yml

screenshots/Screenshot 2025-07-31 at 10.04.35 AM.png

* Committed these and pushed to main and checked to see if Jenkins was able to capture the new changes

screenshots/Screenshot 2025-07-31 at 10.05.29 AM.png

* In the Jenkins container I ran the ansible playbook. Jenkins can now remotely control the Devops VM

```
/var/jenkins_home/workspace/dev ops-petclinic-pipeline/ansible# ansible-playbook
-i inventory deploy- yml
```
screenshots/Screenshot 2025-07-31 at 10.06.33 AM.png

* Added ansible as part of the build step of the pipeline

screenshots/Screenshot 2025-07-31 at 10.07.48 AM.png

* Then made a simple change to test if ansible ran on Jenkins

screenshots/Screenshot 2025-07-31 at 10.08.28 AM.png

* I checked the Jenkins Logs to make sure it ran successfully. It did.

screenshots/Screenshot 2025-07-31 at 10.09.38 AM.png

* Now to check to make sure that code is deployed correctly, I changed the homepage picture again and made sure it ran on the Devops Server’s Address. The image was successfully changed and reflected via the Devops Server Address.

```
http://192.168.64.2:8080/
```

screenshots/Screenshot 2025-07-31 at 10.10.58 AM.png


## SonarQube Setup

* Added SonarQube to the docker-compose.yml file and created a shared network called devops-petclinic-network so that the two containers can communicate.

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

* Re-ran docker-compose to make sure it was visible in the docker container

screenshots/Screenshot 2025-07-31 at 10.17.20 AM.png

* Created Auth Token for Jenkins

screenshots/Screenshot 2025-07-31 at 10.17.40 AM.png

* Sonarqube login password 

```
<AskForPassword>
```

Sonarqube-auth-token

```
<AskForAuthenticationToken>
```

* Added sonarqube-auth-token credentials to Jenkins

screenshots/Screenshot 2025-07-31 at 10.19.40 AM.png

* Installed the SonarQube Scanner Plugin

screenshots/Screenshot 2025-07-31 at 10.20.03 AM.png

* Added SonarQube configurations on Jenkins

screenshots/Screenshot 2025-07-31 at 10.20.23 AM.png

* Added SonarQube Scanner tool to the Jenkins Global Tools

screenshots/Screenshot 2025-07-31 at 10.20.38 AM.png

* Add SonarQube Scanner as part of build step on Jenkins

screenshots/Screenshot 2025-07-31 at 10.21.00 AM.png

* Made a quick change to the spring pet clinic to trigger the jenkins pipeline and see if sonarqube ran successfully.

screenshots/Screenshot 2025-07-31 at 10.21.21 AM.png

* Checked the SonarQube Dashboard to make sure it passed

screenshots/Screenshot 2025-07-31 at 10.21.42 AM.png

* Finally, checked the Logs and SonarQube ran successfully and is now fully integrated into the Jenkins pipeline

screenshots/Screenshot 2025-07-31 at 10.22.19 AM.png


## Prometheus Setup

* Added Prometheus to docker-compose.yml file

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

* Created a prometheus.yml file with configurations

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

* Restarted docker to make sure prometheus was visible

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

* Added prometheus configuration to application.properties file in spring-petclinic to expose metrics endpoints for scraping


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

* Pushed changes to repo

screenshots/Screenshot 2025-07-31 at 11.16.41 AM.png

* Checked Jenkins Logs to make sure pipeline ran successfully

screenshots/Screenshot 2025-07-31 at 11.16.49 AM.png

* Checked Prometheus Dashboard. Prometheus now tracking metrics for Spring Pet clinic

screenshots/Screenshot 2025-07-31 at 11.17.06 AM.png

* Added job to track metrics from Jenkins

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

* Added grafana to docker-compose.yml file

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

* Re-ran docker to make sure it got added to the devops docker container

screenshots/Screenshot 2025-07-31 at 11.25.27 AM.png

* UserName and Password

admin/admin

* Added prometheus as a data source within the grafana dashboard

screenshots/Screenshot 2025-07-31 at 11.29.31 AM.png

* Added basic queries to track spring-petclinic metrics

screenshots/Screenshot 2025-07-31 at 11.29.46 AM.png

* Added base queries to track Jenkin Build metrics.

screenshots/Screenshot 2025-07-31 at 11.29.55 AM.png

* Grafana is now visualizing metrics collected by Prometheus for both Jenkin Builds and the PetClinic application.

screenshots/Screenshot 2025-07-31 at 11.30.04 AM.png

* Made change to trigger pipeline and see if grafana dashboard captured new metrics

screenshots/Screenshot 2025-07-31 at 11.30.12 AM.png

* Grafana successfully capturing metrics for both petclinic app and jenkins dashboard

screenshots/Screenshot 2025-07-31 at 11.30.21 AM.png

## Owasp Zap


* Added owasp zap to the docker-compose file


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

* Re-ran docker to make sure it got pulled successfully

screenshots/Screenshot 2025-07-31 at 11.34.55 AM.png

* Add build step for removing any old zap report artifact and re-adding it. This is to avoid zap report errors

screenshots/Screenshot 2025-07-31 at 11.35.04 AM.png

* Add the build step to run the zap container to generate the reports for the jenkins build

screenshots/Screenshot 2025-07-31 at 11.35.12 AM.png

* Finally added post build action to generate the new zap html report

screenshots/Screenshot 2025-07-31 at 11.35.20 AM.png


