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



## Ansible Setup

## SonarQube Setup

## Prometheus Setup

## Grafana Setup

## Owasp Zap
