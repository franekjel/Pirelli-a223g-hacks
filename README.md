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
1. Original firmware form my IPS (Netia) is in this repo (openrg-4.5.3.DWVL_NET_FON_4.3.1.0046-DRG_A223G_96358.rmt). I can't do anything with it.
2. Openwrt: It's possible to flash openwrt. You can use version for drg-a226g (which is mainlined, you can download it from opewrt site or build). I'am also working on version for a223g (dts file and image in this repo). Currently this is copy of a226g with some modifications mainly for this model leds (this means that about half of them is working (but this more important half)). With minimal openwrt (without luci etc.) device is pretty usable as emergency router. Since it has serial port and  gpio (for leds, also there is jtag and some places for pins (sysfs show 32 pins for gpiochip480, I will test them later)) it probably can be also some kind of SBC. NOTE: It may be necessary to change board signature to succesfully flash nad boot opewrt. In CFE bootloader use
```
b
```
to change board parameters (for signature there are 3 options, in my 0 work best (but I'm unsure why, so it may differ))
