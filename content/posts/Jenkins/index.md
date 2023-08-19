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

### agent ssh jenkins

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





```bash
sudo su
```
```bash
vi docker-compose.yml
```
```yml
1  # docker-compose.yaml
2  version: '3.8'
3  services:
4    jenkins:
5      image: jenkins/jenkins:lts
6      privileged: true
7      user: root
8      ports:
9       - 8080:8080
10      - 50000:50000
11    container_name: jenkins
12    volumes:
13      - /home/${myname}/jenkins_compose/jenkins_configuration:/var/jenkins_home
14      - /var/run/docker.sock:/var/run/docker.sock
```

```bash
docker-compose up -d
```

cd /home/${myname}/jenkins_compose/jenkins_configuration

cd secret

cat initialAdminPassword

### ssh key generate

ssh-keygen -t rsa -f jenkins_slave




Dashboard > Credentials > System > Global Credentials (unrestricted)
+ add credentials

Kind:
ssh username with private key

id:
team01-agent-ssh

Username:
Jenkins

private key Enter directory > add key

In Termninal


cat jenkins slave 
(copy & paste in web)  

and push button "create"

copy private key to slave machine agent

```bash
ssh-copy id -i jenkins_slave.pub jenkins@vs02slave
```
dashboard> manage jenkins> manage nodes and clouds>
+new node
name node:
team01-agent
discription
agent for team01
number of executors
2
remote root directory:
/home/jenkins
labals:
linux
usage:
V only build jobs with label expressions matching this node
launch method:
Launch agents via SSH
host:
(numberofslaveipmachine) or dns host name of slave
Credentials:
jenkins
Host Key Verification Strategy:
Non verifying Verification Strategy

and push save and push on team01-agent and relaunch agent

and see log if all ok




## Jenkins Plugins Installation

- Web UI
- CLI
- /var/lib/Jenkins/plugins


### Jenkins Plugin. Most Useful

- Rebuilder
- Config File Provider
- Ansi Color
- Active Choice
 
 ### Global Tool Configurations

- Maven
- JDK
- Cradle
- SonarQube
- Allure
- Etc















```bash
```

```bash
```

```bash
```








```bash
```