---
title: "Linux Basic Command and Map"
date: 2019-05-18
description: "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags: ["Linux",]
type: post
weight: 20
showTableOfContents: true
---


### Process

all process `ps -auxf` 


see all zombie process `ps -aux | grep 'Z'` 

see systemd
`ps -p 1`



SIGKILL --9 
SIGKILL --15

kill -9 PID

### Symlink & Hardlink


Symlink example: 

`ln original_file symlink1`

for Read: `readlink symlink1`  

for search `find . -type l` 

for see inode `stat original_file` 

Hardlink example:

`ln original_file hardlink1`

see number of **inode** 'stat original_file' 


see how many hardlinks `find . -inum 672135`

inode - inode is a data structure that stores information about a file or directory in a file system.

Hardlink its like original file


**Hardlink -> inode <- file <- Symlink**


### Password

`etc/password` and `etc/shadow`

change password 

`passwd username`

### Group

were can find group `etc/group`and password of groups `etc/gshadow`

```bash
sudo groupadd devops

sudo usermod -a -G devops username
```

delete group `delgroup` 


name

### Add users 

`useradd username -b /home/username -c "Username Usernamov" -g usergroup -p password`

new command 

`adduser  username`

change home directory to user:

`sudo usermod -d /home/evil -m username`

delete `userdel username`


### find someting..

`whereis passwd`

`ls -la /usr/bin/passwd


### SUID GSID Sticky

**USER + S(pecial)**

Commonly noted as **SUID**, the special permission for the user access level has a single function: A file with **SUID** always executes as the user who owns the file, regardless of the user passing the command. If the file owner doesn't have execute permissions, then use an uppercase **S** here.

Now, to see this in a practical light, let's look at the `/usr/bin/passwd` command. This command, by default, has the **SUID** permission set:

```bash
[tcarrigan@server ~]$ ls -l /usr/bin/passwd 
-rwsr-xr-x. 1 root root 33544 Dec 13  2019 /usr/bin/passwd

```
Note: the **s** where **x** would usually indicate execute permissions for the user.\

command 
`chmod u+s file`

**Group + S(pecial)**

Commonly noted as **SGID**, this special permission has a couple of functions:

- If set on a file, it allows the file to be executed as the **group** that owns the file (similar to **SUID**)

- If set on a directory, any files created in the directory will have their **group** ownership set to that of the directory owner

```bash
[tcarrigan@server article_submissions]$ ls -l 
total 0
drwxrws---. 2 tcarrigan tcarrigan  69 Apr  7 11:31 my_articles
```

This permission set is noted by a lowercase **s** where the **x** would normally indicate **execute** privileges for the **group**. It is also especially useful for directories that are often used in collaborative efforts between members of a group. Any member of the group can access any new file. This applies to the execution of files, as well. **SGID** is very powerful when utilized properly.

As noted previously for SUID, if the owning group does not have execute permissions, then an uppercase S is used.
&nbsp;
command: `chmod g+s directory`



**Other + t (sticky)**
The last special permission has been dubbed the "sticky bit." This permission does not affect individual files. However, at the directory level, it restricts file deletion. Only the **owner** (and root) of a file can remove the file within that directory. A common example of this is the /tmp directory:
```bash
[tcarrigan@server article_submissions]$ ls -ld /tmp/
drwxrwxrwt. 15 root root 4096 Sep 22 15:28 /tmp/
```


The permission set is noted by the lowercase **t**, where the **x** would normally indicate the execute privilege.

command 'chmod +t directory'



### Programs for working with packages

internal: 

see all packages in system

`dpkg -l`

search packages in system

`dpkg -s firefox-dbg`

know what files 

`dpkg -s` 

what files belong to the package

`dpkg -L openssh-client`

for install `dpkg -i program.deb` for remove  `dpkg -r program.deb`


search package

`apt-cache search telegram`

for search version of package

`apt-cache policy openssh-client`

and install version 1.8 `apt-get install openssh-client=1.8`


### link from where download files

`cat etc/sources.list` 



### Systemd 

where `usr/lib/systemd/system`

all units `systemctl list-units` for service `systemctl list-units type=service`

reload after change `systemctl daemon-reload`

for see logs `systemctl -u unitname`

Create unit in systemd 

`nano etc/systemd/system/apt.updater.service`

```bash
[Unit]
Description=Example of Systemd Unit

[Service]
Type=oneshot
ExecStart=apt-get update

[Install]
WantedBy=multi.user.target
```
Create unit in systemd timer

`nano etc/systemd/system/apt.updater.timer`

```bash
[Unit]
Description=Runs apt-get update every hour

[Timer]
onUnitActiveSec=1h
Unit=apt-updater.service

[Install]
WantedBy=multi.user.target
```
&nbsp;

### Mount disk iso 

```bash
sudo mkdir /media/ubuntu_iso 

sudo mount /home/victor/Downloads/ubuntu-20.04.2-live-server.amd64.iso /media/ubuntu_iso/ -o loop

```

see mount disk `df -h`  advanced `mount | grep ubuntu_iso` 

&nbsp;

**dd** 
```bash
echo "123456" > file 

dd if=file 
```
**img** from disk

```
dd if=/dev/sda1 of=sda1.img bs=4096
```

* bs - block of file = 4096 kb 

delete all files with zero  0000000000000
```bash
dd if=/dev/zero of=/dev/sdx bs=4096
```


### Mount hardisk


see all partition `fdisk -l` 

```bash
sudo fdisk /dev/sdb
```
- m - see all command

- g - create new GPT partition table

- w - write 

to create ext4 
```
sudo mkfs.ext4 -F /dev/sdb1
```
&nbsp;

mount manual but after restart will disappear

```bash
sudo mkdir /media/data/

sudo mount /dev/sdb1/ /media/data/
```
to auto mount after restart, to know **UUID** `sudo blkid `

```bash
sudo  nana /etc/fstab/
```



## .deb

see in deb package 
```
ar t package.deb
```
- tar.xz - its zip arhive

see in archive files 
```
ar p package.deb debian-binary
```
for see `tar.zx` files 
```
ar p package.deb debian.tar.xz | tar -tv -J
```
unzip arhive 
```
ar x package.deb
```
unzip archive `tar.xz`
```
tar xfv control.tar.xz
```
### Create .deb 

fist
```
sudo apt install dh-make devscripts
```
create need folder
```
mkdir mvdir-0.1
```
need in directory
```
cd mvdir
```

copy file `mvdir.sh` to folder `mvdir-01` 
```
cp ../../mvdir.sh .
```

edit `bashrc` 
```
nano /home/dan/.bashrc
```

add it and save
```
export CITY=Jerusalem
export DEBMAIL="dan@local"
export DEBFULLNAME="Dan"
```


run command source and for test `echo $DEBMAIL`
```
source /home/dan/.bashrc
```


for make sample deb need run in your folder `mvdir-01`
```
dh_make --indep --createorig
```
- indep - its be run for all system linux where have bash

- createorig - the file specified with `-f`  is copied in place. If no `-f`  is supplied either but `--createorig` is, the current directory is created into a new archive

&nbsp;
&nbsp;

you can see new file in folder and remove all `.ex` files `rm *.ex` and  `rm README`

now need create new file `nano install` and write: 
```
mvdir.sh usr/bin/
```
- if we have many bash scripts use `*.sh usr/bin` 

for change file `changelog` use command `dch`

for build .deb 
```
debuild -us -uc 
```
- `-us` - unsigned source it instructs no to sign the source files of the package with gpg key before create the package

- `-uc` - unsigned changes it instructs no to sign `changelog` files before creating the package

to install deb package to remove `-r`
```
sudo dpkg -i  package.deb
```

### Sign package deb

to create keys 
```
gpg --gen-key
```
to change and update version of changelog file.(achtung! email and name must be the same gpg-keys)
```
dch -i
```
`-i` - increment update change version of changelog


see gpg keys
```bash
gpg --list-keys
```
now sign build package 
```bash
debuild -b
```
to export gpg key
```bash
gpg --export -a "dan@local" > public.key
```

&nbsp;

&nbsp;

### Monitoring and Proc


see version linux or `uname -a` uname take information from:
```
cat /proc/version
```

see cpu info
```
cat /proc/cpuinfo
```
see time online
```
cat /proc/uptime 
```
see devices
```
cat /proc/devices
```
see what filesystems support 
```
cat /proc/filesystems
```
see all mounts 
```
cat /proc/mounts
```

&nbsp;

&nbsp;

see mem and swap ram 
```
free -h
```
oom killer score s
```
cat /proc/13/oom_score_adj
```
iftop to see internet traffic
```
iftop
```

### Ports 

see if port 8080 open
```
netstat -lptun | grep 8080
```



## Firewall

### Iptables

see all rules :
```
sudo iptables -L
```
for all rules and numbers and tables
```
sudo iptables --line-numbers -L -v -n
```
see rules only input :
```
sudo iptables -L INPUT  
```
all packet drop from 10.10.10.10 
```
sudo iptables INPUT -s 10.10.10.10 -j DROP 
```
all packet go to 10.10.10.10 drop
```
sudo iptables OUTPUT -s 10.10.10.10 -j DROP 
```
all packege drop to 10.10.10.0/24
```
sudo iptables OUTPUT -s 10.10.10.0/24 -j DROP 
```
all packet from 10.10.10.10 be drop to port 22
```
sudo iptables -A INPUT -p tcp --dport 22 -s 10.10.10.10 -j DROP 
```
to accept all 
```
sudo iptables -P INPUT ACCEPT 
```
disable ICMP answer 
```
sudo iptables -A INPUT -p icmp --icmp-type 8 -j DROP
```
for delete rules: 
```
sudo iptables -D s 10.10.10.0 =j DROP
```
all clean rules:
```
iptables -F
```
### Iptables persistent

we need iptables-persistent for be save after restart:

install
```
apt install iptables-persistent
```
run and save rules
```
sudo service netfilter-persistent save 
```
to see changes netfilter file
```
cat /etc/iptables/rules.v4
```
to restore all rules in file 
``` 
iptables-restore < /etc/iptables/rules.v4
```
```
sudo sh -c "iptables-restore < /etc/iptables/rules.v4"
```


### NAT forward

![linux2](images/linux2.svg)

to destination
```
iptables -t nat -A PREROUTING -p tcp -d 192.168.0.2 --dport 80 -j DNAT --to-destination 192.168.0.3:80
```
to source
```
iptables -t nat -A POSTROUTING -p tcp -d 192.168.0.3 --dport 80 -j SNAT --to-source 192.168.0.2:80
```

to source use masquerade
```
iptables -t nat -A POSTROUTING -p tcp -d 192.168.0.3 MASQUERADE 
```


## MAIL 








### Delete

delete files and folder
```
rm -rf <folder>
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



