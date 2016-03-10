---
layout: post
title:  "Resize a VirtualBox Guest Hard disk in Windows"
date:   2016-03-10
categories: virtualization
---

VirtualBox has a nasty little habit of suggesting small virtual Hard disk sizes when creating Guest Virtual Machines.
Anyone who has worked with Windows guests knows that 25.00 GB is ridiculously small for a standard install of most versions of Windows Server (excluding Windows Server Nano). 

![Figure 1 - Default 25.00 GB Hard disk](../images/Virtulization/VirtualBox-Small-Harddisk.png)

My general rule of thumb is to increase the default Hard disk to between 60 and 80 GB depending on the purpose of the Virtual Machine. Though it is easy to be caught out; And you are now in a position where you will need to resize the guest Hard disk. To complete this task we will make use of the VirtualBox commandline tools. 

The steps below were tested against VirtualBox 5.0.16

## Step 1: Backup Everything!

Ensure that the guest virtual machine is in a `Powered Off` state.

![Figure 2 - Guest Virtual Machine is Powered Off](../images/Virtulization/VirtualBox-Powered-Off.png)

Backup the guests Hard disk. In my case I created a copy of the guest Hard disk on a separate drive. 

## Step 2: Ensure the VirtualBox commandline tools are in your PATH

Open the Environment Variables dialog confirm that the following is present in your PATH variable. 
On Windows 10, this can be done by searching for `environment variables` with Cortana. 

`C:\Program Files\Oracle\VirtualBox`

If it is not present in either the System or User Variables path, add it. My personal preference is to add it to the User variables path as shown in the image below. 

![Figure 3 - Guest Virtual Machine is Powered Off](../images/Virtulization/VirtualBox-Path-Variables.png)

Once completed, Open `powershell` and execute the following:

{% highlight bash %}
vboxmanage
{% endhighlight %}

Which should result in output similar to: 

{% highlight bash %}
Oracle VM VirtualBox Command Line Management Interface Version 5.0.16
(C) 2005-2016 Oracle Corporation
All rights reserved.

Usage:

  VBoxManage [<general option>] <command>


General Options:

  [-v|--version]            print version number and exit
  [-q|--nologo]             suppress the logo
  [--settingspw <pw>]       provide the settings password
  [--settingspwfile <file>] provide a file containing the settings password

... REMOVED FOR BREVITY ...

  Extension package management:

  VBoxManage extpack install [--replace] <tarball>
  VBoxManage extpack uninstall [--force] <name>
  VBoxManage extpack cleanup
{% endhighlight %}


## Step 3: Resize the Virtual Hard disk.

In order to resize a disk we need to execute `vboxmanage` with the `modifymedium` option. 
In previous versions of VirtualBox the option was either `modifyhd` or `modifyvdi`.

`vboxmanage modifymedium disk NAMEOFDISK.EXT --resize SIZEINMB`

In my case I executed the following:

{% highlight bash %}
vboxmanage modifymedium disk Win2012R2.vdi --resize 81920
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
{% endhighlight %}


## Step 4: Boot and Resize the Disk in your Guest OS

If all has gone well, the final step is to boot into your guest OS and resize your drive partitions. 

The steps for which are heavily dependent on your Guest OS and can be readily found on the internet. 