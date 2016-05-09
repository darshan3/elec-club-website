---
layout: post
comments: true

assets_dir: /assets/getting-started-with-beaglebone-black
title: Getting Started with Beaglebone Black
excerpt: Simple tutorial on using the BBB.
author: Dhruv Ilesh Shah
category: [Embedded Development]
tags: [Programming, Development, Tutorial]
---
**<u>You will need</u>** - A BeagleBone Black AM335x (an older version should also do.), a PC (I have used a Linux distribution), Ethernet Cables and Ports <i>and lots of patience.</i>
![The BeagleBone Black]({{page.assets_dir}}/bbb.jpg)
The BeagleBone Black is an embedded development board, just like the <a href="http://www.raspberrypi.org">Raspberry Pi</a>, <a href="https://www.arduino.cc/en/ArduinoCertified/IntelGalileo">Intel Galileo</a> etc. It offers several advantages over the others in the category, and also has its own demerits. We'll get to that in the end. <br>
For the specs, the BBB has a 1Ghz ARM Cortex-A8 and 512MB of DDR3 RAM on board - almost as capable as a medium range smartphone. It houses an on-board 8-bit 4GB flash memory which comes preloaded with the *Angstrom*. It has a total of 92 pins, of which 66 can be used as GPIO pins. For more specification, you may refer <a href="http://beagleboard.org/support/bone101">here</a>.
<br>

Getting Started
--------------
The BeagleBone is amazingly simple to start off with, and is indeed the most convenient of any embedded board, since it comes with a preloaded OS. There are multiple ways to access the BBB from your system, each of which will be described briefly.

Connect the BBB via the USB cable provided in the box. The BBB can be directly accessed using the web browser, and programs can be executed using Bonescript, which is similar to JS. By default, your BBB comes with it's IP address on *usb0* port as `192.168.7.2`. Enter this in your browser, and you are good to go. You can access the GPIO, and run programs with the BBB as your microcontroller, just like you run scripts in Arduino. A sample of the available functions can be seen <a href="http://beagleboard.org/support/bone101">here</a>.

For accessing the *real capabilities of the board* as a computer in itself, you will need to do a little more, but fear not! Plug in the BBB using the USB cable, and go to your *terminal* (Windows users may have to use <a href="http://www.putty.org/">PuTTY</a>) and type:

```javascript
ssh yourIP -l root //or
ssh root@yourIP
//You may have to use sudo, if permissions are an issue.
```
This will enable you to use the BBB as the root user. You will notice that the files and structure of the memory is similar to any other Linux distro. *The BBB is your very own credit-card sized computer!*

Now that you have entered the system, there exist no bounds to what can be done. Some packages as pre-installed, like `Python`, and can be used directly. Have fun tinkering with your cute little pup. To burn bonescripts, the browser sure is a cool bet!

Using the BeagleBone Black remotely
-----------------------------------
You have used the BBB by connecting it to your PC, but what if you want it to act independently, or on the network? There must be a way to access the board via a network, and not just USB. Turns out, there is - and much simpler than the R-Pi in this case! Here's what you must do:

 * Connect the BBB to your system, as stated above. Enter the system using SSH and login as root (use the Terminal or PuTTY)

 * Run `cd /etc/network` and open the file *interfaces* using VI or nano.

```javascript
sudo vi /etc/network/interfaces //or
sudo nano /etc/network/interfaces
//sudo would be used if the user is not root.
```
 * This opens the file interfaces. You will see the following lines written in the end.

```javascript
 iface usb0 inet static
     address 192.168.7.2
     netmask 255.255.255.0
     network 192.168.7.0
```
 This defines the action performed when the BBB is connected via USB. (Note `usb0`) You can choose to keep this or comment it out depending on usage. Now, let's go on with the task of setting it up remotely.

 * Here, I'm using LAN for that purpose. Before starting, create a backup of the file so that you have something to go back to - messing up is allowed!

```javascript
cd /etc/network
cp interfaces interfaces.backup
vi interfaces
//The file opens up. Now, we must edit it.
//Add the following lines in it.NOTE, add values according to your ethernet/router settings. These are the ones appropriate for me.
iface eth0 inet static
       address 10.173.24.69
       netmask 255.255.255.0
       gateway 10.173.24.250
       dns-nameservers 10.200.1.11
       dns-nameservers 10.200.11.1

```

 * Here, we declared the `static` IP for the Ethernet port `eth0`. According to your Service Provider, you may replace `static` with `dhcp`. Thus, the definition of the port is set.

 * To check whether you have established connection, run `ping 10.173.24.69`(IP address) or `ping 10.200.1.11`(DNS Server).

 * Now, leave the BBB with an independent power source, connected to Ethernet. With a system connected to the same network, you can now access the BBB. Say, you have a laptop connected to the router or LAN network. Fire the Terminal/PuTTY and type :

```javascript
sudo ssh root@10.173.24.69
//basic syntax is 'sudo ssh user@yourIP'
```
This would take a while, but is sure to get you into the BBB considering that your Bone and laptop are both connected and active. You can control and run the BBB remotely, and also share/install packages over the network.

Now that you have connected to the Bone directly and remotely, you can do almost anything with the Processor. Tried and tested ideas include using the remote computer to run as a portable unit running heavy Mathematica/MATLAB scripts, or Python codes, or even a host/router. And with 66 GPIO pins, Electronics Projects would surely be cooler, and cleaner! :)

## Merits & Demerits
 * The BeagleBone is a favorable choice for beginners because it is a direct plug-and-play unit (R-Pi needs you to install Raspbian OS before use). Also, the higher no. of GPIO pins ensure that you're never short of pins for your sensors! Also, for the geeks and performance freaks, it packs the best processor and graphics accelerator in the range. The on-board Flash is also very fast.
 * That said, the main issue you'd face would be lack of online support (one of the reasons I wanted to make a beginner tutorial!) for your minor bugs and queries. Also, the basic use over the web may require some use of JavaScript and hence, some background. Also, in general, using any such development board requires immense amount of patience. You don't really get ready-made solutions online. More so with the BBB, than the R-Pi. but it's certainly better than starting off with the Intel Galleo.

Hope you have a good time tinkering around with the BeagleBone Black.

Cheers.
