---
layout: post
title: How to fix the different time on when dual booted Linux and Windows
date: 2019-08-24 17:28
category: [linux-utils, how-to,guide]
author: Nageen
summary: 
image: assets/images/linux-tips-tricks.jpg
featured: true

---

Windows and Linux both are quite different in many respect. This also applies for the interpretition of the time from the hardware Clock. Yes, Computers have the clock which keeps running even if your system if turned off. Thats why we get synced time everytime we boot up computer. 

How linux and windows interpret time differently? 

Windows assumes the time stored in System clock as local time and uses it as it is. But Linux assumes the system time as UTC time and applies the timezone offset (shifts time according to the timezone set, e.g For india, It is +05:30 , So it will shift the time 5 and Half hours ahead and set the local time.).

How to fix this?

Easiest One solution is to make Linux think the System Clock time as Local Time 

Run following command int terminal for this.
<iframe
  src="https://carbon.now.sh/embed?bg=rgba(0%2C0%2C0%2C0)&t=material&wt=none&l=application%2Fx-sh&ds=false&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=42px&ph=56px&ln=false&fl=1&fm=Hack&fs=18px&lh=90%2525&si=false&es=4x&wm=false&code=timedatectl%2520set-local-rtc%25201%2520--adjust-system-clock"
  style="transform:scale(0.7); width:1024px; height:473px; border:0; overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

If You want to revert this behaviour , Heres' the command for that.

<iframe
  src="https://carbon.now.sh/embed?bg=rgba(0%2C0%2C0%2C0)&t=material&wt=none&l=application%2Fx-sh&ds=false&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=42px&ph=56px&ln=false&fl=1&fm=Hack&fs=18px&lh=90%2525&si=false&es=4x&wm=false&code=timedatectl%2520set-local-rtc%25200%2520--adjust-system-clock"
  style="transform:scale(0.7); width:1024px; height:473px; border:0; overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

Other method of changing the way how windows interprets time. This one is bit longer and involves editing regitry which can mess with system(typical Windows thing). So that one is not recommeneded. 

I hope your problem is now solved. And for any doubts please comment below.