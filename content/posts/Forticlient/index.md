---
title: "Fortinet"
date: 2025-11-14
description: "fortinet basic"
tags: ["Route","Firewall"]
type: post
weight: 20
showTableOfContents: true
---

![img00](images/00.png)


### Configuring the LAN interface, including DHCP server

Network > Interfaces > doble-click on `port5`
![img01](images/01.webp)

Alias: `LAN-HQ` Role: `LAN` IP/Netmask `172.16.10.1/24`
![img02](images/02.webp)


Administrative Access IPV4 : `HTTPS`, `PING`, `SSH` after Enable DHCP Server
![img03](images/03.webp)


Address range: `172.16.10.21-172.16.10.256`
![img04](images/04.webp)

Comments: `Lan port connected to HQ` and click `OK`
![img05](images/05.webp)

Double-click on `port6`
![img06](images/06.webp)

Alias: `WAN-ISP` Role: `WAN`
![img07](images/07.webp)

IP/Netmask: `172.16.20.1/30` and IPV4: `SSH`
![img08](images/08.webp)

Comments: `WAN port connected to ISP` and click `OK`
![img09](images/09.webp)

> Configuration of the LAN and WAN interfaces is complete.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Configure and monitor the default route
Network > Static Routes > click create New
![img10](images/10.webp)

Destination Subnet : `0.0.0.0/0` Gateway Address: `172.16.20.2` Interface: `WAN-ISP (port6)` and `OK`
![img11](images/11.webp)

Dashboard > Network > Click to expand 
![img12](images/12.webp)

![img13](images/13.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Create a firewall address of the internal subnet

Policy & Objects > Addresses > Click **Create New** 
![img14](images/14.webp)

Name: `Internal Network` IP/Network: `10.0.1.0/24`
![img15](images/15.webp)

Interface: `port3` and press `OK`
![img16](images/16.webp)
> The firewall address of the internal subnet is created.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Configure the firewall policy

Policy & Objects > Firewall Policy > Click **Create New** 
![img17](images/17.webp)

Name: `Internal Access` Incoming Interface: `port3` Outgoing Interface: `port1`
![img18](images/18.webp)

Click Source + > Select `Internal Network` > Click **Close** 
![img19](images/19.webp)

Destination + > Select `all` > Click **Close**
![img20](images/20.webp)

Service + > Select `HTTP` `HTTPS` `DNS` > Click **Close**
![img21](images/21.webp)

Enable `NAT`
![img22](images/22.webp)
> This is an outgoing traffic policy, and NAT is enabled. This allows FortiGate to translate the private address of the network device to the public IP address of the port1 interface.

Log allowed traffic: `All sessions` and Click `OK`
![img23](images/23.webp)

![img24](images/24.webp)
> Firewall configuration complete! 

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Verify the firewall policy configuration

1. for test open https://www.fortinet.com 

2. Policy & Objects > Firewall Policy > Right-click the policy
![img25](images/25.webp)

3. Click **Show Matching Logs**
![img26](images/26.webp)

![img27](images/27.webp)
> The Forward Traffic logs for all traffic matching this policy appears on the screen, verifying that FortiGate is applying the policy.


5. for test open cmd > telnet www.fortinet.com (its 23 port)

6. Policy & Objects > Firewall Policy > Right Click **Implicit Deny** policy > Click **Show Matching Logs** >
![img29](images/29.webp)
> Because Telnet traffic doesn't match the Internet Access policy, it is processed and blocked by the Implicit Deny policy.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Create a user account
**User & Authentication** > **User Definition** > **Create New**
![img30](images/30.webp)

![img31](images/31.webp)

Username: `fatima`  Password: `password` > **Next**
![img32](images/32.webp)

Click **Next**
![img33](images/33.webp)

![img34](images/34.webp)

![img35](images/35.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Configure remote authentication
**User & Authentication** > **LDAP Servers** > **Create New**
![img36](images/36.webp)

Name: `RemoteAuthServer`
Server IP/Name: `10.0.1.150` 
Server Port: `389` 
Common Name Indentifier: `uid` 
Distinguished Name: `ou=Training,dc=TrainingAD,dc=training,dc=lab.`
![img37](images/37.webp)
Bind Type: `Regular`
Username: `uid=aduser1,ou=Training,dc=TrainingAD,dc=training,dc=lab`
![img38](images/38.webp)

Password: `Training!` and Press **Test Connectivity** and **OK**
![img39](images/39.webp)


![img40](images/40.webp)
> Remote authentication is configured

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Create a user group
**User & Authentication** > **Users Groups** > **Create New**
![img41](images/41.webp)

Name: `sales` Type: `Firewall` members: `fatima`
![img42](images/42.webp)

Remote Groups **+ Add** > Remote Server: **RemoteAuthServer** > **OK**
![img43](images/43.webp)

![img44](images/44.webp)

A user group created
![img45](images/45.webp)

Click **OK**
![img46](images/46.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Add authentication to the firewall policy

**Policy & Objects** > **Firewall Policy** > **Internal Network** > **Edit**
![img47](images/47.webp)
Open **User/group**: `sales` > close  
![img48](images/48.webp)
Click **OK**
![img49](images/49.webp)

![img50](images/50.webp)
> The sales group is now part of the firewall policy source.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Verify and monitor firewall authentication
open website > **Open network login page** 
![img51](images/51.webp)

Username: `fatima` Password: `password` > **Continue**
![img52](images/52.webp)

![img53](images/53.webp)

![img54](images/54.webp)
**Dashboard** > **Status** > **Firewall Users**
![img55](images/55.webp)
The user fatima shows as authenticated and connected.
![img56](images/56.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;





### Apply SSL inspection

**Securtity Profiles** > **SSL/SSH Inspection** > Click on custom-deep-inspection > Click **Edit**
![img57](images/57.webp)

**Invalid SSL certificates** > **Allow**
![img58](images/58.webp)

Click **OK**
![img59](images/59.webp)

**Policy & Objects** > **Firewall Policy** > **Internal Network** > **Edit**
![img60](images/60.webp)

Web filter: `default` &nbsp; &nbsp; SSL inspection: `custom-deep-inspection` > **OK** > Warning **OK**
![img61](images/61.webp)

![img62](images/62.webp)
> The security profiles are now applied to the firewall policy


in browser, go to https://google.com > Advanced > Aceept the Risk and Continue
![img63](images/63.webp)

The lock with the warning indicates that you added a security exception.
![img64](images/64.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Install the CA certificate to avoid certificate warnings

**Securtity Profiles** > **SSL/SSH Inspection** > Click on **custom-deep-inspection** > Click **Edit**
![img57](images/57.webp)

**CA certificate** Download `Fortinet_CA_SSL` 
![img65](images/65.webp)

and install it 

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Create an antivirus profile

**Security Profiles** > **AntiVirus** > **default** > **Edit**
![img66](images/66.webp)

**Use FortiGuard outberak prevention database [X]** > **OK**
![img67](images/67.webp)

![img69](images/69.webp)
> The default antivirus profile edit is complete.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Apply antivirus to a firewall policy

**Policy & Objects** > **Firewall Policy** > **Internal Network** > **Edit**
![img70](images/70.webp)

**Policy & Objects** > **Firewall Policy** > **Internet Access** > **Edit**
![img71](images/71.webp)

Antivirus[V] `default` > SSL Inspection `deep-inspection` > **OK**  
![img72](images/72.webp)

![img73](images/73.webp)

> The security profile is now applied to the firewall policy

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Verify antivirus

Type https://www.eicar.org/download-anti-malware-testfile

download `Com-file` 
![img74](images/74.webp)


![img75](images/75.webp)

**Log & Report** > **Security Events** > **AntiVirus**
![img76](images/76.webp)

One or more **EICAR_TEST_FILE** entries appear, with the **Action** show as **Blocked**.
![img77](images/77.webp)
> Antivirus inspection is verified.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Configure Web Filter on FortiGate

#### Ensure that FortiGate has valid FortiGuard security subscription license

Status > Webfilter [see status]


#### Identify how the FortiGuard service categorizes specific websites

www.fortiguard.com/webfilter
![img111](images/111.webp)

#### Configure a web filtering profile to use FortiGuard category-based filters

Security Profiles > Web filter > default > edit 

Social Networking: block > OK
![img112](images/112.webp)

#### Apply the web filter security profile to a firewall policy

1. **Policy & Objects** > **Firewall Policy** > **Full Access** > edit

2. Web filter: `default` , SSL Inspection `custom-deep-inspection`, Log allowed traffic [Security events] > OK

#### Test the configured actions for FortiGuard category-based filters and examine logs 
1. type site
![img113](images/113.webp)

2. Log & Report > Security Events > Web Filter > logs  
![img114](images/114.webp)

### Configure Authenticate cation for a FortiGuard Category Filter

1. Security Profiles > Web Filter > default > edit 

2. FortiGuard Category Based Filter > Authenticate > Warning Interval : `5 minute(s)` Selected User Groups: `Ovveride_Permissions` > OK > OK

3. User & Authentication > User Definition > + Create New > Local User > Next > Username `student` Password `password`> Next > Next > User Group [x] `Overide_Permissions` > Submit 


#### Test the Authenticate action and examine logs
type in browser
![img115](images/115.webp)

username & password
![img116](images/116.webp)


![img117](images/117.webp)


&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Create a Custom IPS sensor 

**Security Profiles** > **Intrusion Prevention** > **Create New**
![img78](images/78.webp)

**Name:** `Radius_Signatures_Sensor` > **+ Create New**
![img79](images/79.webp)

**Type: Signature** > **Action: Block** > Type **Radius** > **Search Icon** 
![img80](images/80.webp)

**Add All Results** > **Edit IP Exemptions**
![img81](images/81.webp)

Click **Create New** 
![img82](images/82.webp)

Click **Source IP/Netmask**
![img83](images/83.webp)

Type `10.0.0.0/24` > **Apply** > **OK**
![img84](images/84.webp)

Click **OK**
![img85](images/85.webp)

Click **OK**
![img86](images/86.webp)


![img87](images/87.webp)

> This sensor is ready to be applied to a firewall policy

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Configure application control

**Security Profiles** > **Application Control** > **Create New** 
![img88](images/88.webp)

**Name:`Block_Video`**
![img89](images/89.webp)

**Video/Audio**: Block
![img90](images/90.webp)

Click **OK**
![img91](images/91.webp)

**Policy & Objects** > **Firewall Policy** > **Internet Access** > **Edit**
![img92](images/92.webp)

**Application control**: [x] `Block_Video`
![img93](images/93.webp)

Click **OK**
![img94](images/94.webp)

![img95](images/95.webp)

> The application control profile is configured.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Monitor application control 

**Type:** `www.youtube.com` 
![img96](images/96.webp)

**Log & Report** > **Security Events** > Double-Click **Application Control**

![img97](images/97.webp)

![img98](images/98.webp)

![img99](images/99.webp)




&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Comfigure and test a site-to-site IPsec VPN

#### Create the IPSec VPN using the VPN wizard 

1. VPN > VPN Wizard > Tunnel name: `HQ-to-Branch` Select a Template [x][Site to Site] > Begin

2. Remote Site, IP/FQDN `10.200.3.1` Remote site subnet can acess VPN `10.0.2.0/24`> 
![img118](images/118.webp)

3. VPN Tunnel, Pre-shared key: `password`, Next >

4. Outgoing interface that binds to tunnel `port1` local interface `port3` local subnets that can access VPN `10.0.1.0/24`, Next > 
![img119](images/119.webp)

5. Submit
![img120](images/120.webp)

6. 
![img121](images/121.webp)

&nbsp;&nbsp;&nbsp;

7. VPN > VPN Wizard > Tunnel name: `Branch-to-HQ` Select a Template [x][Site to Site] > Begin
![img122](images/122.webp)


8. Remote Site, IP/FQDN `10.200.1.1` Remote site subnet can acess VPN `10.0.1.0/24`> 

9. VPN Tunnel, Pre-shared key: `password`, Next >

10. Outgoing interface that binds to tunnel `port4`, local interface `port6`, local subnets that can access VPN `10.0.1.0/24`, Next > 

11. Submit

12. 
![img123](images/123.webp)

#### Test the site-to site IPsec VPN 

1. cmd > ping 10.0.2.10


VPN > VPN Tunnels > 

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Configure Fortigate for remote access IPsec VPN

**VPN** > **VPN Wizard** > Click **Remote Access**
![img100](images/100.webp)

**Tunnel Name**: `RemoteVPN` > Click **Begin**
![img101](images/101.webp)

**IP Range for connected endpoints** `10.0.0.70-10.0.0.80` **Subnet** `255.255.255.0` > **Next**
![img102](images/102.webp)

**Pre-shared key** `password` **User group** `Sales` > Next  
![img103](images/103.webp)

**Incoming Interface that binds to tunnel** `port1` **Local Interface** `port3` **Local Address** `InternalNetwork` > Next 
![img104](images/104.webp)

Click **Submit**
![img105](images/105.webp)

![img106](images/106.webp)
> VPN has been set up   

**Network** > **Interfaces** > **+**
![img107](images/107.webp)

![img108](images/108.webp)

**Policy & Objects** > **Firewall Policy**
![img109](images/109.webp)
> Examine the entry with the name containing the string **RemoteVPN**

**Policy & Objects** > **Addresses** > **Search**: `romotevpn`
![img110](images/110.webp)


&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Configure and Test SSL VPN with FortiGate

**VPN** > **SSL-VPN Portals** > Double-Click **full-access** >
![img124](images/124.webp)

![img125](images/125.webp)
Click **OK**
![img126](images/126.webp)

#### Configure VPN SSL settings

1. **SSL-VPN status** `Enable` **Listen on Port** `443` **Service Certificate:** `Fortinet_Factory` > **+ Create New**
![img127](images/127.webp)

2. **+ Create New** > UserGroups `RemoteVpnUsers` Portal `Full-access`

3. Apply 
![img128](images/128.webp)


#### Configure Policy Firewall 

1. **Policy & Objects** > **Firewall Policy** > **Create New**

2. 
```
Name: `SSLVPNACCESS` 
Incoming interface: `SSLVPN tunnel Interface (ssl root)` 
Outgoing interface: `port3` 
Source:  `SSLVPN_TUNNEL_ADDR1`
Usergroup: `RemoteVPNUsers`
Destination: `LOCAL_SUBNET`
Service: `ALL`
```
OK >
![img129](images/129.webp)

![img130](images/130.webp)


&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Backup up the configuration and performing a filmwware upgrade 



Click **Admin** > **Configuration** > **Backup** > **OK**
![img131](images/131.webp)

> Now that you have a configuration backup, you can safely upgrade your FortiGate firmware.
> It is a best practice to backup your configuration before you start an upgrade, or you can do it during the upgrade. This simulation shows you how to do both.

Click **Admin** > **System** > **Firmware & Registration** > **local-FortiGate** > **Upgrade**
![img132](images/132.webp)

[x] **FortiGate Only** > **Next** >
![img133](images/133.webp)

**File Upload** > **Upload File** > **Select** `file` > Next  
![img134](images/134.webp)

Click **Next**
![img135](images/135.webp)

Click **Confirm and Backup config** > **System will reboot upon proceeding Are you sure you want to continue?** > **Yes**
![img136](images/136.webp)
> The system will be restart with the new firmware installed.

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Examining traffic logs

**Log & Report** > **Forward Traffic** > Click on log
![img137](images/137.webp)

Scroll down the Log Details window to
view the Action section.

Hover your mouse over the value next to
**Policy ID** to see details about the firewall
policy that generated this log.

Right Click on **Destination**
![img138](images/138.webp)


### Configure and Test Fortinet Security Fabric 


#### Verify the FortiAnalyzer configuration 

Device Manager > 
![img139](images/139.webp)
> Status Down 


#### Configure Logging on Local-FortiGate

Security Fabric > Fabric Connectors > Double-Click Logging & Analytics > Edit 
![img140](images/140.webp)

Status: Enabled Server: 10.0.1.210 Upload option [Real Time] > OK > Accept
![img141](images/141.webp)

![img142](images/142.webp)

#### Configure Local-FortiGate for the Security Fabric and make it Root firewall

Security Fabric > Fabric Connectors > Double-Click > Secure Fabric Setup > Select Server as Fabric Root
![img143](images/143.webp)

Allow other Security Fabric device to join > + > port3 > edit > 
![img144](images/144.webp)

**Security Fabric** [x] **Device detection** [x] > **OK** > **OK** > **Close**
![img145](images/145.webp)

Fabric Name: `FGT Operator Demo` Fabric global obkect [x]
![img146](images/146.webp)

![img147](images/147.webp)

#### Add Task ISFW to the Security Fabric
Security Fabric > Fabric Connectors > Double-Click > Secure Fabric Setup >
![img148](images/148.webp)

Join Existing Fabric > Allow other Security Fabric devices to join [x] > [+]
![img149](images/149.webp)

port1 > edit > 
![img150](images/150.webp)

**Security Fabric** [x] **Device detection** [x] > **OK** > **OK** > **Close**
![151](images/151.webp)

Upsteam FortiGate IP/FQDN `10.0.1.254` Management IP/FQDN [Specify] `10.0.1.200` Default admin profile `super_admin` > **OK**
![152](images/152.webp)

![153](images/153.webp)

#### Authorize ISFW in the Security Fabric 

System > Formware Regestration > `FGVM01000000077464`> Authorize > After Refresh Webpage
![154](images/154.webp)

![img155](images/155.webp)

![img156](images/156.webp)

#### Explore the Security Fabric 

Security Fabric > Physical Topology > Update Now
![img157](images/157.webp)

Security Fabric > Logical Topology 
![img158](images/158.webp)


Policy & Objects > Addresses > Create New 

Name:  MyDemoSubnet
IP/Network: 192.168.100.0/24
Fabric global-object [v]

OK>
![img159](images/159.webp)


Now `MyDemoSubnet` on 2 devices 
![img160](images/160.webp)

![img161](images/161.webp)

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### Examine current Security Fabric rating and apply  recomendations

**Security Fabric** > **Security Rating** > 
![img162](images/162.webp)
![img163](images/163.webp)

![img164](images/164.webp)
![img165](images/165.webp)

![img166](images/166.webp)

**Admin Idle Timeout** The timeout for idle administrator sessions is currently set to 10 minutes.
This is one of the parameters that does not meet industry best practices, which is why it is marked as Failed.
As indicated in the GUI, this parameter should be set to 10 minutes or less.

**System** > **Settings**
![img167](images/167.webp)

Scroll down the **System Settings** page to find the ldle timeout type `10` > **Apple**
![img168](images/168.webp)

Click [x] remove the two filters
![img169](images/169.webp)

> Note that the current number of Passed and Failed parameters was updated by increasing and reducing each one respectively by 1.
> You can repeat the same process to fix other parameters, making your device more secure.


&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

### Fortigate Hight Availability (HA)

![img170](images/170.webp)

#### Configure HA on the primary unit 

**System** > **HA** > **Mode:** Active-Passive  

Device priority `200`, Group ID `5`, Group Name `HA-DEMO`, Password `password`, Session pickup [x], Heartbeat inteface `port7`, > OK
![img171](images/171.webp)

Refresh webpage
![img172](images/172.webp)


#### Configure HA on the secondary unit 

**System** > **HA** > **Mode:** Active-Passive 

Device priority `128`, Group ID `5`, Group Name `HA-DEMO`, Password `password`, Session pickup [x], Heartbeat inteface `port7`, > OK
![img173](images/173.webp)

Refresh webpage
![img174](images/174.webp)


#### Verify the HA cluster

Refresh webpage > Status can be Synchronized
![img175](images/175.webp)


&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;

&nbsp;&nbsp;&nbsp;


### FortiLink
Verify the initial configuration of the three FortiSwitch devices

![img176](images/176.webp)


System > Feature Visibility > Switch Controller
![img177](images/177.webp)


Open CLI Console
![img178](images/178.webp)

System > Feature Visibility > Switch Controller on [x] > Apply
![img179](images/179.webp)


**Wifi & Switch Controller** > **FortiLink Interface** > **Create FortiLink Inerface** 

Name: `Fortilink` Alies `FortiLink`, Inteface members `port3`, IP/Netmask `10.0.13.254/24` Automatically authorize devices [x]
Address range `10.0.13.2-10.0.13.253`
![img180](images/180.webp)

#### Verify and test the management of the FortiSwitch devices form FortiGate

Refresh webpage
![img181](images/181.webp)


**Wifi & Switch Controller** > **Managed FortiSwitches** >
![img182](images/182.webp)

Rename switches 
![img183](images/183.webp)


![img184](images/184.webp)


![img185](images/185.webp)


**Wifi & Switch Controller** > **FortiSwitch VLANs** > **+ Create** >
Name: `DEMOVLAN` VLAN ID `200` > OK
![img186](images/186.webp)

![img187](images/187.webp)