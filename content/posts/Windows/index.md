




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

## SwitchTeam in Windows 

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

