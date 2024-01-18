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



# Re-Architecturing Web App on AWS Cloud [Cloud Native]

## Section 7

### Reason for Re-Architecture:

To improve agility in the existing web application.

### Scenario:

Legacy servers and applications are currently running, requiring multiple teams to manage the workload.

### Problems:

- Operational Overhead
- Struggles with Uptime and Scaling
- Upfront CapEx and Regular OpEx
- Manual Processes/Difficult to Automate

### Solution:

Utilizing PaaS and SaaS services in AWS.

### AWS Services:

- AWS Beanstalk (VM for Tomcat app server)
  - Nginx replacement
  - Automation for VM scaling
  - Storage

### Backend Services:

- RDS for Databases
- Elastic Cache for Memcached
- Active MQ instead of RabbitMQ
- Route53 for DNS
- Cloudfront for CDN

### Objectives:

- Flexible infrastructure
- No upfront cost
- Infrastructure as Code (IAAC)
- PaaS
- SaaS

### Service Comparison:

| AWS Beanstalk            | Tomcat EC2/VM                |
|--------------------------|------------------------------|
| ELB in Beanstalk         | Nginx LB/ELB                 |
| Autoscaling              | None/Autoscaling             |
| EFS/S3                    | NFS/S3/EFS                   |
| RDS                      | MYSQL ON VM/EC2               |
| Elastic Cache            | Memcached ON VM/EC2           |
| Active MQ                | RabbitMQ ON VM/EC2            |
| Route53                  | Local DNS                    |

### Architecture:

- EC2 INSTANCE
- ELB
- AUTOSCALING
- EFS/S3
- RDS
- ELASTIC CACHE
- ACTIVE MQ
- ROUTE53
- CLOUDFRONT

#### Project Architecture Diagram

![Project Architecture](/week4/image2.png)

### Flow of Execution:

1. **Login to AWS account**
2. **Create Key pair for Beanstalk instance login**
3. **Create Security Group for Elastic Cache, RDS & ActiveMQ**
4. Create
    - RDS
    - Amazon Elastic Cache
    - Amazon Active MQ
5. Create Elastic Beanstalk Environment
6. Update SG of the backend to allow traffic from Bean SG
7. Update SG of the backend to allow internal traffic
8. Launch EC2-Instance for DB Initializing
9. Login to the instance and initialize RDS DB
10. Change health check on Beanstalk to /login
11. Add 443 https listener to ELB
12. Build Artifact with Backend Information
13. Deploy Artifact to Beanstalk
14. Create CDN with SSL cert
15. Update Entry in GoDaddy DNS Zones
16. Test the URL

## Summary:

Successfully deployed the project, accessible at [https://re-architecturing-web-app-sumama.sumamazaeem.com/](https://re-architecturing-web-app-sumama.sumamazaeem.com/).
