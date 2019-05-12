# SoraHealth

Measuring blood sugar is not the only way in which a person can detect that it has this high level, there are times when a person detects symptoms in themselves that are not normal and these can trigger a more serious problem, such as a fainting or death.

If someone detects that the sugar level is high, the best thing to do is take medication to lower it, however this may be counterproductive in some cases, because these medications can lower the level of sugar so much that it could be mortal.

If only there was an easier, more natural way of tracking that even outside your home, patients sometimes have relapses and periods when they need help fast when outside and even at home and even more so if they are elders.

For this problem the answer is simple, when there is nothing else to do, asking for help quickly is the best solution to the problem, so that's why I decided to create SoraHealth, a device with the ability to send a notification of help quickly with just pressing a button.

# Table of contents
* [Introduction](#introduction)
* [Materials](#materials)
* [Setup Wio Module](#setup-wio-module)
* [Arduino IDE Setup](#arduino-ide-setup)
* [Soracom Console](#soracom-console)
* [AWS Setup](#aws-setup)
* [The Final Product](#the-final-product)

## Materials:

Hardware: 

- WIO LTE - USA Version (Cortex-M4  microcontroller). 
- Soracom Air Global IoT SIM Card.
- Grove Button (http://wiki.seeedstudio.com/Grove-Button/).
- 2 x LTE antenna.
- Lipo Battery, 1050mAh, 3.7V.

Software: 

- Arduino IDE (https://www.arduino.cc/en/Main/Software). 
- Amazon Web Services (https://aws.amazon.com).
- Soracom Console (https://console.soracom.io).

## Setup Wio Module:

The module is so simple and easy to install, that we just have to connect everything to the WIO and that's it! Here's an image of how it looks when it's ready!

<img src="https://i.ibb.co/njvg20f/3.jpg" width="800">

## PC Configuration:
(All these steps are directly in the official guide http://wiki.seeedstudio.com/Wio_LTE_Cat.1/#getting-started)

- Install USB driver:

Windows Users: Most versions of Windows won't automatically load the built-in driver for USB com ports. You'll have to download ST's USB driver:

Non-Windows XP Users download version 1.4.0 drivers. Unzip the file, run the executable, and then go to C:\Program Files (x86)\STMicroelectronics\Software\Virtual comport driver in Windows Explorer and double-click either dpinst_amd64.exe for 64 bit systems, or dpinst_x86.exe for 32 bit.

Windows XP Users download version 1.3.1 drivers. Unzip the file, run VCP_V1.3.1_Setup.exe, and then go to C:\Program Files\STMicroelectronics\Software\Virtual comport driver in Windows Explorer and double-click the executable.

Linux users to ensure that you have the correct permissions to connect as a normal user you'll need to copy the file 45-espruino.rules to /etc/udev/rules.d, reload rules with udevadm control --reload-rules, and ensure your user is in the plugdev group (you can check by typing groups). You add it by typing sudo adduser $USER plugdev and then logging out and back in. Arch Linux users need to add their user to uucp and lock groups instead.

Mac OS X and Chromebook Users: The board will just plug in and work, without drivers!

- Change DFU driver:

For windows users:

Press and hold BOOT button and connect to computer you will see STM32 Device in DFU Mode at device manager as below.

<img src="https://github.com/SeeedDocument/Wio_LTE/raw/master/img/before_driver_installation.png" width="500">

- This says that you need to use zadig_xx.exe to change DFU driver from STTub30 to WinUSB as below. If we can't see any info on the Zadig, please click Options→ List All Devices, then select STM32 Virtual COM Ports.

<img src="https://github.com/SeeedDocument/Wio_LTE/raw/master/img/zadig.png" width="500">

- You will see the "STMicroelectronics Virtual COM Port" on device manager as below.

<img src="https://github.com/SeeedDocument/Wio_LTE/raw/master/img/after_driver_installation.png" width="500">

## Arduino IDE Setup:

Install Arduino IDE, recommand IDE version upon 1.8.0.

https://www.arduino.cc/en/Main/Software

Follow How to Add Seeed boards to Arduino IDE to add Wio_LTE into arduino board manager.

http://wiki.seeedstudio.com/Seeed_Arduino_Boards/

<img src="https://i.ibb.co/N9zzvh5/1.png" width="500">

Download the Wio_LTE Library from Github.

https://github.com/Seeed-Studio/Wio_LTE_Arduino_Library

<img src="https://i.ibb.co/CPLx8q5/2.png" width="500">

Refer How to install library to install library for Arduino.

http://wiki.seeedstudio.com/How_to_install_Arduino_Library/


## Soracom Console:



## AWS Setup:



## The Final Product:


