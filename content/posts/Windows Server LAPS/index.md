---
title: "Windows Server 2025 LAPS"
discription: Windows Server 2025 LAPS
date: 2024-12-13T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server"]
showTableOfContents: true
--- 



## Local Administrator Password Solution (LAPS)

The "Local Administrator Password Solution" (LAPS) provides management of local account passwords of domain joined computers. Passwords are stored in Active Directory (AD) and protected by ACL, so only eligible users can read it or request its reset.





1. Download Legacy LAPS for old windows servers 

https://www.microsoft.com/en-us/download/details.aspx?id=46899


> Important

> The legacy Microsoft LAPS product is deprecated as of Windows 11 23H2 and later. Installation of the legacy Microsoft LAPS Microsoft Installer (MSI) package is blocked on newer OS versions, and Microsoft no longer considers code changes for the legacy Microsoft LAPS product.

> Use Windows LAPS for managing local administrator account passwords. Windows LAPS is available on Windows Server 2019 and later, and on supported Windows 10 and Windows 11 clients.

> Microsoft will continue to support the legacy Microsoft LAPS product on older versions of Windows (earlier than Windows 11 23H2) on which it was previously supported. That support will end upon the normal end of support for those OS versions.


Computer Configuration > Administrative Templates > System > LAPS



Get-LapsADPassword -Identity "DAN-PC"
