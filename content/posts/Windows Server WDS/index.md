---
title: "Windows Server WDS"
discription: Windows Server
date: 2022-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["WDS","PXE","Windows Server"]
showTableOfContents: true
--- 

## Windows Server WDS

### Create Automatically Install Images

1. First download "ADK download for Windows 10" from Microsoft


2. Install `adksetup.exe`

3. Select the features you want change 

- [X] - Deployment Tools

![wds1](images/wds1.webp)

4. Need Take `install.wim` from windows_10.iso but we have only in ... source\install.esd

5. Mount windows.iso go to folder "souces" and copy install.esd to c:\

We need cover from `install.esd` to `install.wim` and we need know index of windows version

6. open Deployment and Images Tools Environment 
```
dism /get-wiminfo /wimfile:c:\install.esd
```
7. after take your nember of index  what you want  and cover from  `.esd` format to `.vim`
```
dism /export image /sourceimagefile:c:\install.esd /sourceindex:3 /destinationimagefile:c:\install.wim /compress:mix /checkintegrity
```

8. now we need create file config for it go to [site1](https://www.windowsafg.com/) or [site2](https://schneegans.de/windows/unattend-generator/) after generate file config and download with name  "Autounattend.xml" 

9. after create new folder `windows_10` Copy all files from windows10.iso and past in the folder 

10. and copy from **c:\install.wim**  and file **Autounattend.xml** to `windows_10`

11. Now create new ISO 
```
oscdimg -u2 -m -o -lWIN10PROX64 -b "c:\Program Files (x86)\Windows Kits\Assessment and Deployment Kit\Deployment Tools\amd64\Oscimg\etfsboot.com" c:\windows_10 c:\Windows_10.iso
```
12. GUI
![wds2](images/wds2.webp)

![ws3](images/wds3.webp)


## Install WDS

![wds4](images/wds4.webp)

1. Open **Manage > Add Roles and Features**
![wds5](images/wds5.webp)

2. Next.. 
![wds6](images/wds6.webp)

3. Next...
![wds7](images/wds7.webp)

4. Next...
![wds8](images/wds8.webp)

5. Select **Windows Deployment Services**
![wds8](images/wds9.webp)

6. Select **Add Features**
![wds10](images/wds10.webp)

7. Next..
![wds11](images/wds11.webp)

8. Next.. 
![wds12](images/wds12.webp)

9. Next.. 
![wds13](images/wds13.webp)

10. Next.. 
![wds14](images/wds14.webp)

11. Install..
![wds15](images/wds15.webp)

12. Install and Yes..
![wds16](images/wds16.webp)

13. Close
![wds17](images/wds17.webp)


#### Configure WDS

1. **Server Manager > WDS**
![wds18](images/wds18.webp)

2. Left Click on the Server and Right Click on the **Configure Server**
![wds19](images/wds19.webp)

![wds20](images/wds20.webp)

3. Next
![wds21](images/wds21.webp)

4. Select **Standalone Server** Next
![wds22](images/wds22.webp)

5. Next
![wds23](images/wds23.webp)

6. Select **Yes** and Next 
![wds24](images/wds24.webp)

7. Select **Respond to all client computers** and Next 
![wds25](images/wds25.webp)

8. Finish
![wds26](images/wds26.webp)

###  Add Install Images to WDS:

When you configure Windows Deployment Services on your server, the next step is to add an image to your WDS to the client machine. There are two types of images that you need to add. There is an install.wim (actual Windows installation files) and the other is boot.wim (used to boot client machines).

1. Open windows deployment services console.

2. Expand your server.

3. Right-click on **Install Images** and then click **Add Install Image**.
![wds27](images/wds27.webp)

4. Provide the image group name and then click **Next**.
![wds28](images/wds28.webp)

5. Browse to the source folder (located on your Windows installation CD/DVD or local hard drive).
![wds29](images/wds29.webp)

6. Choose the **install.wim** file and click Next.
![wds30](images/wds30.webp)

7. Click Next.
![wds31](images/wds31.webp)

8. Select Image and Click Next.
![wds32](images/wds32.webp)

9. Check **Summary** and Click Next.
![wds33](images/wds33.webp)

10. Wait for the file to be copied. (This can take several minutes to complete).

11. Click On **Finish**.
![wds34](images/wds34.webp)

12. As you can see on the output above we have successfully added the **install.wim** Image File.
![wds35](images/wds35.webp)

### Add Boot Image to WDS:

1. In the Windows deployment services console, right-click **boot image**.

2. Click on the **Add boot image**.
![wds36](images/wds36.webp)

3. Browse to the source folder of your CD/DVD and locate **boot.wim**.
![wds37](images/wds37.webp)

4. Click Next.
![wds38](images/wds38.webp)

5. Rename Image File and Click Next.
![wds39](images/wds39.webp)

6. Check **Summary** and Click Next
![wds40](images/wds40.webp)

7. Wait for the file to be copied. (This can take several minutes to complete).

8. Click On Finish.

![wds41](images/wds41.webp)

9. As you can see on the output above we have successfully added the **boot.wim** Image File.

### Configure DHCP Scope Options:

Enabling bare-metal client systems on Windows Deployment Services (WDS) PXE-boot to kickstart the process of deploying Windows PE on a client system and running the Windows system. In some WDS environments, you might want to configure the following DHCP options to direct your PXE clients to the correct network boot file.

1. Now go to **Server Manager** and right-click on the DHCP server and click on **DHCP Manager**.

2. Right, click on **Scope Options** and click **Configure Options**.
![wds43](images/wds43.webp)

3. Add Boot **Server Hostname or IP** in cope Options.

**66 = DNS name of the WDS server**

![wds44](images/wds44.webp)

4. Option 67 Where can you get the boot file name to configure? This name will be something similar to **boot\x64\wdsnbp.com** where this route corresponds to the Remount folder on the WDS server.

**67 = boot file name**

![wds45](images/wds45.webp)


 5. Finally, we can start the WDS server. The WWS server has not been configured only. Right-click on the WDS server. hover your mouse over **all the tasks**, then click **Start**. Soon after the WDS server starts.

> Note – Connect LAN with WDS server to switch or the client computer and start WDS server. 

![wds46](images/wds46.webp)

As you can see on the output above we have successfully started the WDS server.

![wds47](images/wds47.webp)

> Also Read – Step by Step Configure MDT Server (Microsoft Deployment Toolkit) on windows server 2016


Deploy Windows with WDS:

Steps 1. The client will want to install the Windows os of the system. First of all, connect the first WDS server to the client computer with a LAN cable. To confirm the same you can use the DHCP Address Lease.

![wds48](images/wds48.webp)

2. Boot Client System With Network.
![wds49](images/wds49.webp)

3. Check WDS IP and Hostname and press F12 to Deploy Windows.
![wds50](images/wds50.webp)

The machine is booting via network and windows os files loading starts.

![wds51](images/wds51.webp)

4. Select your location and Keyboard Language.
![wds52](images/wds52.webp)

5. Enter WDS Credentials and connect WDS.
![wds53](images/wds53.webp)

Now you can install windows on a client computer with a normal windows installation process.

---