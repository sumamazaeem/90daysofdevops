# 100daysofdevops
This is my journel of my devops learning journey and projects

I am following the projects from this course
https://www.udemy.com/course/devopsprojects/

The project I am working on is Lift and Shift Web App on AWS

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
time consuming

solution:
using cloud for pay as you go pricing to save cost.

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
<!-- section 6: 5:50 Min -->







