Jenkins by KK FUNDA
====================

Explain the flow diagram : developer --> GitHub --> maven --> SonarQube --> Nexus --> Tomcat  --> mail


Jenkins
=======

IQ] what is Jenkins?

-->Jenkins is an open source Continuous Integration tool, cross-platform tool, written in Java.

--> Kohsuke Kawaguchi is Creator of the Jenkins CI server in 2004

-->Initially, it was called Hudson, but in 2011 it was renamed to Jenkins















Advantages  of Jenkins
======================
--> It’s an open source tool with great community support.

--> Easy to install and It has a simple configuration through a web-based GUI, which speeds up the Job[deployment process]

--> It has around 1900+ plugins to ease your work. If a plugin does not exist, just code it up and share with the community (https://plugins.jenkins.io/).

--> Its built with Java and hence, it is portable on all major platforms.

--> Good documentation and enriched support articles/information available on internet which will help beginners to start easy.














Continuous Integration[CI]:
==========================

---> explain diagram 

Continuous Integration (CI) is the process of automating the build and testing of code every time a team member commits changes to version control.

(OR)

Continuous Integration is a development practice where developers integrate their code into a shared remote repository frequently, preferably several times a day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible.


CI Advantages
-------------
--> Immediate bug detection
--> Less Merge Conflicts
--> Deploy an application at any given point
--> Improved Code Quality [Consistent Testing, Code Reviews]
--> Developers receive immediate feedback on their code changes, allowing them to address issues promptly.

















Continuous Delivery:
====================
every successful build that has passed all the relevant automated tests and quality gates can potentially be deployed in to production via fully automated one click process.

diagram 
-------

code done --> unit tests --> Integrate --> Acceptance Test ---> Deploy into production
         AUTO           AUTO          AUTO                 MANUAL 

NOTE: Once we get the approval from the client and the sign-off mail from the QA team then we are going   to deploy









 

Continuous Deployment: 
======================

The practicing of automatically deploying every successful build directly into production without any manual steps knows as Continuous deployment.


code done --> unit tests --> Integrate --> Acceptance Test ---> Deploy into production
         AUTO           AUTO          AUTO                 AUTO 



IQ] Which one you are using continuous delivery or continuous deployment ?

ANS: I involved in many projects so we are using continuous deployment in "IN-HOUSE PROJECTS"   
     
      EX: Company internal Project : HR ---> 

    Coming to continuous delivery , We used for "client (external) projects"

     EX: jio, flipkart, amazon, etc







What Jenkins can do?


• Integrate with many different Version Control Systems (GitHub, BitBucket , GitLab, ...)
• Generate test reports (JUnit)
• Push the builds to various artifact repositories[nexus/jfrog]
• Deploys directly to production or test environments
• Notify stakeholders of build status (Through Email/slack,etc)














List of popular Continuous Integration tools
============================================

SNO     Product           Is Open Source?
===========================================
1       Jenkins              Yes  [community edition]
2       Cloudbees Jenkins    No   [Enterprise edition] 
2       Bamboo               No
3       Cruise Control       Yes
4       Travis CI            Yes and Paid also
5       Circle CI            Yes and Paid also
6       GitLab CI            Yes and Paid
7       TeamCity             Yes and Paid
















NOTE: default port for jenkins: 8080














Jenkins Installation
====================
prerequsite : JAVA, min: t2.medium

JAVA Installation
-----------------

step 1: launch an ec2 machine(t2.medium), connect to that machine

step 2: switch to root user [sudo su - ]





JENKINS Installation
====================

--> LTS[Long Term Support] ---> stable version without any issues.
--> search on google [ download jenkins ] --> LTS --> Redhat/fedora/centos


step 1: sudo su -

step 2: 

yum install wget tree -y


sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-17-openjdk
sudo yum install jenkins
sudo systemctl daemon-reload




step 3: 
 systemctl enable jenkins

step 4: 

  systemctl status jenkins
  systemctl start jenkins
  systemctl status jenkins

step 5: 

http://13.201.93.75:8080/  --> please make sure to enable 8080 port 

cat /var/lib/jenkins/secrets/initialAdminPassword

step 6:

click on suggested plugins

step 7:

user_name:  kkfunda
password: kkfunda
confirm password: kkfunda
Full Name: KK FUNDA

save And Continue  --> Save and finish --> start using jenkins 



========================================================================================



















SonarQube installation
======================

step 1: launch t2.medium machine [4GB]

step 2: connect to that server

step 3: switch to root user [ sudo su - ]

step 4: install java

   sudo yum install java-11-openjdk-devel -y
  
   javac --version

step 5: go to /opt dir

  cd /opt

step 6: yum install wget unzip tree -y

step 7: wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.6.1.59531.zip

step 7.1 : unzip sonarqube-9.6.1.59531

step 8: mv sonarqube-9.6.1.59531 sonarqube

step 9: 
 As a good security practice, SonarQuber Server is not advised to run sonar service as a root user, so create a new user called sonar user  and grant sudo access to manage nexus services as follows.


useradd sonar

step 10: 

Give the sudo access to sonar user

visudo

sonar   ALL=(ALL)       NOPASSWD: ALL

step 11:

Change the owner and group permissions to /opt/sonarqube/ directory.


chown -R sonar:sonar /opt/sonarqube/
chmod -R 775 /opt/sonarqube/
su - sonar
cd /opt/sonarqube/bin/linux-x86-64/

sh sonar.sh start
sh sonar.sh status

in real time same permission are given to the file ?



step 12: Now you can try to access the SonarQube Server.

NOTE: default port number for Sonarqube server: 9000

http://15.207.110.48:9000

NOTE: pls make sure to allow traffic for 9000 port.

step 13: refresh the page

  default credentials:  uname: admin   pwd: kkfunda

step 14: It will ask to change the password



========================================================================================

Nexus Installation starts
========================



step 1: Launch an EC2 instance [t2.medium]

step 2: connect to that server.

step 3: Before installation check the RAM once: " free -h "

step 4: sudo su -

step 5: check whether java is installed or not.

     javac -version

step 6: Install java
 
#Install JDK 1.8 
sudo yum install java-1.8.0-openjdk-devel -y

javac -version

step 7: 

cd /opt	

yum install tar wget tree -y

step 8: 



https://download.sonatype.com/nexus/3/nexus-3.70.1-02-java8-unix.tar.gz

wget https://download.sonatype.com/nexus/3/nexus-3.70.1-02-java8-unix.tar.gz


step 9: 

tar -zxvf nexus-3.70.1-02-java8-unix.tar.gz


step 10:

mv /opt/nexus-3.70.1-02 /opt/nexus



NOTE: As a good security practice, Nexus is not advised to run nexus service as a root user, so create a new user called nexus and grant sudo access to manage nexus services as follows.


step 11: 

useradd nexus

step 12: Give the sudo access to nexus user

visudo
nexus ALL=(ALL) NOPASSWD: ALL

step 13: Change the owner and group permissions to /opt/nexus and /opt/sonatype-work directories.

chown -R nexus:nexus /opt/nexus
chown -R nexus:nexus /opt/sonatype-work
chmod -R 775 /opt/nexus
chmod -R 775 /opt/sonatype-work


step 14: Open /opt/nexus/bin/nexus.rc file and  uncomment run_as_user parameter and set as nexus user.

vi /opt/nexus/bin/nexus.rc
run_as_user="nexus"

step 15: Create nexus as a service

ln -s /opt/nexus/bin/nexus /etc/init.d/nexus


step 16: Switch as a nexus user and start the nexus service as follows.

sudo su - nexus

step 17: Enable the nexus services

sudo systemctl enable nexus

step 18: Start the nexus service

sudo systemctl start nexus

step 19:  Access the Nexus server from Laptop/Desktop browser.

http://65.1.95.162:8081/

step 20: click on login

NOTE: Before 3.15 version uname and password is admin and admin123 , But now process changed.

cat /opt/sonatype-work/nexus3/admin.password

Uname: admin   pwd: kkfunda

===============================================================================================


Installation of tomcat
======================

prerequisites 
-------------
java 1.8 min required for tomcat 9.x version.

1 GB RAM is enough, because  we are running small web application.

step 1: launch one ec2 m/c for tomcat 

step 2: connect to that server

step 3: sudo su -

step 4: install java
   sudo yum install java-11-openjdk-devel -y
   javac --version

step 5: yum install wget unzip -y

step 6: cd /opt/
   

     wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.100/bin/apache-tomcat-9.0.100.zip
     
step 7: unzip apache-tomcat-9.0.91.zip

 



step 8: goto the bin directory.
     
       cd /opt/apache-tomcat-9.0.91/bin

step 9: chmod u+x *.sh


IQ] How to start/stop the tomcat server?

    step 1: go to bin directory 

    step 2: sh startup.sh    ----> to start
                or
            sh catalina.sh start ---> to start

          sh shutdown.sh  ----> to stop 
               or
          sh catalina.sh stop ----> to stop


NOTE: By default port number of tomcat : 8080




IQ] How to check tomcat is running or not?

    sudo yum install lsof
    lsof -i :8080


   ps -ef|grep -i "tomcat"







IQ] I want to start/stop tomcat server from anywhere instead of bin dir?


ln -s /opt/apache-tomcat-9.0.91/bin/startup.sh /usr/bin/startTomcat
ln -s /opt/apache-tomcat-9.0.91/bin/shutdown.sh /usr/bin/stopTomcat

Now you can go and run in any directory.


http://13.201.60.70:8080/












IQ] How to access tomcat server from PC?

NOTE: default port number for tomcat server is 8080.

 http://65.2.129.199:8080  ---> directly you cant access without enable that port number     				in security group.

  select the server --> go to security --> scroll down --> click on security group ---->
  click on edit in-bound rules ---> click on add rule ---> select custom tcp ---> give port 8080 ----> select anywhere ipv4 ---> save the rules.

Then reload this url http://13.233.32.177:8080

click on server Status, Manager App, Host Manager ----> not able to access.


====================================================================


--> we can place our application into "Manager App"
                                       ===========

step 1: cd /opt/apache-tomcat-9.0.91/webapps

step 2: cd manager 

step 3: cd META-INF

step 4: ls ----> context.xml

step 5: vi context.xml
    
      :se nu
      comment 21st and 22nd lines.
     <!--
     -->
step 6: refresh the main page ----> It will ask the credentials.

step 7: cd /opt/apache-tomcat-9.0.91/conf/ 

step 8: vi tomcat-users.xml ---> goto end of the file and add users.

  <user username="kkfunda" password="kkfunda" roles="manager-gui,admin-gui"/>

  <user username="mahesh" password="mahesh" roles="manager-gui,admin-gui"/>



if you want to login Host-manager do the same process?
                     ============
step 1: cd /opt/apache-tomcat-9.0.91/webapps

step 2: cd host-manager 

step 3: cd META-INF

step 4: ls ----> context.xml

step 6: vi context.xml

step 7: vi context.xml

      comment 21st and 22nd lines.
     <!--
     -->
step 8: refresh the main page ----> It will ask the credentials, give the credentials.

step 9: go and check the logs directory logs are created or not

       cd /opt/apache-tomcat-9.0.91/logs

  uname: admin	pwd: kkfunda


=================================================================================================

Automate all the tasks using jenkins
=====================================

How to create a job
===================

Jenkins DashBoard --> New item --> enter an item name(jio-dev) --> freestyle project --> ok --> general --> description --> scroll down --> source code management -->  select git(GitHub,bitbucket,gitlab) --> repository url --> After that we will get an error --> pls install git in jenkins server --> yum install git -y --> save --> click on configure for any updates --> 


How to add git credentials
--------------------------

click on add --> uname: kkeducation12345 pwd:   --> Description --> github credentials --> Add 

--> branches to build --> */development --> scroll down --> build -->add build --> Invoke maven toplevel targets ---> goals --> save 


--> for run the job click on job --> In left side click on buid now --> failed --> click on job --> console output --> Due maven not instaled uild is failing 

--> check maven is installed or not in jenkins server 

  mvn -version


IQ] Where we need to install maven in linux server or in the jenkins server itself ?

ANS: If you go for linux server only one maven install is possible but in jenkins many versions possible so we are going to install in the jenkins server.



How to download maven in jenkins ?
==================================

go to jenkins dashboard --> manage jenkins --> tools -->scroll down --> add maven -->  maven 3.9.8 --> select version 3.9.8 --> add many versions --> save 

click on job --> configure --> scroll down --> build steps -->  select maven version --> save 

--> again run the job --> It will success 


/var/lib/jenkins/workspace/jio-dev/target/ --> warfile location




SonarQube server integration
============================

take ip address of SonarQube server [http://3.110.105.157:9000/] --> go to GitHub repository --> select the correct branch [development] --> pom.xml --> edit the file --> in <properties> tag keep sonarqube server details 


--> go to job --> configure --> scroll down --> build [ clean package sonar:sonar ] --> save

--> go and see the sonarqube server --> check the projects --> now build the job --> success.





Nexus integration with jenkins
===============================

step 1: connect to maven server, Where java projects are available

step 2: where to keep our repository details?

update the details in pom.xml

          <distributionManagement>

            <repository>
              <id>nexus</id>
              <name>KK FUNDA Releases Nexus Repository</name>
              <url>http://13.233.250.219:8081/repository/jio-release/</url>
            </repository>

            <snapshotRepository>
              <id>nexus</id>
              <name>KK FUNDA Snapshot Nexus Repository </name>
              <url>http://13.233.250.219:8081/repository/jio-snapshot/</url>
            </snapshotRepository>

        </distributionManagement>

step 3: Where we need to keep nexus credentials?

--> In SonarQube we updated credentials in pom.xml


NOTE: maven is installed in jenkins pls check correct path of settings.xml


find / -name settings.xml

--> In nexus, repo details in pom.xml and credentials in 
 cd /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven_3.9.8/conf/


   <server>
      <id>nexus</id>
      <username>admin</username>
      <password>kkfunda</password>
    </server>


--> click on job --> configure --> build --> clean deploy sonar:sonar --> save build now 













SCM Polling
============

1)Jenkins polls SCM based on a defined schedule

2)If new commits are found, the build is triggered.

Pros: Always builds the latest code changes
Cons: Frequent polling can use a lot of server resources.


Periodic Builds
===============

1)Jenkins builds at specified intervals, regardless of code changes,
 
2)You set a cron-like schedule for builds,Builds occur based on this schedule

Pros: Regular builds, independent of code changes.
Cons: May build the same code if no new commits exist



GitHub Webhook
==============

GitHub notifies Jenkins of code changes immediately.

Pros: Immediate build triggering, reducing lag.

Cons: Requires proper webhook configuration.

webhook congiguration
====================== 
 setup build config -> click -GitHub Webhook
Go to ->github ->select your repository  nd branch -> on right side click on settings->select webhooks -> payload block  fill the jenkins <url>http://65.1.95.162:8080/github-webhook/

nd select contextpath : json --> click addit


Code Coverage
=============
Tool: JaCoCo

Goal: Ensure code coverage is at least 80%.


1)Identify untested code using JaCoCo reports.

2)Write more tests for uncovered areas.

NOTE:Set Jenkins to fail builds if coverage is below 80%



discard old builds
===================

create an another job with same configuration of other job
==========================================================



Jenkins server stop and start issue
====================================

old: 52.66.196.163

new: 65.2.171.155


/var/lib/jenkins/jenkins.model.jenconf.xml   ---> update new ip here and restart jenkins


NOTE: remove tomcat,SonarQube and nexus --> explain this

Why Do We Need to Enable and Disable Jobs in Jenkins (Freestyle Jobs)?

=============================================================================


In Jenkins Freestyle Jobs, enabling and disabling jobs is useful for managing workflows, troubleshooting issues, and optimizing performance. Here’s a structured explanation you can use for your students:


 1. What Happens When You Disable a Job in Jenkins?
 -----------------------------------------------------

- Disabling a job prevents it from executing even if it is triggered by:
  - A manual build request  
  - A scheduled cron trigger  
  - A webhook (e.g., GitHub, GitLab, Bitbucket)  
  - Another pipeline or upstream job  

- The job's configuration remains intact, but it won’t run until re-enabled.


 2. Why Would You Disable a Freestyle Job?
 -------------------------------------------

 🔹 Maintenance Mode (Updating Configurations)

- If you are making changes to a job’s script, repository, or settings, disabling it prevents accidental runs.
- Example: If you are updating a deployment job and don’t want it to trigger mid-update, disable it first.

 🔹 Debugging & Troubleshooting

- If a job is failing continuously due to a configuration issue, disabling it helps you investigate without generating unnecessary failures.
- Example: If a build is failing due to a missing dependency, disable it until the issue is resolved.

 🔹 Preventing Accidental Builds

- Sometimes, a job should not run temporarily but should not be deleted either.
- Example: If a project is paused for client review, disable the job to prevent unnecessary builds.

 🔹 Reducing Server Load

- Running too many jobs at once can overload the Jenkins server. Disabling unused or less critical jobs helps optimize performance.
- Example: If a large-scale deployment is in progress, you might disable non-essential jobs to free up system resources.


 3. Why Would You Enable a Job Again?
 -------------------------------------

 🔹 Ready for Execution After Fixing Issues
- If a job was disabled due to bugs, errors, or missing dependencies, re-enable it after fixing the problem.

 🔹 Restarting a Paused Project

- If development resumes on a paused project, you enable the job to resume automated builds.

 🔹 Re-Enabling After Jenkins Restart

- Some jobs may get disabled due to system failures or updates. Enabling them ensures they function normally again.


 4. How to Enable or Disable a Freestyle Job in Jenkins?
 --------------------------------------------------------

 🔸 Using the UI
1. Go to Jenkins Dashboard
2. Click on the Job Name
3. On the left panel:
   - Click "Disable Project" (to disable)
   - Click "Enable" (to enable again)


 5. Real-World Example for Students
 ------------------------------------

Scenario: CI/CD Pipeline with Deployment Job
- A Freestyle Job is set to deploy the application to a test server.
- Developers need to make major changes to the test environment.
- Instead of deleting the job, they disable it to prevent unwanted deployments.
- Once the environment is ready, they enable the job again.


 6. Best Practices for Managing Enabled/Disabled Jobs
 -----------------------------------------------------

✅ Label Disabled Jobs Clearly → Add descriptions explaining why a job is disabled.  
✅ Monitor Disabled Jobs → Remove old jobs if they are permanently disabled.  
✅ Use Folders for Organization → Keep active and inactive jobs organized.  
✅ Avoid Disabling Critical Jobs Without Notice → Inform team members before disabling jobs that impact deployments.  




Plugins in jenkins server
=========================

Deploy to container*
===================

--> Deploy the our application into the tomcat server 


maven integration
==================

if you want to create a maven project type then use this plugin.


safeRestart
===========

systemctl restart jenkins ---> It is in linux server, But we dont have permission to access it , Then how to restart it.


restart --->http://65.0.199.60:8080/restart

safeRestart -->http://65.0.199.60:8080/safeRestart

IQ] what is the difference between restart and safeRestart ?

restart: It will stop the running jobs and then restarted

safeRestart: It will wait for complition of all the jobs and restarted


NOTE: for safeRestart we have plugin called as a safeRestart, Install and try it




next Build Number
=================

--> After insatlling this plugin we can give our own build number, The next number must be greater than the previous build number only.

NOTE: in linux server /var/lib/jenkins/jobs/jio-dev/jobs/nextBuildNumber file is available , But some time times we dont have an access to linux server to use this



JaCoCo
======

By using this plugin we can stop the deployment through code coverage percentage.


ssh agent 
=========
we will discuss in the pipeline project

Audit Trail ***
===========

I will help us to track the jenkins activity like create,delete jobs, etc

--> install the plugin

--> dashboard --> manage jenkins --> system --> audit trail --> add logger --> log file --> log rotation: /var/lib/jnkins/audit-trail.log
    size in MB: 20
    Log file count: 20



audit-trail.log.0

audit-trail.log.1

audit-trail.log.2

audit-trail.log.3

tail -f audit-trail.log.0  --> go and trigger the build





Blue ocean plugin
=================


Build name Description setter
=============================

step 1: Install plug-in

step 2 : Dash board --> job --> configuration --> Build Environment --> select Set Build Name --> jio-dev-#${BUILD_NUMBER}


Thin backup 
===========
will discuss in jenkins backup

Role based authentication
=========================
will discuss in jenkins security


=====================================================================================

Build with parameters
======================

Dashboard --> job --> configuration --> General --> select This Project is parametirized --> add parameter --> choice parameter -->

   names: BranchNames
   Choices: main 
            dev
            qa
--> save 


NOTE: under the source code section pass the names 
    
      */${BranchNames}



How to create a view?
=====================











How to integrate with slack?
=============================

step 1: login slack.com

step 2: channels --> chanel --> create channel --> channel name --> next --> public --> create

step 3: 

    KK FUNDA --> Invite pople to kk funda --> copy invite link 

   https://join.slack.com/t/kkfunda/shared_invite/zt-2nxl4nw11-LipxXXbLvOotWPmIIPjFgw


step 4: KK FUNDA --> tools and settings --> manage apps --> search with "jenkins ci" --> click on "Add to slack" --> select a channel --> Add jenkins ci integration --> 

https://kkfunda.slack.com/services/B08FJJN9ELX?added=1

step 5: install slack notification plugin in jenkins

step 6: manage jenkins --> configure systems --> slack --> workspace: kkfunda -->

   credentials: Add credentials --> Secret text --> paste the secret[lHCwhkIRdaeAqZlRaYEqi2mQ] --> add the description

    default channel: #jio ---> Test connection --> save

step 7: 

select job --> configure --> scroll down to end --> post build actions --> select slack notifications 

--> select slack notifications --> advanced --> select options[enter the specific channel-id] --> save 


======================================================================================

Jenkins security
================

--> procedure of providing security to the jenkins server called as jenkins security.

IQ] where the jenkins users information will store?

Ans: /var/lib/jenkins/users/users.xml


For example
===========

kkfunda --> devops engineer
prasanth --> devops engineer
mani --> devops engineer

jaswanth --> developer
raghu    --> developer

IQ] How to create a user in jenkins?

manage jenkins --> manage users --> create users --> 

  uname :prasanth
  pwd   :kkfunda
  conform pwd: kkfunda
  full name: prasanth
  email: abc@gmail.com

NOTE: by default every user has admin access , How to give read access to developers?

--> [Install role based authorization startegy  plugin]

--> login as a admin	--> manage jenkins --> security --> security --> authentication --> Authorization 

--> select Project based matrix Authorization Strategy

NOTE: add all users and give the permissions, check the activities.

NOTE: pls give job level acces to users.









Freestyle job
Maven job

NOTE: we can not customize the job using maven and free style[ EX: order of exexution]

Create the pipeline project job
===============================

--> Pipeline scripts can be written in the groovy script.

--> we can write in two ways
   1. scripted way
   2. declarative way


NOTE: pls install stageview plug-in

1. scripted way
===============

syntax:
=======

node //by default  built-in(master) node
{

//write code here.

}

step 1: create a pipeline job

goto new item --> Enter an item name(demo-scripted-pipeline) --> pipeline --> ok

step 2: get the code from github repository

select pipeline-job--> configure --> scrool down to pipeline area --> click on pipeline syntax --> sample step --> git: Git

   Repo url: 
   branch:
   credentials:
   
--> generate pipeline script



node
{
   stage('checkout')
   {
   git branch: 'development', credentialsId: 'e5b73679-3105-4429-a5b4-b303fd9e35a0', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
   }
}


step 3: 

node
{
   def mavenHome=tool name: "maven 3.9.8"
   stage('checkout')
   {
   git branch: 'development', credentialsId: 'e5b73679-3105-4429-a5b4-b303fd9e35a0', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
   }
   stage('build')
   {
  sh "${mavenHome}/bin/mvn clean package"
   }
  
}


step 4: execute the sonarqube report

node
{
   def mavenHome=tool name: "maven 3.9.8"
   stage('checkout')
   {
   git branch: 'development', credentialsId: 'e5b73679-3105-4429-a5b4-b303fd9e35a0', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
   }
   stage('build')
   {
  sh "${mavenHome}/bin/mvn clean package"
   }
  stage('SonarQubeReport')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar"
   }
  
}


step 5:  upload artifact into nexus

node
{
   def mavenHome=tool name: "maven 3.9.8"
   stage('checkout')
   {
   git branch: 'development', credentialsId: 'e5b73679-3105-4429-a5b4-b303fd9e35a0', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
   }
   stage('build')
   {
  sh "${mavenHome}/bin/mvn clean package"
   }
  stage('SonarQubeReport')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar"
   }

 stage('Uploadarifacttonexus')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
   }
  
}



step 6: deploy application into tomcat server

--> Deploy the application through admin console
--> copy paste the war file into the webapps directory

--> But Jenkins is in different server and tomcat is in different server.

NOTE: now you need to copy file from jenkins server to tomcat server with the help of SCP command , But we need tomcat credentials to pass.

--> Jenkins unable to enter the credentials, So we need to generate the one "SSH token"

--> insatall the "ssh agent" plugin 

--> select the pipeline job --> pipeline syntax --> snippet generator --> [refresh that page] --> sample step  --> sshagent :SSH Agent

--> copy the penfile of tomcat server --> add --> jenkins --> username with private key --> 

description: tomcat credentials
user-name: ec2-user
private key --> enter directly --> add --> paste pem --> add --> generate pipeline script --> add in the stage.


node
{
   def mavenHome=tool name: "maven 3.9.8"
   stage('checkout')
   {
   git branch: 'development', credentialsId: 'e5b73679-3105-4429-a5b4-b303fd9e35a0', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
   }
   stage('build')
   {
  sh "${mavenHome}/bin/mvn clean package"
   }
  stage('SonarQubeReport')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar"
   }

 stage('Uploadarifacttonexus')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
   }

   stage('deployapptotomcat')
   {
     sshagent(['ff0ba357-7dea-4d4c-bf8f-daffd5c1b173']) 
    {
    
     sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.201.194.103/:/opt/apache-tomcat-9.0.91/webapps/"


     }

     sshagent(['29ffdf5c-84b7-4a5a-aade-e5ca6dd35b6f']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.201.194.103:/opt/apache-tomcat-9.0.91/webapps/"
}
   }
  
}


--> After building you will get an error; permission denied [for ec2-user no permission on webapps directory] , Now give the full permissions to webapps directory.

chmod -R 777 webapps

--> again build it will be sucess.



================================================================================================


How to configure poll SCM?
==========================

jenkins --> select pipeline job --> pipeline syntax --> snippet generator --> properties: Set job properties --> select discard old builds

           --> poll scm
               * * * * *
---> generate script --> copy --> paste at node level



//This is scripted pipeline sample
node
{

  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * * ')])])
  //   /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9.9/bin

   def mavenHome=tool name:  "maven-3.9.9"
  stage('git checkout')
  {
  git branch: 'development', credentialsId: 'd8f5bb40-ee7a-41f2-a72d-0fcc8eb6b07b', url: 'https://github.com/kkeducation12345/maven-web-app-project-kk-funda.git'
  }
  stage('Build')
  {
     sh "${mavenHome}/bin/mvn clean package"
  }
  stage('SQ Report')
  {  
     sh "${mavenHome}/bin/mvn clean sonar:sonar"   
  }

  stage('upload artifactt')
  {
    sh "${mavenHome}/bin/mvn clean deploy"
  }

    stage('Deploy to Tomcat') {
        echo "Deploying WAR file using curl..."

        sh """
            curl -u kkfunda:kkfunda \
            --upload-file /var/lib/jenkins/workspace/jio-scripted-way-pipeline-develoment/target/maven-web-application.war \
            "http://3.108.194.157:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }


}    
}






Global variable reference
==========================

jenkins --> select pipeline job --> pipeline syntax--> global variable reference --> click here

node
{
   echo "git job name: ${env.JOB_NAME}"
   echo "build number is: ${env.BUILD_NUMBER}"
   echo "node name is: ${env.NODE_NAME}"
}



NOTE: In real time jenkins script is placed in the github 


--> copy file --> add file in git hub[Jenkinsfile] --> paste --> goto jenkins job --> pipeline Definition --> select pipeline Script from SCM.


IQ] Is it possible to give the custom name of jenkins file?

ANS: yes

--> edit file name in github --> and trigger the build --> error: unable to find the jenkins file


-->select job --> configure --> change the script path with latest name --> save.


--> Then trigger the build.





slack notifications
===================

node {

   echo "git branch name: ${env.JOB_NAME}"
   echo "build number is: ${env.BUILD_NUMBER}"
   echo "node name is: ${env.NODE_NAME}"
    
    def mavenHome=tool name: "maven3.9.9"

    try {

	stage('git checkout')
	{
      git branch: 'development', credentialsId: '8d341570-e046-4d2c-8060-33c3d5d493f7', url: 'https://github.com/gangavaramdevops/maven-web-app-project-kk-funda.git'
	}
	stage('compile')
	{
    sh  "${mavenHome}/bin/mvn compile"
	}
	stage('Build')
	{
    sh  "${mavenHome}/bin/mvn clean package"
	}
   stage('SQ Report')
	{
    sh  "${mavenHome}/bin/mvn sonar:sonar"
	}
	stage('Deploy into Nexus')
	{
    sh  "${mavenHome}/bin/mvn clean deploy"
	}
	stage('Deploy to Tomcat') {
        echo "Deploying WAR file using curl..."

        sh """
            curl -u kkfunda:kkfunda \
            --upload-file /var/lib/jenkins/workspace/jio-scripted-way-pipeline-develoment/target/maven-web-application.war \
            "http://3.108.194.157:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }

  } catch (e) {
    // If there was an exception thrown, the build failed
    currentBuild.result = "FAILED"
    throw e
  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}








NOTE: To display the Build started message, pls call notifyBuild('STARTED') before checkout stage.



Declarative pipelines
======================


pipeline {
    agent any 
tools
{
  maven "maven3.9.9"
}

    stages {
        stage('checkout stage') { 
            steps {
           git branch: 'development', credentialsId: '8d341570-e046-4d2c-8060-33c3d5d493f7', url: 'https://github.com/gangavaramdevops/maven-web-app-project-kk-funda.git'
            }
        }
            stage('Build') { 
            steps {
         sh "mvn clean package"
                
            }
          }
          stage('SQREPORT'){
            steps{
          sh "mvn sonar:sonar"
          }
        }
         stage('DeployTONexus'){
           steps{
          sh "mvn deploy"
          }
        }
        stage('DeployAppToTomcat'){
           steps{

		echo "Deploying WAR file using curl..."

        sh """
            curl -u kkfunda:kkfunda \
            --upload-file /var/lib/jenkins/workspace/jio-scripted-way-pipeline-develoment/target/maven-web-application.war \
            "http://3.108.194.157:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
         
          }
        }
    } //stages closing

post {
  success {
    notifyBuild(currentBuild.result)
  }
  failure {
notifyBuild(currentBuild.result)
      }
}
}//pipeline closing


def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#airtel-dev')
}





