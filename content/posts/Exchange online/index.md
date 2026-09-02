---
title: "Exchange online"
discription: Exchange online
date: 2026-01-21
draft: false
type: post
tags: ["365","Exchange","Powershell"]
showTableOfContents: true
--- 


```powershell
Install-Module ExchangeOnlineManagement -Scope CurrentUser
```



Connect to Exchange online
```powershell
Connect-ExchangeOnline -UserPrincipalName admin@example.com
```

```powershell
Get-MailboxFolderPermission office@example\ןמוי
```

```powershell
Get-MailboxFolderPermission office@example\ןמוי | fl
```

Check guest permission 
```powershell

Get-MailboxPermission user1@example.com 
```


```powershell
# В Simple.com PowerShell
Add-MailboxFolderPermission -Identity user1@domain1.com:\Calendar -User guest@domain2.eu -AccessRights Editor
```

```powershell
Get-MailboxFolderStatics office@1all.co.il | Select-Object Name, FolderPath
```



## Connect to Microsoft.Graph

Install module Microsoft.Graph
```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
```

- Microsoft.Graph is the official PowerShell module (Microsoft Graph PowerShell SDK) designed to manage Microsoft cloud services through a single Microsoft Graph API interface.
    
    - **Users and groups**: account creation, password resets, license management, role assignment.

    - **Microsoft Teams, SharePoint, and OneDrive**: administration of channels, sites, and access permissions.

    - **Exchange Online**: management of mailboxes and distribution groups.

    -   **Intune (Endpoint Manager)**: device and security policy management.



Connect to Administrator
```poweshell
Connect-MgGraph -Scopes UserAuthenticationMethod.ReadWrite.All
```

check user method MfA
```powershell
Get-MgUserAuthenticationMethod -UserId user@domain.co.il
```
```
Id                                   AdditionalProperties
--                                   --------------------
3179e48a-750b-4051-897c-87b9720928f7  ...
```

