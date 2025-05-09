---
title: "Windows Server APP-V"
discription: Windows Server 
date: 2022-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server, Virtulization, App-v "]
showTableOfContents: true
--- 


![img0](images/img.svg)


## Tools for Create APP-V & and Office 

1. Download ADK for Windows. 

https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install


2. Download Office Deployment Tool

https://www.microsoft.com/en-us/download/details.aspx?id=49117

3. install and open 



3.  `outlook.xml` its config for 32 bit outlook
```
<!-- Office 365 client configuration file sample. To be used for Office 365 ProPlus apps, 
     Office 365 Business apps, Project Pro for Office 365 and Visio Pro for Office 365. 

     For detailed information regarding configuration options visit: http://aka.ms/ODT. 
     To use the configuration file be sure to remove the comments

     The following sample allows you to download and install the 64 bit version of the Office 365 ProPlus apps 
     and Visio Pro for Office 365 directly from the Office CDN using the Current Channel
     settings  -->

<Configuration>
  <Add OfficeClientEdition="32" Channel="PerpetualVL2024">
    <Product ID="Outlook2024Volume">
      <Language ID="he-il" />
    </Product>
  </Add>

  <!--  <Updates Enabled="TRUE" Channel="Current" /> -->

  <!--  <Display Level="None" AcceptEULA="TRUE" />  -->

  <!--  <Property Name="AUTOACTIVATE" Value="1" />  -->

</Configuration>
```
(all file download files in folder `office`)
```
C:\office>setup /download outlook.xml
```

```
C:\office>setup.exe /configure outlook.xml
```



```
C:\office>setup.exe /packager Excel2024.xml .\App-V
```













```
Enable-Appv
```

```
Get-AppvStatus
```


```
Enable EU-V
```
Enable-Uev
```

```
Get-UevStatus
```




------------
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force

-------------------------

----------------------
Import-Module Appvclient
----------------------
   

````````````````````
Set-AppvClientConfiguration -EnablePackageScripts 1	
--------------------

Set-AppvClientConfiguration -EnablePackageScripts 1

-----------------
Add-AppvClientPackage c:\office2013VLAppV\AppVpackages\....x86.appv | Publish-AppvClientPackage -Global
----------------- 