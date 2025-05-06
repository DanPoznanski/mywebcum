---
title: "Windows Server Core"
discription: Windows Server 2025
date: 2024-12-13T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server"]
showTableOfContents: true
--- 



## Only for proxmox

```
egrep -wo 'vmx|svm' /proc/cpuinfo
```

- vmx — Intel VT-x

- svm — AMD-V

Enable nested virtualization for KVM

Intel:
```
cat /sys/module/kvm_intel/parameters/nested
```

AMD:
```
cat /sys/module/kvm_amd/parameters/nested
```
> need be 1


```
nano /etc/pve/qemu-server/100.conf
```
need be
```
cpu: host
```






💥 Command will enable the RDP connections through the firewall on Server Core: 
```
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes 
```

💥 Command Edit the value in the registry to enable remote desktop : 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0 
```



### Install Hyper-V

install hyper-v and management tools
```
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
```

Install Rsat-Hyper-V-Tools
```
Install-WindowsFeature -Name Rsat-Hyper-V-Tools 
```

```
ServerCore.AppCompatibility~~~~0.0.1.0
```

```
Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
```

```
Restart-Computer
```

Run Hyper-V 
```
virtmgmt.msc
```



### Rename Computer
```
Rename-Computer -NewName Serv2025Core
```



#### Add to Domain


```
Add-Computer -DomainName dan.local -Credential dan\Administrator -restart -force
````




```
Get-ComputerInfo | Select-Object HyperV*
```


### Windows Admin Center

https://www.microsoft.com/en-us/evalcenter/evaluate-windows-admin-center

https://go.microsoft.com/fwlink/?linkid=2220149&clcid=0x409&culture=en-us&country=us




10 last restart

```
Get-EventLog system | where-object {$_.eventid -eq 6006} | select -last 10

```

download and repack
```
Invoke-WebRequest https://contoso/test.zip -outfile test.zip
Expand-Archive -path '.\test.zip' -DestinationPath C:\Users\Administrator\Documents\
```

remotely copy file 
```
$session = New-PSSession -ComputerName remotsnode1
Copy-Item -Path "C:\Logs\*" -ToSession $session -Destination "C:\Logs\" -Recurse -Force
```