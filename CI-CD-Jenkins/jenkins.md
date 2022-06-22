Jenkins: (CD/CI)
=================

Q. What is Jenkins? 
-----------------------
Jenkins is one of the most popular automation tool used worldwide for continuous integration and continuous delivery. Jenkins is a free and open-source automation server that enables developers to build, integrate, and test code automatically as soon as it is committed to the source repository.

Q. Why Jenkins?
-----------------
When working on a project with different teams, developers often face issues with different teams using different CI tools, version management, and other tools. Setting up a CI/CD toolchain for each new project will lead to certain challenges like:
    * Slower Releases
    * Manual Builds
    * Non-repeatable processes
    * No Automation

Q. Why Jenkins?
-----------------
Jenkins is the solution to those challenges. its is an Open-source, 1000+ plugins, Free Paid, Enterprise. Jenkins is free and you don’t have to pay for anything.
Jenkins can be hosted on a **Virtual Machine**, **container** Or even locally for development purposes. and services like below:
    * Automated builds
    * Automated Tests
    * Automated CI/CD pipelines
    * Automated deployments
    * Ability to install Jenkins locally
    * Jenkins support and Plugin

What are the practical challenges during the code development & Testng?
-----------------------------------------------------------------------
There are challenges in software development like **slower releses, manual builds, humun errors, lack of automation, non-repetable tasks**. this makes the development of the software is slower. this can be corrected with **Jenkins** which provides **automated builds, automated tests, automated CI/CD pipelines, automated deployment, installed locally and plug-ins** which makes software development faster.

Q. What is CI & CD?
----------------------
**Continuous Integration( CI )** is a process in which the code is merged from multiple contributors and added to a single repository. In simple words, CI is a process to take the code package it and send it to the CD for further processing. **Continuous Deployment( CD )** is an automated process in which the code is taken from the repository and deployed to the system.

CI/CD in simple words is a process to take a code, package it up and deploy it to a system that can be serverless, a VM, or a container.

      * CI – Continuous Integration
      * CD – Continuous Delivery
      * CD – Continuous Deployment

Q. What are the Key process  of CI?
------------------------------------
Key Processes of Continuous Integration
      * Package up the code
      * Test the code (run unit tests, integration tests, etc)
      * Run security checks against the code
      
Q. what is Continuous delivery/deployment?
--------------------------------------------
The basic difference between Continuous Delivery and Continuous Deployment is that in Continuous Delivery to deploy the code after the CI process you have to manually trigger it via some button to deploy on the system whereas in Continuous Deployment this process is automatic. 

source code --> package code --> build code --> 

Q. what are the key process of CD?
-----------------------------------
Key process of Continuous Deployment
      * Ensure you're authenticated to the system or wherever you're deploying
      * Ensure that the code that's being deployed is working as expected once it's deployed

Q. How to install Jenkins?
----------------------------
Jenkins installtion require Java, we can follow Jenkins documentation to install it. 

```
    $ sudo yum install epel-release -y      --> 
    $ sudo yum install java -y              --> java installtion
    $ sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo --no-check-certificate    --> repo installtion
    $ sudo rpm --import http://pkg.jenkins.io/redhat-stable/jenkins.io.key      --> jenkin key for validation
    $ sudo yum install jenkins -y           --> install jenkins
```

Important configuration file are available /lib/systemd/system/jenkins.service file and change Jenkins port to 8090 by updating Environment="JENKINS_PORT=" variable value , It should look like this: Environment="JENKINS_PORT=8090"

```
    $ sudo vi /lib/systemd/system/jenkins.service
    $ sudo systemctl start jenkins      --> start Jenkins service 
    $ sudo systemctl status jenkins     --> status check 
    $ sudo systemctl restart jenkins    --> restart jenkins
```

Q. what is the use of Jenkins plug-ins ?
-----------------------------------------
Jenkins Plugins are used in Jenkins to enhance Jenkins functionality and cater to user-specific needs. Just like how Gmail, Facebook and LinkedIn help you connect your one service to another, plugins also work the same way and allow us to connect one service to other services and work with other products.

Q. How to install a new plugin in Jenkins?
-------------------------------------------
      1. Go to Manage Jenkins -> Manager Plugins
      2. Click Available and search for the desired plugin.
      3. Select the desired plugin and Install.

Note: Few plugins may need a restart To restart Jenkins `$ sudo systemctl restart jenkins`

Q. How to Update Plugins?
--------------------------
To update any existing plugin in Jenkins
      1. Go to Manage Jenkins -> Manager Plugins
      2. Click Updates and search for the desired plugin.
      3. Select the desired plugin and Install.
Note: Few plugins may need a restart To restart Jenkins `$ sudo systemctl restart jenkins`

Q. How to Delete Plugins?
----------------------------
To delete any plugin in Jenkins
      1. Go to Manage Jenkins -> Manager Plugins
      2. Go to Installed and search for the desired plugin.
      3. Click on uninstall button for the plugin you want to delete.

