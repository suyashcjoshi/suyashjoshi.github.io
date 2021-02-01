---
title: "How to setup and Raspberry Pi with for remote development (without physical mouse/monitor/keyboard peripherals)"
published: true
---

## Introduction:

This blog will help you setup your (new) raspberry pi for remote development using Java from your primary machine over SSH and VNC. 
If you're not looking to do java development you can skip the 'java' setps and just focus on the pi setup.

## Pre-requisite :

### Pi Setup
- Raspberry Pi Device - version 3 or above
- Power for Raspberry Pi
- Micro SD Card that will act as storage for the Pi

### Primary / Main Machine Setup
- MacOS / Windows / Linux with Open JDK / Oracle JDK 11 and SSH tool installed
- VNC Viewer : Download and install VNC Viewer if not already (https://www.realvnc.com/en/connect/download/viewer/)

## Raspberry Pi OS Setup
1. Connect the micro-sd card with your main machine
2. Install the Raspberry Pi Imager tool (https://www.raspberrypi.org/software/) and then run it to format and install the latest Raspian OS on the micro SD Card
3. Confirm the installation by removing the micro sd card and insert it again so OS will mount it, you can see a new drive that should be called "Boot"
3. Navigate to the root directory of the micro-sd card storage by using GUI or command line such as `cd /Volumes/boot`
4. Here we will create a configuration file so when the pi boots up will be able to connect to your local network over WiFi. Raspbian has will able to copy wifi 
details from /boot/wpa_supplicant.conf into /etc/wpa_supplicant/wpa_supplicant.conf to automatically configure wireless network access.

Type the following to create a new file in 'boot' directory `touch wpa_supplicant.conf`

Open add the following and edit 'ssid' and 'psk' values with your network configuration. Leave key_mgmt field as is.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
    ssid="YOUR_NETWORK_NAME"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
````
Double check to make sure it is looking good by typing `cat wpa_supplicant.conf`
5. Create another filed called 'ssh' in the 'boot' directory by typing `touch ssh`. 
If it is found, SSH will be enabled on the pi, and the file is deleted. The content of the file does not matter, it could contain text, or nothing at all.
6. First power off the pi if not already then unmount/eject the micro-sd card. On macOS, you can do that typing `diskutil unmount /Volumes/boot` and then physically remove it and insert it onto the Raspberry Pi. Finally connect it to power, this will boot the pi and have it connect to the network using configuration that we provided earlier.
7. Let's find the pi on local network using 'ping' took, all we need is the hostname for the pi it's default hostname is 'raspberrypi'. So type the following command `ping raspberrypi` or `ping raspberrypi.local`, this should show you results of data being recieved from pi and it's IP address.
8. Now we will use the pi's IP address to connect remotely using SSH, the default username for Raspberry Pi is 'pi' so type the following command `ssh pi@pi_ip_address`. If promoted accept the key fingerprint and provide is the default passowrd for the Raspberry Pi which is 'raspberry'
9. Finally you should be remotely connected to the pi, congratulations!


## Remote development on the Pi by setting up VNC and JDK

1. Once you're remotely connected to the pi, first thing I recommend is to enable VNC server so you can remotely connect to the PI GUI and not just via terminal. So let's do that by typing: `sudo raspi-config` this will bring up a GUI menu, navigate to "Interface Options" using keyboard and enable enable VNC by following the prompt. Finally hit 'Escape' key to leave the prompt and return to the terminal.
2. Let's test VNC now, open the VNC Viewer App on your main machine and type the Raspberry PI's IP address in the address bar. It should then ask you for username and password of the remote pi for which use the default credentials: username - 'pi', password - 'raspberry'. Check 'remember passowrd' and finally click 'OK'. Now you should be connected to the pi vis GUI as well. At this point it is a good idea to change the default password for your pi, you can do so by following the GUI wizard and restarting the pi.
3. Now, it's upto you if you want to use GUI and open Terminal app or via command line using ssh from your main machine. We will do a quick check to what we have for development, I'm interested in Open JDK LTS version 11, so let's install the same via command line 'apt' package manager and confirm the same :
```
sudo apt update
sudo apt install default-jdk
java --version
```
4. Setup JAVA_HOME enviornment variable by using 'update-alternatives'. So by type `sudo update-alternatives --config java` and it shoud show you all the JDK versions you have, copy the address of the one you want to your default value for JAVA_HOME
5. Open the envionrment file and add the following variable by typing `sudo nano /etc/environment` and paste following : JAVA_HOME="/usr/lib/jvm/java-11-openjdk-armhf/bin/java" (modify to your path and version of JDK)
6. Let's activate the enviornment variable changes by executing the following command `source /etc/environment`
7. Verify JAVA_HOME has been set as enviornment variable by priting it to the console, make sure the path is correct `echo $JAVA_HOME`

Congratulations, you have a Raspberry Pi ready for development using Java! In the next post, I will go over how to using modern IDE like IntelliJ for Java development and demonstrate it's ability to execute remotely from the IDE itself.



