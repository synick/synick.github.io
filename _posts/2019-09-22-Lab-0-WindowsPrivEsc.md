---
layout: post
title: "Lab 0 - Setup and Intro"
date: 2019-09-22
description: Part of the Windows Privilege Escalation Lab Workshop
share: true
tags:

 - Windows
 - Windows Priv Esc Lab
---
---

# [](#header-1)Lab 0 - Setup.

The first lab is designed to determine if everything is working and going fine on your build. You will need to install the required software, so please download and install the following packages:

* Virtual Box - [www.virtualbox.org](https://www.virtualbox.org){:target="_blank"}
* Vagrant - [vagrantup.com](https://vagrantup.com){:target="_blank"}
* Git - [gitforwindows.org](https://gitforwindows.org){:target="_blank"}

Once the installations are complete, please run the below command

````
git clone https://github.com/synick/Windows-Privilege-Escalation-Labs.git
````

The following command will get Lab0 up and running, this may take a while as the image needs to be downloaded from Vagrant Apps, so go make a cup of tea and watch a Netflix show.

Once complete, you should see the terminal finished and the following presented in front of you. Make sure the output from the terminal is fully complete. As there is a 'slmgr /rearm' performed at the end, followed by a reboot. So, you might login, then all of a sudden the machine is rebooting. 

![Battery Widget]({{ '/assets/images/image-20190922114008609.png' | relative_url }})

From here you can login, and have a play around, but essentially what we are looking for is that everything is up and running.