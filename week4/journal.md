# AWS Lift and Shift Project

I'm currently undertaking a project inspired by the course available on Udemy: [DevOps Projects](https://www.udemy.com/course/devopsprojects/).

## Project Overview

### Section 6: Lift and Shift Web App on AWS

This project involves migrating a multi-tier web application to AWS using the Lift and Shift approach.

#### Scenario:

The application is currently running on physical machines, leading to challenges such as complex management, upfront costs, scaling difficulties, manual processes, and a lack of automation.

#### Solution:

Utilizing AWS for pay-as-you-go pricing to address issues related to cost, flexibility, and modernization.

### Architecture

- EC2 instances for Tomcat, RabbitMQ, Memcache, and MySQL
- ELB as an NGINX replacement
- Autoscaling for VM scaling automation
- S3/EFS for storage
- Route 53 for private DNS service

#### Objectives:

- Flexibility
- No upfront costs
- Effective modernization
- Infrastructure as Code (IAAC)

#### Project Architecture Diagram

![Project Architecture](/week4/image1.png)

### Flow of Project Execution

1. **Login to AWS Account**
2. **Create Key Pairs**
3. **Create Security Groups**
4. **Launch Instances with User Data** (BASH SCRIPTS)
5. **Update IP to Name Mapping in Route 53**
6. **Build Application from Source Code**
7. **Upload to S3 Bucket**
8. **Download Artifact to Tomcat EC2 Instance**
9. **Setup ELB with HTTPS** (Certificate from Amazon Certificate Manager)
10. **Map ELB Endpoint to Website Name in Godaddy DNS**
11. **Verify**
12. **Build Autoscaling Group for Tomcat Instances**

## Summary

I completed the project, and the application is now accessible at [https://awsliftandshiftproject.sumamazaeem.com/](https://awsliftandshiftproject.sumamazaeem.com/).




##Section 7:   
Re-Architecture Web App on AWS Cloud [Cloud Native]

Reason:  
To improve agility

Scenario:
legacy servers, applications are running on  
multiple teams needed for this workload.  

Problem:  
Operational Overhead  
Struggling with uptime and Scaling  
Upfront capEx and Regular OpEx  
Manual Process/Difficult to automate  

Solution:
We will using PaaS and SaaS services in AWS.  

AWS Services:  
Using AWS Beanstalk (VM for Tomcat app server)  
  nginx replacement  
  auotmation for vm scaling  
  storage  

For Backend:  
RDS for Databases  
Elastic Cache for memchached  
Active Mq instead of RabbitMQ  
Route53 for DNS  
Cloudfront for Cdn  

objective:  
Flexible infra  
no upfront cost  
iaac  
paas  
saas  

 Comparison:  
 BEanStalk  Tomcat Ec2/VM  
 ELB in Baeanstalk Nginx LB/ELB  
 AUTOSCALING   None/Autoscaling  
 EFS/S3    NFS/S3/EFS  
 RDS   MYSQL ON VM/EC2  
 ELASTIC CACHE MEMCACHED ON VM/EC2  
 ACTIVE MQ RABIITMQ ON VM/EC2  
 ROUTE53  LOCAL DNS  
   
Architecture:  

EC2 INSTANCE  
ELB  
AUTOSCALING  
EFS/S3  
RDS  
ELASTIC CACHE  
ACTIVE MQ  
ROUTE53  
CLOUDFRONT 

![Image](/week4/image2.png)

FLOW OF EXECUTION

Login to aws account  

Create Key pair for beanstalk instance login  

Create Security Group for Elasticcache, RDS & ActiveMQ  

Create  

* RDS  

* Amazon Elastic Cache  

* Amazon Active MQ  

Create Elastic Beanstalk Environment  

Update SG of backend to allow traffic from Bean SG  

Update SG of backend to allow internal traffic  

Launch Ec2-Instance for DB Initializing  

Login to the instance and Inititialize RDS DB  

Change healthcheck on beanstalk to /login  

Add 443 https Listner to ELB  
 
Build Artifact with Backend Information  

Deploy Artifact to Beanstalk  

Create CDN with ssl cert  
  
Update Entry in GoDaddy DNS Zones  

Test the URL

Summary:
Successfully deployed the project, accessible on:
https://re-architecturing-web-app-sumama.sumamazaeem.com/


