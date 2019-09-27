---
layout: post
title: "Lab 1 - Leakage of Sensitive Information"
date: 2019-09-27
description: This is the first official lab of the Windows Privilege Escalation series. There are setup intrsuctions if you just want to get started. A full approach and walkthrough, showing you exactly what to do. Then the Answers down the bottom if you get really stuck.
share: true
tags:

 - Windows
 - Windows Priv Esc Lab
---
---

# [](#header-1)Lab 1 - Leakage of Sensitive Information.

Information leakage, is a very common vector for privilege attacks. Files containing sensitive information, such as hidden shares, passwords, API keys, etc are commonly found on most interal infrastructure assessments. We are specifically limiting the scope here to only a workstation, so we wont go into the domain enumeration just yet. 

## Lab Up - TLDR;

Firstly, lets get the appropriate lab up and running.

In Windows:

```
c:\git\Windows-Privilege-Escalation-Labs> set LabIndex=1 && vagrant up
```

In Linux or Mac

```
LukaszMac:synick$ export LabIndex=1 && vagrant up
```



## Approach & Walkthrough

### Local File System

Lets start looking at the typical approach for enumerating sensitive information on a local workstation. Firstly, you have to identify specifically what you are looking for. Lets focus on finding information that could lead to a privilege escalation from a low to high privileged user, not as an already Admin level. So, we wont be extracting anything from memory.

There are tools that automate some of these things, but this is not a parrot pentesting blog, we will be doing everything manually. Lets begin.

As a low privileged user, you can enumerate a lot of the file system, even if you cannot read the files, you may identify certain files which may come in handy as a target if you obtain the appropriate privielge. Lets identify if you can firstly browse the entire file system.

```
C:\Users\privesc>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
```

The output from the 'whoami /priv' command is very common to see. Unless specifically modified, the 'SeChangeNotifyPrivilege' setting is often Enabled. The [Microsoft Technet Article](https://blogs.technet.microsoft.com/markrussinovich/2005/10/19/the-bypass-traverse-checking-or-is-it-the-change-notify-privilege/){:target="_blank"} deep dives into exactly what is going on here, the setting means that a process will be notified of changes to parts of the file system. On process is Explorer, so, when you create a folder or file within another directory, it notifies Explorer that the file system has changed and it refreshes the directory showing the change. We also notice that 'Bypass traverse checking' is part of the description, this is very important with what we are trying to achieve with this lab. It allows a user to traverse a directory or directory tree even though they might not have permissions to one of the parent directories. A quick example: A folder called, Secret, with access permissions for Bob. Then a file within this directory called Alice.txt, with read access for the user Alice. Alice can access the Secret directory if the user has 'SeChangeNotifyPrivilege' set. Alice cannot see the contents of the folder, but, can read the file. 

So with that, lets start enumerating the file system for potentially sensitive files. We can do this by running a few commands from the command line, or directly within Windows Explorer itself.

```
C:\> findstr /snip passw *.xml
C:\> findstr /snip passw *.config
C:\> findstr /snip passw *.ini
C:\> findstr /snip passw *.txt
```

* findstr - Finds a string
* /snip - Search current and sub-directories,  print the line number, case-insensitive, and skip file with non-printable characeters
* passw - This is the string we are trying to identify. A few examples on the internet show the full string 'password'. It would avoid this, as certain variables in programs or configs will set is as 'passwd'.
* *.xml - The types of files we want to limit the search to

These commands generally generate a lot of 'interesting' files to manually trawl through, I never said Windows priv esc was not tedious. After a while of doing this you will get to know which files to safely ignore and which to dig a little deeper into.

What if you don't have access to a command prompt, because of that pesky AppLocker or Software Restiction Policy? Lets have a look at how you can still enumerate within files by using the built-in Explorer search.

Fire up Explorer:

![Battery Widget]({{ '/assets/images/Explorer.png' | relative_url }})

Press the 'ALT' key, this will show you the file menu up the top. Click on Tools, Folder Options, Search (tab). Then click the radio button 'Always search file name and contents'. Click Apply and Ok and get out of there, then start searching for the specific strings.

![Battery Widget]({{ '/assets/images/search-file-contents.png' | relative_url }})

So...did you find anything interesting? Lets have a look at the Registry then.

### Registry

The registry is pretty much the database of Windows, in which there are a bunch of configuration settings. Everything from which services are set to start on boot, through to your file and folder search history (another interesting place to look while on a system for juicy info...hint for further labs).

We can search the Registry in a similar manner as the file system, I will list a few commands below to get us started. Then we will go through some interesting areas of the Registry to always keep an eye on. As a low privileged user, you can enumerate a large portion of the registry, and it is very rare that portions of the registry are set to restrict accessing passwords (apart from security hives). 

First up, lets have a search through the registry

```
REG QUERY HKLM /F "passw" /T REG_SZ /S /K
REG QUERY HKCU /F "passw" /T REG_SZ /S /K
```

'REG' is a command line tool native to Windows that can be used to perform tasks on the registry. In this case we are using the query argument to perform certain tasks:

- REG QUERY - Query the registry
- HKLM - This is the HKEY_LOCAL_MACHINE Hive within the registry
- /F - Specifies the data or pattern to search for
- "passw" - The actual string to find, as before we are not using the whole 'password' string
- /T - Specifies the registry value data type to look for
  - REG_SZ - A null-terminated string. This will be either a Unicode or an ANSI string, depending on whether you use the Unicode or ANSI functions.
- /S - Query all subkeys and values
- /K - Seach within key names only

Is there anything that stood out as part of this search? I am sure there was a lot of noise coming out, but this is all a part of it. 

![Battery Widget]({{'/assets/images/image-20190927144902483.png' | relative_url}})

If you do not have access to the command prompt for what ever reason, you can use the command 'regedit' to open the GUI, and search the registry manually using search.

![Battery Widget]({{'/assets/images/image-20190927151416830.png' | relative_url}})

Then type in the string you wish to use to search.

![Battery Widget]({{'/assets/images/image-20190927151621765.png' | relative_url}})

---


### Answer


The registry key 'HKLM\SOFTWARE\VNCServer\' contains the password for the 'Administrator' user.

 ![image-20190927152743981](/assets/images/image-20190927152743981.png)
