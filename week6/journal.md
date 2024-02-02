This week, in week 6 I will be doing 3 Projects as follows
1. Continous Integration using Jenkins, Nexus, Sonarqube and Slack
2. Continous Integration on AWS Cloud
4. Continous Delivery of Java Web Application

# Continous Integration using Jenkins, Nexus, Sonarqube and Slack

## Project Overview

#### Scenario:

The project is in development and agile SDLC is used, dev makes regular code changes, and commits need to be built and tested continuously. Usually, the build and release team will do the job or developers' responsibility to merge and integrate code.

#### Problem:
Frequent code changes, depended on Q/A team. errors keep accumulating.
dev team needs to rework and fix the bugs and error. not fast possible as the test is done manually and inter-team dependencies

#### Solution:

Regular automated build and test for every commit and notify on every step, fix if any problem happens. The process is called Continuous Integration.

#### Benefits:

- Shorter MTTR
- Works well with AGILE
- NO Human intervention
- Fault Isolation

#### Tools:
- Jenkins
- Git
- Maven
- Checkstyle
- Slack
- Email notification integration
- Sonatype Nexus (Artifact/software repo)
- Sonarqube (Code Analysis server)
- AWS Ec2

#### Objective:
- Fault Isolation
- Short MTTR
- Fast Turn around on feature changes
- Less disruptive

### Workflow Diagram

![Project Workflow Diagram](/week6/workflow-diagram.png)

### Architecture

![Project Architectural Diagram](/week6/week6-p1-architectural-diagram.png)

### Flow of Project Execution
1. Login to AWS Account.
2. Create Your Key Pair.
3. Create Security Group.  
  a. Jenkins, Nexus and Sonarqube
4. Create EC2 Instances with User-data.  
  a. Jenkins, Nexus and Sonarqube  
5. Post Installation.  
  a. Jenkins setup and Plugin  
  b. Nexus Setup and repository setup  
  c. Sonarqube login test  
6. Git.  
  a. Create a github repository and migration code  
  b. integrate github repo with Vs-code and test it  
7. Build Job with Nexus integration.
8. Github Webhooks.
9. Sonarqube Server integration stage.
10. Nexus Artifact upload stage.
11. Slack Notification.

### Detailed explanation
I created the keypairs and security groups and added the required inbound rules as required for the traffic

Then I spin up the three ec2 inctances and wrote the scripts for 3 servers

Script for jenkins server

```
#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven wget unzip -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
###
```
Script for Nexus, select the centos for this one

```
#!/bin/bash
yum install java-1.8.0-openjdk.x86_64 wget -y   
mkdir -p /opt/nexus/   
mkdir -p /tmp/nexus/                           
cd /tmp/nexus/
NEXUSURL="https://download.sonatype.com/nexus/3/latest-unix.tar.gz"
wget $NEXUSURL -O nexus.tar.gz
sleep 10
EXTOUT=`tar xzvf nexus.tar.gz`
NEXUSDIR=`echo $EXTOUT | cut -d '/' -f1`
sleep 5
rm -rf /tmp/nexus/nexus.tar.gz
cp -r /tmp/nexus/* /opt/nexus/
sleep 5
useradd nexus
chown -R nexus.nexus /opt/nexus 
cat <<EOT>> /etc/systemd/system/nexus.service
[Unit]                                                                          
Description=nexus service                                                       
After=network.target                                                            
                                                                  
[Service]                                                                       
Type=forking                                                                    
LimitNOFILE=65536                                                               
ExecStart=/opt/nexus/$NEXUSDIR/bin/nexus start                                  
ExecStop=/opt/nexus/$NEXUSDIR/bin/nexus stop                                    
User=nexus                                                                      
Restart=on-abort                                                                
                                                                  
[Install]                                                                       
WantedBy=multi-user.target                                                      

EOT

echo 'run_as_user="nexus"' > /opt/nexus/$NEXUSDIR/bin/nexus.rc
systemctl daemon-reload
systemctl start nexus
systemctl enable nexus
```
Script for Sonarqube

```
#!/bin/bash
cp /etc/sysctl.conf /root/sysctl.conf_backup
cat <<EOT> /etc/sysctl.conf
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
EOT
cp /etc/security/limits.conf /root/sec_limit.conf_backup
cat <<EOT> /etc/security/limits.conf
sonarqube   -   nofile   65536
sonarqube   -   nproc    409
EOT

sudo apt-get update -y
sudo apt-get install openjdk-11-jdk -y
sudo update-alternatives --config java

java -version

sudo apt update
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
sudo apt install postgresql postgresql-contrib -y
#sudo -u postgres psql -c "SELECT version();"
sudo systemctl enable postgresql.service
sudo systemctl start  postgresql.service
sudo echo "postgres:admin123" | chpasswd
runuser -l postgres -c "createuser sonar"
sudo -i -u postgres psql -c "ALTER USER sonar WITH ENCRYPTED PASSWORD 'admin123';"
sudo -i -u postgres psql -c "CREATE DATABASE sonarqube OWNER sonar;"
sudo -i -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;"
systemctl restart  postgresql
#systemctl status -l   postgresql
netstat -tulpena | grep postgres
sudo mkdir -p /sonarqube/
cd /sonarqube/
sudo curl -O https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.3.0.34182.zip
sudo apt-get install zip -y
sudo unzip -o sonarqube-8.3.0.34182.zip -d /opt/
sudo mv /opt/sonarqube-8.3.0.34182/ /opt/sonarqube
sudo groupadd sonar
sudo useradd -c "SonarQube - User" -d /opt/sonarqube/ -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube/ -R
cp /opt/sonarqube/conf/sonar.properties /root/sonar.properties_backup
cat <<EOT> /opt/sonarqube/conf/sonar.properties
sonar.jdbc.username=sonar
sonar.jdbc.password=admin123
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.web.host=0.0.0.0
sonar.web.port=9000
sonar.web.javaAdditionalOpts=-server
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:+HeapDumpOnOutOfMemoryError
sonar.log.level=INFO
sonar.path.logs=logs
EOT

cat <<EOT> /etc/systemd/system/sonarqube.service
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096


[Install]
WantedBy=multi-user.target
EOT

systemctl daemon-reload
systemctl enable sonarqube.service
#systemctl start sonarqube.service
#systemctl status -l sonarqube.service
apt-get install nginx -y
rm -rf /etc/nginx/sites-enabled/default
rm -rf /etc/nginx/sites-available/default
cat <<EOT> /etc/nginx/sites-available/sonarqube
server{
    listen      80;
    server_name sonarqube.groophy.in;

    access_log  /var/log/nginx/sonar.access.log;
    error_log   /var/log/nginx/sonar.error.log;

    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    location / {
        proxy_pass  http://127.0.0.1:9000;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;
              
        proxy_set_header    Host            \$host;
        proxy_set_header    X-Real-IP       \$remote_addr;
        proxy_set_header    X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto http;
    }
}
EOT
ln -s /etc/nginx/sites-available/sonarqube /etc/nginx/sites-enabled/sonarqube
systemctl enable nginx.service
#systemctl restart nginx.service
sudo ufw allow 80,9000,9001/tcp

echo "System reboot in 30 sec"
sleep 30
reboot
```
I installed the Jenkins Server and installed the following plugins in it

![Plugins installed](/week6/plugins-installed.png)

Then I logged into nexus server and in repositories section, I created 4 repositories as shown by vprofile prefix

![Nexus Repo](/week6/nexus-repo.png)

Also, Sonar Server is also successfully running on port 8081

![Sonar-server](/week6/sonar-server.png)

Then, I used gitpod.io, forked the upstream repo and configured/connected it with github.

I installed the OracleJDK8 and OracleJDK11 and installed maven via tools

I added nexus credentials in security>credentials tab

Then, I created a ssh keys, added the public key on github and auth jenkins with private key

After a lot of debugging and successfully failing 14 builds and 4 hours of troubleshooting, I successfully managed to complete the jenkins pipeline. Learned a lot about debugging ways, reading documentations etc.

I configured the sonarqube, and other variables, connected the github webhook in such a way when a commit happens, he code gets pushed to jenkins, where it passes through following stages:

1. Declarative checkout SCM
2. BUILD
3. UNIT TEST
4. INTEGRATION TEST
5. CODE ANALYSIS WITH CHECKSTYLE
6. SONAR ANALYSIS
7. QULAITY CODE
8. UPLOAD ARTIFACT

The Jenkins file used is in my repo: https://github.com/sumamazaeem/vprofile-project/blob/ci-jenkins/Jenkinsfile

![Final Jenkins output](/week6/jenkins-pipeline-final.png)

![Nexus repo](/week6/nexus-repo-final.png)

In Next Step, I connected Slack for the notification system

![Slack](/week6/slack.png)

## Summary

Successfully the completed project.
---

# Continous Integration on AWS Cloud

## Project Overview

#### Scenario:

The project is in development and agile SDLC is used, dev makes regular code changes, and commits need to be built and tested continuously. Usually, the build and release team will do the job or developers' responsibility to merge and integrate code.

#### Problem:
Frequent code changes, depended on Q/A team. errors keep accumulating.
dev team needs to rework and fix the bugs and error. not fast possible as the test is done manually and inter-team dependencies

#### Problem with CI Server
Maintaince, operational overhead to maintain server like jenkins, nexus, sonarqube

#### Solution:

Cloud Services CI to remove OPS overhead

#### Benefits:

- Shorter MTTR
- Works well with AGILE
- NO Human intervention
- Fault Isolation

#### AWS Services
- Code Commit
- Code Artifact
- Code Build
- Code Deploy
- Cloud Pipeline

  other Services Used:
- Sonar Cloud(Non AWS Services)
- CHeckstyle(Non AWS Tool)

### Architecture

![Project Architecture](/week6/CIwithAWS-architecture.png)

### Flow of Execution

**AWS Setup Steps**

**1. Login to AWS Account**

**2. Code Commit Setup**

   - Create CodeCommit repo
   - Create IAM User with CodeCommit Policy
   - Generate SSH key locally
   - Exchange keys with IAM User
   - Put source code from GitHub repo to CodeCommit repository and push

**3. Code Artifact Setup**

   - Create an IAM User with CodeArtifact access
   - Install AWS CLI and configure
   - Export auth token
   - Update `settings.xml` file in source code top-level directory with provided details
   - Update `pom.xml` file with repository details

**4. Sonar Cloud Setup**

   - Create Sonar Cloud account
   - Generate token
   - Create SSM Parameters with Sonar details
   - Create Build project
   - Update CodeBuild role to access SSM Parameter Store

**5. Create Notification for SNS**

**6. Setup Build Project**

   - Update `pom.xml` with artifact version with timestamp
   - Create variables in SSM Parameter Store
   - Create Build Project
   - Update CodeBuild role to access SSM Parameter Store

**7. Create Pipeline**

   - CodeCommit
   - Test code
   - Build
   - Deploy to S3 bucket

**8. Test Pipeline**

In part 1 of the workflow, I created an IAM user, created a policy and attached it, downloaded the user's cli credentials, authenticated using the SSH key, and exchanged the key. then, I cloned the repo locally, removed the remote, git switched all the branches, so it is available locally, and then changed the .config file and added the URL of codecommit so that I could add the codecommit URL. I have successfully pushed all the branches of the local repo to codecommit.


---

# Another Project Section

## Section X: [Another Phase/Type]

[Follow the structure above for another phase or type of project.]
