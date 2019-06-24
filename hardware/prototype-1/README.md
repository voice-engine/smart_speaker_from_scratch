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
   which includes the driver for ReSpeaker 4 Mic Linear Array and some pre-installed packages. Do not use the lite version as we need the desktop enviroment to use a web browser.

2. We can write the image to a micro SD card with [Rufus](https://rufus.akeo.ie/) (very tiny but only for windows) or [Etcher](https://www.balena.io/etcher/).

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
   
5. Boot your Pi with the SD card，find out its IP address and ssh to it (user:pi, password: raspberry).

   For Windows, use a SSH client such as [MobaXterm](https://mobaxterm.mobatek.net/) or [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). For Linux or macOS, run `ssh` command in a terminal.


6. Enable the VNC server.

   In the SSH terminal, run `sudo raspi-config`, navigate to menus `Interfacing Options >> VNC` and enable the VNC server.

   
7. Use a VNC viewer to connect to the Pi's remote desktop.

8. Run some test commands:

   ```
   arecord -l                               # list capture devices
   arecord -f dat -d 6 -V mono audio.wav    # record an audio
   aplay -v audio.wav                       # play the audio
   python ~/respeaker/4mic_linear/aec_ns_kws.py   # detect keyword "snowboy", use ctrl + c to exit
   ```

   The output should be:

   ```
   pi@raspberrypi:~ $ arecord -l
   **** List of CAPTURE Hardware Devices ****
   card 1: seeed8micvoicec [seeed-8mic-voicecard], device 0: bcm2835-i2s-ac10x-codec0 ac10x-codec0-0 []
     Subdevices: 1/1
     Subdevice #0: subdevice #0
   pi@raspberrypi:~ $ arecord -f dat -d 6 -V mono audio.wav
   Recording WAVE 'audio.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Stereo
   ##+                                                | 02%
   pi@raspberrypi:~ $ aplay audio.wav
   Playing WAVE 'audio.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Stereo
   aplay: xrun:1624: read/write error, state = RUNNING
   pi@raspberrypi:~ $ aplay -v audio.wav
   ...
   pi@raspberrypi:~ $ aplay -v audio.wav
   pi@raspberrypi:~ $ python ~/respeaker/4mic_linear/aec_ns_kws.py
   ['arecord', '-t', 'raw', '-f', 'S16_LE', '-c', '8', '-r', '16000', '-D', 'default', '-q']
   detected 1
   detected 1
   ^Cquit
   ```

9. At the remote desktop window, setup Amazon Alexa Voice Service
   
   
   1. the default web browser has some compatible issues, so we need to replace it. open the Chromium web browser and set it as the default web
   2. run `alexa-auth`
   3. open http://127.0.0.1:3000 in the Chromium
   4. click `amazon alexa` and login alexa voice service with your amazon account

10. Run Alexa as a voice assistant

    ```
    cd
    wget https://github.com/voice-engine/smart_speaker_from_scratch/raw/master/hardware/prototype-1/alexa.py
    python alexa.py
    ```

11. Make the python script autorun

    ```
    wget https://github.com/voice-engine/smart_speaker_from_scratch/raw/master/hardware/prototype-1/alexa.service
    sudo cp alexa.service /etc/systemd/system
    sudo systemctl enable alexa.service
    sudo systemctl start alexa.service
    ```
 