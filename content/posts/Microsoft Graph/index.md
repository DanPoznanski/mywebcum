---
title: "Microsoft Graph"
discription: simple commands and tricks  
date: 2026-08-14 
draft: false
type: post
tags: ["Microsoft 365","API","Entra ID"]
showTableOfContents: true
--- 






![img01](images/Graph%20API%20logo.png)

## Microsoft GRAPH



### Install 

Install module
```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
```


Connect to Graph and tenant
```powershell
Disconnect-MgGraph -ErrorAction SilentlyContinue

Connect-MgGraph `
    -TenantId "YOUR_ACCOUNT.onmicrosoft.com" `
    -Scopes "RoleManagement.Read.Directory","Directory.Read.All"
```


See All Global Administrator
```powershell
$role = Get-MgDirectoryRole -Filter "displayName eq 'Global Administrator'"

Get-MgDirectoryRoleMember -DirectoryRoleId $role.Id -All |
ForEach-Object {
    Get-MgUser -UserId $_.Id `
        -Property DisplayName,UserPrincipalName,AccountEnabled
} |
Where-Object {
    $_.UserPrincipalName -ne "admin@temworld25.onmicrosoft.com"
} |
Select-Object DisplayName,UserPrincipalName,AccountEnabled |
Format-Table -AutoSize
```

Check what tenant you connected
```powershell
Get-MgContext | Select-Object Account,TenantId,Scopes
```

Disconnect from Graph
```powershell
Disconnect-MgGraph
```




```powershell
Disconnect-MgGraph -ErrorAction SilentlyContinue

Connect-MgGraph `
    -TenantId "YOUR_TENNANT.onmicrosoft.com" `
    -Scopes "User.ReadWrite.All","RoleManagement.ReadWrite.Directory"

$upn = "root@YOUR_TENNANT.onmicrosoft.com"

# Pop-up New Window  — Add new Password
$cred = Get-Credential -UserName $upn -Message "Please write New Password"

$passwordProfile = @{
    Password = $cred.GetNetworkCredential().Password
    ForceChangePasswordNextSignIn = $true
}

$newUser = New-MgUser `
    -DisplayName "Emergency Global Admin" `
    -UserPrincipalName $upn `
    -MailNickname "root" `
    -AccountEnabled `
    -PasswordProfile $passwordProfile

$gaRole = Get-MgRoleManagementDirectoryRoleDefinition `
    -Filter "displayName eq 'Global Administrator'"

$params = @{
    "@odata.type"    = "#microsoft.graph.unifiedRoleAssignment"
    roleDefinitionId = $gaRole.Id
    principalId      = $newUser.Id
    directoryScopeId = "/"
}

New-MgRoleManagementDirectoryRoleAssignment -BodyParameter $params
```

check new user if create
```powershell
Get-MgUser -UserId "root@YOUR_TENANT.onmicrosoft.com" |
Select-Object DisplayName,UserPrincipalName,AccountEnabled

Get-MgRoleManagementDirectoryRoleAssignment `
    -Filter "principalId eq '$($newUser.Id)'" -All |
Where-Object RoleDefinitionId -eq $gaRole.Id |
Select-Object PrincipalId,RoleDefinitionId,DirectoryScopeId
```

