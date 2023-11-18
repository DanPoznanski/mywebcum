---
title: "SonarQube"
discription: ngrok 
date: 2023-09-03T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","Sql"]
showTableOfContents: true
--- 

![sonarqube1](images/sonarqube1.svg)


## Install SonarQube on Ubuntu

### Modify Kernel System Limits

SonarQube uses Elasticsearch to store its indices in an MMap FS directory. It requires some changes to the system defaults.


Edit the sysctl configuration file

```bash
sudo nano /etc/sysctl.conf
```
Add the following lines:

```bash
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```
Save and exit the file

Reboot the system to apply the changes

```bash 
sudo reboot
```

### install Java

See vesion
```bash
java-version
```

install java 17 need for sonarqube 10  for sonarqube 9 need java 11 

```bash
sudo apt install openjdk-17-jre-headless -y
```
```bash
sudo apt-get install openjdk-17-jdk -y
````
### install PostgreSQL

Install and Configure Postgresql

```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main"   /etc/apt/sources.list.d/pgdg.list'
```
Add the PostgreSQL signing key.

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```
### Manual Repository Configuration

Import the repository key from `https://www.postgresql.org/media/keys/ACCC4CF8.asc`:
```bash
sudo apt install curl ca-certificates
sudo install -d /usr/share/postgresql-common/pgdg
sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc
```
Create `/etc/apt/sources.list.d/pgdg.list`  The distributions are called `codename`-pgdg. In the example, replace bookworm with the actual distribution you are using. File contents:
```bash
deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt bookworm-pgdg main
```
(You may determine the codename of your distribution by running lsb_release -c). For a script version of the above file creation, presuming you are using a supported release:

```bash
sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```
test if all ok :

```
apt update
```

maybe need add

```bash
dev [arch=amd64 signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc]  https://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main
```

Install PostgreSQL.

```bash
sudo apt install postgresql postgresql-contrib -y
```
Enable the database server to start automatically on reboot.

```bash
sudo systemctl enable postgresql
```
Start the database server
 
```bash
sudo systemctl start postgresql
```
Change the default PostgreSQL password

```bash
sudo passwd postgres
```
Switch to the postgres user

```bash
su postgres
```
Create a user named sonar
```bash
createuser sonar
```
 Log in to PostgreSQL:

```bash
psql
```
Set a password for the sonar user. Use a strong password in place of my_strong_password.

```bash
ALTER USER sonar WITH ENCRYPTED password 'my_strong_password';
```
Create a sonarqube database and set the owner to sonar.

```bash
CREATE DATABASE sonarqube OWNER sonar;
```
Grant all the privileges on the sonarqube database to the sonar user.

```bash
GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;
```
see database
```
\l
```
Exit PostgreSQL

```bash
\q
```
 Return to your non-root sudo user account.

```bash
exit
```

### Download and Install SonarQube

Install the zip utility, which is needed to unzip the SonarQube files
```bash
sudo apt-get install zip -y
```
Locate the latest download URL from the SonarQube official download page

```bash
sudo wget <download from offical site or pirate repository>
```

Unzip the downloaded file

```bash
sudo unzip sonarqube-10.2.0.77647.zip
```
Move the unzipped files to `/opt/sonarqube` directory
```bash
mv sudo mv sonarqube-10.2.0.77647 sonarqube && mv sonarqube /opt/
```
Add SonarQube Group and User

Create a dedicated user and group for SonarQube, which can not run as the root user

Create a sonar group

```bash
sudo groupadd sonar
```
Create a sonar user and set `/opt/sonarqube` as the home directory

```bash
sudo useradd -d /opt/sonarqube -g sonar sonar
```

Grant the sonar user access to the `/opt/sonarqube` directory

```bash
sudo chown sonar:sonar /opt/sonarqube -R
```

### Configure SonarQube

Edit the SonarQube configuration file.

```bash
sudo nano /opt/sonarqube/conf/sonar.properties
```

Find the following lines:
```bash
#sonar.jdbc.username=
#sonar.jdbc.password=
```
Uncomment the lines, and add the database user and password you created in Step 2.
```bash
sonar.jdbc.username=sonar
sonar.jdbc.password=my_strong_password
```
Below those two lines, add the sonar.jdbc.url.
```bash
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```
Save and exit the file

Edit the sonar script file for old version Sonarqube !!!

```bash
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
```
ADD to code

```bash
RUN_AS_USER=sonar
APP_NAME="SonarQube"

```
Save and exit



### Setup Systemd service

Create a systemd service file to start SonarQube at system boot

```bash
sudo nano /etc/systemd/system/sonar.service
```
Paste the following lines to the file
```bash
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
```
Save and exit the file

Enable the SonarQube service to run at system startup.
```bash
sudo systemctl enable sonar
```
 Start the SonarQube service

```bash
sudo systemctl start sonar
```

Check the service status

```bash
sudo systemctl status sonar
```


---

### Access SonarQube Web Interface

Access SonarQube in a web browser at your server's IP address on port 9000. For example:

`http://IP:9000` `http://127.0.0.1:9000`

Log in with username admin and password admin. SonarQube will prompt you to change your password.

---







