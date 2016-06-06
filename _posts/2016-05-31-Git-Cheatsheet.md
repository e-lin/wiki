---
layout: post
title:  "Git Cheatsheet"
date:   2016-05-31 00:00:00 +0900
categories: jekyll update
type: Git/Github
excerpt_separator: <!--more-->
---
<!--more-->

[Undo a commit and redo][R1]
---
```
$ git commit -m "Something terribly misguided"
$ git reset --soft HEAD~
<< edit files as necessary >>
$ git add ...
$ git commit -c ORIG_HEAD
```


[R1]: http://stackoverflow.com/questions/927358/how-do-you-undo-the-last-commit