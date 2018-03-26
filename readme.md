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

*