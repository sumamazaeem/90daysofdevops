This week, in week 5, I will be doing 2 Projects as follows
1. Containerization of Java Project using Docker
2. Jenkins Pipeline as a Code(Groovy) Project
   
# Containerization of Java Project using Docker

## Project Overview

### Section 8:

#### Scenario:

for example, the legacy application is a multi-tier application stack and is running on VMs, that could be running on physical machines, and as per today's trend, agile, we need continuous deployments and deployments.

#### Problems:
* High CapEx and OpEx  
* Chance of Human errors in deployments  
* Not compatible with microservices architecture.  
* Resource wastage  
* Not Portable, envs not in sync  

#### Solution:

* Containerization  
* Consumers low resources  
* suits well for microservice design  
* In containerization, deployments are done via images
* Same container images across environments  
* reusable and repeatable  

#### Tools
Docker as a container runtime environment to build images, and containerize the vprofile application stack.
We will be using the docker compose and dockerhub as well

#### STEPS
* Steps to setup our stack services
* Find right base image from dockerhub
* write Dockerfile to customize images
* Write docker-compose.yml file to run multi containers
* Test it & Host images on Dockerhub

#### Project Architecture Diagram

![Project Architecture Diagram](/week5/Docker-diagram.png)

## Summary

Successfully created the docker images and pushed to dockerhub at https://hub.docker.com/repositories/sumamazaeem

## Microservices Project

I containerized an application by the name, Emart, wrote the Dockerfile, Docker compose yml and deployed on gitpod.io

The repo can be accessed at https://github.com/sumamazaeem/emart-app

#### Project Architecture Diagram

![Microservices Architecture Diagram](/week5/microservices-diagram.png)

## Summary  

Successfully deployed the project.  

---


# Jenkins Pipeline as a Code Project

I learned about the purpose of the Jenkins and CI concepts, and why it is important in today's scenario. I also learned about the processes in CI and Jenkins, and its features and plugins.
Before Starting this project, I completed the prereq, gained knowledge about the Jenkins installation on ec2 ubuntu, the difference between freestyle vs pipeline as a code project, tools in Jenkins, how to create a job in Jenkins, build, plugins, flow of continuous integration pipeline, steps for continuous integration, pipeline as a code introduction and code analysis, quality gates, software repositories intro(nexus), nexus PAAC Demo.....

While installing Jenkins, I resolved the CSRF Problem.

While learning, I learned about the build jobs, build processes, maven and OracleJDK installations, troubleshooting through console output logs, build steps and  post-build steps, and other factors.

Then, I created the new item and clicked on pipeline and then wrote the following pipeline script

```
pipeline {
    agent any
    stages {
        stage ('Fetch code') {
            steps {
                git branch: 'paac', url: 'https://github.com/devopshydclub/vprofile-project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```
I observed the output
![Jenkins output](/week5/Jenkins-output.png)


