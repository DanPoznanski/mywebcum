---
title: "Windows Server CA"
discription: Windows Server
date: 2022-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["PKI","CA","Windows Server"]
showTableOfContents: true
--- 

In this Tutorial we’re going to configure a Two-Tier Enterprise PKI with Microsoft Server 2019 intended for Lab use. The advantage of a Two-Tier Enterprise PKI Hierarchy is that clients only trust the Root CA.  So if a Subordinate server gets compromised the Root CA does not have to be replaced. During normal operation the Root CA will be offline and Certificate requests are handled by the Subordinate CA. The Root CA is a non-domain joined device and will only be turned on issue a certificate for the Subordinate CA or to update the  Certificate Revocation List (CRL).


 - Overview

- Setup  Standalone Root CA

- Setup  Enterprise Subordinate CA

- Setup Group policy

- Deploy Policy Templates

In this setup we are going to build this Lab setup.



Before you start with this tutorial create the following servers and install them with Microsoft Server 2019. In this tutorial we are only configuring the servers.

![ws800](images/ws800.svg)


| Servername | 	OS | Role |	Notes |
| ---------- | --- | ---- | ----- |
|  DC01      | 	MS Server 2019 | Domain Controller |
|  OFFENT-CA01 | MS Server 2019 | Offline Standalone Root CA | non-domain joined |
| SUBENT-CA02 | MS Server 2019 | Online Enterprise Subordinate CA | Domain joined |




### Offline Root CA




#### Setup Offline Root CA

First we will create the `CApolicy.inf`. This is a configuration file that defines multiple settings that are applied to the root CA certificate and all other certificates issued by the root CA. This file needs to be created before the ADCS is installed on the root CA. For more information about the Syntax go here.

1. Start Powershell and type the following line and press **Enter**:
```
notepad c:\windows\capolicy.inf
```
![ws801](images/ws801.webp)

2. Select **yes** to create the new file
![ws802](images/ws802.webp)

3.  Because this is a lab setup I will only setup some basic settings for the Root CA. I will configure the following settings:

- Renewalinformation for the CA certificate.

- The validity period for the base CRL.

- Disable the AlternateSignatureAlgorithm 

- Disable the DefaultTemplates, these are not used because this is an offline CA.

For this lab I will use a random generated OID which is based on the Microsoft OID. Because these generated OID may not be unique you should request a private enterprise number at IANA (link). This number can be added to the CAPolicy.inf.

```
[Version]
Signature="$Windows NT$"

[Certsrv_Server]
RenewalKeyLength=4096 
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=20
CRLPeriod=Years
CRLPeriodUnits=1
AlternateSignatureAlgorithm=0
LoadDefaultTemplates=0
```

4. Save the file as **capolicy.inf** using **All files** and **ANSI** Encoding.
![ws803](images/ws803.webp)


5. Now we the role can be added and configured. Start the Server manager and select **Add roles and features**
![ws804](images/ws804.webp)

6. The **Add Roles and Features Wizard** will start, press **Next** to continue.
![ws805](images/ws805.webp)

7. Select **Role-based or feature-based installation** and press **Next**
![ws806](images/ws806.webp)

8. Use the default settings and press **Next** to continue.
![ws807](images/ws807.webp)

9. Select **Active Directory Certificate Services**
![ws808](images/ws808.webp)

10. A pop-up will appear, press **Add Features** to continue.
![ws809](images/ws809.webp)

11. Press **Next** to continue
![ws810](images/ws810.webp)

12. Press **Next** to continue.
![ws811](images/ws811.webp) 

13. Check if the Servername is correct and press **Next** to continue.
![ws812](images/ws812.webp) 

14. Check if the Servername is correct and press **Next** to continue.
![ws813](images/ws813.webp) 

15. Press **install** to add the Active Directory Certificate Services to the server.
![ws814](images/ws814.webp) 

16. When the installation has completed, press the link **Configure Active Directory Certificate Services on the destination server**
![ws815](images/ws815.webp) 

17. Use the default settings and press **Next**
![ws816](images/ws816.webp) 

18. Select **Certification Authority** and press **Next**
![ws817](images/ws817.webp) 

19. Because this server is non-domain joined only Standalone CA can be selected. Press **Next** to continue.
![ws818](images/ws818.webp




### Subordinate CA

With the Offline Root CA completed, we can now setup of the Subordinate CA server. This server is authorized by the Root CA to issue the certificates. During the setup the CA role will be added and configured. The server will also be authorized by the Root CA  The Subordinate CA Server is the SUBENT-CA02. Make sure that the server Subordinate server is domain joined before you start with the ADCS setup and that you have a domain account which is member of the Enterprise admins group.

 




### Setup Group Policy

The CA Servers are now configured. Now the domain computers/servers need to trust the certificates which are created by the Subordinate Server. This is done by adding the Root CA certificate to the “Trusted Root Certification Authorities” store.  The certificate can be added in multiple ways, but the easiest way is by adding it with a Group Policy. In this example a separate policy is created on the Domain Controller in the root of the domain. This is not required but just an example on how it’s possible.












### Deploy Policy Templates

After Setting up an Enterprise CA some Certificate policies are available without additional configuration. In this post I will demonstrate how to add Certificate Template and publish it.

Deploy Policy Templates