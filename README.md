Pirelli DRG A223G is adsl modem and router with 32MB ram and bcm6358. Device have USB port and two 100mbit ethernet. This file contains some investigations I made to make it usable (since I have plenty of them).

## State
With original firmware (by original I meany firmware I got with modem from my ISP, it probably depends on IPS and country) it is possible to use device only as adsl modem. I wanted to use it as an emergency router for travels etc.

## Serial connection
Board have serial port with enabled console:
![UART pins](https://raw.githubusercontent.com/franekjel/Pirelli-a223g-hacks/master/uart.jpg)

It is possible to connect with baud rate 11520.

By pressing ESC on early booting you can enter CFE bootloader.

## CFE bootloader
With CFE bootloader you can flash new firmware. You need start tftp sever in directory with firmware and connect computer to router eth0. Then you must set static ip and you can use:
```
flashimage <your static ip>:filename.img
```
to flash new firmware.

## Firmware
#### Original firmware
File ```openrg-4.5.3.DWVL_NET_FON_4.3.1.0046-DRG_A223G_96358.rmt``` is original firmware from my IPS (Netia). To flash it you must first strip from some metadata on the beginning. Real image starts with sequence (in hex):
```
38 00 00 00 50 69 72 65 6c 6c 69 42 53
```
or (ASCII)
```
8...PirelliBS
```
in hexdump.
![Hexdump of .rmt image](https://raw.githubusercontent.com/franekjel/Pirelli-a223g-hacks/master/uart.jpg)

You need to remove all bytes before these, so (in case of image from this repo):
```
dd if=openrg-4.5.3.DWVL_NET_FON_4.3.1.0046-DRG_A223G_96358.rmt of=openrg.img bs=1 skip=166
```
Then it should be posibble to flash this image in CFE, as mentioned above.

It should be possible to crack password for the root user using
https://github.com/antnks/pirelli-decryptor
but I didn't investigated this option furter. This firmware is old (2010), buggy (I encountered some segfaults and panics) and very limited, so I decided to try run openwrt instead.

#### Openwrt.
Openwrt supports bcm6358 so it's easy to port it. You can build mainline openwrt for Pirelli a226g - it's similar device, and it should work (I tried it and booted). You can also try .dst file I prepared for a223g in ```openwrt/``` folder in this repo. It handles leds a bit better.

You can also try compiled image (```openwrt-bcm63xx-generic-pirelli_a223g-squashfs-cfe.bin```). It is compiled with files in ```openwrt\files\``` to use eth0 as wan and make open wifi named `pirelli-a223g`.

## Troubleshooting

If you run into a problem while flashing about an incompatible device you can try to change device parameters (board signature) in CFE using command ```b```. In my case it helped.
