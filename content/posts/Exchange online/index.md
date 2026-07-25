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
