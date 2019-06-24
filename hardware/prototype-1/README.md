Prototype #1
============

[中文版](zh.md)

To make an open source smart speaker.

### Hardware

![](/img/prototype1.png)

The hardware includes:

+ Raspberry Pi 3B
+ A micro SD card
+ ReSpeaker 4 Mic Linear Array
+ 45mm Speaker
+ Laser-Cut Paper Case
+ 2 Nylon Rivets (R3045), the diameter of mounting holes is 3mm (ф3). Screws also work, while rivets are easier to use.

  ![](/img/nylon_rivet.png)


Raspberry Pi and ReSpeaker 4 Mic Linear Array can be found at Seeed Studio, and we get the 45mm speaker and nylon rivets from Alibaba or Taobao.

### Software
+ [Raspbian for ReSpeaker](https://v2.fangcloud.com/share/7395fd138a1cab496fd4792fe5?folder_id=188000207913&lang=en)
+ [Etcher](https://www.balena.io/etcher/) or [Rufus (only for Windows)](https://rufus.akeo.ie/)
+ [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/chrome/)
+ A SSH client such as MobaXterm or Putty


### Steps
1. Download [a customized Pi image - 2018-08-06-raspbian-for-respeaker.zip](https://v2.fangcloud.com/share/7395fd138a1cab496fd4792fe5?folder_id=188000207913&lang=en)，
   which includes the sound card's driver and some voice related packages (Do not use the lite version as we need desktop enviroment to use a web browser).

2. We can write the image to a SD card with [Rufus](https://rufus.akeo.ie/) (very tiny but only for windows) or [Etcher](https://www.balena.io/etcher/).

3. (Optional) Use your computer or your phone to setup a hotspot.

4. Enable SSH and setup WiFi before the first boot of the Pi.

   Add a file named `ssh` to the boot partition of the SD card, which enables SSH, and then create a file named `wpa_supplicant.conf` with the following content (replace `WiFi SSID` and `WiFi Password` with yours):
   
   ```
   country=GB
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1
   network={
	    ssid="WiFi SSID"
	    psk="WiFi Password"
   }
   ```
   
5. Boot your Pi with the SD card，find out its IP address and use VNC viewer to access it.
   
   We can also use SSH to access your Pi. For Windows, use a SSH client such as [MobaXterm](https://mobaxterm.mobatek.net/) or [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). For Linux or macOS, run `ssh` command in a terminal.



