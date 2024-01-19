This week, in week 5 I will be doing 3 Projects as follows
1. Containerization of Java Project using Docker
2. Jenkins Pipeline as a Code(Groovy) Project
3. Continous Integration using Jenkins, Nexus, Sonarqube and Slack
   
# Containerization of Java Project using Docker

## Project Overview

### Section 8: [Project Phase/Type]

#### Scenario:

for example, the legacy application is a multi-tier application stack and is running on VMs, that could be running on physical machines, and as per today's trend, agile, we need continuous deployments and deployments.

#### Problems:
High CapEx and OpEx  
Chance of Human errors in deployments  
Not compatible with microservices architecture.  
Resource wastage  
Not Portable, envs not in sync  

#### Solution:

Containerization  
Consumers low resources  
suits well for microservice design  
In containerization, deployments are done via images
Same container images across environments  
reusable and repeatable  

#### Tools
Docker as a container runtime environment to build images, and containerize the vprofile application stack.
We will be using the docker compose and dockerhub as well

#### STEPS
* Steps to setup our stack services
* FInd right base image from dockerhub
* write Dockerfile to customize images
* Write docker-compose.yml file to run multi containers
* Test it & Host images on Dockerhub

### Architecture


#### Project Architecture Diagram

![Project Architecture Diagram](/week5/Docker-diagram.png)

## Summary

Successfully created the docker images and pushed to dockerhub at https://hub.docker.com/repositories/sumamazaeem

## Microservices Project

---

# Next Project Section

## Section X: [Next Phase/Type]

[Follow the structure above for the next phase or type of project.]

---

# Another Project Section

## Section X: [Another Phase/Type]

[Follow the structure above for another phase or type of project.]
