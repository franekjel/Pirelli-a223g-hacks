Pirelli DRG A223G is adsl modem and router with 32MB ram and bcm6358. Device have USB port and two 100mbit ethernet.

# State
With original firmware (by origninal I meany firmware I got with modem from my ISP, it probably differ) it is possible to use device only as adsl modem. My goal is to configure it as router (use one ethernet port as WAN). 

# Serial connection
Board have serial port with enabled console:
![UART pins](https://raw.githubusercontent.com/franekjel/Pirelli-a223g-hacks/master/IMG_20200223_183606.jpg)
It is possible to connect with baud rate 11520.
Then it is possible to stop boot on CFE bootloader (by pressing ESC on early booting) or to router firmware

# Orginal router firmware
Router use openrg as firmware, i couldn't log in or do anything

# CFE bootloader
From CFE bootloader it is possible to flash new firmware. You need to download new firmware and place it in directory, then connect router to computer via ethernet, set static IP and start TFTP server. On linux:
```
# ip address add 192.168.1.100/24 broadcast + dev enp12s0
<plug cable>
# dnsmasq -d --port=0 --enable-tftp --tftp-root=/directory/where/firmware/is
```
Of course you must use proper interface and directory (and you can choose other IP)
Then on CFE:
```
flashimage 192.168.1.100:filename.img
```

# Firmware
1. Some version of orginal firmware
2. Openwrt //TODO
