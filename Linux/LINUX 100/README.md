<h1 align="center">LINUX 100</h1>
  <p align="center">
     Kali Linux and VMware 
  </p>

### Table of contents

- [Introduction](#introduction)
- [Installing VMware Workstation Pro](#installing-vmware-workstation-pro)
- [Installing Kali Linux](#installing-kali-linux)
- [Bonus Steps](#bonus-steps)
- [Creators](#creators)


## Introduction
This course will walk you through getting a Linux virtual machine onto your machine with VMware Workstation Pro. Having a Linux VM and knowing how to use Linux is necessary in this field. For this course, we will be installing a Linux operating system called Kali Linux and we will be running it with VMware. If you have not yet heard of Kali Linux, it is a Linux distribution that is tailor-made for cybersecurity-related tasks. It comes with many tools pre-installed that are meant for cyber so you don't have to. There are other Linux distributions you can use as well, such as Parrot, Ubuntu, and others.

We will be using VMware to run our virtual machine, but if you want to, some people prefer to use Virtual Box instead. 

## Installing VMware Workstation Pro
You can go to this site and download it : https://customerconnect.vmware.com/en/downloads/info/slug/desktop_end_user_computing/vmware_workstation_pro/16_0


## Installing Kali Linux
1. If you don't already have it, install 7zip (https://www.7-zip.org/download.html)
2. Go to https://www.kali.org/get-kali/#kali-virtual-machines and click on the VMware downoad. You should get a large .7z file
3. Use 7zip to unzip the file
<p align="center">
    <img src="https://github.com/ursusShooock/CTF-by-eldiablo/tree/main/images/linux/7zip.png" width=60%  height=60%><br>
</p>

4. You should no have the unzipped folder that has the following contents. You should move this folder to a location where you want to keep your VMs. VMware will default to creating VMs in a "Virtual Machines" folder it will place in your "Documents" folder (if you end up making VMs from .iso files. You don't really need to worry about this because you can change that location too). I just make my own "VMs" folder to put them in. 
<p align="center">
    <img src="https://github.com/ursusShooock/CTF-by-eldiablo/tree/main/images/linux/unzipped.png" width=50%  height=50%><br>
</p>

5. You should see a `.vmx` file somewhere near the top. Right click on it and open it with VMware Workstation 
<p align="center">
    <img src="https://github.com/ursusShooock/CTF-by-eldiablo/tree/main/images/linux/vmx.png" width=60%  height=60%><br>
</p>

6. Now you can power on the VM. If you get a pop-up right away, just press "I Copied It"
7.  On the login screen, the default username is `kali` and the password is `kali`. 
8.  You're in!

## Bonus Steps
- Configure a shared folder so you can share files between your virtual machine and your host
  - here's a good video: https://www.youtube.com/watch?v=bUDup4RibEs&ab_channel=VonnieHudson
  - google is your friend
  - if you can't get it working, reach out to us
- Edit the virtual machine settings
  - Press "Edit virtual machine settings"
  - Set the settings however you would like depending on the laptop/computer you are running on. 2GB of memory should be enough, but if you have enough to spare, bumping it up to 4GB would be nice. You can also increase/decrease the # of processors and Hard Disk space as well if you want to 
<p align="center">
    <img src="https://github.com/ursusShooock/CTF-by-eldiablo/tree/main/images/linux/first-open.png" width=85%  height=85%><br>
</p>

Enjoy :metal:
