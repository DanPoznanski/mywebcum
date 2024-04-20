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

![ws4](images/wds4.webp)

1. Open **Manage > Add Roles and Features**
![ws5](images/wds5.webp)

2. Next.. 
![ws6](images/wds6.webp)

3. Next...
![ws7](images/wds7.webp)

4. Next...
![ws8](images/wds8.webp)

5. Select **Windows Deployment Services**
![ws8](images/wds9.webp)

6. Select **Add Features**
![ws10](images/wds10.webp)

7. Next..
![ws11](images/wds11.webp)

8. Next.. 
![ws12](images/wds12.webp)

9. Next.. 
![ws13](images/wds13.webp)

10. Next.. 
![ws14](images/wds14.webp)

11. Install..
![ws15](images/wds15.webp)

12. Install and Yes..
![ws16](images/wds16.webp)

13. Close
![ws17](images/wds17.webp)


#### Config WDS

1. **Server Manager > WDS**
![ws18](images/wds18.webp)

2. Left Click on the Server and Right Click on the **Configure Server**
![ws19](images/wds19.webp)