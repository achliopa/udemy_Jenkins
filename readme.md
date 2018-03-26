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

* 