---
title: "NFS"
discription: NFS on Linux  
date: 2024-04-01T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","NFS"]
showTableOfContents: true
--- 

## Install Server NFS
1. update
```
sudo apt update
```

2. install nfs server
```
sudo apt install nfs-kernal-server
```

3. view if nfs ok
```
rpcinfo -p | grep nfs
```

4. other ways view if ok 
```
cat /proc/filesystems | grep nfs
```

5. enable nfs-server start after restart
```
sudo systemctl enable nfs-server
```

6. create folder
```
sudo mkdir /var/nfs
```

7. admin privilege on folder
```
sudo chmod -R 777 /var/nfs
```

8. edit exports file
```
sudo nano /etc/exports
```

9. The /etc/exports Options

| General Options | Description | 
| --------------- | ----------- |
| secure          | Requires request originate on secure ports, those less than 1024. This is on by default. |
| insecure        | Turns off the secure option. |
| ro              | Allows only read-only access. This is the default. |
| rw              | Allows read/write access. |
| sync            | Performs all writes when requested. This is the default.|
| async           | Performs all writes when the server is ready. |
| no_wdelay       | Performs writes immediately, not checking to see if they are related. |
| wdelay          | Checks to see if writes are related, and, if so, waits to perform them together. Can degrade performance. This is the default. |
| hide            | Automatically hides an exported directory that is the subdirectory of another exported directory. The subdirectory has to be explicitly mounted to be accessed. Mounting the parent directory does not allow access. This is the default. |
| no_hide         | Does not hide an exported directory that is the subdirectory of another exported directory (opposite of hide). Only works for single hosts and can be unreliable. |
| subtree_check   | Checks parent directories in a file system to validate an exported subdirectory. This is the default. |
| no_subtree_check | Does not check parent directories in a file system to validate an exported subdirectory. |
| insecure_locks | Does not require authentication of locking requests. Used for older NFS versions. |

| User ID Mapping | Description |
| --------------- | ----------- |
| all_squash      | Maps all UIDs and GIDs to the anonymous user. Useful for NFS-exported public FTP directories, news spool directories, and so forth. |
| no_all_squash   | The opposite option to all_squash. This is the default setting. |
| root_squash     | Maps requests from remote root user to the anonymous UID/GID. This is the default. |
| no_root_squash  | Turns off root squashing. Allows the root user to access as the remote root. |
| anonuid         |                                                                              |
| anongid         |Sets explicitly the UID and GID of the anonymous account used for all_squash and root_squash options. The defaults are nobody and nogroup. |

exemple:
```
/var/nfs 127.0.0.1(rw,sync,no_subtree_check)
```
```
/var/nfs 192.168.1.0/24(rw,sync,no_subtree_check)
```
for all ip = 0.0.0.0/0 or *
```
var/nfs 192.168.1.0/24(rw,sync,all_squash,no_subtree_check,root_squash,anonuid=1001,anongid=1001)
```

> iMPORTANT for update settings
```
exportfs -a
```
10. restart systemctl 
```
sudo systemctl restart nfs-server
```


### FIREWALL

Need open ports 111 and 2049
```
sudo ufw allow 111
sudo ufw allow 2049 
```


### On Client

1. install client nfs common
```
sudo apt install nfs-common
```
2. mount (ip server) + (shared link) (to folder)
```
sudo mount 192.168.1.149:/var/nfs /mnt/
```
3. test if all ok 
```
cd /mnt/
```
for always attach add to fstab service 
```
sudo /etc/fstab
```
