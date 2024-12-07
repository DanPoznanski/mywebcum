---
title: "OpenWrt"
discription: openwrt and wrt clone
date: 2024-11-01T21:29:01+08:00 
draft: false
type: post
tags: ["Linux","Routers"]
showTableOfContents: true
--- 



![img](images/OpenWrt_Logo.svg)

&nbsp;
&nbsp;

&nbsp;
&nbsp;

## BRI-R3 Mini

https://wiki.banana-pi.org/Getting_Started_with_BPI-R3_MINI

https://downloads.openwrt.org/snapshots/targets/mediatek/filogic/

https://firmware-selector.openwrt.org/?version=SNAPSHOT&target=mediatek%2Ffilogic&id=bananapi_bpi-r3-mini

https://openwrt.org/toh/sinovoip/bananapi_bpi_r3_mini?s[]=bananapi&s[]=bpi&s[]=r3&s[]=mini


&nbsp;
&nbsp;

&nbsp;
&nbsp;


### Unbrick Nand from Emmc U-Boot


```
usb start
```

```
usb storage

Device 0: Vendor: SanDisk Rev: 1.00 Prod: Cruzer Blade
            Type: Removable Hard Disk
            Capacity: 14664.0 MB = 14.3 GB (30031872 x 512)
```

```
usb tree

USB device tree:
  1  Hub (5 Gb/s, 0mA)
  |  U-Boot XHCI Host Controller
  |
  +-2  Mass Storage (480 Mb/s, 200mA)
       SanDisk Cruzer Blade 03025908011622061257
```

```
ls usb 0:1 
```

```
fatload usb 0:1 $loadaddr openwrt-mediatek-filogic-bananapi_bpi-r3-mini-snand-factory.bin
```
```
fatload usb 0:1 $loadaddr mtk-bpi-r3mini-NAND-20231115-single-image.bin
```
```
nand erase 0x0 0x8000000
```
```
nand write $loadaddr 0 0x8000000
```
switch to nand

&nbsp;
&nbsp;

&nbsp;
&nbsp;

&nbsp;
&nbsp;

### Immortalwrt on Emmc from nand Terminal

download all:

https://firmware-selector.immortalwrt.org/?version=SNAPSHOT&target=mediatek%2Ffilogic&id=bananapi_bpi-r3-mini

mount usb to system
```
mount -t vfat /dev/sda1 /mnt
```
go to directory
```
cd /mnt/sda1
```
Write new GPT table
```
dd if=immortalwrt-mediatek-filogic-bananapi_bpi-r3-mini-emmc-gpt.bin of=/dev/mmcblk0 bs=512 seek=0 count=34 conv=fsync
```
Erase and write new BL2
```
echo 0 > /sys/block/mmcblk0boot0/force_ro
dd if=/dev/zero of=/dev/mmcblk0boot0 bs=512 count=8192 conv=fsync
dd if=immortalwrt-mediatek-filogic-bananapi_bpi-r3-mini-emmc-preloader.bin of=/dev/mmcblk0boot0 bs=512 conv=fsync
```
Erase and write new FIP:
```
dd if=/dev/zero of=/dev/mmcblk0 bs=512 seek=13312 count=8192 conv=fsync
dd if=immortalwrt-mediatek-filogic-bananapi_bpi-r3-mini-emmc-bl31-uboot.fip of=/dev/mmcblk0 bs=512 seek=13312 conv=fsync
```
Set static IP on your PC:
IP 192.168.1.254/24, GW 192.168.1.1
Serve ImmortalWrt initramfs image using TFTP server.
Cut off the power and re-engage, wait for TFTP recovery to complete.
After ImmortalWrt has booted, perform sysupgrade.
Additionally, if you want to have eMMC recovery boot feature:
(Don’t worry! You will always have TFTP recovery boot feature.)




```
dd if=immortalwrt-mediatek-filogic-bananapi_bpi-r3-mini-initramfs-recovery.itb of=/dev/mmcblk0p4 bs=512 conv=fsync
```




dd if=immortalwrt-mediatek-filogic-bananapi_bpi-r3-mini-squashfs-sysupgrade.itb of=/dev/mmcblk0p5 bs=512 conv=fsync

1. Ставим пакет cfdisk
2. Набираем команду SSH/Serial "cfdisk /dev/mmcblkX" (без кавычек и Х это номер раздела у опенврт и имортал могут различатся). Откроется окно программы cfdisk. Внимание, в режиме serial окно может глючить, моргать и отображаться криво. выбрать с помощью кнопок вверх-вниз строку с разделом "/dev/mmcblkXpX". (X у разных прошивок разная)
3. После выбора нужно строки с помощью кнопок влево-вправо выбираем пункт меню "Resize" и нажимаем Enter.
4. Подтверждаем расширение раздела до максимума (программа сразу подставит весь доступный объём памяти, что-то около 7.2Гб): нажимаем Enter.
5. С помощью кнопок влево-вправо выбираем пункт меню "Write" - записать изменения.
Подтверждаем запись, напечатав "yes" и нажав Enter.
6. Выходим из программы, выбрав пункт меню Quit кнопками влево-вправо.
7. Перезапускаем устройство.
8. Перешиваем sysupgrade через веб-морду
Эт примерное что я делал



```
lsblk
```

```
fdisk -l /dev/mmcblk0
```
```
fdisk -l /dev/mmcblk0p4
```


### fan


autorun
```
/etc/init.d/script enable
```

start script
```
/etc/init.d/script start
```


```
#!/bin/sh /etc/rc.common

# fan scripts
START=99
STOP=10

# way to file with temp
TEMP_FILE="/sys/class/thermal/thermal_zone0/temp"

# way to controller fan
FAN_PWM="/sys/class/hwmon/hwmon1/pwm1"

# way to save to fan
LAST_CMD=""

# function log_message
log_message() {
    :
}

start() {
    # check  TEMP_FILE and FAN_PWM

    if [ ! -f $TEMP_FILE ]; then
        log_message "Error: TEMP_FILE not found at $TEMP_FILE"
        return 1
    fi

    if [ ! -f $FAN_PWM ]; then
        log_message "Error: FAN_PWM not found at $FAN_PWM"
        return 1
    fi

    # background for controller
    (
        while true; do
            # Read temp in militemps ( example 45000 = 45 C)
            TEMP=$(cat $TEMP_FILE)
            TEMP_C=$((TEMP / 1000))  # convert the temperature to degrees

            # determine the command for the fan depending on the temp
            if [ $TEMP_C -lt 55 ]; then
                CMD="130"  # fan off
            elif [ $TEMP_C -ge 55 ] && [ $TEMP_C -lt 65 ]; then
                CMD="70"   # fan normal speed
            else
                CMD="20"   # max fan speed
            fi
            # when command change fan speed
            if [ "$CMD" != "$LAST_CMD" ]; then
                echo $CMD > $FAN_PWM
                LAST_CMD=$CMD
            fi

            sleep 15  # delay 15 second
        done
    ) &
}

stop() {
    pkill -f "/etc/init.d/fan_control"
}

```


help for me:
```
dd if=immortalwrt-mediatek-filogic-bananapi_bpi-r3-mini-squashfs-sysupgrade.itb of=/dev/mmcblk0p5 bs=512 conv=fsync
```

### Immortalwrt all package

```
https://downloads.immortalwrt.org/releases/packages-23.05/powerpc_464fp/luci/Packages.manifest
```


```
autocore base-files block-mount bridger busybox ca-bundle default-settings-chn dnsmasq-full dropbear firewall4 fstools kmod-hwmon-pwmfan kmod-crypto-hw-safexcel kmod-gpio-button-hotplug kmod-leds-gpio kmod-mt7915e kmod-mt7986-firmware kmod-nft-offload libustream-mbedtls kmod-nf-nathelper kmod-nf-nathelper-extra kmod-nft-offload kmod-phy-aquantia kmod-phy-airoha-en8811h libc libgcc libustream-openssl logd luci luci-app-opkg luci-compat luci-lib-base luci-lib-ipkg mt7986-wo-firmware kmod-usb3 mtd netifd nftables odhcp6c odhcpd-ipv6only luci-i18n-firewall-zh-cn luci-i18n-ddns-zh-cn openssl-util wget-ssl kmod-bonding proto-bonding luci-proto-bonding kmod-wireguard wireguard-tools luci-proto-wireguard mkf2fs airoha-en8811h-firmware apk-openssl kmod-usb-net-cdc-mbim libmbim luci-proto-mbim umbim umbim kmod-usb-net-qmi-wwan libqmi luci-proto-qmi uqmi modemmanager luci-proto-modemmanager luci luci-base luci-i18n-base-ru luci-i18n-firewall-ru luci-i18n-package-manager-ru kmod-usb-net-qmi-wwan kmod-usb-serial-qualcomm kmod-usb-storage kmod-usb-serial kmod-usb-serial-option mc mmc-utils libmountl libmount1 kmod-mtd-rw nano minicom mbim-utils qmi-utils wget-ssl apk luci-compat luci-i18n-3ginfo-lite-ru uci

```
