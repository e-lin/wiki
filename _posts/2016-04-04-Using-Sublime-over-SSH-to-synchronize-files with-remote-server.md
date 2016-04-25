---
layout: post
title:  "Using Sublime over SSH to Synchronize Files with Remote Server"
date:   2016-04-04 00:00:00 +0900
categories: jekyll update
type: Handy
excerpt_separator: <!--more-->
---
<!--more-->

Steps
---
#1. Download sublime SFTP plugin: [https://wbond.net/sublime_packages/sftp][P1]

#2. Invoke Package Control (*hotkey: `cmd+shift+p`*) and filter with “SFTP” to Setup Server

#3. Edit the pop-out file like below:

```
    "host": "192.168.1.116",
    "user": “xxxx",
    "password": “xxxx”,
    "remote_path": "/home/pi/“,
```

#4. In an empty file, type `cmd+shift+p` -> SFTP: browse server to download files from remote server

#5. If you have a copy from remote server, skip step 4, type `cmd+shift+p` -> sftp: map to remote and set config in sftp-config.json file. Notice that the remote path is

```
    "remote_path": "/home/pi/tensorflow/tutorials”
```

That's it. After this, evertime you edit a synced file at the local side, just do:

- `cmd+shift+p` to invoke package control
- SFTP: Sync Local -> Remote

Reference
---
[How to use Sublime over SSH][R1]<br />
[Sublime Text FTP/SFTP 遠端檔案同步][R2]

[P1]: https://wbond.net/sublime_packages/sftp
[R1]: http://stackoverflow.com/questions/15958056/how-to-use-sublime-over-ssh
[R2]: http://www.barryblogs.com/sublime-text2-ftp-sftp-remotefilesync/