---
layout: post
title: "Guide for complete nvidia and tensorflow setup on arch linux using System76 Drivers"
date: 2020-01-21 22:54
author: Nageen
category: [linux-utils, how-to,guide,nvidia,tensorflow,linux]
summary: 
image: assets/images/linux-tips-tricks.jpg
featured: true
published: true
---
Setting up nvidia drivers and making tensorflow to work on gpu on linux system is always mind 
boggling task. I have many times reinstalled/formatted my laptop just to get it working. No matter 
what there is always something that messes up while setting up working environment for tensorflow 
on grphic card. 

In this post, I will explain an easy to perform procedure for installing everything on Arch Linux 
so that you can finally execute tensorflow programs on GPU.


Note: I am assuming that you have working Arch Linux installed with any Desktop Environment(Gnome, KDE, i3 etc) with no extra packages related to nvidia or tensorflow(e.g Bumblebee, optimus, nvidia etc). 
Make sure you have yay installed.<br>
We will install system76-power from **Arch User Repository(AUR)**. [ **System76 have their own linux distribution Pop OS based on Ubuntu which have inbuilt nvidia suppoprt.**] 

---
---

So lets get started:
```bash
yay -S system76-power system76-dkms 
sudo systemctl enable system76-power.service
```

this will take some time to install. 

System76 Drivers require "ec_sys.write_support=1" argument to be passed for kernel to work.
open grub file with:
```shell
sudo nano /etc/default/grub
```
and search for line starting with GRUB_CMDLINE_LINUX_DEFAULT
and append this in it. It should look something like this:
```conf
...
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash ec_sys.write_support=1"
...
```
then we will regenerate grub config with this new setting using:

```shell
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
install bumblebee and optimus:
```shell
sudo pacman -S bumblebee primus 
sudo gpasswd -a $USER bumblebee
sudo systemctl enable bumblebeed.service 
sudo pacman -S nvidia-dkms nvidia-settings
```
reboot your system.

system76-power is used to switch the graphic cards between intel and nvidia, but it need a reboot after the change. 

swtich the graphics to intel:
```shell
sudo system76-power graphics intel
```
and to swtich to nvidia, use:
```shell
sudo system76-power graphics nvidia
```
after reboot, Turn on graphic card and load nvidia drivers using:
```shell
sudo system76-power graphics power on
sudo modprobe nvidia-drm nvidia-modeset nvidia
```
to confirm if nvidia drivers have successfully loaded and working, use:
```shell
primusrun glxinfo | grep -i renderer
```
output shoulf be something like :
```shell
OpenGL renderer string: GeForce 940MX/PCIe/SSE2
```

And to turn off:
```shell
sudo rmmod nvidia-drm nvidia-modeset nvidia
sudo system76-power graphics power off
```
for easy use you can you bash alias as follow:
```shell
alias nvidia-settings="optirun -b none nvidia-settings -c :8 "
alias nvidia-on="sudo system76-power graphics power on ; sudo modprobe nvidia-drm nvidia-modeset nvidia"
alias nvidia-off="sudo rmmod nvidia-drm nvidia-modeset nvidia ; sudo system76-power graphics power off"
```
Now you can install tensorflow from directly arch linux official reposotory:
```shell
sudo pacman -S python-tensorflow-cuda
```
If everything goes fine. You are ready to run tensorflow programs with GPU now.

If you want to install nvidia drivers and tensorflow from frmo official nvidia website follow this <a href="{{ site.url }}/blog/How-To-install-Nvidia-drivers-,-Cuda-and-tensorflow-on-Linux"> post</a>.


If you have any query, Comment below.





References:
1. [https://wiki.archlinux.org/index.php/NVIDIA](https://wiki.archlinux.org/index.php/NVIDIA)
2. [https://ebobby.org/2018/07/15/archlinux-on-oryp4/](https://ebobby.org/2018/07/15/archlinux-on-oryp4/)
3. [https://www.archlinux.org/packages/community/x86_64/python-tensorflow-cuda/](https://www.archlinux.org/packages/community/x86_64/python-tensorflow-cuda/)








