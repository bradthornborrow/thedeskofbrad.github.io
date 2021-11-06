+++
title = "What Happened to Windows Update?"
date = 2018-07-07
tags = [ "microsoft", "windows" ]
+++

Recently, my wife mentioned she was seeing some warning on her computer that it was almost out of disk space. I thought this odd as her system has a 500GB SSD, and last I checked it still had over 400GB free. To play it safe, I took a quick look at her system and found it had a new (and unknown) drive labeled D: with just under 50MB available. This was the drive causing the _Low Disk Space_ warning. 

{{< image src="Low_Disk_Space.png" alt="Low Disk Space" position="left" style="border-radius: 4px; margin-left: 1em;" >}}

Of course, my first thought was the system had some form of malware on it (it is a Windows system after all). Upon further investigation, I discovered this was the _OEM partition_, which is normally hidden, but for whatever reason it now had a drive letter assigned. After some research I discovered this issue was caused by a recent update from Microsoft[^1]. Unfortunately, as this is a protected partition, there is no way to remove the drive letter and hide the partition using the Windows __Disk Management__ tool.

{{< image src="Disk_Management.png" alt="Disk Management" position="left" style="border-radius: 4px; margin-left: 1em; width: 95%;" >}}

I found several posts on the _Microsoft Answers_ forums which described a complex, multistep process using `diskpart` to remove the drive letter. I dug further and found the simplest (and safest) solution was to open __Command Prompt__ as _Administrator_, and run the `mountvol D: /d` command to remove the drive letter and re-hide the partition. This would finally stop the bogus _Low Disk Space_ warnings caused by the bad Windows April 2018 Update. 

Once that was fixed, I decided to run __Windows Update__ to check for new updates. The system reported it had checked for updates late yesterday and was _up to date_ (at least that's what it reported).

{{< image src="Windows_Update.png" alt="Windows Update" position="left" style="border-radius: 4px; margin-left: 1em;" >}}

However, I decided to click the __Check for updates__ button anyway, and was surprised when the system reported new updates were available (some almost a month old) and was now downloading and installing the updates.

So, what has happened at __Microsoft__ that the software update process has gotten this bad? I've lost count the number of times in recent months I have needed to fix a family member or client’s computer due to a bad update from Microsoft. Years ago, before the Windows 8/10 debacle, I recall a time when Windows Update generally “just worked”. I could setup a PC for a client or friend, enable Windows Update, and just forgot about it. After that, the only time I heard from them was when their system had been infected with some form of malware (we’re talking Windows PCs after all). Now it seems each month on _Patch Tuesday_ it has become a _crap shoot_ where you have no idea what __Microsoft__ is going to break next.

In my opinion, this is an untenable situation where __Microsoft__ is actively increasing risk for the entire industry. The poor handling[^2] and quality assurance of recent updates is causing both businesses and home users to _shy away_ from installing them. Many large enterprises were always somewhat reticent to install updates, but now I’m even seeing this in my smaller business clients who, after being burned by one too many bad updates, suggest I wait _a month or two_ before installing the new updates. This only increases risk for everybody, as the most effective security practice available is to ensure that systems are kept __up to date__ with software patches. I don’t know what the solution is to this issue, other than for significant changes at Microsoft. As users, all we can do is keep pointing out the impact of these mistakes and hope someone eventually listens and takes action.

As always, I hope you found this post useful, or at least entertaining. To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems).

Thanks for reading.

[^1]:[New partitions may appear in File Explorer after installing the Windows 10 April 2018 Update (version 1803)](https://answers.microsoft.com/en-us/windows/forum/windows_10-files/new-partitions-may-appear-in-file-explorer-after/115d2860-542e-410f-983c-2aeb8bbd7d13)
[^2]:[Microsoft kills patch notes, will no longer explain most Windows 10 updates](https://www.extremetech.com/computing/212724-microsoft-kills-patch-notes-will-no-longer-explain-most-windows-10-updates)