---
title: "Windows"
discription: Windows my old posts
date: 2020-01-07T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","vagrant","Terraform"]
showTableOfContents: true
--- 




## IP

free ip address
```cmd
ipconfig / release
```

see all ip address
```
ipconfig / all
```
renew dns
```
ipconfig / flushdns
```
renew ip to other
```
ipconfig / renew
```

set DNS to interface
```
netsh int ip set dns name="name_of_network_interface" static 8.8.8.8 8.8.4.4

```

fix net reset winsock 
```
netsh winsock reset
```

## Aggregation link in Windows 7-10 

Open PowerShell as Administrator: 
```powershell
Get-NetAdapter
```
Create a New NetSwithTeam
```powershell
NetSwitchTeam -Name "MyTeam" -TeamMembers "Enternet1", "Enternet2"
```

to remove
```powershell
Remove-NetSwitchTeam -Name "MyTeam"
```
see config
```powershell
Get-NetSwitchTeam
```


## Wim Capture Image

need external disk image capture to `d:\`

old method for windows 7
```
e:\imagex.exe /capture c: d:\install.wim "my windows 7 install" /compress fast /verify
```
new method
```
dism /capture-image /imagefile:"d:\Win7.wim" /capturedir:c:\ /name:“Windows10” /compress:fast /verify 
```
## Godmod

```
.{ED7BA470-8E54-465E-825C-99712043E01C}.
```


## 

```
dsregcmd /status
```

### Change password

```
net user administrator *
```

### Windows 11 bypass 
```
oobe\bypassnro
```

or

```
start ms-cxh:localonly
```


### Find Windows key

```
wmic path softwarelicensingservice get OA3xOriginalProductKey
```


### Powershell Aggrigate LAN

############# New-NetSwitchTeam ##################################

List adapters
```
Get-NetAdapter
```

Create new  aggrigate adapters
```powershell
New-NetSwitchTeam -Name "SwitchTeam01" -TeamMembers "Ethernet 2","Ethernet 3"
```

Remove aggirate LAN's
```powershell
Remove-New-NetSwitchTeam -Name "SwitchTeam01"

```