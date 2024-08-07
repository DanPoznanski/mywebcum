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

## MDT

### Installing MDT​

All done with your break? I did encourage you to take one. Now we are going to install MDT. MDT is the toolkit we will use to control and add and modify our images remember? The link is below under the link section. The same place you grabbed the server ISO. For this we will want to make sure we download the 64bit version.

![wds54](images/wds54.webp)

Lets get started.

Once you have it downloaded go ahead and open it up to start the installer. It is more or less a "Next" fest but I will let you know if you need to do anything else. So click "Next" to begin.

![wds55](images/wds55.webp)

It's ok for this to go to the `C:\` drive so don't worry about that quite yet. Go ahead and click "Next".

![wds56](images/wds56.webp)

You will get one more notice about the customer experience program. Simply click "Install" and finally when its done, "Finish".

![wds57](images/wds57.webp)

Nice! Now for fun lets go ahead and open it, you guessed it. So we can pin it to our taskbar. It will be called "Deployment Workbench".
h
![wds58](images/wds58.webp)

## ADK

### Installing the ADK

Once you have the ADK lets begin the install. Again we will let this install to "C:\".

![wds59](images/wds59.webp)

After a few EULA windows we are given the selection menu. It is ok to leave it default. Simply press "Install".
![wds60](images/wds60.webp)

The ADK installed!

![wds61](images/wds61.webp)

Now that the main ADK is installed, lets install the PE add-on. Again like before, it is more or less a next fest. don't worry about the paths.

![wds62](images/wds62.webp)

"Wait doesn't this say Windows 10?" Yes! The Windows 11 ADK PE dropped support for x86. This is because Windows is going x64 only. However their is a bug in the control panel that still attempts to read the x86 directory, so we are working around it by installing the 10 ADK PE. Which is fine for our use case. You can read more about it here: https://learn.microsoft.com/en-us/a...oyment-workbench-crashes-when-opening-the-win

Nice! the PE finished installing!

![wds63](images/wds63.webp)


Finally, with the x86 issue taken care of. We need to fix one last bug with this specific build. The errata is here: https://learn.microsoft.com/en-us/mem/configmgr/mdt/known-issues

Run this command in an admin terminal:
```
reg.exe add "HKLM\Software\Microsoft\Internet Explorer\Main" /t REG_DWORD /v JscriptReplacement /d 0 /f
```
Then navigate too and edit.

```
C:\Program Files\Microsoft Deployment Toolkit\Templates\Unattend_PE_x64.xml
```
and replace the entire contents with the following:
```
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <component name="Microsoft-Windows-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State">
            <Display>
                <ColorDepth>32</ColorDepth>
                <HorizontalResolution>1024</HorizontalResolution>
                <RefreshRate>60</RefreshRate>
                <VerticalResolution>768</VerticalResolution>
            </Display>
            <RunSynchronous>
                <RunSynchronousCommand wcm:action="add">
                    <Description>Lite Touch PE</Description>
                    <Order>1</Order>
                    <Path>reg.exe add "HKLM\Software\Microsoft\Internet Explorer\Main" /t REG_DWORD /v JscriptReplacement /d 0 /f</Path>
                </RunSynchronousCommand>
                <RunSynchronousCommand wcm:action="add">
                    <Description>Lite Touch PE</Description>
                    <Order>2</Order>
                    <Path>wscript.exe X:\Deploy\Scripts\LiteTouch.wsf</Path>
                </RunSynchronousCommand>
            </RunSynchronous>
        </component>
    </settings>
</unattend>
```

### Configure MDT


Now that we have everything installed grab the script from my repo or the attachment on this post. We need to configure MDT and then create our first image!

To get started lets open MDT.

Once open right click on "Deployment Shares" and select "New Deployment Share".

![wds64](images/wds64.webp)

The first thing we are asked is important! Just like WDS we will change the drive letter. We want to keep the path but we want to send anything we put here to our data drive. In my case, it is "E:\"

![wds65](images/wds65.webp)

We will be asked some other default questions. We can leave the share name as is.

![wds66](images/wds66.webp)

The share description can also be left default.

![wds67](images/wds67.webp)

The next screen gives us some default options for the actual deployment. Remember we can be as hands on or hands off as we want. These boxes check configuration options that we will be changing later. They can be changed at any time, so for now lets leave them as is. Click "Next".

![wds68](images/wds68.webp)

We are now presented with a summary screen, click "Next" to move forward.

![wds69](images/wds69.webp)

The configuration will now begin and build the share that WDS/MDT will use when deploying images.

![wds70](images/wds70.webp)

Once its complete just hit finish.

![wds71](images/wds71.webp)

Sweet!! We can now expand the share in MDT we have options!

![wds72](images/wds72.webp)

Great job!

Lets set some basic properties. This is how we will handle modifications to our settings. These changes are stored in the "boot/pe .wim" that is loaded over PXE into ram. I am sure you remember talking to me about it. With that said, anytime you want to make changes we will need to update this wim since these changes are written to it. Lets start by right clicking on the share and selecting properties.

![wds73](images/wds73.webp)

Now that the configuration window is open, we land on the "General" tab. For starters, since you should embrace this century we can uncheck x86 under "Platforms Supported". It will save us some space.

![wds74](images/wds74.webp)

Next we want to go to the "Rules" tab. This tab controls the main sequencing of MDT. Those checkboxes I mentioned earlier were just a few of the ones that show up here. You can take a look at articles regarding the options that can be used: https://www.google.com/search?q=windows+mdt+rules&sxsrf=APwXEdcVNgiWeQXk0i1roFJX3qbOegwvCQ:1681511045527&ei=hdI5ZPbbH5HpxgHgrp3YCA&ved=0ahUKEwi2srfktKr-AhWRtDEKHWBXB4sQ4dUDCBA&uact=5&oq=windows+mdt+rules&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzIFCAAQgAQyBggAEBYQHjIGCAAQFhAeMggIABCKBRCGAzIICAAQigUQhgMyCAgAEIoFEIYDMggIABCKBRCGAzoKCAAQRxDWBBCwA0oECEEYAFDSCViOD2CQEGgBcAB4AIABqAGIAdcEkgEDMC40mAEAoAEByAEIwAEB&sclient=gws-wiz-serp

These are the defaults for "Rules" specifically:

```
[Settings]
Priority=Default
Properties=MyCustomProperty

[Default]
OSInstall=Y
SkipCapture=NO
SkipAdminPassword=YES
SkipProductKey=YES
SkipComputerBackup=NO
SkipBitLocker=NO
```
ere are some sane defaults you can use:

Grab you TZ info etc from here: https://www.ronnipedersen.com/2017/08/14/find-the-timezonename-for-your-sccmmdt-deployments/

You will see the attributes I added that say "techpowerup" and "Solaris17 Install". You can change these, but I left them as is so you can see where they appear during the install process.

```
[Settings]
Priority=Default
Properties=MyCustomProperty

[Default]
OSInstall=Y
SkipBDDWelcome=YES
SkipCapture=YES
SkipAdminPassword=YES
SkipProductKey=YES
SkipComputerBackup=YES
SkipBitLocker=YES
SkipUserData=YES
SkipTimeZone=YES
SkipLocaleSelection=YES

_SMSTSORGNAME="Techpowerup"

_SMSTSPackageName="Solaris17 Install"

KeyboardLocale=en-US
TimeZoneName=Pacific Standard Time
```

It should look like this. For now we will leave "Bootstrap.ini" as is.


![wds75](images/wds75.webp)

With this complete lets move on to the "Windows PE" tab. Select "x64" from the "Platform" dropdown at the top.

![wds76](images/wds76.webp)

We only want the wim. So we will **uncheck** the box that generates the ISO image.

![wds77](images/wds77.webp)

It should be noted that we can do all kinds of things here. Like change the image name. Change the background we see during install, add drivers etc. Today we will only work on these modifications though.

Now that that is done, go to the "Monitoring" tab. Its always good to enable monitoring, even if we wont be using it initially. Just check the box "Enable monitoring for this deployment share"

![wds78](images/wds78.webp)

With the configuration now complete click "Apply" followed by "OK".


## Getting our Images​

Now that we have MDT configured, we need to import some images into it. To do that we need to get them. This is where the script I wrote comes in. You should already have it so lets open it up.

The script will download the windows media creation tool and using some fancy command flags we can tear out and generate some windows images. I will tuck away the screenshots as they are really self explanatory. I will pick back up when its time to do stuff. For now when the script complete DO NOT delete the files it generates, we need them.

![wds79](images/wds79.webp)


![wds80](images/wds80.webp)

![wds81](images/wds81.webp)

![wds82](images/wds82.webp)

![wds83](images/wds83.webp)

![wds84](images/wds84.webp)

![wds85](images/wds85.webp)

![wds86](images/wds86.webp)

![wds87](images/wds87.webp)

![wds88](images/wds88.webp)

finally have some images!

![wds89](images/wds89.webp)

We don't need to go again for this guide so just select "No".

![wds90](images/wds90.webp)

Finally it will ask us if we want to delete the files we need. This is good if we have already worked with them, but we have not. We will select "No" here.

![wds91](images/wds91.webp)

The script will close and we move forward!

With our images in hand we need to import them into MDT and prepare WDS.

Open MDT and right click on "Operating Systems" then select "New Folder". Folders help us manage where we are placing images. Since we can have several flavors of the same OS. We will make a folder named "Windows 11".

![wds92](images/wds92.webp)

Go ahead and press "Next" a few times followed by "Finish" to create the folder.

![wds93](images/wds93.webp)

With our folder created we are going to right click on it and select "Import Operating System". Since we used the script to get us a wim we are going to select "Custom image file".

![wds94](images/wds94.webp)

![wds95](images/wds95.webp)



Links/Further reading​

Windows Server Evaluation Download: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
Download MDT: https://www.microsoft.com/en-us/download/details.aspx?id=54259
Download ADK: https://go.microsoft.com/fwlink/?linkid=2196127
Download ADK PE Add-on: https://go.microsoft.com/fwlink/?linkid=2120253
Add your own MDT steps to select a specific drive: https://social.technet.microsoft.co...m-pane-to-select-disk-in-mdt-wizard?forum=mdt

Extras​

Remember that monitoring we wanted enabled? You can see the machines being formatted in MDT. It also gives you the % completion and step. If a step is taking a long time, you can always take a look at what that step does in the task sequence.

