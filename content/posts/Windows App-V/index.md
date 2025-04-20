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










## Tools for



1. Download ADK for Windows. 

https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install


2. Download Office Deployment Tool







 ==============
Enable-Appv
===============
Get-AppvStatus
==================



Enable EU-V
===================
Enable-Uev
===================
Get-UevStatus
===================




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