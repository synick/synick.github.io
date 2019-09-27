---
layout: post
title: "Windows Privilege Escalation Lab"
date: 2019-09-17 17:00:00
description: First post of the Windows Privilege Escalation Series. This provides some of the background behind the project, and how to get started.
share: true
tags:
 - Windows
 - Windows Priv Esc Lab
---
---


# [](#header-1)Intro.
I have been playing around with Windows Privilege Escalation for a while now. I love being in front of a Kiosk, Citrix session or compromised computer, then looking at what the next move might be.

There is a pretty finite set of attack vectors for privilege escalation, especially in a standalone environment. Be it targeting a Unix/Linux server, or a Windows workstation the approach is always similar. I have been playing with the idea of creating a lab environment to test out some techniques, and also offer these out to the rest of the community as free training.

Typically, we see a lot of Linux based virtual machines with varying challenges on them, however, it has always been difficult to distribute a Windows virtual machine, because licence. There are some ways around this and we have seen some virtual machines coming out utilising the freely available images on the Microsoft site. Nice work team!
We see these in sites such as:

* [https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/){:target="_blank"}
* [https://www.microsoft.com/en-us/evalcenter/](https://www.microsoft.com/en-us/evalcenter/){:target="_blank"}

These are freely available evaluation images and ISO’s. So, I have also been playing with Packer and Vagrant tools over the past 12 months or so. Essentially tools you can use to automate the provisioning/building of operating systems, you can Google em. 

I have created a tool that leverages Packer and Vagrant to automatically build vulnerable machines, specifically Vagrant to provision vulnerable Windows workstations. 

Over the coming blog posts, I will be going through different Windows privilege escalation techniques and specify how to approach, enumerate and exploit them.

So, stay tuned!
