
Jenkins: (CD/CI):
==================================================================

Q. What is Jenkins? 
--------------------------------------
Jenkins is one of the most popular automation tools, that used for continuous integration and continuous delivery and Continuous Deployment. Both CI and CD aim to accelerate software delivery, improve quality, and enable rapid, iterative development.

Jenkins is a free and open-source, that enables developers to package, build, integrate, and test code automatically as soon as it is commits the source code in the repository. it perform security checks and deploy the code. With the help of jenkins you can automate the continuous intigration, delivery and deployments.  

Alternate options for Jenkins: 	
	GitLab CI/CD, Bamboo, TeamCity, Azure DevOps, Travis IC, Circle CI, Go CD, Codeship, Jenins X, Drone.  

Q. Jenkins lifecycle?                                         
---------------------------------------
Jenkins is a powerful automation server, manages a code lifecycle by orchestrating various stages from code development to deployment. Here’s an elaboration of the Jenkins code lifecycle.

Development:
	Code Creation: Developers write and modify code on their local machines.
	Version Control: The code is committed to a version control system (e.g., Git, SVN) where Jenkins can access it.
 
Triggering:
	Code Change Event: Jenkins is configured to listen for events (like commits or pull requests) in the version control system.
	Build Trigger: When a change is detected, Jenkins triggers a build process based on defined criteria (e.g., specific branches, specific files).
 
Checkout:
	Workspace Creation: Jenkins creates a dedicated workspace for the build.
	Code Retrieval: Jenkins pulls the code from the version control system to the workspace.
 
Build:
	Compilation: If applicable, the code is compiled into an executable or deployable format.
	Testing: Automated tests (unit tests, integration tests, etc.) are executed to ensure code quality.
	Artifact Generation: Build artifacts like binaries, packages, or documentation are created.
 
Quality Checks:
	Static Analysis: Tools like SonarQube, Checkstyle, or PMD are used to analyze code quality, identify bugs, or enforce coding standards.
	Code Coverage: Measure how much of the code is covered by tests.
 
Deployment:
	Deployment Process: If the build passes all checks, Jenkins can initiate deployment to various environments (e.g., staging, production).
	Configuration Management: Jenkins can manage configurations for different environments or deployment targets.
 
Monitoring and Reporting:
	Build Logs and Reports: Jenkins generates logs and reports for each build, providing detailed information about the process and any issues encountered.
	Notifications: Notify stakeholders about build success/failure via email, Slack, or other communication channels.
 
Post-Build Actions:
	Artifact Archiving: Store generated artifacts for future use or reference.
	Trigger Downstream Jobs: Trigger other Jenkins jobs or workflows based on the build status.
	Clean-up: Optionally clean up workspace and resources used during the build.
 
Maintenance:
	Monitoring and Optimization: Continuous monitoring and optimization of Jenkins configurations and pipelines to ensure efficiency and reliability.
	Pipeline Updates: Update Jenkins pipelines to incorporate changes in the development process or technology stack.
 
This lifecycle demonstrates how Jenkins automates and orchestrates different stages of code development, testing, and deployment, promoting continuous integration and continuous delivery (CI/CD) practices.

Q. Jenkins role as a DevOps engineers job?
-----------------------------------------------------
Installation and Configuration:
	Setting up Jenkins, configuring master and agent nodes.
	Understanding system requirements, installation methods, and initial setup steps.
 
Pipeline as Code:
	Creating and managing pipelines using Jenkinsfile (Declarative or Scripted Pipeline).
	Defining stages, steps, and conditions for automation, enabling CI/CD processes.
 
Plugins and Integrations:
	Utilizing and managing plugins for source control systems (e.g., Git, SVN), build tools (e.g., Maven, Gradle), testing frameworks, deployment tools (e.g., Docker, Kubernetes), and more.
	Integrating with other tools and services using Jenkins plugins for enhanced functionality.
 
Job Types and Configurations:
	Creating various job types (freestyle, pipeline) to automate build, test, and deployment processes.
	Configuring job parameters, triggers, and post-build actions for specific requirements.
 
Version Control Integration:
	Integrating Jenkins with different version control systems (e.g., Git, Bitbucket) to fetch, track, and manage code for builds.
 
Distributed Builds:
	Setting up and managing distributed build environments, including master-slave configurations for distributing build workloads across multiple machines.
 
Security and Access Control:
	Implementing security measures within Jenkins, managing user access, and configuring authentication methods to ensure a secure environment.
 
Monitoring and Logging:
	Monitoring Jenkins builds, analyzing logs, and using monitoring tools to troubleshoot issues, optimize performance, and ensure smooth operation.
 
High Availability and Scalability:
	Implementing strategies to make Jenkins highly available and scalable, including load balancing and redundancy configurations.
 
Containerization and Orchestration:
	Integrating Jenkins with containerization tools like Docker and orchestration tools like Kubernetes for efficient application deployment and management.
 
Backup and Recovery:
	Setting up backup plans and recovery strategies for Jenkins configurations, jobs, and settings in case of failures or data loss.
 
Best Practices and Optimization:
	Following best practices for Jenkins usage, optimizing build pipelines, minimizing build times, and efficiently using resources to improve overall performance.
 

These detailed topics form the core knowledge base essential for a DevOps engineer working with Jenkins, empowering them to design, implement, and maintain robust CI/CD pipelines and automation workflows.

Q. Why Jenkins?
-----------------
Before jenkins, When working on a project with different teams, developers often face issues with different teams using different CI tools, version management, and other tools. Setting up a CI/CD toolchain for each new project will lead to certain challenges like

    * Slower Releases
    * Manual Builds
    * Non-repeatable processes
    * No Automation

Q. Why Jenkins?
-----------------
Jenkins is the solution to those challenges. its is an Open-source, 1000+ plugins, Free Paid, Enterprise. Jenkins is free and you don’t have to pay for anything. Jenkins can be hosted on a **Virtual Machine**, **container** Or even locally for development purposes. and services like below.

    * Automated builds
    * Automated Tests
    * Automated CI/CD pipelines
    * Automated deployments
    * Ability to install Jenkins locally
    * Jenkins support and Plugin

What are the practical challenges during the code development & Testng?
-----------------------------------------------------------------------
There are challenges in software development like **slower releses, manual builds, humun errors, lack of automation, repetable tasks**. this makes the development of the software is slower. this can be corrected with **Jenkins** which provides **automated builds, automated tests, automated CI/CD pipelines, automated deployment, installed locally and plug-ins** which makes software development faster.

Q. What is CI & CD?
----------------------
**Continuous Integration( CI )** is a process in which the code is merged from multiple contributors and added to a single repository. In simple words, CI is a process, take the code and package it, test it, do security checks and  send it to the CD for further processing. 

**Continuous Deployment( CD )** is an automated process in which the code is taken from the repository and deployed to the system.

CI/CD in simple words is a process that takes the code, package, test, do security check and deploy it to a system that can be serverless, a VM, or a container.

      * CI – Continuous Integration
      * CD – Continuous Delivery
      * CD – Continuous Deployment

source code --> package code --> build code --> test code --> run security checks --> deploy code -->VM/Docker

Q. What are the Key process  of CI?
------------------------------------
Continuous Integration (CI): 
	Code Integration: 	Developers frequently merge their code changes into a shared repository, ensuring that the codebase is continuously updated and integrated.
	Automated Builds: 	Automatically building the application whenever new code changes are detected. This involves compiling code, running tests, and generating artifacts.
	Automated Testing: 	Executing automated tests (unit tests, integration tests, etc.) to validate changes and ensure they don’t break existing functionality.
	Code Quality Checks: 	Running static code analysis tools to maintain code quality standards, identify bugs, and enforce coding guidelines.
	Early Feedback: 	Providing quick feedback to developers about the success or failure of their code changes, enabling rapid bug fixes and improvements.
      
Q. what is Continuous delivery/deployment?
--------------------------------------------
Continuous Delivery (CD) and Continuous Deployment (CD):
	Deployment Automation: 			Automatically deploying applications or changes to various environments, ensuring consistency and reliability in the deployment process.
	Environment Configuration: 		Managing and configuring different environments (development, staging, production) consistently and reliably.
	Rollback and Rollforward Strategies: 	Implementing strategies for reverting changes (rollback) or moving forward (rollforward) in case of deployment failures or issues.
	Release Orchestration: 			Orchestrating the release process, including approval gates, notifications, and monitoring to ensure a smooth and controlled release.
	Automated Monitoring and Feedback: 	Setting up automated monitoring tools to track application performance and gather feedback post-deployment.
	Feedback Loop and Improvement: 		Collecting feedback from production environments to drive continuous improvements in the development and deployment processes.

In summary, CI focuses on integrating code changes frequently, running tests, and maintaining code quality, while CD involves automating deployment processes and ensuring a reliable, automated path from code changes to production release. Both CI and CD aim to accelerate software delivery, improve quality, and enable rapid, iterative development.

Q. what are the key process of CD?
-----------------------------------
Key process of Continuous Deployment
      * Ensure you're authenticated to the system or wherever you're deploying
      * Ensure that the code that's being deployed is working as expected once it's deployed

Q. How to install Jenkins?
----------------------------
Jenkins installtion on Linux, jenkins require Java, we can follow Jenkins documentation to install it. 

    $ sudo yum install epel-release -y 		--> install all dependencies/repos
    $ sudo yum install java -y  		--> java installtion mandate to run jenkins
    $ sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo --no-check-certificate
    $ sudo rpm --import http://pkg.jenkins.io/redhat-stable/jenkins.io.key  		--> jenkin key for validation
    $ sudo yum install jenkins -y 		--> install jenkins

Jenkins installation files are available in **`/var/lib/jenkins/`**, in this directory we have `config.xml` and jobs are stored here. to start the jenkins service the configuration file are available **`/lib/systemd/system/jenkins.service`** file and change Jenkins port to `8090` by updating Environment="JENKINS_PORT=" variable value , It should look like this: Environment="JENKINS_PORT=8090"

	/var/lib/jenkins			--> Jenkins Home directory consist of config.xml, nodes, users, plugins, jobs,secrets. 
 	/var/lib/jenkins/secrets		--> Jenkins initialAdminPassword is available here.
 	/lib/systemd/system/jenkins.service	--> Jenkins service file location
  	
Under the Jenkins directory there are mutiple files are available:

	/var/lib/jenkins
		|-> config.xml
		|-> users/
		|-> nodes/
		|-> plugins/
		|-> jobs/
		|-> secrets/ 

    $ sudo vi /lib/systemd/system/jenkins.service
    $ sudo systemctl edit jenkins      	--> edit Jenkins service file
    $ sudo systemctl start jenkins      --> start Jenkins service 
    $ sudo systemctl status jenkins     --> status check 
    $ sudo systemctl restart jenkins    --> restart jenkins

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

Q. How to change the Jenkins default listner port?
----------------------------------------------------
by changing the **/lib/systemd/system/jenkins.service** file and edit `Environment="JENKINS_PORT=8090"` to desired open port. 
      * by default jenkins listen on port 8080
 
Q. Important file in Jenkins installtion?
-----------------------------------------
1. jenkins service file `/etc/systemd/system/jenkins.service`
2. jenkins service file also available in `/lib/systemd/system/jenkins.service`
3. jenkins installed files are available in `/var/lib/jenkins/config.xml` is the main file to take backup of.

Q. How to restart jenkins from web-ui?
----------------------------------------
we can do it by using `jenkins-url/restart`. this will do the job.

Q. how to use jenkins-cli?
---------------------------
java -jar jenkins-cli.jar -s http://localhost:8085 -auth 'admin:Adm!n321' <command>

Q. Jenkins-UI :
----------------
Jenkins Dashbaord:
```
   |-> New Item --> FreeStyle, pipelie, Multipipeline, 
   |-> people
   |-> Build History
   |-> Manage Jenkins
   |-> Myview
   |-> Lockable Resource
   |-> New View
```
Q. how to add secrets in Jenkins
----------------------------------
	Goto --> manage jenkins --> security --> manage credentials --> select domain --> add credentials -> credential type and add.

Q. Managing Users in Jenkins:
-----------------------------
We can manage users in the jenkins and create roles for the users and restric them what they can do and what they can't do. this can be achived with a plugin known as "Role Based Authentication". this plugin allow the admin to create roles for the users and restrict their access. 

	Manage Jenkins --> Manage users --> create new user --> login credentials.
	Manage Jenkins --> Manage Plugin --> install Plugin--> Role based Authorization Plugin.
	Manage Jenkins --> security section --> configure global security --> authorization --> Role based strategy
	Goto --> manage jenkins --> Manage and Assign Roles section will appear--> from here you give permissions to the users. 

Q. Jenkins Dashboard :
-----------------------
![image](https://user-images.githubusercontent.com/33980623/230021738-5ea6d3dc-15f7-446b-809f-9a63f9e101d2.png)

Q. Managing system configuration:
---------------------------------
In system configuration section
```
   |-> Configure system --> Jenkins server configurations. setting paths, environments, etc.
   |-> Global Tool configuration --> Maven, JDK, information avaiable here
   |-> manage plugins   --> Install, Update, Delete plugins
   |-> manage nodes and clouds --> jenkins nodes list
```
Q. Install role-based authentication strategy:
-----------------------------------------------
Installing the plugin "Role-based Authorization Strategy", this allow Jenkins admin can assign roles to the users in jenkins. post install must enable RoleBased Strategy in Global Security section.

```
   |-> Install plugin "Role-based Authorization strategy
      |-> Goto Globalsecurity ->Authorization section
         |-> enable "RoleBased Strategy" 
            |-> Now it will show in the manage Jenkins page as "Manage and Assign roles"
```

Q. Assinging project based metrix Authorization strategy:
----------------------------------------------------------
this will allow user to do only the limited authorizations required for that user. for this we install plugin  "Project-based Matrix Authorization Strategy" and add the user provide required permissions what he can do. then save apply. the user can only see the given permissions with his user credentials

Q. Administering Jenkins:
-------------------------
**Backup:** 
Backing the Jenkins is full/snapshot. which files to backup? $JENKINS_HOME && config.xml && Jobs. so its mandetory to backup the JENKINS_HOME directory. Backup jenkins can be done in different ways 
   
   1. Filesystem backup 
   2. Plugin backup (ThinBackup )
   3. shell scripts that backsup. (https://github.com/sue445/jenkins-backup-script)

backup using ThinBackup plugin, goto --> Manange jenkins --> tools & Actions --> thinkbackup --> add the backup settings --> create backup.

**Restore:** 
Restore the backup that has taken from plugin ThinBackup. goto--> Thinbackup --> restore --> select restore date --> select check box "Restore Plugin" --> click restore. 
      
      $ systemctl restart jenkins
      
**Monitor:** 
we can monitor the system using the plugins such as "Prometheus and graphana" or "Datadog" or other. install plugin prometheus metrix. install Prometheus and Grafana on the same machine called jenkins-server. 

   configure Prometheus --> /etc/prometheus/prometheus.yaml --> add jenkins in Targets. as below
```
scrape_configs:
  - job_name: 'Prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'jenkins'
    metrics_path: /prometheus/
    static_configs:
      - targets: ['localhost:8085']   # -targets: ['jenkins-IP:Jenkins-listen-port']
```
restart the prometheus 

   $ systemctl restart prometheus 
   $ service prometheus restart/status/
   $ 

**Scale:**
**manage:**

Q. What is a jenkinsFile:
--------------------------
Jenkins file is a text file that provide set of instruction what to do with the code. jenkins file is a way to define pipeline.
   1. free style
   2. pipeline (single stage pipeline)
   3. multi-pipeline (multi stage jenkinsfile is called multi line pipeline)  
        
Example:
```
pipeline{  
    agent any  
    stages {
        stage("build") {
            steps {
                echo"This is the build step"
            }
        }
        stage("test") {
            steps{
                echo"This is the test step"
            }
        }
        stage("deploy") {
            steps{
                echo"This is the deployment stage"
            }
        }
    }
}
```
multi-stage pipeline:
----------------------
```
pipeline {
    agent {
        label {
            label 'master'
            customWorkspace "${JENKINS_HOME}/${BUILD_NUMBER}/"
        }
    }
    environment {
        Go111MODULE='on'
    }
    stages {
        stage('Git clone') {
            steps {
                git 'https://github.com/kodekloudhub/go-webapp-sample.git'
            }
        }
        stage('docker image') {
            steps {
                script {
                    image = docker.build('adminturneddevops/go-webapp-sample')
                }
            }
        }
        stage('Run container') {
            steps {
                script {
                    image = docker.build('adminturneddevops/go-webapp-sample')
                    sh "docker run -p 8090:8000 -d adminturneddevops/go-webapp-sample"
                }
            }
        }
    }
}
```

Multi-stage pipeline:
---------------------
```
pipeline {
    agent none
    
    stages {
        stage('Build Dev') {
			agent {
				label {
					label 'Dev'
					customWorkspace "/opt/go-app"
				}
			}
            steps {
                sh 'git pull '    
            }
        }
        stage('Build Test') {
			agent {
				label {
					label 'Dev'
					customWorkspace "/opt/go-app"
				}
			}		
            steps {
                sh 'go test ./...'    
            }
        }
        stage('Build Deploy') {
			agent {
				label {
					label 'Dev'
					customWorkspace "/opt/go-app"
				}
			}		
            steps {
                script {
                    withEnv ( ['JENKINS_NODE_COOKIE=do_not_kill'] ) {
                    sh 'go run main.go &'
                    } 
                }
            }
        }
		stage('Build prod') {
			agent {
				label {
					label 'Prod'
					customWorkspace "/opt/go-app"
				}
			}		
            steps {
                sh 'git pull '    
            }
        }
        stage('Build Test-prod') {
			agent {
				label {
					label 'Prod'
					customWorkspace "/opt/go-app"
				}
			}
            steps {
                sh 'go test ./...'    
            }
        }
        stage('Build Deploy-prod') {
			agent {
				label {
					label 'Prod'
					customWorkspace "/opt/go-app"
				}
			}
            steps {
                script {
                    withEnv ( ['JENKINS_NODE_COOKIE=do_not_kill'] ) {
                    sh 'go run main.go &'
                    }    
                }
            }
        }
    }
}
```

Q. Build Agent:
----------------
Build agent are worker agent for Jenkins server, Build servers/agent are used to perform build, test, security checks, and many more. this way the load on the Jenkins server is less, and we can scale the Build agents according to our need. we can deploy the agents in container platform also. 

Q. Adding a BUild server:
-------------------------
For adding build agent install plugin "SSH Build Agent". create a credentials for the new build agent. similerly we need to create use in the linux server. 

	Goto --> manage Jenkins --> manage credentials --> select domain --> create new credentials --> create 
	Goto --> manage jenkins --> manage nodes and clouds --> New Node --> add required details.--> create node.

for creating pipeline jobs need to install the plugin pipeline.
```
pipeline {
    agent {
       label "node01"
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

Q. Docker as build agent:
----------------------------

```
pipeline {
    agent {
       docker { image 'golang:latest'}
    }
    stages {
        stage('Hello') {
            steps {
                git 'https://github.com/AdminTurnedDevOps/go-webapp-sample.git'
                echo 'Hello World'
            }
        }
    }
}
```

Q. Blue Ocean :
-----------------
Blue ocean will improve the jenkins Ui, to invoke Blue Ocean install plugin "Blue Ocean". to acces blue Ocean use the below URL
   https://Jenkins_URL/blue

Q. Jenkins security:
--------------------


Q. How to provide limited access to users role based  authentication?
-----------------------------------------------------------------------
to provide restrictions at user level, we need to install a plugin know as **Role-based Authorization Strategy** need to install 

1: Go to Manage Jenkins, then click on Manage and Assign Roles tab.
2: Click on Manage Roles button and then under the Global Roles section, input your role named developers in Role to add box, then click Add button. Now your role will be visible in matrix.
3: Then check mark the box under Read section for developers role only.
4: Now click on Save button on your bottom of the window.

Steps to assign role to the user:

1: Go to Manage Jenkins, then click on Manage and Assign Roles tab..
2: Click on Assign Roles button, then under the Global Roles section input your user named tony in User/group to add box, then click Add button. Now your user will be visible in matrix.
3: Then check mark the box under developers section for tony user only.
4: Now click on Save button on the bottom of the window.

Q. how assign project based metrix authentication?
---------------------------------------------------
1: Go to Manage Jenkins, then click on Manage Plugins tab.
2: Click on Available section and search for Matrix Authorization Strategy plugin.
3: Then mark the box check and click on Install without restart button.
4: After that click on Restart Jenkins when installation is complete and no jobs are running.

Steps to enable Project-based Matrix Authorization Strategy:

1: Go to Manage Jenkins, then click on Configure Global Security tab.
2: Select Project-based Matrix Authorization Strategy under Authorization section.
3: Click on Add User and enter user name tony. After that it will appear in the matrix.
4: Enable Read under Overall column, Build and Read under Job column for user tony.
5: Now click on Save button on your bottom of the page.
6: Login as tony user into the Jenkins server and build the job named mytest.

   
   

      
