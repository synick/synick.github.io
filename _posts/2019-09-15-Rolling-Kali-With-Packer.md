---
layout: post
title: "Rolling Kali with Packer"
date: 2019-09-15 16:25:06
description: Built-in share buttons!
share: true
tags:
 - packer
 - kali
 - vagrant
---
---

I have been playing around with this for a while, and every time it because one of those 20% done projects in a private repo somewhere.

I finally pulled my finger out and got some thing sorted with the latest release of Kali. I had a look around and there are a few really good projects already out there:
* The man himself [@vortexau](https://twitter.com/vortexau) with this [GitHub Repo](https://github.com/vortexau/Kali-Packer). It looks like it builds the the .json file required for packer as well as has some smarts around downloading the ISO file and checking the hash. Packer has that particular feature built in now which is great. I have played around with creating my own downloader and blew out to a 300 line PowerShell script.
* This guy [@secureideasllc](https://github.com/secureideasllc). This is a really cool project, where it integrates some CI into the build process and can be configured to create a new image every month. I copied a chunk of his code, especially the preseed.cfg file, as he seemed to have gone through the majority of pain. I halted my previous attempts because of this annoying config file, so I was really happy to find his repo.

My implementation is pretty simple. I just wanted an easy to run, on Windows, project that downloads Kali, builds it to a spec, then compacts it back up to a .box file ready for Vagrant consumption. You can also crontab (Linux) it or scheduled task (Windows) it to create a new build for you on a regular basis, [@secureideasllc](https://github.com/secureideasllc) did a lot of this work already.

# [](#header-1)How it works.
[Packer](https://packer.io),is a tool that can be used to automate the building of virtual machines. In this instance it is Kali, but it can be Windows, Ubuntu, Debian, etc.

Then,[Vagrant](https://www.vagrantup.com/) steps in. It takes the output from Packer, a .box file in this case, then extracts it, runs some user specified scripts or binaries. You can use it to essentially create a base virtual machine, and instead of having to install the same stuff every time it boots, you can create some code and have it run.

With that preamble out of the way, lets dive into exactly what is going on in our little world. There are a number of files, as specified below:
* ./install/kali.json – The configuration file Packer consumes
* ./install/http/kali-linux-rolling-preseed.cfg – The unattended installation file
* ./install/kali-rolling-build.template –The file that Vagrant consumes
* ./scripts - The directory in which Packer and Vagrant use to run various commands, essentially all of the auto configuration lives here
* ./rollingkali-virtualbox.box – The box file that Packer spits out after it is finished
* ./VagrantFile – The file that Vagrant requires to run, it specifies what to do

You can do some Googling for extra information regarding the above.

---

If you want to have a play around, you can find it here on my GitHub:

[https://github.com/synick/Kali-Packer](https://github.com/synick/Kali-Packer)