---
layout: post
title:  "Using Screen"
date:   2016-04-04 00:00:00 +0900
categories: jekyll update
type: Handy
excerpt_separator: <!--more-->
---
Screen allows you to:

- Use multiple shell windows from a single SSH session.
- Keep a shell active even through network disruptions.
- Disconnect and re-connect to a shell sessions from multiple locations.
- Run a long running process without maintaining an active shell session.
<!--more-->

Using Screen
---
- `screen` to enter screen mode
- `^a` + `d` to detach
- `screen -ls` show current screens
- `screen -r [pid]` resume screen

### multiple windows:
- `^a` + `c` to create a new window (with same screen pid)
- `^a` + `n` switch to next window
- `^a` + `p` switch to previous window

### logging:
- `^a` + H to start\end logging or use `screen -L` in essential
- `cat screenlog.0` to print log

```
alias stresc='sed -r "s/\x1B\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g"'
stresc screenlog.0 > screenlog.0.txt
```
to get rid of escape sequences to see log in text editor

### lock:
- `^a` + `x`

### kill window:
- `^a` + `k` or `exit`

Reference
---
[使用 Screen 指令操控 UNIX/Linux 終端機的教學與範例][R1]

[R1]: http://blogger.gtwang.org/2013/11/screen-command-examples-to-manage-linux-terminals.html