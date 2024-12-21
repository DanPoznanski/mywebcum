---
title: "Exchange 2019"
discription: Windows Server Exchange
date: 2024-12-13T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server"]
showTableOfContents: true
--- 

## Add New User to AD for Exchange

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

if you have many DC servers you need sync  
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

1. you can see change:
![img18](images/18.png)

2. resync now 
```
repadmin /syncall 
```
![img16](images/16.png)


### Install ISO

Back to Exchange Server and Install iso 

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

Error its OK (i dont have connect to internet)
![img30](images/30.png)
Done
![img31](images/31.png)

Now test if all ok
![img32](images/32.png)

all ok "true"
![img33](images/33.png)


## On DC Add DNS Records

1. DNS > Server > Forward Lookup Zones > dan.local > New Hosts (A or AAAA) > Add `192.168.1.169` (IP Exchange server) > Add Host
![img34](images/34.png)

2. Add autodiscover for Outlook 
![img35](images/35.png)

3. DNS > Server > Forward Lookup Zones > dan.local > _tcp > Other New Records > SRV > Create Record
![img36](images/36.png)

4. Service: `_autodiscover` > Protocol: `_tcp` > Port `443` > Host `mail.dan.local` > Done
![img37](images/37.png)
5. Done
![img38](images/38.png)

6. Need Add MX record: 
![img39](images/39.png)


### Login on Web Admin Exchange Center

1. Open web browser and write
```
https://mail.dan.local/ecp
```
2. login with exchange account:
![img40](images/40.png)

3. Set time:
![img41](images/41.png)

4. Now Need Create address mail policies:
![img42](images/42.png)

5. mailflow > email address policies > +
![img43](images/43.png)

6. Click on plus 
![img44](images/44.png)

7. Select your fotmat and click on Save
![img45](images/45.png)

8. Give to police name `local` and click Save 
![img46](images/46.png)

9. Warning say i need apply police
![img47](images/47.png)

10. Apply police 
![img48](images/48.png)

11. Done
![img49](images/49.png)


### Move the DB to another disk 

1. First i need create folders `DB` after in folder `LOGS`, `EDB`
![img50](images/50.png)

2. First I need unmount database
![img51](images/51.png)

3. Dismount it
![img52](images/52.png)

4. Done
![img53](images/53.png)

5. Open Exchange Management Shell > write `Get-MailboxDatabase`
![img54](images/54.png)

6. Move database to disk d:
```
Move-DatabasePath -Identity DB -EdbFilePath d:\DB\EDB\db01.edb -LogFolderPath d:\DB\LOGS
```
![img55](images/55.png)

Key for EX2019
```
YCQY7-BNTF6-R337H-69FGX-P39TY
```
7. Watch if all OK
![img56](images/56.png)

8. Now Mount and Done
![img57](images/57.png)


### Add Mail to AD User 

1. recipients > + > Alies: `test` > Browser > Save
![img58](images/58.png)

2. Go to Outlook and send  
```
https://mail.dan.local/owa
```
Test to send messege , all ok 
![img59](images/59.png)