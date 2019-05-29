Known Issues
============


### bluetooth
Try to enable bluetooth on Nanopi Neo Air. After a lot of search, get bluetooth work finally. A few details are:

 

1. enable UART3 and its CTS & DTS. In /boot/armbianEnv.txt, add something like:

  
   ```
   param_uart3_rtscts=1
   overlays=uart3
   ```

2. control bluetooth reset pin PG13
3. enable RTC LOSC output

 
I use a script to test:

```bash
#!/bin/sh

# sudo apt install sunxi-tools devmem2

echo 'Enable 32K RTC LOSC output'
devmem2 0x1f00060 b 1 
echo 'reset bt'
sunxi-pio -m PG13=0 
sleep 1
sunxi-pio -m PG13=1
sleep 1
echo 'attach'
hciattach /dev/ttyS3 bcm43xx 1500000 flow bdaddr 11:22:33:44:55:66
echo 'config'
#hciconfig hci0 up
```



### u-boot can't boot from emmc (U-Boot 2019.04-armbian)
`linux-u-boot-nanopiair-next` in the latest Ambian image can't boot from emmc.
We need to roll back to the old u-boot `linux-u-boot-nanopiair-default`.

To boot from emmc, we need replace u-boot.

```bash
sudo apt install linux-u-boot-nanopiair-default
sudo aptitude hold linux-u-boot-nanopiair-default
sudo armbian-connfig  # then navigate to `System/Install`
```

U-Boot 2019.04-armbian log (not work):
```
U-Boot 2019.04-armbian (May 04 2019 - 09:12:53 +0200) Allwinner Technology

CPU:   Allwinner H3 (SUN8I 1680)
Model: FriendlyARM NanoPi NEO Air
DRAM:  512 MiB
MMC:   mmc@1c0f000: 0, mmc@1c10000: 1
Loading Environment from EXT4... MMC: no card present
In:    serial
Out:   serial
Err:   serial
Net:   No ethernet found.
MMC: no card present
MMC: no card present
starting USB...
No controllers found
Autoboot in 1 seconds, press <Space> to stop
Card did not respond to voltage select!
MMC: no card present
starting USB...
No controllers found
USB is stopped. Please issue 'usb start' first.
starting USB...
No controllers found
No ethernet found.
missing environment variable: pxeuuid
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/00000000
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/0000000
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/000000
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/00000
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/0000
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/000
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/00
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/0
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/default-arm-sunxi
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/default-arm
No ethernet found.
missing environment variable: bootfile
Retrieving file: pxelinux.cfg/default
No ethernet found.
Config file not found
starting USB...
No controllers found
No ethernet found.
No ethernet found.
=> <INTERRUPT>
=>
```

U-Boot 2018.05-armbian
```
U-Boot 2018.05-armbian (Sep 19 2018 - 12:03:31 +0200) Allwinner Technology

CPU:   Allwinner H3 (SUN8I 1680)
Model: FriendlyARM NanoPi NEO Air
DRAM:  512 MiB
MMC:   SUNXI SD/MMC: 0, SUNXI SD/MMC: 1
Loading Environment from EXT4... MMC: no card present
** Bad device mmc 0 **
Failed (-5)
In:    serial
Out:   serial
Err:   serial
Net:   No ethernet found.
MMC: no card present
** Bad device mmc 0 **
MMC: no card present
** Bad device mmc 0 **
starting USB...
No controllers found
Autoboot in 1 seconds, press <Space> to stop
switch to partitions #0, OK
mmc1(part 0) is current device
Scanning mmc 1:1...
Found U-Boot script /boot/boot.scr
3708 bytes read in 8 ms (452.1 KiB/s)
## Executing script at 43100000
U-boot loaded from eMMC or secondary SD
Boot script loaded from mmc
289 bytes read in 4 ms (70.3 KiB/s)
MMC: no card present
** Bad device mmc 0 **
8141541 bytes read in 634 ms (12.2 MiB/s)
7322968 bytes read in 574 ms (12.2 MiB/s)
Found mainline kernel configuration
```