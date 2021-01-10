---
title: "How to setup and Raspberry Pi with for remote development (without mouse/monitor/keyboard)"
published: false
---

## Introduction:

This blog will help you setup your (new) raspberry pi for remote development using Java from your primary machine over SSH and VNC. 
If you're not looking to do java development you can skip the 'java' setps and just focus on the pi setup.

## Pre-requisite :

### Pi Setup
- Raspberry Pi Device - version 3 or above
- Power for Raspberry Pi
- Micro SD Card that will act as storage for the Pi

### Main Machine
- MacOS / Windows / Linux with Open JDK / Oracle JDK 11 and SSH tool installed
- VNC Viewer : Download and install VNC Viewer if not already (https://www.realvnc.com/en/connect/download/viewer/)
- IDE : I will to use either IntelliJ for Java development and demonstrate it's ability to execute remotely from the IDE itself. You can also try other IDE's such as Netbeans or VS Code which also have similar capability.

## Raspberry Pi OS Setup  :
1. Connect the micro-sd card with your main machine
2. Install the Raspberry Pi Imager tool (https://www.raspberrypi.org/software/) and then run it to format and install the latest Raspian OS on the micro SD Card
3. Confirm the installation by removing the micro sd card and insert it again so OS will mount it, you can see a new drive that should be called "Boot"
3. Navigate to the root directory of the micro-sd card storage by using GUI or command line such as `cd /Volumes/boot`
4. Here we will create a configuration file so when the pi boots up will be able to connect to your local network over WiFi. Raspbian has will able to copy wifi 
details from /boot/wpa_supplicant.conf into /etc/wpa_supplicant/wpa_supplicant.conf to automatically configure wireless network access.

Type the following to create a new file in 'boot' directory
`touch wpa_supplicant.conf`

Open add the following and edit 'ssid' and 'psk' values with your network configuration. Leave key_mgmt field as is.

`
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
    ssid="YOUR_NETWORK_NAME"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
`
Double check to make sure it is looking good by typing `cat wpa_supplicant.conf`
5. Create another filed called 'ssh' in the 'boot' directory by typing `touch ssh`. 
If it is found, SSH will be enabled on the pi, and the file is deleted. The content of the file does not matter, it could contain text, or nothing at all.
6. Remove/Unmount the micro-sd card and insert/mount it into the Raspberry Pi and connect it to power. This will boot the pi and have it connect to the network.
7. Ping the pi by it's default hostname 'raspberrypi' like so `ping raspberrypi` or `ping raspberrypi.local`, this should show you results of data being recieved from pi and it's IP address.
8. Let's connect to the pi remotely using SSH, the default username is 'pi' so type the following command `ssh pi@pi_ip_address`. You should now be connected over ssh, pat yourself on the back!


IDE Setup for remote development and deployment:

1. Open IntelliJ IDEA
2. 
