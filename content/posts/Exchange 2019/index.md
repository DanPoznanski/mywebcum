---
title: "Exchange 2019"
discription: Windows Server Exchange
date: 2024-12-13T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server"]
showTableOfContents: true
--- 

## Add New User to AD 

1. Tools > Active Directory Users and Computers
![img1](images/1.png)

2. Create New User `exchange`
![img2](images/2.png)

3. Password never expires
![img3](images/3.png)

![img4](images/4.png)

4. Users > exchange > Right click >  Member of > Add `Domain Admins`, `Schema Admins`, `Enterprise Admins`> OK
![img5](images/5.png)

![img6](images/6.png)


## On Echange 2019 Server 


1. Add to DNS DC to server for me its 192.168.1.220

![img7](images/7.png)


2. Dont forget disable dhcpv6 and join to domain 

![img8](images/8.png)


![img9](images/9.png)

3. Download and Install Net Framework 4.8 
```   
https://support.microsoft.com/en-us/topic/microsoft-net-framework-4-8-offline-installer-for-windows-9d23f658-3b97-68ab-d013-aa3c3e7495e0
```
![img10](images/10.png)

4. Download and Install Visual C++ Redistributable Package for Visual Studio 2012
```
https://www.microsoft.com/en-us/download/details.aspx?id=30679
```

5. Download and Install Visual C++ Redistributable Package for Visual Studio 2013
```
https://support.microsoft.com/en-us/topic/update-for-visual-c-2013-redistributable-package-d8ccd6a5-4e26-c290-517b-8da6cfdf4f10
```
![imgs11](images/11.png)


6. Download and Install Microsoft Unified Communications Managed API 4.0
```
https://www.microsoft.com/en-us/download/details.aspx?id=34992
```
![imgs12](images/12.png)


6. Restart machine


## Install components in Powershell 

1. Next step is to install the Remote Tools Administration Pack. Open Windows PowerShell and run below command.

![img13](images/13.png)


2. Open Powershell with Administrator

```
Install-WindowsFeature Server-Media-Foundation, NET-Framework-45-Core, NET-Framework-45-ASPNET, NET-WCF-HTTP-Activation45, NET-WCF-Pipe-Activation45, NET-WCF-TCP-Activation45, NET-WCF-TCP-PortSharing45, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
```
![img14](images/14.png)


3. Download and Install IIS URL Rewrite.
```
https://www.iis.net/downloads/microsoft/url-rewrite
```




2. Install from disk go to `f:\` iso file : 

```
cd f:
.\Setup.EXE /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareSchema

```
![img15](images/15.png)





### DC sync 

if you have many dc servers you need sync 

```
repadmin /syncall 
```
![img16](images/16.png)


### Back to Exchange Server
```
.\Setup.EXE /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareAD /OrganizationName:"dan"
```
![img17](images/17.png)


its my site domain `641514.cc` for other domain add command :
```
.\Setup.exe /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /PrepareDomain:641514.cc
```





### Back to DC 

you can see change
![img18](images/18.png)

resync now 
```
repadmin /syncall 
```
![img16](images/16.png)


### Back to Exchange Server

now i can run `setup.exe` > Next :
![img19](images/19.png)

![img21](images/21.png)

![img22](images/22.png)

![img23](images/23.png)

![img24](images/24.png)

![img25](images/25.png)

![img26](images/26.png)

![img27](images/27.png)

![img28](images/28.png)

![img29](images/29.png)

![img30](images/30.png)





