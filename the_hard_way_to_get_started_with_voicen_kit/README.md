The Hard Way To Get Started with VOICEN Kit
===========================================


## Hardware
+ VOICEN Kit (Nanopi Neo Air, VOICEN Linear 4 Mic Array, Nano AMP)
+ A SD card

## Setup
1. Download [Armbian for Nanopi Neo Air](https://www.armbian.com/nanopi-neo-air/)
2. Use [Rufus (very tiny but only for windows)](https://rufus.ie/) or [Ether](https://www.balena.io/etcher/) to write Armbian to a SD card.
3. Boot Nanopi Neo Air from the SD card, and then login it via USB virtual serial console. The default user is `root`, its password is `1234`. When you first login it, you will be ask to change the password and create a new user.
4. Use `nmtui` to connect to a WiFi.
5. Install a customized linux kernel (todo: upload) which contains the driver of the VOICEN Linear 4 Mic Array.

## IO Access
[Read/Write IO to control LEDs and to detect touch event ot the touch button.](GPIO)

