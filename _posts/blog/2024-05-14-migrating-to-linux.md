---
title: Migrating to Linux
description: "My adventures of moving from Windows to Linux"
date: 2024-05-14T15:27:39.304Z
preview: ""
tags: ["Linux", "OS"]
categories: ["Blog"]
type: default
image: /assets/img/blog/migrating-to-linux/windoes.webp
pin: false
---
## Introduction
This post is a documentation of my transition from Windows to Linux. It involves the things I've considered before moving over, the different Linux distros I've tried, the things I had to do to set up my new Operating System, and my use cases to work around things. Hopefully, if you're looking to do something similar, this may provide you with some useful information to make your own decisions. 

![](https://i.pinimg.com/originals/e9/3d/d5/e93dd5040011c099c624f17653db0d5c.png){: .normal h="80"}

## Some common terms and phrases
I'll be using some terms that may not be familiar to the Linux ecosystem. Skip to the next section if you're already familiar.

**Linux** - _Technically_, this is the Kernel of the Operating System (a low-level thing that interacts with your computer's hardware). Many Distributions are built on top of the Linux kernel. The more accurate term to use would be "GNU + Linux" or so I've heard. However, for the sake of simplicity, I will use "Linux" to refer to the general branch of Operating Systems that use the Linux kernel. (because it's easier to type Linux vs GNU+Linux every time)

**Distribution (aka Distro)** - Different flavors of Operating Systems built using the Linux kernel. Some examples include Ubuntu, Debian, Fedora etc. Each one is different in terms of the software packages being packaged together. Some distros are forked from others, so there may be some distros that share a lot more similarities with each other than others. For example. Ubuntu is forked from Debian. Most debian stuff will work on Ubuntu.

**Package manager** - A software that helps to look for and install software on Linux systems. A common one for Debian-based distros is `apt`. You might see a common command like `sudo apt update` to update the system's repositories using the package manager.

**Package** - A collection of files that gets downloaded by the package manager. Packages can contain library files that applications require to run (called **dependencies**), or contain the files that make up the application itself. Many applications use the same few packages as dependencies so having these bits of files packaged as modular packages allows these applications to use the same files that are already downloaded rather than downloading another copy of the file for each application. This saves tons of space compared to Windows applications.

**Repositories (aka repo)** - An external place, usually a server, that provides packages to be downloaded. Different distros have different repositories, so that they can control the specific version of a package that goes into the distro to ensure compatibility. There can be more third-party repos that contain more packages not provided by the official repos of the distro, that allow users to install applications that are maintained by third-party developers.

**Flatpak** - An alternative to downloading applications via the package manager. Flatpak apps are downloaded together with ALL the required dependencies, which results in a larger size but better compatibility across different distros. This ensures that every application has the exact version of a package they need to work. Flatpaks are also containerized, making them slightly more secure due to the sandboxing feature that containerization provides. However, starting up flatpak applications tend to be slower than the package manager counterparts, so it is generally advised to use the package manager version of an app over a flatpak one.

## Why switch to Linux?
TLDR, Windows is enshittifying. I don't like it. Hearing the news of Windows [trying to force ads to users](https://www.zdnet.com/article/microsoft-is-now-showing-ads-in-windows-11s-start-menu-heres-how-to-block-them/), or trying to [take screenshots of the user's screen every second](https://www.tomshardware.com/tech-industry/artificial-intelligence/total-recall-the-only-copilot-ai-feature-that-matters-is-a-huge-privacy-risk), to [harvesting user data](https://www.zdnet.com/article/windows-10-telemetry-secrets/), I am growing very uncomfortable with the increasing user hostility that Microsoft is shoving down everybody's throats. It would be great to move to another OS before the user hostility practices get too out of hand.

Linux was always on my radar as a potential alternative, for a few years now. I had never sought to fully jump ship back then due to a multitude of reasons. But, times have changed. Linux gaming support has improved substantially (thanks to Valve), and I am no longer tied down by being required to work using a Windows environment for school/work. With the stars aligning, I decided to finally make the jump now.

My reasons are very subjective and based on my current circumstances. Not everyone will be able to move to Linux, and not everyone should move to Linux. If your stars align too, then maybe you should!  

![](https://media.pinatafarm.com/protected/B183D0EF-49B8-47BF-A523-E72FD0CFFAAC/annoyed-bird.3.meme.webp){: .normal }

## Gotcha's of moving to Linux from Windows
### Linux is a new Operating System to learn
Linux is not like Windows. Some parts are similar, some parts are not. Be prepared to re-learn how to use a new Operating System. Especially for Linux, be prepared to get your hands dirty, because many applications are not designed with the best User Experience in mind (it's getting much better though). Some things may not have been configured properly out of the box, so get ready to do some Googling when things don't work as expected. Learning a new Operating System, especially one that is as technical as Linux, can be a huge time sink. 

However, it is possible to just "install and use" Linux for some mainstream uses. If you're planning to just browse the web, no problem! Firefox is already installed on most distros, launch and open! Play Steam games? Install Steam, download your game, and get gaming! The difficult part comes when you want to start doing "less mainstream" stuff like playing non-Steam games that are Windows-native. It is possible with Wine, but be prepared to get your hands dirty to look up how to do it.
![](https://i.pinimg.com/originals/d7/f6/8e/d7f68e1be0eea2f5d563418622fd66a7.jpg){: .normal h="200" }

### Nvidia and Linux don't go well together
Nvidia GPUs are notorious for not working in various scenarios on Linux. Nvidia is also known to not be very cooporative with Linux developers, making life hard for Linux devs and users.

Below is an image of Linus Torvalds, creator of Linux, showing his appreciation for Nvidia's actions back in 2012:  
![](https://www.zdnet.com/a/hub/i/r/2014/10/04/b9ae21e0-4b69-11e4-b6a0-d4ae52e95e57/resize/770xauto/64fc0ac9b0123858cea16bff9018aa91/18-06-2012-13-33-24.jpg)  

With that being said, Nvidia support has been getting better, but your experience may vary. Depending on what you want to use your GPU for, you may or may not get shafted for a lack of Nvidia GPU support. Games nowadays usually run fine on Nvidia GPUs. Certain Nvidia features like shadowplay may not work properly. If you're planning to upgrade soon, and want to use Linux, do consider an AMD GPU.

Some things that don't work well with Nvidia GPUs:
- Waydroid (Android emulator), but there's a workaround. Not an optimal one, though.
- Some distros can't display properly with Nvidia GPUs during installation. Pop!_OS Is a recommended distro to use with Nvidia GPUs if this is a concern.
- Distro upgrade: Once, Nvidia released a broken driver, causing much crashing. Users had to roll back their updates until Nvidia fixed their drivers.
- Random crashes that happen across various programs related to Nvidia GPUs. Will probably get better over time, but it is up to Nvidia as their drivers are closed-source.

### Some programs can't work on Linux
Most programs from Windows have a Linux version to use too. Some have unofficial applications that mostly work the same as the official Windows programs. Some don't have a Linux version but alternate programs exist to fill the use case. Some Windows-only programs can be run on Linux using Wine. And then there are some programs that just can't work at all, short of installing the program in a Virtual Machine running Windows.

If these programs that can't be run on Linux need to be used for work, it may be a dealbreaker. I suggest sticking to Windows instead of hopping through several hoops to _kind of_ get the program working but in an unstable state.

A non-exhaustive list of things that don't work on Linux:  

**Riot's Games and maybe more**  
Specifically, it's due to their need for the kernel-level anti-cheat called Vanguard. Since they can't feasibly create Vanguard anti-cheat on Linux due to technical limitations, Riot Games completely dropped support for Linux users. And no, VMs can't run these games either. There is NO WAY to play these games on Linux.

This also means that any developers in the future that may want to implement kernel-level anti-cheats may also decide to do the same. League of Legends was playable on Linux up until Vanguard was implemented. So, any game may face this treatment one fine day, but most developers will probably not go down this route.

These games from Riot will be unplayable:  
- Valorant
- League of Legends and its derivatives (TFT, Aram, idk I don't play league)

**Adobe Suite**  
Generally, all Adobe programs cannot run on Linux. This includes Photoshop, Premire Pro, After effects etc. If you somehow manage to get it working, it's not gonna be great. Alternative programs may not come close to the functionalities of these programs, let alone the compatibility of file types when you have to collaborate with others. Not good.

**Visual Studio 2019/2022/whatever**  
VS is meant to build executables using MSVC. MSVC builds Windows executables. VS will not work properly on Linux, unless you somehow get it working using some magic. Even so, compilation may not work properly. Even if compilation somehow works, you can't run the executable natively on Linux. You'd have to run it through Wine. And if the app does not run, who knows which part went wrong? Was it your code, or was it due to Wine, or was it due to your Nvidia GPU being wonky on Linux? At that point, you'd be adding so many possible points of failure to your workflow, it's way easier to just dual boot with Windows. Or use VS Code.  

**Android Emulation**
You'd think that Android being a modified Linux kernel, Android emulation on Linux would be the easiest thing, right? WRONG. DEAD WRONG. Android emulators on Windows like Blue Stacks and Nox Player don't have Linux ports. In fact, none of them do. What does Linux have? Many options. But what options work to play your favorite gacha games like Azur Lane? ONE. And it was not easy to set things up. Waydroid on Linux was the ONLY emulator that could:
- Support games that require Android 10 and above (Waydroid has Android 11 and 13)
- Has properly working ARM translation for Android 10 and above
- Does not have issues with manually increasing storage limits of the virtual disk
- Does not have access permissions that prevent apps from downloading updates  

I am planning to write a blog about setting up Android Emulation on OpenSUSE soon, I'll link it here if it's up.

For more incompatible programs, check out these Reddit posts:  
[Whats some Software that you just can't use on Linux?](https://www.reddit.com/r/linux4noobs/comments/10icl7y/whats_some_software_that_you_just_cant_use_on/)  
[Apps that do NOT work on Linux (even with Wine)](https://www.reddit.com/r/linux4noobs/comments/rt22gb/apps_that_do_not_work_on_linux_even_with_wine/)

## The Pros and Cons of Linux and Windows
### Pros
**No ads, tracking, or spying**  
Yeah boy! Bye-bye, hostile user experience!

**No pre-installed bloatware**  
No stupid Microsoft Edge that you can't uninstall. However, some distros might still come pre-packaged with things you might not need, like KDE's Kmail applications. You can uninstall them, thankfully.

**Control your Updates**  
Your system will update when you tell it to update. Your system will do background magic if you tell it to do background magic. Your system will not do background magic if you don't tell it to do background magic. Your system will not force and nag at you to upgrade to the latest and greatest version. You are in control, not the damn OS.  

![](https://i.pinimg.com/originals/ee/b0/d6/eeb0d690e3c18bb5c801410c4c22d498.jpg){: .normal h="200" }

**Package-based system**  
No repeated packages when most software needs to use the same library files. Much less space taken. Windows takes around 100GB, while Linux installs take roughly 2-20GB of space.  
Package-based systems also make installing stuff easy, as it allows the installations of programs to be centralized to one place, using a common package manager to manage these packages. Updating everything becomes easier, just tell the package manager to update everything and boom, updated. No need for individual programs to run their self-updaters.

**Lighter system resource use**  
The OS on average eats less RAM and CPU as the processes for the operating system is much lighter compared to Windows. Booting times are somewhat faster for me, but it may have been a placebo. My external soundcard keeps having issues on Linux due to the OS booting up too fast, requiring a restart. That never happens on Windows (because bootup was slower)
. I guess you win some, you lose some. ¯\\_ (ツ)_/¯  

**Extensive Customizability**  
You can customize _many_ aspects of Linux, from the appearance and behavior of your desktop, to what kind of Window Manager you want to use, to the specific kernel modules you want to enable and disable (Bluetooth and WiFi for example).  
Aesthetically pleasing desktops have a [whole community](https://www.reddit.com/r/unixporn/) around it. "Ricing" your system is an art.

**Wine**  
No, not the drinking kind. "Wine is not an emulator" (Wine for short) is a software that allows you to run native Windows programs on Linux. This includes games. And oh boy, can you play so many games on it. Most games should be able to run on either Wine or Proton (Steam's custom implementation of Wine). Obscure games should be able to run too, with a little bit of tweaking. Unfortunately, not ALL games are able to run. I was OK with it, as I will be dual-booting WIndows anyway for these scenarios.

Additionally, I used foobar2000 as my music player back on Windows. foobar is not abailable on Linux natively, but I was able to install it in Wine and use it as if it was native. Wine is really versatile, but keep your expectations in check.

### Cons  
**When things don't work**  
When things work, amazing. When things don't work, damn. Is there another way to make it work? Time to google. Wow. Many results. 8 years ago, 5 years ago, damn these are outdated. Ooh, 4 months ago! Which distro is this guy on? Not mine? Shit. Try this fix. Nope doesn't work. Try that. Nope. Try another one. Oh no my system broke, I didn't know what I was doing with that command. Now how do I restore my system? Time to Google....  

_5 hours later_  

And I just have to install this one package, and MAYBE IT WILL WORK. OH GOD YES IT WORKS OH SWEET BABY JESUS. Now what was I doing again...?  
{% include embed/youtube.html id='_UZFI-8D5uA' %}  
![](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExZDQ3YW5uaHJ3MnZsYWphbm1tN2p5ZG5vd3hzeG9wMHZqb3F0M3lvbyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oKHWn2hsiAKF0xIsM/giphy.gif){: .normal}  

**Games lack support on Linux**  
Game devs focus on Windows users. On launch day, the game sometimes may not work. Sometimes, the game isn't even available on Linux. As you watch your freinds play the latest and greatest game, you can only imagine yourself doing the same 1 week later after things have been fixed.  
![](https://i.kym-cdn.com/photos/images/newsfeed/001/312/751/536.jpg){: .normal}  

Sometimes, QA testing isn't the greatest. Apex Legends [falsely banned legitimate players on Linux and Steam Deck MULTIPLE TIMES](https://www.gamingonlinux.com/2023/08/linux-players-getting-banned-on-apex-legends-again/). The issue has since been resolved, but that's pretty shitty.

**Laptops...**  
Laptops have varied results from people. Some people claim that their battery life isn't great. Solutions involve installing packages to help govern power settings or something, with mixed results. Some have claimed battery life has been great. I haven't tried Linux on laptops but the general consensus is that battery life may be impacted.  

Touchpads generally also have bad support. Not much can be done for this, as there's a limited amount of things that can be done to replicate hardware drivers. Just gotta deal with it.

## Distro Searching
I didn't know what I was looking for. I read up on a bunch of distros and tested some by installing them on a spare SSD. Here are some distros I came across when picking one.

### Ubuntu
Everybody's first distro. I tried this years ago, didn't really like the desktop, and went back to Windows. Nowadays, people are [unhappy with Ubuntu's snap store](https://www.reddit.com/r/linux/comments/j3ajnf/whats_wrong_with_snaps_why_so_many_people_hate_it/) and the overall enshittification of Canonical (Ubuntu's parent company). Sounds really similar to Microsoft, so that's a nope for me.
![](https://cdn.arstechnica.net/wp-content/uploads/2020/05/ubuntu2004-desktop.jpg)

### Linux Mint
A common recommendation for users coming from Windows was **Linux Mint**. It has UI features very similar to Windows, and a friendly user interface to get everything set up. Overall, it felt ok. However, I ran into the issue of my dual monitors not having the proper refresh rates I wanted. I had a 1440p 165Hz monitor as my main, and a 1080p 120Hz monitor at my side. If I recall correctly, Linux Mint was unable to set my secondary monitor's refresh rate above 60Hz. I was using a Nvidia GPU back then, so that may have been an issue. Anyway, this was a deal breaker, so I moved to another distro.
![](https://www.lifewire.com/thmb/n0qftcgZ-ynnC3T_Moh3RyoCGks=/1600x900/filters:no_upscale():max_bytes(150000):strip_icc()/slingshot-56a5aab83df78cf7728952b3.png)

### Wayland?
After googling for a solution, I stumbled across this video of a guy that has somewhat of a similar problem with me. He has an entire video documenting his Linux experiences too, which may be informative.  
{% include embed/youtube.html id='moYwK0YMFjQ?t=705' %}  
In the video, he mentioned using Wayland over X11 (graphical compositors) as a fix for refresh rate issues. Hence, I started searching for distros that supported Wayland, and found that the KDE desktop environment supports Wayland. Looking at the [list of distros that use KDE](https://kde.org/distributions/), I started researching.

### Kubuntu
Kubuntu is a flavor of Ubuntu that uses KDE. That means all the bad stuff with Ubuntu comes along with it. It's still an option to use, as it's pretty popular, but I skipped it due to the same reasons as Ubuntu.

### OpenSUSE Tumbleweed
OpenSUSE Tumbleweed is a distro that didn't fork from any existing distros. Many of its systems are unique. OpenSUSE Tumbleweed defaults to X11 session instead of Wayland. Poking around the monitor settings, I realized I could set the refresh rate of both monitors to 120Hz, the lower refresh rate of the two. That's good enough, I can live with 120Hz. I then tried Wayland for a bit, and things started getting slightly wonky. Switched back to X11, and all was well. I decided to stay on OpenSUSE.
>Note: A few months later, OpenSUSE upgraded the desktop environment from Plasma 5 to 6, and I also got an AMD GPU to replace my Nvidia one. Now, I could run both monitors at their own native refresh rates. Wayland also worked pretty well. Not sure if the desktop environment or AMD GPU fixed the issues, it's great now.

On top of KDE, OpenSUSE also provided an amazing GUI-based settings manager called YaST. This manager helps to provide a graphical interface for package management, bootloader settings and more system-related stuff.
![](/assets/img/blog/migrating-to-linux/yast.webp)

Many of these things could be done via the command line, but that would require me to look up how to do it every time. Having a handy GUI to go in and modify settings in a few clicks feels way better than manually adjusting settings via the command line and editing specific config files. Other distros don't seem to provide this functionality.

KDE's Plasma 6 (because I'm on Plasma 6 now) is very similar to Windows desktop. Customizing the Plasma desktop to your own liking is also possible, to a greater degree than what Windows provides. Plasma also allows you to customize the behavior of your entire Desktop environment, and easily switch themes, colors, icons and more.

OpenSUSE is a rolling distro, which means it gets the latest updates almost every day. Despite being constantly updated with the latest stuff, the distro itself has been regarded to be extremely stable. Personally, I do agree, OpenSUSE has been very stable so far.

The package manager it uses, zypper, is pretty slow. OpenSUSE is based in the European region, so their repository servers are nearby those regions. This introduces some latency when grabbing data from their servers, which can add up. Catastrophically, zypper does NOT do parallel downloads, while almost all other package managers do. [Zypperoni](https://github.com/pavinjosdev/zypperoni) is a third-party tool to help alleviate this bootleneck quite considerably.

Overall, OpenSUSE TW is pretty good.

### Fedora KDE
At one point, in desperation to get Waydroid working, I installed Fedora KDE. Visually, it looks the same as OpenSUSE, as both are using Plasma 6. Fedora boots into a Wayland session by default while OpenSUSE uses X11 (with the option to switch to Wayland). One huge difference I noticed was that Fedora, being a more well-supported distro, contained MANY more programs and apps in its repositories. OpenSUSE on the other hand relies on flatpaks for many apps that are not found in their official repos.

More "obscure" applications like Waydroid are well supported on Fedora, with the installation being a one-liner. OpenSUSE has no such support, and has to rely on third-party repos that may or may not work properly, in addition to manually tweaking boot settings.

Unfortunately, as I was going about installing apps on Fedora, I noticed instabilities in random programs. Programs started crashing while scrolling through them, and others just wouldn't start up. Restarting fixed the issues, but it came back soon. This may be due to the fact that Fedora's implementation of KDE or Wayland wasn't that great, or I might have messed up some settings (unlikely though, I did a fresh install TWICE and it happened both times).

So, I went back to OpenSUSE with Wayland.

## Installing and setting up OpenSUSE
### General setup
Some references:  
[https://www.techhut.tv/opensuse-5-things-you-must-do-after-installing/](https://www.techhut.tv/opensuse-5-things-you-must-do-after-installing/0)

If you have Nvidia GPU, ensure the Nvidia drivers are installed. Make sure the proprietary drivers are installed instead of the open-source ones (called nouveau). The open-source ones are wonky.

Update the system with `sudo zypper ref && sudo zypper update`
At this stage, restart at least once so that there's less chance of something getting borked.

Add the [packman repo](https://en.opensuse.org/Additional_package_repositories):
```
sudo zypper ar -cfp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman
```
Switch system packages to Packman:
```
sudo zypper dup --from packman --allow-vendor-change
```

If possible, use a packman mirror http://packman.links2linux.org/mirrors closer to you. Change the repository URL via YaST > Software Repositories.  
Add the [wine repo](https://download.opensuse.org/repositories/Emulators:/Wine/openSUSE_Tumbleweed/) if you're planning to play games outside of Steam, or use Windows applications (that hopefully won't break with wine)  

Install codecs to allow playback of different multimedia formats
```
sudo zypper install opi && opi codecs
```
If you love Microsoft fonts, install them:
```
sudo zypper in fetchmsttfonts
```
For any other things you may want to install/uninstall, browse YaST > Software Management. Click on the "Patterns" tab, and you will find many common packages grouped according to their use case. Once you're satisfied with the things you want to install and uninstall,  click "Accept" to apply the changes.  
![](https://en.opensuse.org/images/6/6a/500px-Yast-software-installation2.png){: .normal}  
_This is an old screenshot without the "patterns" tab yet..._
>I HECKIN' LOVE GUIS HECK YEAH

### Setting PC name
Go to YAST \> Network Settings \> Hostname/DNS \> Static Hostname, and change the value to a new name. Restart to take effect.

### Setting Static IP
Go to Settings \> Wi-Fi & Networking
select enp6s0 (for LAN. Not sure what WiFi would be)
select ipv4
Set the method to manual, add DNS and static IP.

### Reducing bootloader timeout
The bootloader is the part during booting where it allows you to choose which Operating System to boot. You can pick openSuse, Windows, or pick an older snapshot to boot when something gets borked.  
![](https://lh3.googleusercontent.com/-dak7FrDQNG0/YFtRiHsqejI/AAAAAAAAe2k/GHEnJPTi0Hs5zkO-MvAhPMjjeYoTY5digCLcBGAsYHQ/opensuse-finished-bootloader.png){: .normal}  
Anyway, to reduce the time spent on this menu (for slightly faster bootup times), go to YaST > Bootloader > Bootloader Options.
You can set the timeout to however long you want. I set mine to 1 second. 0 seconds skips this menu entirely. If you want to boot into Windows, you might not want to set this time to 0.

### Fixing slow Zypper (not really)
Explanation of why zypper is slow: [https://github.com/openSUSE/zypper/issues/104](https://github.com/openSUSE/zypper/issues/104)  
A (partial) solution: [https://github.com/Firstyear/mirrorsorcerer](https://github.com/Firstyear/mirrorsorcerer)  
Use Mirror Sorcerer to auto-set the fastest mirrors on PC start. Avoids the need to connect to the main server to obtain a mirror.

Try to use the default settings on Mirror Sorcerer. Adding custom third-party mirrors is not a good idea, stick to the official mirrors.

See also [https://en.opensuse.org/openSUSE:Mirrors](https://en.opensuse.org/openSUSE:Mirrors) for more info on openSuse mirrors.

### Setting up RGB using OpenRGB
Install [OpenRGB for openSuse](https://software.opensuse.org/download.html?project=hardware&package=OpenRGB).  
OpenRGB and the corresponding udev rules should be installed. A new repository should have been added too.  
However, right now, OpenRGB won't be able to detect stuff like RAM lighting and USB device lighting, because it isn't running with admin privileges. Follow [the OpenRGB documentation](https://openrgb-wiki.readthedocs.io/en/latest/) to fix these issues.  
If, for whatever reason, you need to run a `sudo` command on PC start, you can do that via a cronjob as root.  
Run `sudo crontab -e`, you'll be using `vim` to edit the cronjob. Add the line `@reboot sudo <your command here>` to run the command as sudo on reboot.  
Hint: Save and quit vim with :wq

#### RGB with Razer devices
If OpenRGB does not sufficiently control the RGB on your Razer devices, you can install [openrazer](https://openrazer.github.io) to get some RGB control, as well as DPI control.  

Download and install [openrazer for openSuse](https://software.opensuse.org/download.html?project=hardware%3Arazer&package=openrazer-meta)
Then run `sudo gpasswd -a $USER plugdev`
Then reboot!!

You'll need a GUI frontend for OpenRazer. There are a few to pick from. I use RazerGenie.  
Install [RazerGenie](https://github.com/z3ntu/RazerGenie). Should be searchable in YAST. If not, look at their GitHub maybe, or try some other frontend apps from the list on [openrazer](https://openrazer.github.io/#download)

### Autostarting custom applications
Go to Settings > Search for Autostart. Add your applications or scripts to the list.
If running an application that can only be run via command line (like SyncThing), you can put the run command in a bash script and put the bash script in the Autostart list. An example bash script to start SyncThing is like so:
```
#! /bin/bash
/usr/bin/syncthing -no-browser -home="/home/azoor/.config/syncthing"
```
This starts SyncThing in the specified directory `/home/azoor/.config/syncthing`.
Save this file somewhere with an example name `start_syncthing.sh`, and mark it as executable. (Right-click in Dolphin file manager > Permissions > Check the "Is Executable" box)

## Dual Booting with Windows 11
I would still want a backup option of native Windows if I ever needed to use an application that can't run on Linux.
I settled on the hardware layout of:
- 2TB SSD for my main Linux installation
- 2TB SSD for additional storage
- 500GB SSD for Windows 11

There won't be much space on the Windows drive for too many games or videos, but that's fine as most games that I play all work on Linux.

Initially, I had Windows 10 installed on my main 2TB SSD. openSuse will replace Windows 10 on this drive eventually, but I can't nuke the system right now. First, I installed Windows 11 onto my 500GB SSD and made sure it was set up nice and comfy, in case I had to bail on Linux entirely and move back to Windows.

Next, I had to move my data from my Windows 10 install onto the additional storage 2TB drive. I re-formatted the storage drive using the EXT4 filesystem instead of NTFS. I had to use a Linux live boot USB to do the formatting because Windows can't format things in EXT4. Why EXT4? Because I'm going to use this drive mainly with Linux and not Windows. Non-NTFS filesystems like BTRFS and EXT4 work much better with Linux than NTFS.

Unfortunately, Windows _can't_ natively read an EXT4 formatted drive. I will go into detail on how to access Linux filesystems in the next section. TLDR, I used WSL to access the 2TB storage drive and managed to move my data off of my Windows 10 drive. I also moved my saved games and data to the Windows 11 drive. Now, it is time to nuke the Windows 10 Drive!

**HOLD ON!** If you nuke the drive right now, you're not going to be able to boot into Windows!
I need to move the Windows boot partition from Windows 10 to Windows 11. When the PC boots, it will look for boot partitions. If there are none for windows, windows may be unable to boot at all. I have to make a boot partition on the 500GB SSD where Windows 11 is currently installed.

![](/assets/img/blog/migrating-to-linux/partition.webp){: .normal}
Following this [guide](https://superuser.com/a/1536486) on stackoverflow, I managed to move the partition.

Now, I could nuke the 2TB main drive and install openSuse on it. Following the install instructions for openSuse was pretty straightforward. Tada, openSuse is installed.

### Accessing Linux filesystems with Windows
Why? Because sometimes, on Windows, I may want to access my music or other files on the storage drive. This drive is formatted in EXT4, which can't be read natively by Windows. Windows Subsystem for Linux can act as a bridge for this.

The following guides are for reference:  
[https://learn.microsoft.com/en-us/windows/wsl/wsl2-mount-disk](https://learn.microsoft.com/en-us/windows/wsl/wsl2-mount-disk)  
[https://medium.com/nerd-for-tech/windows-11-shenanigans-how-to-mount-any-linux-filesystem-in-windows-e63a60aebb05](https://medium.com/nerd-for-tech/windows-11-shenanigans-how-to-mount-any-linux-filesystem-in-windows-e63a60aebb05)

First, you will have to install WSL. Using Ubuntu here is fine. It's the default distro for WSL. Make sure WSL is run at least once first. Set up the commands to mount and dismount the drive using WSL, and dump these commands into a .bat file that will get called during Windows start and shutdown. Task Scheduler can automatically run scripts on startup and shutdown.

#### Mounting
`wsl --mount <DiskPath> --partition <PartitionNumber> --type <Filesystem>`
- <DiskPath\> The path to disk to mount.
  - Run this command in powershell: `GET-CimInstance -query "SELECT * from Win32_DiskDrive"`. Identify the drive you want to mount, and note it. e.g. `\\.\PHYSICALDRIVE2`
- \<PartitionNumber\> Optional. The specific partition of this drive to mount. If not specified, will mount the entire disk.
  - Do a bare mount of the target drive using `wsl --mount <DiskPath> --bare`. At this point, the disk should be visible in WSL. Use WSL to run the command `lsblk`, and identify the disk/partition. E.g. Disk may be sda/b/c/d, and under it may be children labeled as sda1/2/3/4. This number behind sda[x] indicates the partition number. If your target partition is sda2, then the partition number should be `2`.
- \<Filesystem\> Optional. The type of filesystem that you are mounting. Defaults to `ext4`
  - `blkid -k` in WSL/linux will list out all possible file systems. For BTRFS, do `--type btrfs`
- \<MountOptions\> Optional. Additional options for mounting, specific to the type of filesystem (btrfs, ext4 etc)
#### Unmounting
`wsl --unmount <DiskPath>`
>NOTE: Unmounting is highly recommended before shutting down to prevent bad things from happening!

#### Accessing the Linux folder
You can access the mounted folder directly via the little "Linux" folder in your Windows File Explorer sidebar.  
You'd have to navigate into the corresponding folder that holds your mounted drive. For example, a possible path would be `\\wsl$\Ubuntu\mnt\wsl\PHYSICALDRIVE2p1`  
![alt text](/assets/img/blog/migrating-to-linux/folder1.webp){: .normal}  
To make it easier to access your folder, you can add this folder to your File Explorer by adding a new "Network Location", and using the same URL mentioned above. Not to be confused with "Map network drive", that option only allows you to map the `\\wsl$\Ubuntu` folder and not any more folders nested under this one.  
![alt text](/assets/img/blog/migrating-to-linux/folder2.webp){: .normal}  

>NOTE: Some caveats include certain applications not playing well with accessing network locations. For example, a music player may want access to a folder of your music. Some applications work ok with network paths, some don't. Your mileage may vary.

## Conclusion
openSuse is great. Some things are a breeze to set up. Customization is great. However, some things are just a pain to set up. I'll be sticking with this distro for the foreseeable future, and maybe hop back to Windows to play some obscure games or do development with Visual Studio.

In the future, I'll be posting more about how to set up game environments outside of Steam (using Wine and Lutris) and how to set up an Android Emulator to play games (Azur Lane in my case).

Hope this was somewhat of an informative read!
