---
layout: post
title: "MacOS on Ubuntu 18.04 with Sosumi"
categories: Miscellaneous
---

I have long been an advocate of Linux, but there always advantages to having choices, especially for testing code cross-platform, but Apple doesn't make it easy to do. While it's relatively easy to spin up a Windows or Linux virtual machine, Macs are still pretty closed. Recently, I got turned onto a program called "sosumi" (I see what you did there, Alan Pope), which does just that. It's packaged as a snap, which Linux people have varying, strong, and loud opinions on, but I'm not going to get into that. All I know is that snaps are relatively easy to install, and relatively annoying to deal with, because of the weird updating, but they're a *good enough* solution to whatever I need to do. It was also a long and annoying enough process that I think it's worth writing up, but it would have been easy had I done everything right the first time. This is not meant to be an authority, it's just whatever I did that eventually got it working. 

## Installation of Sosumi

Installation seemed easy at the time, but I went through a bunch of reinstalls, because I read a comment online that said the stable version did not work. Turns out for me, at this point, the solution was to use the stable version. I downloaded Sosumi with: 

`snap install sosumi --stable`

To start sosumi, I ran:
`sudo sosumi`

But wait! I immmediately hit received this error:
`Could not initialize SDL(x11 not available) - exiting`

To fix this, I ran
`xhost &&  xhost local: && xhost localhost`

## Installation of MacOS

The next part is a pretty standard installation I think. I've never installed MacOS, but it looks pretty similar to an Ubuntu installation. First, click `Boot macos install from macos base system`. Then Select *Disk Utilities* and erase the first partition. Then close the Disk Utilities menu, and Reinstall MacOS. 

This took a couple tries for me, as the installation kept hanging on parts for me, but restarting the installation seemed to work. If the installation finishes, you should have the option to Boot into MacOS from the partition that you erased earlier. From there just follow the MacOS setup instructions, are voila! you should have your own Mac VM. 


