I am following the projects from this course
https://www.udemy.com/course/devopsprojects/

The project I am working on is 

##Section 6: Lift and Shift Web App on AWS

Its a multi tier WEb app
we will use aws cloud
we will use lift and shift

scenario:
application is already running on physical machines

the problem:
managing is complex
upfront cost
scaling cost
manual process
difficult to automate
time-consuming

solution:
using cloud for pay-as-you-go pricing to save cost.

Architecture:  
We will be using EC2 instance for tomcat, rabbitmq, memchache and MySQL
ELB as replacement of NGINX REplacement
Autoscaling for automationfor VM Scaling
S3/EFS for storage
and route 53 for private DNS Service

objective:  
flexible 
no upfront cost
modernize effectively
IAAC

Diagram of the Project Architecture

![Image](/week4/image1.png)

The Flow of Porject Execution is as follows:
FLOW OF EXECUTION

1. Login to AWS Account

2. Create Key Pairs

3. Create Security groups

4. Launch Instances with user data [BASH SCRIPTS]

5. Update IP to name mapping in route 53

6. Build Application from source code

7. Upload to S3 bucket

8. Download artifact to Tomcat Ec2 Instance

9. Setup ELB with HTTPS [Cert from Amazon Certificate Manager]

10. Map ELB Endpoint to website name in Godaddy DNS

11. Verify

12. Build Autoscaling Group for Tomcat Instances

#Summary
Successfully did the project. The app is running on https://awsliftandshiftproject.sumamazaeem.com/



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

Test the url

