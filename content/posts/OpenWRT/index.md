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




## BRI-R3 Mini

https://wiki.banana-pi.org/Getting_Started_with_BPI-R3_MINI

https://downloads.openwrt.org/snapshots/targets/mediatek/filogic/

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


### Immortalwrt on Emmc from nand Terminal

download all:

https://firmware-selector.immortalwrt.org/?version=SNAPSHOT&target=mediatek%2Ffilogic&id=bananapi_bpi-r3-mini

mount usb to system
```
mount -t vfat /dev/sda1 /mnt
```
go to directory
```
cd mnt
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

Вот как удалось решить вопрос с вентилятором. Закидываем а init.d, делаем файл исполняемым. /etc/init.d/script enable

/etc/init.d/script start

```
#!/bin/sh /etc/rc.common

# Имя скрипта
START=99
STOP=10

# Указываем путь к файлу с температурой
TEMP_FILE="/sys/class/thermal/thermal_zone0/temp"

# Указываем путь к контроллеру вентилятора
FAN_PWM="/sys/class/hwmon/hwmon1/pwm1"

# Храним последнюю команду для вентилятора
LAST_CMD=""

# Функция log_message теперь пустая
log_message() {
    :
}

start() {
    # Проверяем наличие TEMP_FILE и FAN_PWM
    if [ ! -f $TEMP_FILE ]; then
        log_message "Error: TEMP_FILE not found at $TEMP_FILE"
        return 1
    fi

    if [ ! -f $FAN_PWM ]; then
        log_message "Error: FAN_PWM not found at $FAN_PWM"
        return 1
    fi

    # Фоновый процесс для контроля температуры
    (
        while true; do
            # Читаем температуру в миллиградусах (например, 45000 = 45°C)
            TEMP=$(cat $TEMP_FILE)
            TEMP_C=$((TEMP / 1000))  # Преобразуем температуру в градусы

            # Определяем команду для вентилятора в зависимости от температуры
            if [ $TEMP_C -lt 55 ]; then
                CMD="130"  # Вентилятор выключен
            elif [ $TEMP_C -ge 55 ] && [ $TEMP_C -lt 65 ]; then
                CMD="70"   # Вентилятор работает на средней скорости
            else
                CMD="20"   # Вентилятор работает на максимальной скорости
            fi

            # Если команда изменилась, отправляем её на вентилятор
            if [ "$CMD" != "$LAST_CMD" ]; then
                echo $CMD > $FAN_PWM
                LAST_CMD=$CMD
            fi

            sleep 15  # Задержка в 15 секунд перед следующей проверкой
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