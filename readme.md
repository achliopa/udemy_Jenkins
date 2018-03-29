# Udemy Course - [Master Kenkins CI for DevOps and Developers](https://www.udemy.com/the-complete-jenkins-course-for-developers-and-devops/learn/v4/overview)

## Section 1 - Getting Started with Jenkins

### Lecture 5 - Introduction to Continuous Integration

* Jenkins is a CI and build tool
* CI requires developers to integrate code in a shared repository on a regular basis
* Version control system is monitored, when a commit is detected a build will be triggered automatically
* if the build or test is not green, developers will be notified immediately
* results: better quality software, catch bugs early
* Stages of CI
	* 1st: there are no build servers, there isa VCS but regular commit is not enforced, manual integration and test before release by a dev or release manager => Few releases
	* 2nd: the VCS,CI tools exist. Build and test run is done regularly (night build), end of day commit, alerts in case of build failure
	* 3rd: any commit triggers build and test, immediate feedback
	* 4th: auto code quality and code coverage metrics now run along with nit tests in CI tools to continuously evaluate code quality
	* 5th: confidence has built to the tests that succesful test and build triggers autodeployement to production
* Continuous Integration: constantly merginf dev work to the main branch
* COntinuous delivery: continuous delivery of code to an environmaent when code is ready to ship. test env or prod env. for review and inspection
* Continuous deployment: deploy or release code to prod as soon as it is ready
* Tools to implement CI: Jenkins(non hosted) CircleCI(hosted)
* CI mentality: fixing broken build highest priority, auto deploy process, writing high quality tests

### Lecture 6 - Intro to Jenkins and history of Jenkins

* is a CI and auto build environment build in Java.
* it can support any sorts of projects
* easy to use, easy to learn, extensible, 
* lots of plugins , vcs, code quality, build notifications, ui custom
* started as Hudson within Sun. 
* Jenkins is not ready for Java 9 yet stick with 8
* install java on our machine (we have v8 so OK)
* we install from oracle download the package and install
* we have JAVA_HOME ready `echo $JAVA_HOME`
* to add in linux 

```
Configure Java_Home on Linux:
Login to your account and open the startup script file which is usually ~/.bash_profile  file (or can be .bashrc depending on your envrionment settings)
$ vi ~/.bash_profile

In the startup script, set JAVA_HOME  and PATH 

C shell:

setenv JAVA_HOME jdk-install-dir
setenv PATH $JAVA_HOME/bin:$PATH
export PATH=$JAVA_HOME/bin:$PATH
jdk-install-dir  is the JDK installation director, which should be something similar to /usr/java/jdk1.5.0_07/bin/java

Bourne shell:

JAVA_HOME=jdk-install-dir
export JAVA_HOME
PATH=$JAVA_HOME/bin:$PATH
export PATH
Korn and bash shells:

export JAVA_HOME=jdk-install-dir
export PATH=$JAVA_HOME/bin:$PATH
Type the following command to activate the new path settings immediately:

$ source ~/.bash_profile 

Verify new settings:
$ echo $JAVA_HOME

$ echo $PATH
```

### Lecture 10 - Install Jenkins

* we go to [jenkins site](https://jenkins.io/download/) and download for ubuntu. there also a docker release
* add key to system `wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -`
* add entry `deb https://pkg.jenkins.io/debian-stable binary/`
* install `sudo apt-get update ; sudo apt-get install jenkins`
* it starts jenkins deamon on port 8080. we edit `/etc/default/jenkins` to use port 8081 instead.
* jenkins init script is in `/etc/init.d/jenkins`
* when we finish the courrse we woul like to see how to start it on demand to avoid wasting resources.
* we hit localhost:8080 and see the welcome screen. it prompts to a file `/var/lib/jenkins/secrets/initialAdminPassword for pswd` we sudo more it and cp the pswd
* it asks to install plugins. we skip this step and enter our dashboard
* at admin -> configure we change the password

### Lecture 11 - Jenkins Architecture and Jenkins Terms

* jenkins uses master slave architecture
	* master: schedules build jobs, dispatches buils to slaves for the actual job execution, monitors slaves and records build results, presents build reports. can also execute build jobs
	* slave is a small java program: executes build jobs dispatched by the master
* easy to configure slave to run on aparticular machine
* job/project => runnable tasks controlled/monitored by jenkins
* slave/node: slaves are computers set up to build projects for a master
* jenkins runs 'slave agent' on slaves
* when slaves register to master, master starts distributing load to slaves
* a node =>  all machines in jenkins grid. slaves and master
* executor => separate stream of build to run on a node in parallel  (node 1:n executors)
* build => result of one of projects
* plugin => a sw that extends the core functionality of the core jenkins server

### Lecture 12 - Jenkins UI: Dashboard and Menus

* in dashboard center ajob listing section
* left: menu items, build queue and build exec status
* build history includes build from master and slaves
* manage jenkins has all sorts of control
* my view filters jobs per user
* credential item removed from menu

### Lecture 13 - Create our first jenkins job

* it will be a path so avoid spaces in name / select freestyle and ok
* we can discard old builds to save resources, we can keep latest builds
* we can disable build entirely by: disable this project till we enable it again
* SCM supports CVS and SVN out of the box, git with a plugin
* if we dont specify triggers build will only trigger manually
* build steps tell how we want to build our project, we select execute shell. we get a box to write our command. we write `echo "Hello jenkins"` to test the flow.
* post build actions are run after the build executres, we use it to send notifications or emails to the devteam
* we save and go to dashboard

### Lecture 14 - Run our First Jenkins Job

* in the Dashboard we see the build button on the right, then we click on the project and wenter its screen.
* in the menu we see an entry in build history (blue ball means success). we click on it and then on console to see output
* S shows the last build status (blue-ok) W shows the weather - aggregated build status (Sun = all OK)

## Section 2 - Continuous Integration with Jenkins

### Lecture 15 - Install Git and Jenkins Github Plugin

* we install git on our machine (we have it)
* we will install github plugin for jenkins
* in jenkins control panel => manage plugins => avaliable tab=> search for github (github plugin now named github). we can install without restart innew jenkins versions. if we update plugins we need to restart

### Lecture 16 - Install and Configure Maven

*  (we dont need it for JS as we can build from shell but...)
* maven describes how sw is built, describes its dependencies
* we download apache maven (hope it will run at home dir)
* we add the bin dir of the maven folder to PATH we add `export PATH=/home/<home folder>/apache-maven-3.5.3/bin:$PATH` to~/.profile
* we test by typing `mvn -v`

### Lecture 17 - Configure Jenkins for our Maven-based project

* we go to jenkins -> settings -> global tool configuration 
* we add jdk, name it, disable auto install set path to our jvm `echo $JAVA_HOME` to find out
* we add git , name it, dsable install automatic, set path to `git` as it is installed globally
* we add maven, name it, disable auto install, add path to maven installation path without the bin folder
* we save

### Lecture 19 - Create our first Maven-bases Jenkins project

* for complex maven projects use the Maven+Project+Plugin
* [git repo](https://github.com/jleetutorial/maven-project.git)
* the porject is a simple web app that prints hello worlds to browser. it has 2 modules
	* webapp: has web.xml for deploy
	* server
* maven used pom.xml to describe the sw project to build (dependencies to external modules, directory structure, required plugins, predefined targets to perform compilation and packaging)
* we create a new project *maven-project* as a freestyle project
* we select git as SCM and add the github url, no credential (public, master branch)
* for build step we select top-level maven target, select our maven installation
* different phases of maven lifecycle
	* validate: project correct all the info available
	* compile: source code
	* test: test using a suitable unit testing framework
	* package: package code in its distributable format
	* verify: run checks, integration tests
	* install: install package to local repo, use it as depecdency to other projects locally
	* deploy: copy final package to remote repo for sharing
* in jenkis maven build we need to specify only the last phase to be executed
* we set our goal to `clean package`. we want to clean any previous assets and start our clean build up to package phase

### Lecture 21 - Run our first Jenkins Build and Jenkins Workspace

* in our project page we run build now, wait and get a build success.
* the console log of teh build is massive. jenkins build in a shared workspace on the local machine `/var/lib/jenkins/workspace/maven-project`, clonse github repo, it specifies the modules, does the cleanup, moves to first module and executes the sequentially build commands proints unit test reports
* in the project page if we click to workspace, we see the dir structure of the space used to build the project

### Lecture 23 - Configure Jenkins to poll source code changes periodically

* we want to trigger build when we commit on github
* we go to project space, and in menu=> configure
* in build triggers we select poll scm option
* the scedule field follows the syntax of Cron (unix task scheduler)
	* Cron syntax 5 fields separated by TAb or whitespace 1st: minute(0-59, 2nd hour (0-23),3rd day of month (1-31), 4th month (1-12), 5th day of week (0-6) * is wildcard.
	* e.g 0 2-4 * * * is every day at 2am 3am and 4am
* we put * * * * *  to poll SCM every minute
* we test git polling log to see the polling activity. no change detected
* we need to fork the repo to be able to make changes
* we link jenkins to our repo.
* we clone the repo to our machine
* make a change commit and push. this should trigger a build to jenkins. it does and we see the change we made as commit comment

### Lecture 25 - Other Build Triggers of jenkins

* we go to project configuration in build triggers
* we can trigger remotely via API using a Token, if we hit the url `JENKINS_URL/job/maven-project/build?token=TOKEN_NAME` with an ajax request eg it will trigger a build
* build after build sets a build pipeline. we can even set jobs to run different stages of the build or take metrics. in the field we pipe jenkins project names e.g project1, project2. any of the projects finish will trigger the build
* the build periodically schedules builds according to the CRON schedule periodinc builds are still relevant in CI for very long builds where quick feedback is less critical, intesive load tests
* NEXT option binds build to git commit instead of polling git. to use it we need to setup the hook between github and jenkins

## Section 3 - Continuous Inspection with jenkins

### Lecture 27 - Code Quality and Code Coverage Metrics Report

* we now see code coverage and code quality metrics
* we will see how to setup jenkins to produce checkstyle rpeort during the builds
* checkstyle plugin runs at each build and produces reports with warnings. we find it in the available plugins and install it.
* we configure our maven project to execute checkstyle. in project configuration we go to build goals we modify it adding `checkstyle:checkstyle` 
* we also want a published report. we add it in postbuild actions selecting publish checkstyle reports. we leave output dir empty. report will be placed i project workspace.
* we manually trigger a build. after it finishes we get a new menu item in project page called checkstyle warnings
* we check the warnings one by one and add fixes we commit and build, we get more warnings so we do more fixes until there are no more warnings in checkstyle warnings
* for java pmd is used or findbugs

### Lecture 29 - Jenkins support for Gradle, Ant and Shell Scripts

* jenkins provides support for almost all popular build systems
* ANt is a low level build language for Java
* to use ant in jenkins we need to add ant plugin. when we install it we can invoke ant in build options, targets are similar to maven
* gradle is a build tool for jvm, once installed it gives an invoke gradle script option
* Shell script is handy to perform low level OS related tasks, in the build we have built in the execute shell option

## Section 4 - Continuous Delivery with jenkins

### Lecture 31 - Archive Build Artifacts

* our aim to stage the app for qe testing
* one way is to archive the generated artifact and then in  next job take the archived artifact an deploy it to staging environemnt.
* we got othe project page -> cofiguration -> post build actions and add one more action *archive the artifacts* 
* for java ee this file is the *.war file so we add **/*.war
* we trigger the build and after completion we click on it. we see the artifact there waiiting. the artifact was built anyway. post-build action just archives it

### Lecture 32 - Install and configure Tomcat as staging environment

* tomcat is an open-source webserver offers pure java http web environment
* we download v8 and extract in our workspace/tools
* jenkins runs at 8080 (we set it to 8081) and tomcat at 8080 so we set it to 8082
* in the installation folder we open conf/server.xml and look for connector
* we have to make the run script executable. we enter bin
* the script that starts tomcat is startup.sh (in our installtion it is executable) we do chmod +x
* we run the script ./startup.sh and visit localhost:8050
* tomcat is live and running
* to connect jenkins with tomcat we need tomcat credentials
* we go to conf/tomcat-users.xml and go to the end of the file
* we need to assign two new roles to the user. security in tomcat is role based, we add manager-script role and admin-gui and leave only tomcat user, we remove all the rest, we set password of tomcat, we set the new roles to user, we remove the comments around user

```
  <role rolename="manager-script"/>
  <role rolename="admin-gui"/>
  <user username="tomcat" password="tomcat" roles="manager-script,admin-gui"/>
```

* we restart tomcat

### Lecture 33 - Deploy to Staging Environment

* to deploy our app to tomcat staging environment we need to install 2 plugins to jenkins: copy artifact & deploy to container
	* copy artifact allows jenkins to copy artifact from other jobs
	* deploy plugin actually deploys the artifact from jenkins to tomcat
* we install copy artifact plugin
* we install deploy to container plugin
* we rename maven-project as we will create another job (project) to deploy our app. maven project is now called package as it actually build and packages the project
* we create anew item called *deploy-to-staging* as a freestyle project
* we add a build step => copy artifacts from another project
	* project name: package (copy from)
	* artufacts to copy: **/*.war (all war files)
* we add a post-build action (deploye war/ear to container)
	* at war/ear we put all war files(**/*.war)
	* containers => tomcat 8.x we add credentials from the tomcat xml file and url=> http://localhost:8082
* this new job should be automaticaly triggered if previous job finishes successfully
* we do this by adding a post build action to package job to trigger the next job. we go to package job configuration => post build actions and select build other projects and select deployt-to-staging project
* we will do a manual build in package console and hopefully have our app deployed to tomcat.
* both jobs finish. we click on the deploy job and see it was successful. in its page we see the upstream project package (clue=ok). we see the log and it says successful deploy. we go to the tomcat app path localhost:8082/webapp and see the app!! sucess

### Lecture 36 - Jenkins Build Pipeline

* we have implemented a build pipeline of 2 jobs:
	Upstream Job: compile->test-> package
	Downstream job: Staging for QE testing
* jenkins supports multi-step build pipelines and trigger rebuild of project if one of the dependencies is updated
* in real life large proojects we might have dozens or hundrends of jobs. its difficult to kkep track. we need a tool to visualize the pipeline to understand how jobs connect. we use the build pipeline plugin
* we install the build pipeline plugin
* we click the + sign over the project table in main dashboard.
* there we can create a new view (a way to organize our jobs)
* we call the view buils pipeline, we check buils pipeline typw and ok
* in the naxe form we need to set the initial job this is the start of the pipelineview (select iniital jb -package) and click ok
* we get the view.. we can truigger a run with run. if we refresh page we see the status is green

### Lecture 37 - Parallel Jenkins Build

* up to know we run our build steps sequentially
* some times we want parallelisation to spped up especially if a step takes a long time
* after packaging we will run in parallel staging and checkstyle report
* we go to package job configuration  and remove checkstyle from build goal
* we create anew job that only runs checkstyle (job name: static-analysis)
* at source control we add the git repo and build steps we add a maven targer (checkstyle:checkstyle)
* we now need to trigger the job from the post build actions of package job. at the post build action tha build other projects we add the new job with a comma
* at the build pipeline view we see the parallelization

### Deploy to Production

* we now going to add the deploy to production step in the pipeline. however we dont want it to be fully auto. we need the approval of QA manager/release manager after he reviews the app at staging env. so this step we want to be manually triggered
* we will now run a second tomcat server (production server)
* we shutdown tomcat (8082)
* we cp the whole installation `cp -r apache-tomcat-8.0.50/ apache-tomcat-8.0.50_prod/`
* at conf/server.xml we set prod srv `<Connector port="8083" protocol="HTTP/1.1"`
* we startup both servers
* in jenkins we create anew job named *deploy-to-prod*
* the config of this job is similar to the *deploy-to-staging*
job. we add a copy article from another project build step. we copy from package and the artifacts are `**/*.war`
* at post build actions we add war file pswd select tomcat 8.x and the url *http://localhost:8083*
* we finished config now we  need a build trigger. we want it to finish after deploy-to-staging finishes
* we go to deploy-to-staging job config and add a new post-build action (build other projects manual step) and enter deploy-to-prod
* our pipeline view is complete. we manually trigger the pipeline. all is done ecept deploy-to-prod
* at bottomright of the step in pipeline view we have the manual trigger

### Section 5 - Jenkins Pipeline as Code(Jenkinsfile)

### Lecture 42 - ocerview of Pipeline as code

* we will convert our pipeline into code
* we include kenkins commands in the repo
* pipeline into text file-> jenkinsfile
* Benefits: version control, best practice, less error-prone execution of jobs, logic based exectuion of steps
* sample

```
pipeline {
	agent any
	stages {
		stage ('Initialize') {
			steps {
				sh '''
					echo "PATH=${PATH}"
					echo "M2_HOME = ${M2_HOME}"
				'''
			}
		}
		stage ('Build') {
			steps {
				echo "Hello World!"
			}
		}
	}
}
```

* for proper systax highlight of jenkinsfile set the code editor to groovy (java based scripting lang, jenkinsfile is a groovy file)
* we mostly work in stages space defining stages and steps
* our initialize stage has 1 step that runs a shell script
* build stage has 1 step also a shell command
* we will impleemnt this pipeline in jenkins console.
* we need to have pipeline plugin in jenkins (we install it)
* we will create a new job which will be connected to the scripts and the repo. we name it PipelineAsCodeExample and we select type=pipeline
* at its configuration we go to Pipeline section. select Pipeline scritp from SCM, SCM->git, repo URL (we fork https://github.com/CJRivas/jenkinspipeline and put a link to our repo)
we have it public so no credentials needed
* the file used will be Jenkinsfile. we save
* in the dashboard we manually run it
* we clone the project locally
* we open the jenkins file
* in the project screen we see the stage view with staging, stages and output
* [jenkinsfile spec](https://jenkins.io/doc/book/pipeline/jenkinsfile/)
* [groovy lang](http://groovy-lang.org/)

### Lecture 44 - Automate our existing Jenkins Pipeline

* we will execute our existing pipeline but from code
* we can write a jenkins file as groovyscript or as declarative pipeline
* as agents we declare the nodes to be used for executing the pipeline. with `agent any` we mean use any available node
* we can specify the agen in the stage if we want it to run on a specific node
* each stage is made of steps
* the declarative pipeline is a good tradeoff between using ui or using a groovyscript
* we write a jenkinsfile for our package build. we will place it in the maven-project repo from previous section so that  the package job will be converted to a pipeline job

```
pipeline {
	agent any 
	stages {
		stage('Build'){
			steps {
				sh "mvn clean package"
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
	}
}
```

* in this script we have one build step a shell script to run maven up to package level cleaning the artifacts first
* in the post build actions specified we use conditional . if the build is successful show a message and archive the artifacts.
* with the file in our repo we create a new package-pipeline project of type pipeline. we set pipeline script from scm. set git and se repo url to our forked naven-project where we have pushed the jekinsfile
* we need to set a tools section in jenkinsfile to set maven to localMaven

```
	tools {
		maven 'localMaven'
	}
```

* we now want to call a non pipelined job from our pipeline script. we do this by

```
stage('Deploy to Staging') {
			steps {
				build job:  'deploy-to-staging'
			}
		}
```

* at build job: name we give the job name
* we do a small change to our code to ferify that all pipeline executed and correct package is on staging env. OK
* we will add now deploy to production

```
stage('Deploy to Production') {
			steps {
				timeout(time:5, unit:'DAYS'){
					input message: 'Approve PRODUCTION Deployment?', submitter: admin
				}

				build job: 'deploy-to-prod'
			}
			post {
				success {
					echo 'Code deployed to PRODUCTION'
				}
				failure {
					echo 'Deployment failed'
				}
			}

		}
```

* this script sets a timeout for this step . if its not terminated (success or fail) in 5 days it will auto fail. also it asks for input to peocwwd (confirmation), we can also specify the submitter you can conform for this stage to proceed. the build step calls an existing job and uppon termination prints amessage depnding on the outcome.

### Lecture 47 - Fully Automated jenkins Pipeline

* setup git repo polling
* deployment to our tomcat servers, no dependence to other jobs. the pipeline script does all the job
* setup tasks to run in parallel
* how to setup tomcat on ec2 or AWS
* we go to aws to make a new ec2 instance
* we need to creTE A SECURITY GROUP
* AWS servers are bound to a local network and not accessible from outside
* we set group name to tomcat and open 3 ports (1 http pport, 1 tcp port 8080, and an ssh port vible only to our private ip)=> set this to myip
we need access through ssh so we need a key, we go to keypairs in the menu list. we create a new one named tomcat-demo
* we create 2 instances in aws foe our 2 tomcat servers. we go to instances -> launch instance-> amazon linux-> t2-micro->defaults...->security gorup existing->tomcat and launch using our tomcat-demo keypair
* we view it and we see its public ip
* we go to where the ssh key from aws was downloaded and we `chmod 400` it
* we ssh to it from the folder where we have the key `ssh ec2-user@18.184.17.239 -i tomcat-demo.pem 
`
* we are logged in the aws
* we install tomcat `sudo yum install tomcat7`
* we lt -lt in /etc/tomcat7 to verify installation
* our workspace is at /var/lib/tomcat7
* before we go out we start tomcat `sudo service tomcat7 start`
* we need to install in total two instances in AWS an install tomcat to both of them. they can have same port as they will have different ip
* there is no need to configure tomcat users and roles as we wont use deploy to container plugin in the pipeline

* we will create a new pipeline to use aws tomcats. we call it FullyAutomated and select type=pipeline, select pipeline script from SCM add the repo (jenbkinspipeline)
* we start writing the jenkinsfile
* we set the staging and prod server connection ips as params to avoid hardcoding


```
pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.197.151.138', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.184.17.239', description: 'Production Server')
    } 
```

* we set the tools

```
	tools {
		maven 'localMaven'
	}
```

* we add the trigger section to triger our repo in github to check for commits (every 1 minute)

```
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
```

* build is local so same as before

```
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
```

* we group deployment to staging and deployment to production into one stage *Deployments*
* we have now a directive call parallel, saying to jenkins that he can run them in parallel if possible

```
stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/achliopa/workspace/tools/tomcat_aws
/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/achliopa/workspace/tools/tomcat_aws
/tomcat-demo.pem  **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
```

* to work we need to be loggin in the local machine with the same username we use to connect to ec2
* to deploy to local machines
	* Use cp instead of scp command to copy artifact to tomcat directory on the local box.
	* Use localhost as the server IPs.

## Section 6 - Distributed Builds

### Lecture 51 - Introduction to Distributed Jenkins Build

* for big organizations a lot of jenkis nodes are used one is the master and the others are the slaves