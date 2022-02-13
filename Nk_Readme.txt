Resources:
1. Youtube : Automation Step by Step 
	https://www.youtube.com/playlist?list=PLhW3qG5bs-L_ZCOA4zNPSoGbnVQ-rp_dG
	
Jenkins
	- Is Java application
	- Used for Continuous Integration and Continuous Delivery (CICD)

-- --------------------------
Jenkins Beginner Tutorial 3 - How to change Home Directory
Change Jenkins HOME folder

-- --------------------------
Jenkins Beginner Tutorial 4 - How to use CLI (command line interface)
	Easier
	Faster
	Memory management (efficient)
	CI
	
	Step1:	Start Jenkins (java -jar jenkins.war)
	Step2:	Manage Jenkins >> Configure Global Security >> Enable Security
	Step3:	Goto localhost:8080/cli >> See wiki
	
	Step4:	Download jenkins-cli.jar
			> java -jar jenkins-cli.jar -s http://localhost:8888/ -webSocket help
			Note: If it asks for Passphrase enter the value from SSH Public keys from Jenkins
	
	Step5:	> java -jar jenkins-cli.jar -s http://localhost:8888/ -webSocket safe-restart
			ERROR: anonymous is missing the Overall/Read permission

	Step6:	Manage Jenkins >> Configure Global Security >> Authorization >> Anyone can do anything
			Repeat Step5


-- --------------------------
Jenkins Beginner Tutorial 5 - How to create Users + Manage + Assign Roles

Create New users:
	Manage Jenkins >> Manage Users >> Create Users
	
Create/Manage User roles:
	Step1:	Download : Role Strategy plugin (Plugin Id: role-strategy)
			Add the .hpi file in /plugins folder
			Goto to Plugin Manager >> Available >> "Download not and install after restart"
	
	Step2:	Configure Global Security >> Authorization >> Enable "Role based Strategy"
	Step3:	Restart
	Step4:	Manage Jenkins >> Manage And Assign Roles >> Manage Roles
			It will show:	
					- Global roles	
									(Add role: employee;					Access: Read+View)
					- Project roles	
									(Add role: developer;	Pattern: Dev.*;		Access: Job,Run,SCM)
									(Add role: tester;		Pattern: Test.*;	Access: Job,Run,SCM)
					- Slave roles
					
	Step5:	Manage Jenkins >> Manage And Assign Roles >> Assign Roles
					- Global roles
									User/Group		admin	employee
									admin			Y		-
									User1			-		Y
									User2			-		Y
					- Project roles	
									User/Group	developer	tester
									admin		-			-
									User1		Y			-
									User2		-			Y
									
	Step6:	New Item: "Dev_Project1" >> Freestyle project >> Save
			New Item: "Test_Project1" >> Freestyle project >> Save
	
	Note: 	User1 login will see Dev_Project1
			User2 login will see Test_Project1

-- --------------------------
Jenkins Beginner Tutorial 6 - Basic Configurations

Configure Jenkins:
	- Home directory
	- System message
	- # of Executors			: No. of parallel jobs at a time
	- Labels 					: For multi-node Jenkins (WindowsMachine1)
	- Usage						: "Use this node as much as possible"
	- Quiet period				: Time(in sec.) interval to trigger the build
	- SCM checkout retry count	: Retry count in case the build fails
	- Restrict project naming	: Default/Pattern/Role-based strategy
	- Global properties	
		- Environment variables	: Can be used as ${key1}
		- Tool Locations
	- Jenkins url
	- SSH Server
		- SSHD Port				: Fixed/Random/Disable
	- Shell 
		- Shell executable		: Path of shell
		

-- --------------------------
Jenkins Beginner Tutorial 7 - Getting started with JOBS

New Item:
	1. General
	2. Source Code Management
	3. Build Triggers
	*  Build environment
	4. Build
	5. Post-Build Actions
	
Trigger the Job remotely:
	#3 Build Triggers
		- Trigger build remotely (with a URL and auth-token)
		
Chain Job executions:
	Project #1, #2, #3.

	Project #2:
	#3 Build Triggers
		- Build after other projects are built (Project #1)
		- Trigger if build is Stable/Unstable/Failed

	#5 Post-Build Actions:
		- Build other projects (Project #3)
		- Trigger if build is Stable/Unstable/Failed


-- --------------------------
Jenkins Beginner Tutorial 8 - Jenkins integration with GIT (SCM)

Create Jenkins Job and run simple Java program:
	Step1: Create a Java pgm
	Step2: Create Jenkins Job "HelloWorld" >> Freestyle project >> OK
	Step3: #4 Build
			Execute Shell
			> cd /appdata/JavaProject1
			> javac Hello.java
			> java Hello

Create Jenkins Job and integrate with Github:
	Step1: Create Jenkins Job "HelloWorld2" >> Freestyle project >> OK
	Step2: #1 General
				Use Custom Workspace : "F:\AdvancedComputing\RecyclebinStuff\JenkinsJob"
	Step3: #2 Source Code Management
				Git: 
				Repository Url: https://github.com/nikhil2050/Tut_Jenkins_MultibranchPipeline.git
				Credentials: Select
	Step4: Build Now
	
	
-- --------------------------
Jenkins Beginner Tutorial 9 - How to use CATLIGHT (Jenkins Build Monitor)
https://catlight.io/

-- --------------------------
Jenkins Beginner Tutorial 10 - What is Automated Deployment (Step by Step)
Jenkins Beginner Tutorial 11 - How to do Automated Deployment (Step by Step)

BUILD >> DEPLOY >> TEST >> RELEASE

	Step1:	Install "Deploy to container plugin"
			OR
			Download : Deploy plugin (Plugin Id: deploy)
			Add the .hpi file in /plugins folder
			Goto to Plugin Manager >> Available >> "Download not and install after restart"
	
	Step2: Create Jenkins Job "Test3_AutomatedDeploymentTest" >> Freestyle project >> OK

	Step3: Copy sample.war (https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/) to location:
			C:\ProgramData\Jenkins\.jenkins\workspace\{Project}\
	
	Step4: Add user in Tomcat file TOMCAT_HOME/conf/tomcat-users.xml :
			<user username="deployer" password="deployer" roles="manager-script"/>
			
	Step5: #5 Post-Build Actions:
			- WAR/EAR files : sample.war
			- Tomcat8 
				- Credentials: deployer/deployer
				- Tomcat Url : http://localhost:8080
	
	Step6: Build Now
    Step7: Visit and verify the deployment on http://localhost:8080/sample/

-- --------------------------
Jenkins Beginner Tutorial 12 - Notifications - How to send Email from Jenkins

Plugins:
	1. Notification plugin (Plugin Id: notification)
	2. Extreme Notification plugin (Plugin Id: extreme-notification)
	3. Email-ext plugin (Plugin Id: email-ext)

Configure System >> Email Notification
	SMTP server : smtp.gmail.com
	Use SMTP Authentication
	User Name 	: abcde@gmail.com
	Password	: Abcde@123
	Use SSL
	SMTP Port	: 465
	Test configuration by sending test email
	Test email recipient : defgh@gmail.com
	
	#5 Post-Build Actions:
		Email Notification:
			Recipients
	
-- --------------------------
Jenkins Beginner Tutorial 13 - What is Pipeline in Jenkins (DevOps)

Pipeline:
    - It is workflow with group of events/jobs that are chained and integrated with eachother in sequence
    - Every job in pipeline has dependency on 1/more other jobs.
    
-- --------------------------
Jenkins Beginner Tutorial 14 - How to setup DELIVERY PIPELINE in Jenkins (Step by Step)

	Step1: Chain required Jobs in sequence (using Build Triggers in DeployJob and TestJob)
	Step1a: Create Jenkins Job "Test4_Pipeline_BuildJob" >> Freestyle project >> OK
			Build:
				echo "Build Job completed"

	Step1b: Create Jenkins Job "Test4_Pipeline_DeployJob" >> Freestyle project >> OK
			Build:
				echo "Deploy Job completed"

	Step1c: Create Jenkins Job "Test4_Pipeline_TestJob" >> Freestyle project >> OK
			Build:
				echo "Test Job completed"
	
	Step2: Install "Delivery Pipeline Plugin" OR Download : Delivery Pipeline Plugin (Plugin Id: delivery-pipeline-plugin)
	
	Step3: "Jenkins dashboard" >> Add "View" (+ button on tab)
			View: MyTestDeliveryPipeline
			Select "Delivery Pipeline View"
			Components >> Add
				- Initial Job:	"Test4_Pipeline_BuildJob"
				- Name: 		My Build Job
			Save
	
-- --------------------------
Jenkins Beginner Tutorial 15 - How to setup BUILD PIPELINE in Jenkins (Step by Step)

Also refer Tutoria #14
	Step1: Chain required Jobs in sequence (using Build Triggers in DeployJob and TestJob)
	
	Step2: Install "Build Pipeline Plugin" OR Download : Build Pipeline Plugin (Plugin Id: ?build-pipeline-plugin)

	Step3: Add Build Pipeline View:
			"Jenkins dashboard" >> Add "View" (+ button on tab)
			View: MyTestBuildPipeline
			Select "Build Pipeline View"
			Components >> Add
				- Initial Job:	"Test4_Pipeline_BuildJob"
				- Name: 		My Build Job
			Save

-- --------------------------
