---
title: "Windows Server"
discription: Windows Server 
date: 2022-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server"]
showTableOfContents: true
--- 






## Before  

Server Manager > Local Server > Computer name > Change > `YOUR_NAME` > [OK]

Firewall off

Ethernet > (Right click on) Ethernet > Properties > Internet Protocol Version 4 IPv4 

IP address: 192.168.1.3 

Subnet Mask 255.255.255.0

Default gateway: 192.168.1.1

DNS: 8.8.8.8

### Create Active Directory


Manage > Add Roles and Features Wizard > 

Next > Next > Next >

[x] Active Directory Domain Services

   [x] include management tools (if applicable)

   [Add Features]
   
Next > 

[x] .NET Freamework 4.6 features (2 of 7 installed)

Next > Next >

[x] Restart the destination server automatically if required

[Install]

Promote this server to a domain controller

[x] Add a new forest 

root domain name: XXXXXX.local  

Next >

[x] Domain Name System (DNS) server

[x] Global Catalog (GC)

Type the Directory Services Restore Mode (DSRM) password:

Password: my_strong_password 
 
Confirm password: my_strong_password 

Next > Next > Next > Next > Next > Next > [Install]

Reboot and Done!


***DHCP***

Manage > Add Roles and Features Wizard > 

Next > Next > Next >

[x] DHCP Server

   [x] include management tools (if applicable)

   [Add Features]

Next > Next > Next > 

   [x] Restart the destination server automatically if required

[Install]

Complete DHCP configuration

[Next] > [Commit] > [Close]

DHCP > Service name (Right Click) > DHCP Manager 


DHCP > IPv4 (right click) > New Scope > 

Name : DHCP Scope 

Start IP address: 192.168.1.50
End IP address: 192.168.1.254


[Next] > [Next] > [Next] >

[x] Yes, I want configure these options now

[Next] > Route (Default Gateway)

         IP address: 192.168.1.1 

         [Next] > Domain Name and DNS Servers
                  
                  IP address: 192.168.1.3
                  
                              8.8.8.8     [Add]

                  [Next] > [Next] > 
                                    [x] Yes, I Want to active this scope now





## Add Host to DNS 

DNS > (Right Click) Server name > DNS Manager > DC.mydomain.local > Forward Lookup Zones > mydomain.local > dc Host (A) > (Right-Click) on Blank > New Host > 

```
Name (uses parent domain name if blank): MyNameofHost

IP Address: 192.168.1.3
```



## Add disk ISCSI

```
File and Storage Services > iSCSI > to install iSCSI Target Server, start the Add Roles and Features Wizard

[x] iSCSI Target Server

[x] File Server 

Next > Next > Install 
```

To create an iSCSI virtual disk, start the New iSCSI Virtual Disk Wizard.
```

Select iSCSI Virtual Disk Location 
[Next] > 
iSCSI Virual Disk Name 
[Next] > 
iSCSI Target > New iSCSI Target >  
[Next] > 
Target Name Access > 
[Next] > 
Access Servers > [Add] > 
[x] Enter a Value for selected type >

[IP Address] Value: 192.168.1.5 (ip access hosts ) 

[Next] > [Next] > [Create] 
```
> Add Static IP Address to new Network Adapter and Restart
---


