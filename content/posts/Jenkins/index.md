---
title: "Jenkins"
discription: Jenkins 
date: 2020-05-08T21:29:01+08:00 
draft: false
type: post
tags: ["CI/CD","Devops"]
showTableOfContents: true
--- 

## Jenkins 

![jenkins](images/jenkins.svg)




Check for the latest updates and install `wget`

```bash
sudo apt-get update && sudo apt install wget
```
### Configuring limits 

Edit the file `limit.conf`

```bash
sudo vi /etc/security/limits.conf
```
Adding content:
```bash
jenkins      soft     core    unlimited
jenkins      hard     hard    unlimited
jenkins      soft     fsize   unlimited
jenkins      soft     fsize   unlimited
jenkins      soft     nofile  4096
jenkins      hard     nofile  8192
jenkins      soft     nproc   30654
jenkins      hard     nproc   30654
```

### Configure the firewall

```bash
sudo apt-get install ufw
```
```bash
sudo ufw allow OpenSSH
```
```bash
sudo ufw allow 8080
```
```bash
sudo ufw enable
```
Check ufw’s status to confirm the new rules:
```bash
sudo ufw status
```
### Checking java

```bash
java --version
```
If java is missing, install
```bash
sudo apt install openjdk-11-jre-headless
```

Java 11 Docker installation instructions are included in "Downloading and running Jenkins in Docker". Alternatively, the jenkins/jenkins:jdk17 Docker image allows you to run the Jenkins controller on Java 17.

All other Java versions are not supported.
The Jenkins project performs a full test flow with the following JDK/JREs:

OpenJDK JDK / JRE 11 - 64 bits

OpenJDK JDK / JRE 17 - 64 bits


### Installing Jenkins

The version of Jenkins included with the default Ubuntu packages is often behind the latest available version from the project itself. To ensure you have the latest fixes and features, use the project-maintained packages to install Jenkins.

First, add the repository key to your system:
```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
```
The `gpg --dearmor` command is used to convert the key into a format that `apt` recognizes.

Next, let’s append the Debian package repository address to the server’s `sources.list`:
```bash
sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```
The `[signed-by=/usr/share/keyrings/jenkins.gpg]` portion of the line ensures that `apt` will verify files in the repository using the GPG key that you just downloaded.

After both commands have been entered, run apt update so that apt will use the new repository.
```bash
sudo apt update
```
Finally, install Jenkins and its dependencies:
```bash
sudo apt install jenkins
```
now that Jenkins is installed, start it by using `systemctl`:

```bash
sudo systemctl start jenkins.service
```
Since `systemctl` doesn’t display status output, we’ll use the status command to verify that Jenkins started successfully:
```bash
sudo systemctl status jenkins
```

auto run after restart
```bash
systemctl enable jenkins
```
Configure additional Java parameters:

Garbage collection options

```bash
sudo vi /lib/systemd/system/jenkins.service
```


```
"JAVA_OPTS=-Djava.awt.headless=true -XX:+AlwaysPreTouch -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/var/lib/jenkins/log -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+ParallelRefProcEnabled -XX:+DisableExplicitGC -XX:+UnlockDiagnosticVMOptions -XX:+UnlockExperimentalVMOptions -Xlog:gc=info,gc+heap=debug,gc+ref*=debug,gc+ergo*=trace,gc+age*=trace:file=/var/lib/jenkins/gc.log:utctime,pid,level,tags:filecount=2,filesize=100M -Xmx512m -Xms512m"
```
Note: `-Xmx512m` `-Xms512m` set depending on the available memory in the virtual machine

### Setting Up Jenkins

To set up your installation, visit Jenkins on its default port, `8080`, using your server domain name or IP address: `http://your_server_ip_or_domain:8080`

You should receive the Unlock Jenkins screen, which displays the location of the initial password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
The next screen presents the option of installing suggested plugins or selecting specific plugins


Slave and Master

### Agent SSH Jenkins

```bash
java -- version
```

 ```bash
sudo apt install openjdk-11-jre-headless
```

```bash
useradd -m -s /bin/bash jenkins 
```
```bash
passwd jenkins
```
openssh installed or not
```bash
apt list -installed | grep openssh-server
```
```bash
sudo service ssh status
```
install openssh-server
```bash
sudo apt install openssh-server
```

### install jenkins on docker-compose.yml

```bash
sudo su
```
```bash
vi docker-compose.yml
```
```yml
 # docker-compose.yml
version: '3.3'
services:
  jenkins:
    image: jenkins/jenkins:lts-jdk11
    restart: always
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - /home/${myname}/jenkins_compose/jenkins_configuration:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
```

```bash
docker-compose up -d
```
```bash
cd /home/root/jenkins_compose/jenkins_configuration
```
```bash
cd secret
```
```bash
cat initialAdminPassword
```

### SSH-Key Generate

ssh-keygen -t rsa -f jenkins_slave -m PEM

Execute the command and copy the contents of the file:
```bash
cat jenkins_slave
```
Open the jenkins web interface, go to the Manager Jenkins -> Manager Credentials

![jenkins2](images/jenkins2.png)

Go to the global domain

![jenkins3](images/jenkins3.png)

Click `Add Credentials`, fill in as in the screenshots below, enter the previously copied private key in the key field


Kind
: - [x] `SSH Username with private key`

id
: `team01-agent-ssh`

Username
: `jenkins`

![jenkins4](images/jenkins4.png)

and push button "create"

### Copy private key to slave machine agent

```bash
ssh-copy-id -i jenkins_slave.pub jenkins@vs02slave
```





Go to the Jenkins web interface - Manage Jenkins -> Manage Node and Cloud

![jenkins5](images/jenkins5.png)
Click New Node, enter name agent, and - [x] Permanent Agent box
![jenkins6](images/jenkins6.png)

Create and fill all:

name node: `team01-agent`

discription: `agent for team01`

number of executors: `1`

remote root directory: `/home/jenkins`

labals: `linux`

Usage:  

- [x] 'Only build jobs with label expressions matching this node'

launch method: `Launch agents via SSH`

host: `(number_of_slave_ip_machine) or dns host name of slave'

Credentials: `jenkins`

Host Key Verification Strategy: `Non verifying Verification Strategy`

`Save` and We return to Manage Node -> select our agent

push `Relaunch agent` and see log if all ok

```groovy
pipline{
  agent
}
```
## Jenkins Plugins Installation

- Web UI
- CLI
- /var/lib/Jenkins/plugins


### Jenkins Plugin. Most Useful

- Rebuilder
- Config File Provider
- Ansi Color
- Active Choice


Open the Jenkins web interface and go to Manage Jenkins-> Manage Plugins

In the Available list, look for plugins, select them and click “Download now and install after restart”

It is important to remember that restarting Jenkins running in a production environment is not recommended during working hours, although Jenkins waits for all tasks to finish before starting and only then restarts the kernel. Still, it is better to postpone such work for non-working hours. Or try to use the option “Install without restart”
You also need to remember that for such an installation of Jenkins plugins, the master must have access to the internet. If there is no direct access, you can use the proxy settings, for this you need to go to the Advanced tab


 ### Global Tool Configurations

- Maven
- JDK
- Cradle
- SonarQube
- Allure
- Etc

Add maven and npm to Global Tool Configuration

Consider the example of installing maven, for this we go to Manage Jenkins-> Tools

### Add Maven
```
Name: maven-3.9.4
Version:

- [x] 3.9.4
```
apply and save



### Jenkins Credentials ID 
best practices

<projectID><env><distination>


### Jenkins Secret Password Decrypt

cat credentials.xml 

copy past password

https://jenkins.com/script/

println(hudson.util.Secret.decrypt("{passwordcopypast}"))




### Authentification

- Jenkins OWN database
- LDAP
- Unix User/Group Database



### Create New item and Manage Roles


Opening the Jenkins web interface

To do this, click New Item

To get started, install the plugin Role-based Authorization Strategy



Go to Manage Jenkins -> Configure Security and activate Role-Base Strategy for Authorization

Security Realm:

- [x] Jenkin's own user database

- [x] Role-Based Strategy

Save!

Go to Manage Jenkins -> Manage and Assign Roles -> Manage Roles

Role to add:
QA

Pattern:
`qa-.*`




## Deployment

### Frestyle Jobs

one of the most popular configuration happens in web ui


### Folder

- Common Namespace
- Shared Libraries
- Separate rights


### Multibranch Pipeline
![jenkins7](images/jenkins7.webp)

### Pipeline

Groovy DSL to describe tasks:
1. Stored in SCM ( Jenkinsfile)
2. Defined in task settings
```groovy
node('worker01') {
   stage('Source') {
   git 'http://github.com/silabeer/example-project'
   sh ‘mvn clean install’
   }
}
```

pipeline features
- Independence from agent or worker
directories
- Parallelization
- Resist from Master restart
- Resist from network break

Building in Jenkins

Pipeline always runs on `master`

Groovy DSL

Scripted:
```groovy
node {
  stage('Hello world') {
    sh 'echo Hello World'
  }
}
```

Scripted pipline:

Plus:
- Less number of sections and specifications
- Ability to use more procedural code
- More flexibility for custom operations
- Ability to model more complex processes
Minus:
- Needs more programming
- Far from the traditional Jenkins model
- Potentially more complicated


Declarative:
```groovy
pipeline {
    agent any
    stages {
        stage('Hello World') {
            steps {
              sh 'echo Hello World'
      }
    }
  }
}
```
Declarative Pipeline:
Plus:
- More structured
- More readable
- Can be generated through Blue Ocean
- Better syntax checking
- Improved consistency
Minus:
- Less support for iterative logic
- More stiffer construction


Diagram of Declarative Pipeline sections

![jenkins8](images/jenkins8.webp)


### Snippet Generator



### Environment Setup
VS Code:
- Jenkins Jack
- Jenkins Pipeline Linter
- Jenkins Runner 
- Jenkins Doc

hostname test
```groovy
pipeline {
  agent {
    label "linux"
  }
  stages {
    stage ("test agent") {
      steps {
        sh 'hostname'
      }
    }
  }
}    
```

maven download and test version maven
```groovy
pipeline {
    agent {
        label "linux"
    }
    tools {
        maven "maven-3.9.4"
    }
    stages {
        stage('test marven') {
            steps { 
                sh "mvn --version"
            }
        }
    }
}
```
hello word stages
```groovy
pipeline {
    agent { 
        label "linux"
    }
    stages {
        stage('hello') {
            steps {
                sh 'echo "Hello World"'
            }
        }
        stage('hello2') {
            steps {
                sh 'echo "Hello World2"'
            }
        }
        stage('hello3') {
            steps {
                sh 'echo "Hello World3"'
            }
        }
    }
}
```

first.Jenkinsfile

```groovy
pipeline {
    agent {
      label "linux"
    }
    tools {
        maven "maven-3.9.4"
    }
    stages {
        stage('checkout') {
            steps {
                git credentialsId: 'team1', url: 'https://github.com/nayaks/spring-boot-rest-example.git'
            }
        }
        stage('build') {
            steps {
                sh "mvn clean install -Dmaven.test.skip"
            }
        }
    }
}
```




git branch: 'main', credentialsId: 's043218-dev-gitlab', url: 'https://gitlab.slurm.io/jenkins/spring-boot-rest-example.git'


`mvn clean install -Dmaven.test.skip.` Tests are run with `mvn test`


```bash
pipeline {
<pipeline code>
}

```



The trigger section is used to define the pipeline launch schedule. It accepts 4 values: `cron`, `upstream`, `githubPush`, `pollSCM`



cron - runs a task according to a given schedule, and pollSCM launches a check for source code updates, if a change is detected, it starts the execution of the pipeline


Example:

Launch of the conveyor in a quarter of an hour
`trigger { *cron(15 * * * ) }`
```groovy
pipeline {
    agent any
    triggers {
        cron('H */4 * * 1-5')
    }
```
Scan for SCM changes at 20-minute intervals

`trigger { pollSCM(/20 * * * ) }`


***upstream*** - starts a task based on the condition of another task being completed
Example:

`trigger { upstream(upstreamProjects: 'trigger-job', threshold:hudson.model.Result.SUCCESS) }`

will start execution after successful completion of the trigger-job

***githubPush*** - starts the pipeline in case of receiving a webhook from github







For additional pipeline options, use the options section
buildDiscarder - saves a given number of artifacts and console logs for a job
Example:
`options { buildDiscarder(logRotator(numToKeepStr: '20')) }` 
leaves a history of the last 20 builds
disableConcurrentBuilds - disables parallel execution of the same pipeline
Example:
`options { disableConcurrentBuilds() }`
retry - if the pipeline fails, it will be restarted the specified number of times
Example:
` options { retry(2) }` 
in case of failure, restart the pipeline
timeout - sets the timeout value for the pipeline, if the execution time exceeds timeout, the pipeline will exit
Example:
` options { timeout (time: 15, unit: 'MINUTES') } `
sets timeout to 15 minutes
timestamps - adds a timestamp to the console log
Example:
`options { timestamps() }` 


The environment directive allows you to specify the names and values of environment variables that will be available within the pipeline. Defining a variable at the top of the pipeline will make the variable available in all parts of the pipeline
```
environment {
TEST_VAR = "test"
}
```
in the pipeline itself can be used as follows:
```
stage('test') {
steps {
echo "${TEST_VAR}"
}
```



```bash
```

```bash
```
hostname test
```groovy
pipeline {
  agent {
    label "linux"
  }
  stages {
    stage ("test agent") {
      steps {
        sh 'hostname'
      }
    }
  }
}    
```

maven download and test version maven
```groovy
pipeline {
    agent {
        label "linux"
    }
    tools {
        maven "maven-3.9.4"
    }
    stages {
        stage('test marven') {
            steps { 
                sh "mvn --version"
            }
        }
    }
}
```


### Environment

```groovy
pipeline {
    agent {
        label "linux"
    }
    stages {
        stage('test') {
            steps { 
                sh "env | sort"
            }
        }
    }
}
```
Global environment exemple:

```groovy
pipeline {
    agent {
    label "linux"
    }
    environment {
        MY_GLOBAL_VAR = 'myEnv'
    }


    stages {
      stage('test') {
        steps { 
          sh 'echo $MY_GLOBAL_VAR'
          sh "env | sort"
        }
      }
    }
}
```
local environment exemple :

only stage2 not stage3

```groovy
pipeline {
    agent {
    label "linux"
    }
    environment {
        MY_GLOBAL_VAR = 'globalVar'
    }


    stages {
      stage('stage1') {
        steps { 
          sh 'echo $MY_GLOBAL_VAR'
          sh "env | sort"
        }
      }

      stage('stage2') {
        environment {
          MY_STAGE_VAR = 'stageVar'
        }
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh "env | sort"
        }
      }
      stage('stage3') {
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh "env | sort"
        }
      }
    }
}
```
```groovy
pipeline {
    agent {
    label "linux"
    }
    environment {
        MY_GLOBAL_VAR = 'globalVar'
        MY_RANDOM_VAR = sh(script: 'openssl rand -base64 32', returnStdout: true)
        MY_SECRET_VAR = credentials('secret-username')
    }


    stages {
      stage('stage1') {
        steps { 
          sh 'echo $MY_GLOBAL_VAR'
          sh 'env | sort'
        }
      }
      stage('stage2') {
        environment {
          MY_STAGE_VAR = 'stageVar'
        }
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh 'env | sort' 
        }
      }
      stage('stage3') {
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh 'env | sort'  
        }
      }
      stage('stage4') {
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh 'echo $MY_SECRET_TEXT'
          sh 'env | sort'
        }
      }
    }
}
```

only change in 1 stage

```groovy
pipeline {
    agent {
    label "linux"
    }
    environment {
        MY_GLOBAL_VAR = 'globalVar'
        MY_RANDOM_VAR = sh(script: 'openssl rand -base64 32', returnStdout: true)
        MY_SECRET_VAR = credentials('secret-username')
    }
    stages {
      stage('stage1') {
        steps { 
          sh 'echo $MY_GLOBAL_VAR'
          sh 'env | sort'
        }
      }
      stage('override variables') {
              steps {
                  script {
                      env.MY_GLOBAL_VAR = "IT DOES WORK"
                      println '=====override====='
                      println "MY_GLOBAL_VAR"
                  }
                  echo "My_GLOBAL_VAR = ${env.MY_GLOBAL_VAR}"
                  withEnv(["MY_GLOBAL_VAR=localVar"]) {
                      echo "MY_GLOBAL_VAR = ${env.MY_GLOBAL_VAR}"
                  }
                  withEnv(["BUILD_NUMBER=1"]) {
                      echo "BUILD_NUMBER = ${env.BUILD_NUMBER}"
                  }
              }
      }
      stage('stage2') {
        environment {
          MY_STAGE_VAR = 'stageVar'
        }
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh 'env | sort' 
        }
      }
      stage('stage3') {
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh 'env | sort'  
        }
      }
      stage('stage4') {
        steps { 
          sh 'echo $MY_STAGE_VAR'
          sh 'echo $MY_SECRET_TEXT'
          sh 'env | sort'
        }
      }
    }
}
```

def myTestVar before pipeline

```groovy
pipeline {
    agent {
    label "linux"
    }
    environment {
        MY_GLOBAL_VAR = 'globalVar'
        MY_RANDOM_VAR = sh(script: 'openssl rand -base64 19', returnStdout: true)
        MY_SECRET_VAR = credentials('secret-username')
    }
    stages {
      stage('stage1') {
        steps { 
          sh 'echo $MY_GLOBAL_VAR'
          sh "env | sort"
        }
      }
      stage('override variables') {
              steps {
                  script {
                    def myTestVar = "myTestVar"
                    println "myTestVar"
                  }
              }
      }
      stage('stage2') {
        steps {
          script{
              println = "myTestVar"
          }
          sh 'echo $MY_GLOBAL_VAR'
          sh 'env | sort'
        }
      }
    }
}
```

### CatchError
catchError - ignore error step and go to stage 2

'exit 1' - its exit from stage

but pipeline finish with status ***FAILURE***

```groovy
pipeline { 
    agent {
    label "linux"
    }
    stages {
      stage('Hello') {
        steps { 
          catchError {
            sh 'exit 1'
          }
        }
      }
      stage('stage2') {
        steps { 
          sh "echo stage2"
        }
      }
    }
}
```
if you want status ***UNSTABLE***
you can change in catch error command

`catchError(message:'Skip because unstable',buildResult:'UNSTABLE',stageResult:'UNSTABLE')`

`retry`  - its retry code in { }

```groovy
pipeline { 
    agent {
    label "linux"
    }
    stages {
      stage('Hello') {
        steps { 
          catchError(message:'Skip because unstable',buildResult:'UNSTABLE',stageResult:'UNSTABLE') {
              retry(3) {
            sh 'exit 1'
              }
          }
        }
      }
      stage('stage2') {
        steps { 
          sh "echo stage2"
        }
      }
    }
}

```

Script pipeline

```groovy
pipeline { 
    agent {
    label "linux"
    }
    stages {
      stage('Hello') {
        steps { 
          script {
            try {
              retry(2) {
              sh 'exit 1'
              }
            }
            catch (err) {
              currentBuild.result = 'Unstable'
            }
          }
        }
      }
      
      stage('stage2') {
        steps { 
          sh "echo stage2"
        }
      }
    }
}
```

### WaitUntil

WaitUntil = wait to .. 

fileExists = file be

```groovy
pipeline { 
  agent {
  label "linux"
  }
  stages {
    stage('stage1') {
      steps { 
        sh 'touch 1'
        timeout(5) {
          waitUntil {
            fileExists 'myFile.yaml'
          }
        }
      }  
    }
 }
}
```

### Lockable Resource database

its plugin need install (lockable Resource) after

Configure System => System => Lockable Resources Manager

Resource:

Name: SCHEMA_1

Labels: database

**SAVE**



```groovy
pipeline { 
  agent {
  label "linux"
  }
  stages {
    stage('stage1') {
      steps { 
        lock(label: 'database', variable: 'LOCKED RESOURCE', quantity: 1) {
          echo env.LOCKED_RESOURCE
          sh 'echo Hello World'
          sleep 60
        }       
      }  
    }
 }
}
```

### Groovy Scripts

* pipeline done failed its ok

* + ansi color plugin 


```groovy
pipeline { 
  agent {
  label "linux"
  }
  options {
    ansiColor('xterm')
  }
  stages {
    stage('stage1') {
      steps { 
        script {
          getFile().each { p ->
          sh "mkdir -p $p.folder"
          sh "touch $p.folder/$p.name"
          }
        println getProjectName()
        error("error message")
        }
      }
    }
  }
}
def getProjectName() {
  return 'JenkinsPipeline'
}
def List getFile() {
  return [
    [name: 'file01', folder: 'folder01'],
    [name: 'file02', folder: 'folder02'],
  ]
}

def loadColors() {
  CEND = '\033[0m'
  CVIOLENTBG2 = '\033[91m'
}
def error(String message) {
  loadColors()
  sh """
  set +x; echo "${CVIOLENTBG2} $message ${CEND}"
  """
}
```   

### From Pipeline from file hello.groovy

2 files :

Jenkinsfile
```groovy
def code 
pipeline { 
    agent {
    label "linux"
    }
    stages {
      stage('Stage1') {
        steps { 
          sh 'echo "stage1"'
          script {
            code = load 'scripts/hello.groovy'
            code.example1()
          }
        }
      }
      stage('stage2') {
        steps { 
          sh 'echo "stage2"'
          script {
              code.example2()
          }
        }
      }
    }
} 
```
hello.groovy
```groovy
def example1() {
  printnl 'Hello from example1'
}
def example2() {
  printnl 'Hello from example2'
}

return this
```
### Description
```groovy
getAllIteams()?.
findAll { !it.getDescription() }?.
each {
  println "'${it.fullName}' doesn't explain it does..."
}
println''
```

### Space in name 
```groovy
getAllIteams()?.
findAll { !it.name.contains(' ') }?.
each {
  println "'${it.fullName}' contains spaces!"
}
println''
```

### Log rotation
```groovy
getAllItems(Job.class)?.
each {
  found = true
  if (it.getBuildDiscarder() )
    found = it.getBuildDiscarder().getArtifactDaysToKeep() < 0 &&
            it.getBuildDiscarder().getArtifactNumToKeep() < 0 &&
            it.getBuildDiscarder().getArtifactDaysToKeep() < 0 &&
            it.getBuildDiscarder().getDaysToKeep() < 0 &&
            it.getBuildDiscarder().getNumToKeep() < 0 
  if (found)
  println"'${it.fullName}' doesn't have any log rotator configuration"
}
println ''
```


### Parameters in Jenkins

To add parameters to the Job, you must check the box "This project is parameterised":

![jenkins7](images/jenkins7.png)

To parameterize the pipeline in jenkins, the parameter section is used. The following types of parameters are available in the box:


- **Boolean Parameter** - defines boolean parameters. Can take values ***true or false***. You can also set a default value for a parameter. 

`booleanParam (name: «DryRun», defaultValue: true, description: «test run»)`


- **String Parameter** - defines a single line parameter. Supports removing spaces on both sides of the entered value. 

`string (name: «version», defaultValue: «r48», trim: false, description: «enter verision component»)`


- **Multi-line String Parameter** - define a multiline parameter.

`text (name: «releaseNotes», defaultValue: «none», description: «Оdescription of changes in the release»)`

- **Password** - allows you to define a password entry. Password details will not appear on startup `Job` and in `console log`.

`password (name: «password», defaultValue: «changeme», description: «enter password»)` 


- **Choice Parameter** - allows you to select multiple options from a list of previously preset options.

`choice (name: «env», choices: [«PROD», «DEV», «UAT»], description: «choose an environment to install the release»)` 



### How to declare parameters in Jenkinsfile

```groovy
pipeline {
    agent any
    parameters {
        booleanParam(name: "dryrun", defaultValue: true, description: "test run")
        string(name: "version", defaultValue: "r48", trim: true, description: "Entry verion of component")
        text(name: "releaseNotes", defaultValue: "Добавлены новые feature", description: "Description of changes in the release")
        password(name: "password", defaultValue: "changeme", description: "Entry password")
        choice(name: "env", choices: ["PROD", "DEV", "UAT"], description: "Sample multi-choice parameter")
    }
    stages {
        stage('DryRun') {
            when {
                expression { params.dryrun }
            }
            steps {
                echo "THIS IS DRYRUN!"
            }
        }
        stage("Build") {
            steps {
                echo "Build stage."
                echo "Hello $params.version"
            }
        }
        stage("Test") {
            steps {
                echo "Test stage."
            }
        }
        stage("Release") {
            steps {
                echo "Defined release notes $params.releaseNotes"
                echo "Starting release on $params.env"
            }
        }
    }
}
```

### Active Choice Parameter

The Active Choice Parameter is not added by default. To use it, you first need to install the Active Choices plugin.

Active Choices is used to parameterize a Jenkins Job and to create dynamic and interactive options. 
Active Choices options can be dynamically updated and displayed in the form of list fields, flags, switches, or user interface widgets with HTML..

![jenkins8](images/jenkins8.png)


3 additional options in the parameters section:

![jenkins9](images/jenkins9.png)

- ***Active Choices Parameter*** - allows to use Groovy script or Scriplet(plugin), to identify, whether the input will be calculated or it's already predetermined, and return results depending on the scripts being executed.

- ***Active Choices Reactive Parameter*** - like **Active Choice Parameter**. It will allow you to use Groovy or Scriplet, and its value varies depending on the value of the selected dependent parameter.

- ***Active Choices Reactive Reference Parameter*** contains parameters Active Choice Parameter + Active Choice Reactive Parameter, also adds new options.For example, HTML-widget, labeled or numbered lists, and input fields.



Let's move to practice: case study of using Active Choice Parameter

et's imagine that you have a task to make a Job that will allow development or L2 support to install a service of a certain version in the right environment. It seems that everything should be automated, but in practice there is a situation when a developer or QA engineer wants to test his version of a microservice on a certain environment. This could be done using regular string parameters, but let's simplify the task and provide a more user-friendly interface.

Let's start by adding component selection using the gitlab api, write a simple groovy script to get a list of projects, and then create a Jenkinsfile and add our parameter definition to it:



```groovy
properties([
    parameters([
        [$class: 'ChoiceParameter',
            choiceType: 'PT_SINGLE_SELECT',
            description: 'Select a choice',
            filterLength: 1,
            filterable: false,
            name: 'component',
            script: [$class: 'GroovyScript',
                fallbackScript: [classpath: [], sandbox: false, script: 'return ["Could not get component"]'],
                script: [classpath: [], sandbox: false, 
                    script: """
                         import groovy.json.JsonSlurperClassic
                            def list = []
                            def connection = new URL("https://run.mocky.io/v3/e406ee99-be79-4d50-818f-b186dad7f4f4")
                            .openConnection() as HttpURLConnection
                            connection.setRequestProperty('Accept', 'application/json')
                            def json = connection.inputStream.text
                            data = new JsonSlurperClassic().parseText(json)
                            data.each { component ->
                                list += component.name
                            }
                            return list
                    """
                ]]]])
])
pipeline {
    agent any

    stages {
        stage("Component Name") {
            steps {
                sh "echo Selected component  ${params.component}"
            }
        }
    }
}
```
Don't forget that new scripts need to be validated

Now let's move on to creating Active Choices Reactive Parameter. Depending on the selected component, we will be shown all versions of the component in the artifactor.

Let's create a simple groovy script and add a description to Jenkinsfile:

```groovy
[$class: 'CascadeChoiceParameter', 
        choiceType: 'PT_SINGLE_SELECT', 
        description: 'Select Version', 
        filterLength: 1, 
        filterable: true, 
        name: 'version', 
        referencedParameters: 'component', 
        script: [
            $class: 'GroovyScript', 
            fallbackScript: [
                classpath: [], 
                sandbox: false, 
                script: 
                    'return[\'Could not get version\']'
            ], 
            script: [
                classpath: [], 
                sandbox: false, 
                script: 
                    """
                            import groovy.json.JsonSlurperClassic
                            def list = []
                            def connection = new URL("https://run.mocky.io/v3/c782ae33-98a2-4994-acc4-14c0b5cc7655")
                            .openConnection() as HttpURLConnection
                            connection.setRequestProperty('Accept', 'application/json')
                            def json = connection.inputStream.text
                            data = new JsonSlurperClassic().parseText(json)
                            data.data.each { it ->
                              if  (it.component == component ) {
                                	list += it.version
                              		}
                               }
                            return list
                            """
            ]
        ]
    ]
```
An important change is the `referencedParameters` option. In it we specify the parameter on which the script execution depends. Run our Job, select the component and version and look at the result:

![jenkins10](images/jenkins10.png)





## integration with Gitlab

To get started, install the `Gitlab` plugin and `Gitlab branch`

![jenkins11](images/jenkins11.png)


After installation, need restart Jenkins

Go to Gitlab and go to project Access Tokens => Add New Token

![jenkins12](images/jenkins12.png)

select all scopes and copy past personal access token (do not copy stars)

Go to => Manage Jenkins => Credentials => Global => Add Credentials

![jenkins13](images/jenkins13.png)

 past personal access token to api token and **create!** do not copy stars

Dashboard => Manage Jenkins => System

![jenkins15](images/jenkins15.png)


test connection 

Create for user token

![jenkins16](images/jenkins16.png)

Go to Gitlab -> Settings -> Webhook

![jenkins17](images/jenkins17.png)

**!!! Webhook not work from local machine !!! i used ngrok**

![jenkins18](images/jenkins18.png)

Webhook need for synchronization change jenkins branch


https://user:password@mywebrepository.com/project/nameofproject.git

Let's go to New Item => Multibranch pipeline

**Click** Add source

Select **Git**

Enter your Repository URL (e.g.: git@your.gitlab.server:group/repo_name.git)

save 

after it jenkins scan all branch `Jenkinsfile` on Gitlab


for examlpe:

```groovy 

pipeline {
    agent any
    tools {
        maven 'maven-3.9.4'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                sh "env | sort"
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage(Only 'MR') {
          when {
            branch 'MR-*'
          }
          steps {
            sh "env | sort"
          }
        }
    }
}
```
MR = Merge Request









\\\





```groovy
pipeline {
    agent any
    post {
      failure {
        updateGitlabCommitStatus name: 'build', state: 'failed'
      }
      success {
        updateGitlabCommitStatus name: 'build', state: 'success'
      }
      aborted {
        updateGitlabCommitStatus name: 'build', state: 'canceled'
      }
    }
    options {
      gitLabConnection('your-gitlab-connection-name')
    }
    triggers {
        gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    }
    stages {
      stage("build") {
        steps {
          updateGitlabCommitStatus name: 'build', state: 'running'
          echo "hello world"
        }
      }
    }
   [...]
}
```
**gitLabConnection** - allows ours within a single Jenkins , work with multiple gitlab installations, by specifying the right one for each pipeline

**triggers** - Added an option to the triggers section: gitlab - allows you to configure a trigger by event from gitlab

**gitlabCommitStatus** - allows you to notify gitlab of build steps

**updateGitlabCommitStatus** - allow you to notify giltab of status pipeline
\\\

![jenkins14](images/jenkins14.png)

## Maven

Install plugin `pipeline maven integration` 

![jenkins19](images/jenkins19.png)

After installation, we have an additional snippet inside steps

Need Create new account in jenkins `name_of_user-gitlab-user`

if you get his error need install git on agents
```
ERROR: Error cloning remote repo 'origin'
hudson.plugins.git.GitException: Could not init /home/jenkins/workspace/maven
```
or path in gloobal jenkins => tools => git `/usr/bin/git`

***withMaven*** - any mvn command executed inside this block will be processed by the withMaven wrapper

***maven*** - version of maven declared in Tools



```groovy
pipeline { 
    agent {
    label 'linux'
    }
    stages {
      stage('checkout') {
        steps {
          git branch: 'master', credentialsId: '15388941-gitlab-user', url: 'https://gitlab.com/DanPoznanski/spring-boot-rest-example.git'
        }
      }
      stage('build') {
        steps { 
          withMaven(maven: 'maven-3.9.4')
           {
            sh "mvn clean install -DskipTests=true"
           } 
          }
        }
      stage('test') {
        steps {
          withMaven(maven: 'maven-3.9.4') {
            sh "mvn test"
          }
        }
      }
    }
} 
```

### Create XML Manage Files

когда нету доступа к интернету.. и хранить секреты.. 

its plugin 

when there is no access to the Internet.. and you nedd keep secrets..

Manage Jenkins => Add new Config

![jenkins20](images/jenkins20.png)


![jenkins22](images/jenkins22.png)



hot to integrate to pipeline xml file 


***mavenSettingsConfig*** - id settings.xml file from config file provider 

*** -s ${env.MVN_SETTINGS} *** - its take from tmp settings xml `/home/jenkins/workspace/maven@tmp/withMaven86ec651/setting.xml`

***mavenOpts*** - additional mvn options such as XmX, XmS, proxy settings and others

*** '-Xmx1G' *** = 1G RAM MEMORY

***mavenLocalRepo*** - path to the local repository

```groovy
pipeline { 
    agent {
    label 'linux'
    }
    environment {
      ADDITIONAL_MVN_OPTS = '-Xmx1G'
    }
    stages {
      stage('checkout') {
        steps {
          git branch: 'master', credentialsId: '15388941-gitlab-user', url: 'https://gitlab.com/DanPoznanski/spring-boot-rest-example.git'
        }
      }
      stage('build') {
        steps { 
          withMaven(maven: 'maven-3.9.4', mavenSettingsConfig: 'my-project', mavenOpts: '$ADDITIONAL_MVN_OPTS') {
            sh "env | sort"
            sh "mvn clean install -s ${env.MVN_SETTINGS} -DskipTests=true"
           } 
          }
        }
      stage('test') {
        steps {
          withMaven(maven: 'maven-3.9.4', mavenSettingsConfig: 'my-project', mavenOpts: '$ADDITIONAL_MVN_OPTS') {
            sh "mvn test -s ${env.MVN_SETTINGS}"
          }
        }
      }
    }
} 

```





Also withMaven offers the ability to work with '.jar' files obtained as a result of pipeline execution and test results.
`artifactsPublisher(disabled: true)`  - publish jar files

```groovy
pipeline { 
    agent {
    label "linux"
    }
    environment {
        ADDITIONAL_MVN_OPS = '-Xmx1G'
    }
    stages {
      stage('checkout') {
        steps {
          git branch: 'master', credentialsId: 'user15388941-gitlab-token', url: 'https://gitlab.com/DanPoznanski/spring-boot-rest-example.git'
          }
      }
      stage('build') {
        steps { 
          withMaven(maven: '3.9.4',mavenSettingConfig: 'my-project' mavenOpts: '${ADDITIONAL_MVN_OPS}',options: [artifactsPublisher(disabled: true)])
           {
            sh "evn | sort"
            sh "mvn clean install -s ${env.MVN_SETTINGS} -DskipTests=true"
           } 
          }
        }
      stage('test') {
        steps {
          withMaven(maven: '3.8.6' mavenOpts: '${ADDITIONAL_MVN_OPS}',options: [artifactsPublisher(disabled: true)]) {
            sh "mvn test -s ${env.MVN_SETTINGS}"
          
          }
        }
      }
    }
} 
```















### Sonarqube

Install plugin `sonarqube scanner`

need server sonarqube






### EMAIL

plugin in stock `extension email notifaction`


```groovy
pipeline {
    agent {
              label:linux
    }
    parameters {
      choice(name: "env", choices: ["PROD", "DEV", "AUT"], description: "choice env")
    }
    stages {
        stage(maintenance 'PROD') {
          when {
              expression {
                params.env = 'PROD'
              }
          }
          steps {
                   sh 'sleep 10'
          }
        }
    }  
        post {
        always {
            emailext body: 'SUCCESS', subject: 'Maintenance finished', to 'danpoznansky@gmail.com'
        } 
      } 
}
```

### Nexus

Install plugin `nexus artifact upload`

pupeline utility steps


```groovy
pipeline {
    agent {
      label 'linux'
    }
    environment {
        ADDITIONAL_MVN_OPTS = '-Xmx256m -Xx:+UseConcMarkSweepGC'
        NEXUS_VERSION = 'nexus3'
        NEXUS_PROTOCOL = 'http'
        NEXUS_URL = "192.168.1.333:8081"
        NEXUS_REPOSITORY = 'maven-local'
        NEXUS_CREDENTIALID = 'dev-nexus'
    }
    stages {
      stage('checkout') {
        steps {
          git branch: 'main',  url: 'https://github.com./silabeer/spring-boot-example.git'
        }
      }
      stage('build') {
        steps { 
          withMaven(maven: '3.8.6',mavenSettingConfig: 'my-project' mavenOpts: '${ADDITIONAL_MVN_OPS}',options: [artifactsPublisher(disabled: true)])
           {
            sh "evn | sort"
            sh "mvn clean install -s ${env.MVN_SETTINGS} -DskipTests=true"
           } 
          }
        }
      stage('test') {
        steps {
          withMaven(maven: '3.8.6' mavenOpts: '${ADDITIONAL_MVN_OPS}',options: [artifactsPublisher(disabled: true)]) {
            sh "mvn test -s ${env.MVN_SETTINGS}"
          
          }
        }
      }
    }
} 
}


```
```groovy```
pipeline {
    agent {
        label "master"
    }
    tools {
        maven "Maven"
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "you-ip-addr-here:8081"
        NEXUS_REPOSITORY = "maven-nexus-repo"
        NEXUS_CREDENTIAL_ID = "nexus-user-credentials"
    }
    stages {
        stage("Clone code from VCS") {
            steps {
                script {
                    git 'https://github.com/javaee/cargotracker.git';
                }
            }
        }
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}
```


Добавить условия запуска stage, если PROD, то предлагать пользователю сообщение о подтверждении действия
На любом этапе выполнения у нас может быть условие исполнения данного этапа.
Есть несколько различных условий:
1.	when { branch ‘develop’ } - выполнить stage если branch = develop
2.	when { environment name: "BUILD", value: "DEBUG" } - выполнить если переменная BUILD имеет значение DEBUG
3.	when {expression {expression { return params.DEPLOY } } - выполнить если переменная DEPLOY = true ( не false и не null )
Операторы И, ИЛИ, НЕ
allOf - работает как оператор И, все условия должны быть выполнены для запуска stage
Пример:
when { allOf {
environment name: "BUILD", value: "DEBUG"
branch 'develop' }
}
anyOf - работает как оператор ИЛИ, для выполнения этого stage должно быть выполнено любое из условий
when { anyOf  {
environment name: "BUILD", value: "DEBUG"
branch 'develop' }
}
not  - для выполнение stage, условие не должно быть выполнено
when { not {
branch  ‘main’}
}
Одной из главных особенностей Jenkins, является возможность менять поведение pipeline в зависимости от пользовательского ввода.
Шаг input позволяет нашему pipeline останавливаться и ждать ответа пользователя
Пример:
input 'Continue to deploy on PROD?'
Шаг input имеет несколько параметров:
message - сообщение которое будет отображаться пользователю
ok - надпись на кнопке Ok
submitter - список, разделенный запятыми, пользователей или групп пользователей, которым разрешено реагировать
submitterParameter - переменаня для хранения пользователя, который подтверждает
Пример:
input {
message "Ready to deploy?"
ok "Yes"
submitter "admin"
submitterParameter "SUBMITTER_USERNAME"
}
В данном пример, продолжение pipeline разрешено только пользователю admin
