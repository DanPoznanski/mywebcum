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


## DNS

display dns in cache 
```powershell
ipcofig /displaydns
```

clear dns cache
```powershell
ipconfig /flusdns
```

### DNS Server Role

DNS Forwarders

DNS Manager > Properties > Forwarders > Edit > (MY_DNS_ADDRESS) > OK > OK

To configure DNS forwarders on Windows using the graphical user interface:

Click Start and then **Administrative Tools**. Click **DNS**.

Right-click the **DNS** server that you want to configure as a forwarder.

In the Action menu, select **Properties**.

Click the Forwarders tab.

Click **Edit**.

In the Edit Forwarders dialog, enter the primary IP address of the ​SIA​ recursive DNS server and press **Enter**.

Enter the secondary IP address of the ​SIA​ recursive DNS server and press **Enter**.

If other servers are listed as forwarders, delete this information. The primary and secondary recursive DNS servers should be the only forwarders listed.

To change the number of seconds that a DNS server waits for a response before it tries the IP address of the other DNS server, enter a new value in the **Number of seconds before forward queries times out** field.

Click **OK**.

Enable the Use **roots hints if no forwarders are available option**. This option ensures that DNS servers in a root hints file resolve the name locally.

In the properties dialog, click OK.

Powershell command CLI ADD DNS ROLE on dns server
```powershell
add-windowsfeature dns -includemanagementtools
```

 Display DNS in cache on Server DNS 
```powershell
show-dnsservercache
```

Clear DNS cache on Server DNS
```powershell
clear-dnsservercache
```

### DNS Conditional Forwarder

Open **DNS Manager**

Open the Run box using **Win+R**, type **dnsmgmt.msc**, and click **OK**

![ws1](images/ws1.webp)

**Open the New Conditional Forwarder Window**

Right click **Conditional Forwarders** under the server of your choosing, then select **New Conditional Forwarder…**

![ws2](images/ws2.webp)

**Configure the new conditional forwarder**

Enter the domain for which you would like queries forwarded in the **DNS Domain** box

Under IP addresses of the master servers, enter the IP Address(es) or FQDN(s) of the server(s) you will be forwarding these queries to

![ws3](images/ws3.webp)

You will see a check mark or X next to the servers you enter. Do not worry about an X if you know the IP is correct. This just means it could not do a reverse lookup on that IP.

If you entered a FQDN and it shows an X then you have a problem. You will need to do some DNS troubleshooting to determine why this will not resolve or enter the IP address instead.

When you done, simply click **OK** and you will now see this under the list of conditional forwarders.


![ws4](images/ws4.webp)


add Conditional Forwarder in powershell
```powershell
Add-DnsServerConditionalForwardZone pdan.dev 8.8.8.8
```

### DNS Configure Root Hints


**Open the DNS server properties**

Right click the DNS Server you would like to change the select **Properties**

![ws5](images/ws5.webp)

**Configure the new conditional forwarder**

**Open the New Name Server window**

Click the **Root Hints** tab and click and **Add** button

![ws6](images/ws6.webp)

**Add a new root server**

Type in a FQDN and click **Resolve**

OR

Type in a FQDN and enter an IP address or multiple IP addresses

![ws7](images/ws7.webp)

The IP address(es) should validate (green check mark) and then you can click **OK**
![ws8](images/ws8.webp)

All  cache dns save in file :
```
MyPc > local Disk (C:) > Windows > System32 > dns > CACHE.dns
```

Add Root hint in powershell 
```powershell
Add-DnsServerRootHint server1.test.com 8.8.8.8
```

### DNS Records Overview


|Record type |	Description                                                      |
| ---------- | ---------------------------------------------------------------- |
|A	          | Address record, which maps host names to their IPv4 address. dnc |
|AAAA	       |IPv6 address record, which maps host names to their IPv6 address. |
|CNAME	    |Canonical name record, which specifies alias names. |
| NS	| Name server record, which delegates a DNS zone to an authoritative server. |
| PTR | Pointer record, which is often used for reverse DNS lookups |
| MX	| Mail exchange record, which routes requests to mail servers. |
| SRV	| Service locator record, which is used by some voice over IP (VoIP), instant messaging protocols, and other applications. |
|  TXT | Text record, which can contain arbitrary text and can also be used to define machine-readable data, such as security or abuse prevention information. |
| SOA	| Start of authority record, which specifies authoritative information about a DNS zone. An SOA resource record is created for you when you create your managed zone. You can modify the record as needed (for example, you can change the serial number to an arbitrary number to support date-based versioning). |


**Powershell commands:**
```powershell
nslookup www.google.com
```
A Record command
```powershell
nslookup -type=A www.google.com
```
AAAA Record command
```powershell
nslookup -type=AAAA www.google.com
```
NS Record command
```powershell
nslookup -type=NS www.google.com
```
PTR Record command (test my site reverse ip)
```powershell
nslookup -type=ptr 96.96.136.185.in-addr.arpa
```
MX Record command 
```powershell
nslookup -type=mx pdan.dev
```
SOA Record command 
```powershell
nslookup -type=soa pdan.dev
```
TXT Record command (for trust servers)
```powershell
nslookup -type=txt pdan.dev
```

Powershell command to show all dns recorders
```powershell
Resolve-DnsName www.pdev.dev
```

### DNS Stub Zones

To configure a stub zone:

 Create a new zone:

 ![ws9](images/ws9.webp)

 Click next on the wizard welcome screen:

![ws10](images/ws10.webp)

Choose to create a stub zone:

![ws11](images/ws11.webp)

Select the replication scope of the stub zone:

![ws12](images/ws12.webp)

Select the domain name for this stub zone:

![ws13](images/ws13.webp)

Insert one or more name servers from where to load the zone info. Notice that zone transfer must be allowed:

![ws14](images/ws14.webp)

Review settings and complete the wizard:

![ws15](images/ws15.webp)

The stub zone has been created:

![ws16](images/ws16.webp)



### DNS Zone Transfer

### Stub Zone on Master Server

Here's how to create a stub zone up zone using DNS Manager.

1.From the Windows desktop, open the Start menu, select **Windows Administrative Tools** > **DNS**.

2.In the console tree, expand a DNS server then right-click, then select **New Zone**.

3.On the New Zone Wizard page, select **Next**.

4.On the Zone Type page, select Stub zone. If the DNS server is also an AD DS domain controller, you can store the zone information in Active Directory.

5.If you have chosen to store the zone data in AD DS, choose one of the following options:

- All DNS servers running on AD DS domain controllers in the forest.'

- All DNS servers running on AD DS domain in the domain.

- All domain controllers in this domain (for Windows 2000 compatibility).

- All domain controllers enrolled in a specific directory partition.

Specify the zone name. For example, `west.contoso.com`

7.On the Master DNS Servers page, provide the IP address of a DNS server that is authoritative for the target zone. For example, `172.23.90.124`.

8. Select Finish on the Completing the New Zone Wizard.

in powershell cli 
```powershell
Add-DnsServerStubZone -Name "west.contoso.com" -MasterServers "172.23.90.124" -PassThru -ZoneFile "west.contoso.com.dns"
```
---

### Create duplicate on Slave Zone

Here's how to create a secondary look up zone using DNS Manager.

1. From the **Windows desktop**, open the **Start** menu, select **Windows Administrative Tools** > **DNS**.

2. In the console tree, expand a DNS server then right-click, then select **New Zone**.

3. On the New Zone Wizard page, select **Next**.

4. On the Zone Type page, select Secondary zone.

5. On the Zone Name page, specify the name of the secondary zone. The name of the zone must match the name of the primary zone to replicate from. For example, `south.contoso.com`.

6. On the Master DNS Servers page, specify the IP addresses of one or more DNS servers that host copies of the primary zone. You need to ensure that the primary zone allows transfers to the DNS server hosting the secondary zone. For example, `172.23.90.124`.

Select Finish on the Completing the New Zone Wizard.

Here's how to create a secondary DNS zone using the Add-DnsServerSecondaryZone PowerShell command.

Add the secondary zone `western.contoso.com` using the zone file name `south.contoso.dns` and using the primary zone server at IP address `172.23.90.124` use the following command:
```PowerShell
Add-DnsServerSecondaryZone -Name "south.contoso.com" -ZoneFile "south.contoso.com.dns" -MasterServers 172.23.90.124
```
### Allow Configure Slave Server Zone on Master


**Configure the source DNS server to allow for zone transfers**

These steps will be accomplished on both DNS Servers.

To forward lookup zone properties,

1. From the Windows desktop, open the **Start** menu, select **Windows Administrative Tools** > **DNS**.

2. Click on the **Forward Look Zone** that you desire so configure.

3. Click on **Properties**.

4. Select the **Zone Transfers tab**.

![ws17](images/ws17.webp)

5. Select **Only to the following servers**.

![ws18](images/ws18.webp)

6. Click on **Automatically notify** and add the IP of the Forest B. Make sure the IP is resolved and the green check mark appears.

7. Click **OK**.

![ws19](images/ws19.webp)


in powershell command:
```powershell
Set-DnsServerPrimaryZone -name "west.contoso.com" -SecureSecondaries "TransferToZoneNameServer" -PassThru
```

### DNS PTR Records

Create Reverse Lookup Zone

1. Open the DNS Management Console

On your Windows Server type DNS in the search box to quickly find the DNS console.

![ws20](images/ws20.webp)

2. Create New Reverse Lookup Zone

In the DNS console right click on **Reverse Lookup Zones** and Select **New Zone**.

![ws21](images/ws21.webp)

3. Choose Zone Type (New Zone Wizard)

On the Zone Type page select Primary Zone.

![ws22](images/ws22.webp)

Choose to replicate to all DNS servers running on domain controllers in this domain.

![ws23](images/ws23.webp)

Choose IPv4 or IPv6, for this demo I’m setting up IPv4.

![ws24](images/ws24.webp)

Now, type in the start of the subnet range of your network.

For this demo, I’m creating a zone for subnet 192.168.0.0/24.

![ws25](images/ws25.webp)

Choose the dynamic update option.

I recommend picking the first option **Allow only secure dynamic updates**.

![ws26](images/ws26.webp)

That completes the wizard, click finish.


![ws27](images/ws27.webp)


**Verify Reverse Lookup Zone**

Back in the DNS console click on **Reverse Lookup Zone**

I can now see the new zone listed. The subnet will display backwards which is normal.

![ws28](images/ws28.webp)

Now I’ll click the 0.168.192.in.addr.arpa zone to view the DNS records.

![ws29](images/ws29.webp)

So far I have only the SOA and NS resource records, no PTR records.

Once clients start dynamically updating their DNS the PTR records should start populating. You can also manually create PTR records for systems that are not configured to dynamically update.


**How to Create PTR Records**

Let’s walk through manually creating a PTR record. This is only needed if a system is not configured to dynamically update. This may be the case for systems with static IP addresses like servers.

Right click the zone and select **New Pointer (PTR)**.

![ws30](images/ws30.webp)

Enter the Host IP Address and Host name fields and click **OK**.

I’m creating a record for IP, 192.168.0.206 with the hostname of `pc1`.

![ws31](images/ws31.webp)

Back in the DNS console, I can see the PTR record listed.

![ws32](images/ws32.webp)

How to Verify PTR Record Is Working (the answer must come `Name: pc1`) 
```powershell
nslookup 192.168.0.200
```

### Configure Round Robin 

Installing the DNS service role:

1. Open Server Manager

Click Manage in the top right corner and select **Add Roles and Features**.
Go through the installation wizard, select "DNS Service" (DNS Server) in the list of roles and install it

2.Create a zone:

Start > Server Manager > Tools > DNS Manager

Right click on **Forward Lookup Zones** and select **New Zone**

Go through the zone creation wizard, select a zone type (usually **Primary Zone**) and enter (for example, example.com).

In **DNS Manager** open the created zone.

To create an entry, right-click on the zone and select **New Host (A or AAAA)** for IPv4 or IPv6 addresses respectively.
Enter the entry name and IP addresses. _Create multiple entries with the same name but different IP addresses_. This will be your Round Robin.

```
Name Server:
name1.server.local
name2.server.local
```
### Configure DNS caching 

(optional, but recommended for improved performance):

Open the **DNS Manager**.

Right-click on your DNS server and select **Properties**.

Navigate to the **Advanced** tab.

Uncheck the **Enable round robin** option.

Click **OK** to save the changes.


### DNS Delegation

1. From the Windows desktop, open the **Start menu**, select **Windows Administrative Tools** > **DNS**.

2. In the console tree, expand a DNS server, right-click the DNS zone to delegate, then select **New Delegation**.

3. On the Delegated Domain Name page, enter the delegated domain name. For example, to delegate the subdomain `south.west.contoso.com``, enter south. The fully qualified domain name (FQDN) name is automatically be appended.

4. Select Add to specify the names and IP addresses of the DNS server to host the delegated zone.

Enter either:

The FQDN of the DNS server that is authoritative for the delegated zone, then select **Resolve**. Add other DNS servers if necessary, when validated select **OK**.

Or

Manually enter the IP address of the DNS server that is authoritative for the delegated zone. Add other DNS servers if necessary, when validated select **OK**.

Select Finish to complete the New Delegation Wizard.

Powershell command:
```powershell
Add-DnsServerZoneDelegation -Name "west.contoso.com" -ChildZoneName "south" -NameServer "west-ns01.contoso.com" -IPAddress 172.23.90.136 -PassThru -Verbose
```


### DNS Global Zone (CName)

```powershell
Set-DnsServerGlobalNameZone -AlwaysQueryServer $True
```


```powershell
Set-DnsServerGlobalNameZone -Enable $True -PassThru
```

```powershell
AddDnsServerPrimaryZone -name GlobalNames -ReplicationScope Domain
```
```powershell
AddDnsServerResourceRecordCName -HostNameAlias srv.test.local -name testhost -ZoneName GlobalNames 
```

### DNS Configure Recursion (anti DDOS DNS  )

disable recursion (also disables forwarders)
```powershell
Add-DnsServerRecusionScope -name . -EnableRecusion $false
```

Clear DNS cache
```powershell
Clear-DnsServerCache
```

Add scope 
```powershell
Add-DnsServerRecursionScope -name "OurScope" -EnableRecursion $true
```

Add Policy 
```powershell
Add-DnsServerQueryResolutionPolicy -name "OurRecursionPolicy" -action ALLOW -ApplyOnRecursion -RecursionScope "OurScope" -ServerInterfaceIP "EQ,192.168.1.10"
```
next example
```powershell
Add-DnsServerQueryResolutionPolicy -name "Allow anly facebook" -action ALLOW -ApplyOnRecursion -RecursionScope "OurScope" -Fqdn "EQ,*.facebook"
```


Delete my policy  `OurRecursionPolicy`
```powershell
Remove-DnsServerQueryResolutionPolicy -name "OurRecursionPolicy"
```

Delete my scope `QutScope`
```powershell
Remove-DnsServerRecursionScope -name "OurScope"
```


### DNS Security (DNSSEC)

Digital signatures (using public/private key pairs)

Trust Anchors (Trust Points, storing public keys)

Name Resolution Policy Table (NRPT, to force clients)

KSK - Key Signing key (public/private)

ZKS - Zone Signing Key (public/private)


**Step to configure DNSSEC**

1 – Open **Server Manager**,  click Tools and open **DNS Manager**.

2 – In the **DNS Manager**, browse to your Domain name, then right click domain name, click **DNSSEC** and then click **Sign the Zone**.

![ws33](images/ws33.webp)

2 - In the **Zone Signing Wizard** interface, click **Next**.

![ws34](images/ws34.webp)

3 - On the Signing options interface, click **Customize zone signing parameters**, and then click **Next**.

![ws35](images/ws35.webp)

4 – On the Key Master interface, ensure that **The DNS server CLOUD-SERVER is selected as the Key Master**, and then click **Next**.

![ws36](images/ws36.webp)

5 – On the **Key Signing Key (KSK) interface**, click **Next**.

![ws37](images/ws37.webp)

6 – On the **Key Signing Key (KSK) interface**, click **Add**.

![ws38](images/ws38.webp)

7 – **On the New Key Signing Key (KSK) interface**, click **OK**.

~*~ please spend some time to go through about key properties on the New Key Signing Key (KSK) interface.

![ws39](images/ws39.webp)

8 – On the **Key Signing Key (KSK) interface**, click Next.

![ws40](images/ws40.webp)

9 – On the **Zone Signing Key (ZSK) interface**, click **Next**.

![ws41](images/ws41.webp)

10 – On the **Zone Signing Key (ZSK) interface**, click **Add**.

![ws42](images/ws42.webp)

11 – On the **New Zone Signing Key (ZSK) interface**, click **OK**.

![ws43](images/ws43.webp)

12 – On the **Zone Signing Key (ZSK) interface**, click **Next**.

![ws44](images/ws44.webp)

13 – On the **Next Secure (NSEC) interface**, click **Next**.

~*~ NSEC is when the DNS response has no data to provide to the client, this record authenticates that the host does not exist.

![ws45](images/ws45.webp)

14 – On the **Trust Anchors** (TAs) interface, **check the Enable the distribution* of trust anchors for this zone check box**, and then click **Next**.

~*~ A trust anchor is an authoritative entity that is represented by a public key. The TrustAnchors zone stores preconfigured public keys that are associated with a specific zone.

![ws46](images/ws46.webp)

15 – On the **Signing and Polling Parameters interface**, click **Next**.

![ws47](images/ws47.webp)

16 – On the **DNS Security Extensions (DNSSEC) interface**, click **Next**, and then click **Finish**.

![ws48](images/ws48.webp)

![ws49](images/ws49.webp)

17 – In the DNS console, expand **Trust Points**, expand ae, and then click your domain name.

Ensure that the **DNSKEY resource records display**, and that their status is valid.

![ws50](images/ws50.webp)

18 – Open **Server Manager**, click Tools and open **Group Policy Management**.

19 – Next, open **Group Policy Management**, expand Forest: `Windows.ae`, expand Domains, expand `Windows.ae`, right-click **Default Domain Policy**, and then click **Edit**.

![ws51](images/ws51.webp)

20 – In the **Group Policy Management Editor interface**, under **Computer Configuration**, expand **Policies**, expand **Windows Settings**, and then click **Name Resolution Policy**.

~*~ In the right pane, under Create Rules, in the **Suffix box**, type `Windows.ae` to apply the rule to the suffix of the namespace.

~*~ Select both the `Enable DNSSEC in this rule check box` and the `Require DNS clients` to check that the name and address data has been validated by the DNS server check box, and then click Create.

![ws52](images/ws52.webp)


Update Groups Policy
```powershell
gnupdate /force
```

see information about Groups Policy
```powershell
gpresult /r
```
show information about Groups Policy
```powershell 
netsh namespace show policy
```
See information about TrustAnchor 
```powershell
Get-DnsServerTrustAnchor -name test.local
```

See information about Zone Settings
```powershell
Get-DnsServerDnsSecZoneSettings -zonename test.local
```
See all Zone Settings
```powershell
Get-DnsServerDnsSecZone
```
 
```powershell
Clear-DnsServerCache
Clear-DnsClientCache
```
 

**Configure the DNS Socket Pool**

- 2500 ports for use dns connection

1 – In domain Server, open **Windows PowerShell** and type : `Get-DNSServer`

~*~ This command displays the current size of the DNS socket pool (on the fourth line in the ServerSetting section). Note that the current size is 2,500.

~*~ Please take note that the default DNS socket pool size is 2,500. When you configure the DNS socket pool, you can choose a size value from 0 to 10,000. The larger the value, the greater the protection you will have against DNS spoofing attacks.

2 – Now lets change the socket pool size to 3,000.
```powershell
dnscmd /config /socketpoolsize 3000
```
3 – **Restart your DNS Server** for the changes to take effect.

~*~ confirm that the new socket pool size now is 3000

**Configure the DNS Cache Locking**

- 300 second Time to Live = 100 percent (%)

In Windows PowerShell, type `Get-Dnsserver`

~*~ This command will displays the current percentage value of the DNS cache lock.

~*~ Note that the current value is 100 percent.

This changes the cache lock value to 70 percent
```powershell
Set-DnsServerCache –LockingPercent 70
```
~*~ Please take note that you configure cache locking as a percentage value.

3 – Looking your DNS Manager Verify.

![ws53](images/ws53.webp)

### DNS Response Rate Limiting (RRL)

See information about Resonse time
```powershell
GEt-DnsServerResponseRateLimiting
```

- `WindowInSec`: This parameter specifies the time window, in seconds, during which the DNS server collects statistics about how many requests have been received. For example, if you set WindowInSec to 7 seconds, the DNS server will collect statistics for the last 7 seconds.

- `LeakRate`: This parameter specifies the rate of leakage or "leakage" in DNS server queries over a specified period of time. If the number of queries exceeds the set value, they will be filtered out or restricted.

- `TruncateRate`: This parameter specifies the rate at which DNS server responses are truncated or "pruned" over a specified period of time. If the number of responses exceeds the set value, they will be truncated or limited.

- `ErrorsPerSec`: This parameter specifies the maximum number of DNS errors that the server can handle per second. If the number of errors exceeds this value, they can be filtered or limited.

- `ResponsesPerSec`: This setting specifies the maximum number of DNS responses that the server can send per second. If the number of responses exceeds this value, they may be filtered or limited.

```powershell
Set-DnsServerResponseRateLimiting -WindowInSec 7 -LeakRate 4 -TruncateRate 3 -ErrorsPerSec 8 -ResponsesPerSec 8
```
Reset RRL settings to default values
```powershell
Set-DnsServerResponseRateLimiting -ResetToDefault -force
```

Set RRL to LogOnly mode
```Powershell
Set-DnsServerRRL -Mode LogOnly
```

Disable RRL 
```powershell
Set-DnsServerResponseRateLimiting -mode Disable -force
```

---

### DNS Policy based Load Balancing

![sw57](images/ws57.svg)


Create in domain scope 
```powershell
Add-DnsServerPrimaryZone -name "loadbalance.com" -ReplicationScope Domain
```
```powershell
Add-DnsServerPrimaryZone -ZoneName "loadbalance.com" -name "Scope-Heavy"
```
```powershell
Add-DnsServerPrimaryZone -ZoneName "loadbalance.com" -name "Scope-Light"
```
see status of Zonescope
```powershell
Add-DnsServerPrimaryZone -ZoneName "loadbalance.com"
```

```powershell
Add-DnsServerResourceRecord -ZoneName "loadbalance.com" -A -name "LB-WWW" -IPv4Address "192.168.1.10"
```
```powershell
Add-DnsServerResourceRecord -ZoneName "loadbalance.com" -A -name "LB-WWW" -IPv4Address "192.168.1.20" -ZoneScope "Scope-Light"
```
```powershell
Add-DnsServerResourceRecord -ZoneName "loadbalance.com" -A -name "LB-WWW" -IPv4Address "192.168.1.30" -ZoneScope "Scope-Heavy"
```


Create Policy 
 ```powershell
 Add-DnsServerQueryResolutionPolicy -name "Our-LB-Policy" -Action ALLOW -Fqdn "EQ,*" -ZoneScope "loadbalance.com,1;Scope-Light,1;Scope-Heavy,9" -ZoneName "loadbalance.com"

 See Status Zone Policy
 ```poweshell
 Add-DnsServerQueryResolutionPolicy -ZoneScope "loadbalance.com"
 ```

 On Client test :
```powershell
Clear-DnsClientCache
nslookup LB-WWW.loadbalance.com
Resolve-DnsName LB-WWW.loadbalance.com
ipconfig /displaydns
```



### DNS Time Policy

```Powershell
Add-DnsServerPrimaryZone -name "time.com" -ReplicationScope Domain
```

add subname test
```Powershell
Add-DnsServerResourceRecord -ZoneName "Time.com" -A -name test  -IPv4Address "10.10.10.10."
```
See time on server
```Powershell
 Get-Date -DisplayHint Time
```
 See Status Zone Policy
 ```poweshell
 Add-DnsServerQueryResolutionPolicy -ZoneScope "time.com"
 ```

Block site from 4:00 to 23:00
 ```poweshell
 Add-DnsServerQueryResolutionPolicy -ZoneScope "time.com" -name "Time-Policy" -Action DENY -TimeofDay "eq,04:00-23:00"
 ```
Test on Client
```powershell
Resolve-DnsName test.time.com
```

