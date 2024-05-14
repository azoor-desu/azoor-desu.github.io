---
title: Migrating to Linux
description: "Adventures ond documentations of moving from Windows to Linux"
date: 2024-05-14T15:27:39.304Z
preview: "/assets/img/blog/migrating-to-linux/windoes.webp"
tags: []
categories: []
type: default
---
# Things to clarify
This post is about my journey transitioning from Windows to Linux, and a brief documentation of the processes I went through to live happily ever after on my new OS environment. It is by no means a smooth transition, and this post is definitely not to convince anyone to do the same as I did. However, if you're somewhat interested in what it takes to make the leap, this post may contain some useful tips to keep in mind, especially for someone moving from Windows.

# Background
I've been using Windows forever, starting with Windows XP as a kid. As the years go by, Microsoft has been steering in the _bad_ direction of intrusive ads, bloating the Operating System, and general anti-consumer practices. This irks me on a personal level. I don't like it. As of now, Windows 11 is still usable. However, this could all change one day. Microsoft could start implementing some hostile and anti-consumer feature one day in the future, in the pursuit of profit. That would render the OS hostile and unusable for me. If that threshold for usableness is eventually crossed one day, I'd be screwed using an OS I don't want to. Thus, I started to look into alternative OSes to use. Not MacOS, I don't have that much money for a Mac. Hence, Linux became the best option to migrate to.

> Side note: Calling Linux an "Operating System" may rustle some people's jimmies, because it's _"ackchually GNU + Linux"_. For the sake of writing less words, I'm going to continue to refer to operating systems running using the Linux kernel as Linux. 

> Side side note: There's many _distributions_ of Linux, referred to as _distros_. These are just different flavours of Operating systems that run on the Linux kernel, to put it simply. I'll be using this term frequently. More about distros will be explained later.

# Pros and Cons
Before moving on to fully commiting to a completely new OS, I had to find out more about what I'd gain and lose from moving to Linux. Here's as many pros and cons I encountered that may be of significance.
## Pros:
- Major reduction in bloatware. No random pre-installed programs that you don't need (with maybe some minor exceptions)
- Centralised package-based system allows applications to share common packages.
  - Significantly reduces space taken by applications comapred to Windows. Many windows applications have to come packaged with repeated libraries. Linux mostly avoid this. Windows OS takes up roughlt 100gb of space. Linux OSes take up around 2-20gb of space, depending on distro.
  - Centralized package management makes it super easy to install/uninstall software via the Terminal/Command line.
  - Centralized package management makes it super convinient to update everything in one go
- Less bloatware and unwanted systems running means lesser RAM consumption on average.
- Almost unlimited customizability. Ranges from simple customization of your desktop environment provided by your distro, to fully customizing your Linux OS install down to the kernel options.
- Loads of people maintaining various projects, and loads of people submitting bug reports to make everything better.
- No need to install drivers (other than Nvidia drivers and maybe some other specific drivers), all devices work out of the box (if it's supported by the Kernel)
- No telementry or tracking! No ads! No copilot/cortana/OneDrive/funny Microsoft product forced onto every user!
- Supports _most_ software and games that Windows people use and play. That's pretty great comapred to the past where _most_ games and software have no support on Linux. Disclaimer: Some major games/software STILL have no support, and likely will never be supported.
- WINE compatibility layer on Linux allows _many_ Windows applications and games to be run on Linux, increasing the number of applications that can be run on Linux. However, not all applications can be properly run with WINE, so your mileage may vary depending on the app.
- Proton compatibility layer by Steam allows almost ALL games on steam to be run on Linux. If it's on Steam, it can probably work on Linux. If it's not on Steam, WINE may be still able to run the game. Check [ProtonDB](https://www.protondb.com/) to verify compatibility.
- Startup times are super fast (at least on my current distro) compared to Windows 10/11.
- If dual booting Linux with Windows, Linux is able to access Windows drive partitions without any issue. Windows needs some fancy hacks to access Linux partitions.
  >Windows uses NTFS filesystem, while Linux uses BTRFS/EXT4/XFS etc. Linux natively supports all of these but Windows only supports NTFS

## Cons:
- If something _does_ go wrong, it can be difficult for a non-technical person to diagnose the issue and fix
  - Sufficient knowledge of Linux as an OS is required to use Linux on a daily basis, to avoid borking the system, and to fix the system if it's somehow borked. Not having sufficient knowledge is a risky move, but not impossible. Knowledge can be aquired by more frequent use.
  - As of right now, most distros are not on the level of user-friendliness of MacOS and Windows. If everything works, Linux is pretty cool. But if something doesn't work, the troubleshooting process can be pretty impossible for a non-technical person to do. Software and hardware support mainly exist for Windows, and not much for Linux. This is likely the biggest con for Linux.
- Nvidia GPUs are notoriously finnicky with Linux. Sometimes, some things just outright does not work, or are really buggy. The best option would be to avoid Nvidia if possible and use an AMD GPU.
- Some games just don't get QA'd on Linux often enough. Apex Legends once accidentally banned Linux users that had a specific AMD setting on. The issue has since been resolved but that wasn't very nice.
- Game launches may not support Linux systems on the first few days of launch. Linux gamers waiting to play the latest games may need to wait a while longer to play newly released games as compared to Windows players.
- Some games straight up don't work on Linux, because the game developers decided so. Riot games implemented their Vanguard anti-cheat to work on Windows, but that can't work properly on Linux. Hence, Linux players are unable to play Valorant and League of Legends on Linux. A dual boot of Linux + Windows would be necessary, and that is a huge pain-in-the-ass.
- Adobe products don't work. At all. Alternatives to Photoshop and friends need to be used.
- Some niche Windows applications do not exist on Linux. Android emulators like Bluestacks are not supported. Alternatives like Genymotion need to be used.
- Laptops may experience poor battery life. However, some have reported better battery life instead. Your mileage may vary.
- Laptop touchpads are known to not work very nicely with Linux.

In summary, moving to Linux is NOT everyone's cup of tea. For me, the Pros outweigh the Cons, and I am able to live with the Cons. However, if a part of your workflow is outright dependent on something not supported on Linux, there is no shame in staying on Windows. Linux is trading convinience for increased control over what you use and avoid being at the mercy of one multi-billion dollar corporation, which is driven purely by profit instead of customer satisfaction.

# Searching for a Distro
I used a little bit of Ubuntu, everyone's first starter distro back in the 2016-ish era. Got bored of it real quick because I couldn't really customize much of the desktop layout, and the default layout was kind of confusing to me. Most importantly, I couldn't play any of my games. WINE wasn't even usable properly back then.

Fast forward to 2023. I have a spare SSD to try out different distros on, and jump back to Windows to continue doing schoolwork and playing my games.

At this point of time, I was running on a GTX 1060 3GB GPU and a AMD Ryzen 5600X CPU.

## Ubuntu
In recent times, I have heard bad things about Ubuntu and the Snap store. Stories of the [snap store being annoying to deal with](https://www.reddit.com/r/linux/comments/j3ajnf/whats_wrong_with_snaps_why_so_many_people_hate_it/) and the overall enshittification of Canonical, the company that created Ubuntu, didn't sit right with me. It reminded me of Windows. I didn't want to get locked into some company's propietarty product, so Ubuntu was a hard pass for me.

![](https://cdn.arstechnica.net/wp-content/uploads/2020/05/ubuntu2004-desktop.jpg)

## Linux Mint
The first distro I tried was Linux mint, as it was supposedly very similar to Windows. It didn't really appeal to me that much as I wasn't able to really customize the desktop the way I wanted it. The installation process was smooth though, everything seemed to work out of the box, even with a Nvidia GPU. I did not test much more before moving on to another distro.

![](https://www.lifewire.com/thmb/n0qftcgZ-ynnC3T_Moh3RyoCGks=/1600x900/filters:no_upscale():max_bytes(150000):strip_icc()/slingshot-56a5aab83df78cf7728952b3.png)

I later learned what a "Desktop Manager" was, and that Linux Mint was using the [Cinnamon desktop environment](https://wiki.archlinux.org/title/Cinnamon). It's overall pretty cool, but I wanted more cuztomizability. Hence, I googled for a customizable Desktop Environment, and KDE was highly reccomended.

## KDE and Kubuntu
There are many [distros that come with KDE](https://kde.org/distributions/). Kubuntu was the first option. Now, I'm wondering why Kubuntu has Ubuntu in the name. This is where I learned that some distros are forks of other distros. In this case, Kubuntu is a fork of Ubuntu, but has KDE as its Desktop Environment. Unfortunately, the snap store is in this distro, which I'm trying to avoid. So that's a pass.

## openSuse Tumbleweed
Next on the list that uses KDE is openSuse. Cool. I installed it. It has this thing called YaST.
![](/assets/img/blog/migrating-to-linux/yast.webp)
It comes with many, many cool GUI tools that didn't exist in other distros I tried before.
1. It has a package manager GUI that you can use to _search_ for packages by name and install them. That's cool, because in other distros, you'd need to somehow know the exact name of a package and type it in the terminal to install.
2. It has a repo manager to help you manage your repositories that the package manager will download packages from, again, using a GUI. Not common for many distros to have a GUI for these things.
3. Other settings include boot loader, network settings, firewall, snapshots (backup) etc. It becomes super easy to just click your way around to configure settings instead of manually googling the exact config file to modify in the system and what values you can use. Having a GUI for all of these things is amazing, because I won't need to write down a guide somewhere to recall how to change the boot loader timeout time. I can just click in the GUI to do it.

YaST is amazing, and I stuck with openSuse JUST because of this one tool. Having a GUI to avoid sifting through config files is amazing. It makes using Linux so much more bearable.

Furthermore, the KDE settings menu allows an insane level of customization of the desktop, with themes, icons, cursors and more. KDE is a popular desktop environment, and there are many people creating various custom themes and colour schemes on the KDE store to pick from and use.

openSuse Tumbleweed is also a rolling disto, which means there's constant updates almost everyday. Other distros tend to have versions of the distro that get released once every few months. Rolling distros are constantly updated, so they get the latest updates more frequently than the non-rolling distro counterparts. This may potentially lead to software containing more bugs and break the system, but openSuse has an amazing backup system called Snapper. It allows rolling back updates if anything gets borked, giving me the confidence to jsut update and rolllback if things don't work. I can always choose not to update my system during mission critical times, when I don't want my system to possibly get borked, and update at a later convinient time.

Unfortunately, the few downsides of openSuse is that the package manager (called zypper) and the Discover store can be very slow. Depending on the region, the mirrors for these package repos can be slow. Additionally, some software packages may not be available in the openSuse repos, as this distro is not as popular as the bigger ones like Debian, Ubuntu and Fedora. Flatpaks provided by Discover store can cover most of the applications not available on the repos. If an application _also_ does not exist as a flatpak, then an AppImage is your final choice. This depends on the developer making an AppImage version of the app though.

The recommended isntall priority is to always find the application on the openSuse repos first, then flatpaks, then appimages.

In conclusion, I like YaST and the rolling distribution that openSUse Tumbleweed provides. Updates may be slower than other distros, but that's a pain I can live with. I will stick to using openSuse Tumbleweed.

# Installing Linux and Dual Booting with Windows
After using openSuse for awhile, I decided to fully commit to openSuse.
I would still want a backup option of native Windows if I ever needed to use an application that can't run on Linux.
I settled on the hardware layout of:
- 2TB SSD for my main Linux installation
- 2TB SSD for additional storage
- 500GB SSD for Windows 11

There won't be much space on the Windows drive for too many games or videos, but that's fine as most games that I play all work on Linux.

Initially, I had Windows 10 installed on my main 2TB SSD. openSuse will replace Windows 10 on this drive eventually, but I can't nuke the system right now. First, I installed Windows 11 onto my 500GB SSD and made sure it was set up nice and comfy, in case I had to bail on Linux entirely and move back to Windows.

Next, I had to move my data from my Windows 10 install onto the additional storage 2TB drive. I re-formatted the storage drive using the EXT4 filesystem instead of NTFS. I had to use a Linux live boot USB to do the formatting because Windows can't format things in EXT4. Why EXT4? Because I'm going to use this drive mainly with Linux and not Windows. Non-NTFS filesystems like BTRFS and EXT4 works much better with Linux than NTFS.

Unfortunately, Windows _can't_ natively read an EXT4 formatted drive. I will go into detail on how to access Linux filesystems in the next section. TLDR, I used WSL to access the 2TB storage drive and managed to move my data off of my WIndows 10 drive. I also moved my saved games and data to the Windows 11 drive. Now, it is time to nuke the Windows 10 Drive!

**HOLD ON!** If you nuke the drive right now, you're not going to be able to boot into Windows!
I need to move the Windows boot partition from Windows 10 to Windows 11. When the PC boots, it will look for boot partitions. If there are none for windows, windows may be unable to boot at all. I have to make a boot partition on the 500GB SSD where Windows 11 is currently installed.

![](/assets/img/blog/migrating-to-linux/partition.webp)
Following this [guide](https://superuser.com/a/1536486) on stackoverflow, I managed to move the partition.

Now, I could nuke the 2TB main drive and install openSuse on it. Following the install instructions for openSuse was pretty straightforward. Tada, openSuse is installed.

## Accessing Linux filesystems with Windows
Why? Because sometimes, on Windows, I may want to access my music or other files on the storage drive. This drive is formatted in EXT4, which can't be read natively by Windows. Windows Subsystem for Linux can act as a bridge for this.

These following guides are for reference:  
https://learn.microsoft.com/en-us/windows/wsl/wsl2-mount-disk  
https://medium.com/nerd-for-tech/windows-11-shenanigans-how-to-mount-any-linux-filesystem-in-windows-e63a60aebb05

First, you will have to install WSL. Using Ubuntu here is fine. It's the default distro for WSL. Make sure WSL is run at least once first. Set up the commands to mount and dismount the drive using WSL, and dump these commands into a .bat file that will get called during Windows start and shutdown. Task Scheduler can automatically run scripts on startup and shutdown.

### Mounting
`wsl --mount <DiskPath> --partition <PartitionNumber> --type <Filesystem>`
- <DiskPath\> The path to disk to mount.
  - Run this command in powershell: `GET-CimInstance -query "SELECT * from Win32_DiskDrive"`. Identify the drive you want to mount, and note it. e.g. `\\.\PHYSICALDRIVE2`
- \<PartitionNumber\> Optional. The specific partition of this drive to mount. If not specified, will mount the entire disk.
  - Do a bare mount of the target drive using `wsl --mount <DiskPath> --bare`. At this point, the disk should be visible in WSL. Use WSL to run the command `lsblk`, and identify the disk/partition. E.g. Disk may be sda/b/c/d, and under it may be children labeled as sda1/2/3/4. This number behind sda[x] indicates the partition number. If your target partition is sda2, then the partition number should be `2`.
- \<Filesystem\> Optional. The type of filesystem that you are mounting. Defaults to `ext4`
  - `blkid -k` in WSL/linux will list out all possible file systems. For BTRFS, do `--type btrfs`
- \<MountOptions\> Optional. Additional options for mounting, specific to the type of filesystem (btrfs, ext4 etc)
  - https://btrfs.readthedocs.io/en/latest/ch-mount-options.html
  - should be in example format: `autodefrag,noacl,commit=15,nossd,clear_cache,space_cache=v2,noatime`
  - https://www.reddit.com/r/btrfs/comments/hiqetu/what_are_optimal_btrfs_mount_options_best/ TLDR: Leave as default is best.

### Unmounting
`wsl --unmount <DiskPath>`
>NOTE: Unmounting is highly recommended before shutting down to prevent bad things from happening!

### Accessing the folder
You can access the mounted folder directly via the little "Linux" folder in your Winodws File Explorer sidebar.  
You'd have to navigate into the corresponding folder that holds your mounted drive. For example, a possible path would be `\\wsl$\Ubuntu\mnt\wsl\PHYSICALDRIVE2p1`  
![alt text](/assets/img/blog/migrating-to-linux/folder1.webp)  
To make it easier to access your folder, you can add this folder to your File Explorer by adding a new "Network Location", and using the same URL mentiled above. Not to be confused with "Map network drive", that option only allows you to map the `\\wsl$\Ubuntu` folder and not any more folders nested under this one.  
![alt text](/assets/img/blog/migrating-to-linux/folder2.webp)  

>NOTE: Some caveats include certain applications not playing well with accessing network locations. For example, a music player may want access to a folder to your music. Some applications work ok with network paths, some don't. Your mileage may vary.

# Setting up openSuse
## General setup
Some references:  
https://www.techhut.tv/opensuse-5-things-you-must-do-after-installing/

If you have Nvidia GPU, ensure the Nvidia drivers are installed. Make sure the proprietary drivers are installed instead of the open-source ones (called noveau). The open source ones are wonky.

Update the system with `sudo zypper ref && sudo zypper update`
At this stage, restart at least once so that there's less chance of something getting borked.

Add the [packman repo](https://en.opensuse.org/Additional_package_repositories):
```
sudo zypper ar -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman
```
Switch system packages to packman:
```
sudo zypper dup --from packman --allow-vendor-change
```

If possible, use a [packman mirror](http://packman.links2linux.org/mirrors) closer to you. Change the repository URL via YaST > Software Repositories.  
Add the [wine repo](https://download.opensuse.org/repositories/Emulators:/Wine/openSUSE_Tumbleweed/) if you're planning to play games outside of Steam, or use Windows applications (that hopefully won't break with wine)  

Install codecs to allow playback of different multimedia formats
```
sudo zypper install opi && opi codecs
```
If you love Microsoft fonts, install them:
```
sudo zypper in fetchmsttfonts
```
For any other things you may want to install/uninstall, browse YaST > Software Management. Click on the "Patterns" tab, and you will find many common packages grouped according to their use-case. Once you're satisfied with the things you want to install and uninstall,  click "Accept" to apply the changes.  
![](https://en.opensuse.org/images/6/6a/500px-Yast-software-installation2.png)  
_This is an old screenshot without the "patterns" tab yet..._
>I HECKIN' LOVE GUIS HECK YEAH

## Setting PC name
Go to YAST \> Network Settings \> Hostname/DNS \> Static Hostname, change value to new name. Restart to take effect.

## Setting Static IP
Go to Settings \> Wi-Fi & Networking
select enp6s0 (for LAN. Not sure what WiFi would be)
select ipv4
Set method to manual, add DNS, add static IP.

## Reducing bootloader timeout
The bootloader is the part during booting where it allows you to choose which Operating System to boot. You can pick openSuse, Windows, or pick an older snapshot to boot when something gets borked.  
![](https://lh3.googleusercontent.com/-dak7FrDQNG0/YFtRiHsqejI/AAAAAAAAe2k/GHEnJPTi0Hs5zkO-MvAhPMjjeYoTY5digCLcBGAsYHQ/opensuse-finished-bootloader.png)  
Anyway, to reduce the time spent on this menu (for slightly faster bootup times), go to YaST > Bootloader > Bootloader Options.
You can set the timeout to however long you want. I set mine to 1 second. 0 seconds skips this menu entirely. If you want to boot into windows, you might not want to set this time to 0.

## Fixing slow Zypper (not really)
Explanation on why zypper is slow: https://github.com/openSUSE/zypper/issues/104  
A (partial) solution: https://github.com/Firstyear/mirrorsorcerer  
Use mirror sorcerer to auto-set fastest mirrors on pc start. Avoids the need to connect to the main server to obtain a mirror.

Try to use the default settings on mirror sorcerer. Adding custom third-party mirrors is not a good idea, stick to the official mirrors.

See also https://en.opensuse.org/openSUSE:Mirrors for mirror stuffs.

## Setting up RGB using OpenRGB
Install via this link: https://software.opensuse.org/download.html?project=hardware&package=OpenRGB
OpenRGB and the corresponding udev rules should be installed. A new repositorey should have been added too.  
However, right now, OpenRGB won't be able to detect stuff like RAM lighting and USB device lighting. Follow https://openrgb-wiki.readthedocs.io/en/latest/ to fix these issues.  
If, for whatever reason, you need to run a `sudo` command on PC start, you can do that via a cronjob as root.  
Run `sudo crontab -e`, you'll be using `vim` to edit the cronjob. Add the line `@reboot sudo <your command here>` to run the command as sudo on reboot.  
Hint: Save and quit vim with :wq

### RGB with Razer devices
If OpenRGB does not sufficiently control the RGB on your Razer devices, you can install [openrazer](https://openrazer.github.io) to get some RGB control, as well as DPI control.  

Download and install: https://software.opensuse.org/download.html?project=hardware%3Arazer&package=openrazer-meta
Then run `sudo gpasswd -a $USER plugdev`
Then reboot!!

You'll need some GUI frontend for OpenRazer. There's a few to pick from. I use RazerGenie.  
Install [RazerGenie](https://github.com/z3ntu/RazerGenie). Should be searchable in YAST. If not, look at their github maybe, or try some other frontend apps from the list on [openrazer](https://openrazer.github.io/#download)

## Autostarting custom applications
Go to Settings > Search for Autostart. Add your applications or scripts to the list.
If running an application that can only be run via command line (like syncthing), you can put the run command in a bash script and put the bash script in the Autostart list. An example bash script to start syncthing is like so:
```
#! /bin/bash
/usr/bin/syncthing -no-browser -home="/home/azoor/.config/syncthing"
```
This starts syncthing in the specified directory `/home/azoor/.config/syncthing`.
Save this file somewhere with an example name `start_syncthing.sh`, and mark it as executable. (Right click in Dolphin file manager > Permissions > Check the "Is Executable" box)

# Conclusion
openSuse is great. Some things are a breeze to set up. Customization is great. However, some things are just a pain to set up. I'll be sticking with this distro for the forseeable future, and maybe hop back to Windows to play some obscure games or do development with Visual Studio.

In the future, I'll be posting more about how to set up game environments outside of Steam (using Wine, Lutris) and how to set up an Android Emulator to play games (Azur Lane in my case).

Hope this was somewhat of a informative read!
