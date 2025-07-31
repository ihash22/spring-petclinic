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



## Installed UTM VM

## Ansible Setup

## SonarQube Setup

## Prometheus Setup

## Grafana Setup

## Owasp Zap
