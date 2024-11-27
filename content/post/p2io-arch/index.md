+++
date = '2024-11-27T00:09:41-06:00'
draft = true
title = 'Project Outfox on Arch Linux with Betson P2IO'
+++

## Introduction
I previously purchased a Betson Supernova 2 almost complete with working hardware except a monitor (that nice Kortek monitor 😔). And while I do enjoy older DDR mixes, I'm a big fan of custom tracks and perfer to have Stepmania install. I ran the [Stepmania Starworlds](https://github.com/chrispable/stepmania/tree/starworlds) branch for a while but I the branch is no longer updated so it was up to other forks to implement the P2IO/P3IO library.

This is when I found out about [Project Outfox](https://projectoutfox.com/) which is a continuation of Stepmania 5. This is when I found out that I **had** to run Windows 7 and that I got luckly last time running Starworlds that I installed it onto a Windows 7 machine. Without knowing the full technical details, it had something to do with the USB stack in anything above Windows 7. So I did what everyone else did, installed Windows 7 onto my old Dell Latitude E6420 laptop, hooked everything up and played Stepmania.

Now since I was running older hardware, I started to run into some hardware related issues. Performance was not great with the hardware I had, and I didn't want to buy some other old hardware that may potentionally be better. I did have something a little better with a Dell Optiplex 3020 I had with an Intel i5-4570 and then I had a spare video card with a AMD RX550. While the hardware isn't great, it's better. I at first put Windows 7 onto it and I found that while the performance did improve, it wasn't perfect still.

Project Outfox did have Linux builds, but it didn't really seemed that anyone was running the Linux version of Project Outfox with a Betson P2IO. I put Xubuntu 24.04 onto a drive, installed it onto a spare SSD I had and gave it a try. To my surprise, it worked without any issues. Now this made me think, why not use my perfer distro: Arch Linux to create my new setup.

## Arch Linux Setup
I've set up Arch Linux a few too many times and was a bit lazier now to run through the wiki and do the manual install. I saw they introduced the `archinstall` script so I decided to try that out. During the setup, I select Pipewire as the audio server and NetworkManager as the network manager. It worked great and I now had a working Arch Linux install.

To fix my user groups:
```bash
sudo usermod -aG audio,video,input,wheel <username>
```

For the programs I installed:

```bash
sudo pacman -S xorg xorg-apps xorg-xinit thunar firefox alacritty openbox ttf-dejavu ttf-liberation vim nano
```

I was reading a bit and found that compositors sometimes cause extra latency and may not be the best for pure performance. This ruled out Wayland as those compositors are linked together so I was going to stick with Xorg. With a pure windows manager, I wanted something easy to use and configure so I decided on Openbox.

### Openbox
Following the Arch Linux [Openbox Wiki](https://wiki.archlinux.org/title/Openbox), I configured my Openbox setup. I wanted a simple menu config that has almost nothing in it.

```bash
mkdir -p ~/.config/openbox
cp -a /etc/xdg/openbox ~/.config/

vim ~/.config/openbox/menu.xml
```

### Autostart
I wanted my system to automatically log in and start Outfox. This requires that we set it so that it auto logs into the [virtual terminal](https://wiki.archlinux.org/title/Getty#Automatic_login_to_virtual_console), start [Xorg](https://wiki.archlinux.org/title/Xinit#Autostart_X_at_login) and [Openbox](https://wiki.archlinux.org/title/Xinit#xinitrc), then start [Outfox](https://wiki.archlinux.org/title/Openbox#Starting).