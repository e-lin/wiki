---
layout: post
title:  "Bash Cheatsheet"
date:   2016-04-25 00:00:00 +0900
categories: jekyll update
type: Handy
excerpt_separator: <!--more-->
---
<!--more-->
- to tell how much memory is left on SD card (RasPi): `df -h /dev/root` or `df -h`
- free memory: `free -m`
- restart your shell: `$ exec -l $SHELL`
- sudo previous command: `$ sudo !!`
- copy directories: `cp -R <sourcedir> <destdir>`
- rename folder: `mv /home/user/oldname /home/user/newname`
- see java folder path: `whereis java`