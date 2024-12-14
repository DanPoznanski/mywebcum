---
title: "Proxmox"
discription: Proxmox 
date: 2023-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["Virtualization"]
showTableOfContents: true
--- 





## Before Create Datacenter 


add ssh public key to hosts

```bash
nano /etc/ssh/ssh_config
```
```
PermitRootLogin yes

```

copy key to node1  
```bash
ssh-copy-id -i root@192.168.1.128
```
copy key to node2
```bash
ssh-copy-id -i root@192.168.1.129
```
copy key t node3
```bash
ssh-copy-id -i root@192.168.1.130
```
### Config Local DNS 

**Node1** 

connect to ssh 
```
ssh root@192.168.1.129
```

```bash
nano /etc/hosts 
```

```
192.168.1.128 pve1.local pve1
192.168.1.129 pve2.local pve2
192.168.1.130 pve3.local pve3
```
> test for ping  `ping pve2` and `ping pve3` 


**Wrong Repository**

```bash

nano /etc/apt/sources.list
```
```
# deb https://enterprise.proxmox.com/debian/pve stretch pve-enterprise

dev https://download.proxmox.com/debian/pve stretch no-subscription 
```
install htop and mc
```bash
apt install htop mc 
````
```bash
apt update
```
**Node2** 

connect to ssh 
```
ssh root@192.168.1.129
```

```bash
nano /etc/hosts 
```

```
192.168.1.128 pve1.local pve1
192.168.1.129 pve2.local pve2
192.168.1.130 pve3.local pve3
```
> test for ping  `ping pve1` and `ping pve3` 


**Wrong Repository**

```bash

nano /etc/apt/sources.list
```
```
# deb https://enterprise.proxmox.com/debian/pve stretch pve-enterprise

dev https://download.proxmox.com/debian/pve stretch no-subscription 
```
install htop and mc
```bash
apt install htop mc 
````
```bash
apt update
```
---

**Node3** 

connect to ssh 
```
ssh root@192.168.1.130
```

```bash
nano /etc/hosts 
```

```
192.168.1.128 pve1.local pve1
192.168.1.129 pve2.local pve2
192.168.1.130 pve3.local pve3
```
> test for ping  `ping pve1` and `ping pve2` 


**Wrong Repository**

```bash

nano /etc/apt/sources.list
```
```
# deb https://enterprise.proxmox.com/debian/pve stretch pve-enterprise

dev https://download.proxmox.com/debian/pve stretch no-subscription 
```
Install `htop` and `mc`
```bash
apt install htop mc 
````
```bash
apt update
```
---
## Create Cluster


**On Node1**

on Website:

Datacenter > Create Cluster > 
```
Cluster Name : cluster1
Ring 0 Address: 
```

Datacenter > Cluster > Join Information > Copy information (copy)

---

**On Node2**

Datacenter > Cluster > Join Cluster > Past (ctrl+V) > 
```
password: *******  # from Node1 
```
---

**On Node3**

Datacenter > Cluster > Join Cluster > Past (ctrl+V) > 
```
password: *******  # from Node1 
```
---




## CLI Command

Status node
```bash
pvecm status
```
if you only want a list of all nodes, use:
```
pvecm nodes
```

```
qm list
```
migrate machine from node1 (101 = id of machine), to (pve2 = name of node2)
```
qm migrate 101 --online pve2 --with-local-disks
```

## Root

```bash
# PermitRootLogin yes
PermitRootLogin without-password
```
### Repository non-subscription

![img1](images/1.png)


![img2](images/2.png)

### Notifications

![img3](images/3.png)

### Trusted TLS Certifications Requirements



### Windows VirtIO Drivers

after install windows need install `virtio.iso` last versions:
```
https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/?C=M;O=D
```


### Ventoy Install of Proxmox 8.1 halts at "Loading initial ramdisk"

I ran into the exact same problem.
(installed `proxmox-ve_8.1-2.iso` using `ventoy-1.0.97-linux.tar.gz`)

The file `/etc/default/grub.d/installer.cfg` introduces the `rdinit=/vtoy/vtoy` option:
```
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX    rdinit=/vtoy/vtoy"
```
In order to remove the vtoy grub leftovers permanently, change it to:
```
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX"
```
Then run `update-grub` and reboot.
