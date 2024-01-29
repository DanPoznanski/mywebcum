---
title: "Steam OS"
discription: Steam Os Based on arch linux 
date: 2022-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["linux","games"]
showTableOfContents: true
--- 

![steamos](images/SteamOS.svg)

# Installing PORTPROTON:

All command do in terminal

1. out from mode "Read only"
```bash
sudo steamos-readonly disable
```
2. Initialize the key
```bash
sudo pacman-key --init
```
3. Custom Keys
```bash
sudo pacman-key -u
```
4. adding keys from the repository
```bash
sudo pacman-key --populate
```
5. Install the `.cab` unzipping program
```bash
sudo pacman -S cabextract
```
6. Install dependencies
```bash
sudo pacman -Syu bash icoutils wget bubblewrap zstd cabextract bc tar openssl gamemode desktop-file-utils curl dbus freetype2 gdk-pixbuf2 ttf-font zenity lsb-release nss xorg-xrandr vulkan-driver vulkan-icd-loader lsof lib32-freetype2 lib32-libgl lib32-gcc-libs lib32-libx11 lib32-libxss lib32-alsa-plugins lib32-libgpg-error lib32-nss lib32-vulkan-driver lib32-vulkan-icd-loader lib32-gamemode lib32-openssl
```

7. Installing the program
```bash
wget -c "https://github.com/Castro-Fidel/PortWINE/raw/master/portwine_install_script/PortProton_1.0" && sh PortProton_1.0
```
8. Returning "Read-Only" mode
```bash
sudo steamos-readonly enable
``` 

