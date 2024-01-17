I am following the projects from this course
https://www.udemy.com/course/devopsprojects/

The project I am working on is Section 6: Lift and Shift Web App on AWS

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







