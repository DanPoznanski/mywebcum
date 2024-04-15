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
![ws818](images/ws818.webp)

20. As this server is the root of the PKI hierarchy select **Root CA** and press **Next**
![ws819](images/ws819.webp)

21. Select **Create a new private key** and press **Next** to continue.
![ws820](images/ws820.webp)

22. Because this is the Root CA Certificate I use a longer Key length of 4096. This will increase the security.
![ws821](images/ws821.webp)

23. Use the default settings and press **Next** to continue.
![ws822](images/ws822.webp)

24. Because this server will be used in a Test Environment I extend the validity period to 10 years. Press **Next** to continue.
![ws823](images/ws823.webp)

25. Use the default settings and press **Next** to continue.
![ws824](images/ws824.webp)

26. Press **Configure** to configure the server.
![ws825](images/ws825.webp)

27. Press **Close** to continue.
![ws826](images/ws826.webp)

28. Press **Tools** in the Server Manager and select **Certification Authority**
![ws827](images/ws827.webp)

29. Right click the Servername and select **Properties**
![ws828](images/ws828.webp)

30. Select the **Extensions** tab
![ws829](images/ws829.webp)

31. In the **Extensions tab** select the extension **CRL Distribution Point (CDP)** and remove all locations except the `C:\*` Location.
![ws830](images/ws830.webp)

32. Because this server will be offline it cannot be contacted, therefore a location needs to be added to the subordinate server. Press **Add** to add the CDP on the Subordinate Server.
![ws831](images/ws831.webp)

33. Enter the following location and press **OK**
```
http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl
```
Replace <serverDNSName> with the dnsname of the Subordinateserver in this demo the location will be:
```
http://subent-ca02.vmlabblog.com/CertEnroll/%3CCaName%3E%3CCRLNameSuffix%3E%3CDeltaCRLAllowed%3E.crl
```
![ws832](images/ws832.webp)

Check the boxes beginning with __Include in CRLs*__ and __Include in the CDP*__ and press **Apply**
![ws833](images/ws833.webp)

35. Press **No** when asked to restart the service.
![ws834](images/ws834.webp)

36. Select in **Select extension** the **Authority Information Access (AIA)** and remove all locations except the `C:\*` Location.
![ws835](images/ws835.webp)

37. Press **Add** to add the AIA location on the Subordinate Server.
![ws836](images/ws836.webp)

38. Enter the following location and press **OK** 

```
http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt
```
Replace <serverDNSName> with the dnsname of the Subordinateserver in this demo the location will be:
```
http://SUBENT-CA02.vmlabblog.com/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt
```
![ws837](images/ws837.webp)

39. Check the box **Include in the AIA extension of issued certificates** and press **Apply**
![ws838](images/ws838.webp)

40. Press **Yes** when asked to restart the service.
![ws839](images/ws839.webp)

41. Select the **General** and select the Root Certificate and press **View Certificate**.
![ws840](images/ws840.webp)

42. Select the tab **Details** and press **Copy to File…**.
![ws841](images/ws841.webp)

43. In the Certificate Export Wizard press **Next**.
![ws842](images/ws842.webp)

44. Select **DER encoded binary X.509 (.CER)** and press **Next**.
![ws843](images/ws843.webp)

45. In File name enter **C:\Windows\System32\CertSrv\CertEnroll\<CA-NAME>-CA.cer** and press **Next**.
![ws844](images/ws844.webp)

46. Press **Finish** to export the RootCA Certificate.
![ws845](images/ws845.webp)

47. A popup will appear when the export was successful, press **OK** to continue.
![ws846](images/ws846.webp)

The setup of the Offline RootCA is now completed.

### Subordinate CA

With the Offline Root CA completed, we can now setup of the Subordinate CA server. This server is authorized by the Root CA to issue the certificates. During the setup the CA role will be added and configured. The server will also be authorized by the Root CA  The Subordinate CA Server is the SUBENT-CA02. Make sure that the server Subordinate server is domain joined before you start with the ADCS setup and that you have a domain account which is member of the Enterprise admins group.

**Setup Subordinate CA**

1. Start the Server manager and select **Add roles and features**
![ws847](images/ws847.webp)

2. The **Add Roles and Features Wizard** will start, press **Next** to continue.
![ws848](images/ws848.webp)

3. Select **Role-based or feature-based installation** and press **Next**
![ws849](images/ws849.webp)

4. Use the default settings and press **Next** to continue.
![ws850](images/ws850.webp)

5. Select **Active Directory Certificate Services**
![ws851](images/ws851.webp)

6. A pop-up will appear, press **Add Features** to continue.
![ws852](images/ws852.webp)

7. Select **Web Server (IIS)**
![ws853](images/ws853.webp)

8. A pop-up will appear, press **Add Features** to continue.
![ws854](images/ws854.webp)

9. Press **Next** to continue
![ws855](images/ws855.webp)

10. Press **Next** to continue.
![ws856](images/ws856.webp)

11. Check if the Servername before you start, this cannot be changed after the AD CS role has been installed and press **Next** to continue
![ws857](images/ws857.webp)

12. Keep the default role services **Certication Authority** and press **Next**
![ws858](images/ws858.webp)

13. On the Web Server Role (IIS) page press **Next**
![ws859](images/ws859.webp)

14. On the Role Services page select **Basic Authentication** and **Windows Authentication**. Press **Next** to continue.
![ws860](images/ws860.webp)

15. In the confirmation screen press **Install** to start the installation.
![ws861](images/ws861.webp)

16. When the installation has completed, press the link **Configure Active Directory Certificate Services on the destination server**
![ws862](images/ws862.webp)

17. Make sure your Domain credentials have been entered and not your local admin credentials. Otherwise you will not be able to configure a Enterprise CA. Press **Next** to continue.
![ws863](images/ws863.webp)

18. Select the box **Certification Authority** and press **Next** to continue.
![ws864](images/ws864.webp)

19. Select **Enterprise CA** and press **Next** to continue. (if Enterprise CA is not available check if the server is domain joined and the credentials entered in step 17)
![ws865](images/ws865.webp)

20. Select **Subordinate CA** and press **Next** to continue.
![ws866](images/ws866.webp)

21. Select **Create a new private key** and press **Next**.
![ws867](images/ws867.webp)

22. Use the default settings and press **Next** to continue.
![ws868](images/ws868.webp)

23. Use the default settings and press **Next** to continue
![ws869](images/ws869.webp)

24. Select the folder to save the Certificate Request and press **Next** to continue. (default is **c:\**)
![ws870](images/ws870.webp)

25. Use the default settings and press **Next** to continue.
![ws871](images/ws871.webp)

26. Press **Configure** to apply the configuration.
![ws872](images/ws872.webp)

27. When the configuration has succeeded a warning is shown. This is just a notification that the untill a certificate of the RootCA has been obtained and applied to the subordinate ca the Configuration is not completed.
![ws873](images/ws873.webp)

28. Switch over to the Offline Root CA (OFFENT-CA01) and browse to the folder `c:\windows\system32\certsrv\certenroll`. There should be three files, select and copy all files
![ws874](images/ws874.webp)

29. Switch back to the Subordinate CA (SUBENT-CA02) and browse to the folder `c:\windows\system32\certsrv\certenroll`. Paste all the files copied in the previous step.
![ws875](images/ws875.webp)

30. Rightclick the Root CA certificate which you just copied and select **Install Certificate**
![ws876](images/ws876.webp)

31. Select **Local Machine** and press **Next**
![ws877](images/ws877.webp)

32. Press **Browse** and select the **Trusted Root Certification Authorities** store. Press **Next** to continue.
![ws878](images/ws878.webp)

33. Press **Finish** to continue.
![ws879](images/ws879.webp)

34. After some time a popup will appear when the import has finished. Press **OK** to continue
![ws880](images/ws880.webp)

35. Create a new folder in `C:\inetpub\wwwroot` with the name `CertEnroll`
![ws881](images/ws881.webp)

36. Copy the RootCA Certificate and Certifate Revocation List from `C:\Windows\System32\CertSrv\CertEnroll` to `C:\inetpub\wwwroot\CertEnroll`
![ws882](images/ws882.webp)

37. Browse to the location entered in step 20 (default `c:\`) and copy the __*.Req__ file to the C: Drive on RootCA server.
![ws883](images/ws883.webp)

38. On the Root CA Server open **Certification Authority** rightclick the servername and select **All Tasks -> Submit new request…**
![ws884](images/ws884.webp)

39. Browse to the request file on the C: driver and press **Open**
![ws885](images/ws885.webp)

40. Select **Pending Requests**. Rightclick the pending request and select **All Tasks** -> **Issue**
![ws886](images/ws886.webp)

41. Select **Issued Certificates**. Rightclick the issued certificate and select **Open**
![ws887](images/ws887.webp)

42. Select **Details** and press **Copy to file…**
![ws888](images/ws888.webp)

43. Press **Next** to continue
![ws889](images/ws889.webp)

44. Select **Cryptographic Message Syntax Standard – PKCS #7 Certificates (.P7B)** and check the box **Include all certificates in the certification path if possible**. Press **Next** to continue
![ws890](images/ws890.webp)

45. Press **Browse…**
![ws891](images/ws891.webp)

46. Enter a name for the certificate and press **Save** (the default location is the Documents folder)
![ws892](images/ws892.webp)

47. Press **Next** to continue.
![ws893](images/ws893.webp)

48. Press **Finish** to export the CA Certificate.
![ws894](images/ws894.webp)

49. After some time a popup will appear when the export has finished. Press **OK** to continue.
![ws895](images/ws895.webp)

50. Copy the CA Certificate from the RootCA ( step 46) and switch to the subordinate server to paste the file.
![ws896](images/ws896.webp)

51. On the Subordinate CA open the Certification Authority. Rightclick the Servername and select **All Tasks** -> **Install CA Certificate**
![ws897](images/ws897.webp)

52. Select the copied CA Certificate and press **Open**
![ws898](images/ws898.webp)

53. Rightclick the Servername and select **All Tasks** -> **Start Service**
![ws899](images/ws899.webp)

### Setup Group Policy

The CA Servers are now configured. Now the domain computers/servers need to trust the certificates which are created by the Subordinate Server. This is done by adding the Root CA certificate to the “Trusted Root Certification Authorities” store.  The certificate can be added in multiple ways, but the easiest way is by adding it with a Group Policy. In this example a separate policy is created on the Domain Controller in the root of the domain. This is not required but just an example on how it’s possible.












### Deploy Policy Templates

After Setting up an Enterprise CA some Certificate policies are available without additional configuration. In this post I will demonstrate how to add Certificate Template and publish it.

Deploy Policy Templates