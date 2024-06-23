---
title: "NFS"
discription: NFS on Linux  
date: 2023-08-01T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","NFS"]
showTableOfContents: true
--- 

1. 
```
sudo apt update
```

2. 
```
sudo apt install nfs-kernal-server
```

3. view nfs
```
rpcinfo -p | grep nfs
```

4. view if ok 
```
cat /proc/filesystems | grep nfs
```

5. 
```
sudo systemctl enable nfs-server
```

6.
```
sudo mkdir /var/nfs
```

7. 
```
sudo chmod -R 777 /var/nfs
```

8. 
```
sudo nano /etc/exports
```

9. 
```
sudo /etc/exports
```

The /etc/exports Options

| General Options | Description | 
| --------------- | ----------- |
| secure          | Requires request originate on secure ports, those less than 1024. This is on by default. |
| insecure        | Turns off the secure option. |
| ro              | Allows only read-only access. This is the default. |