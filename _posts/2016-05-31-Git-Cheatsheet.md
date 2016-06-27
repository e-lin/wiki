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

[Syncing a fork][R2]
---
```
$ git fetch upstream
$ git checkout master
$ git merge upstream/master
```

[Setting the upstream for a fork][R3]
---
```
$ git remote -v
origin  https://github.com/nohe427/developer-support.git (fetch)
origin  https://github.com/nohe427/developer-support.git (push)

$ git remote add upstream https://github.com/esri/developer-support

$ git remote -v
origin  https://github.com/nohe427/developer-support.git (fetch)
origin  https://github.com/nohe427/developer-support.git (push)
upstream        https://github.com/esri/developer-support (fetch)
upstream        https://github.com/esri/developer-support (push)
```

[Git rm all untracked files][R4]
---
```
$ git clean -df
```

[Discard unstaged changes][R5]
---
```
$ git stash save --keep-index
```
After that, you can drop that stash with a `git stash drop` command if you like.

or

```
$ git clean -dfx
$ git checkout .
```


[R1]: http://stackoverflow.com/questions/927358/how-do-you-undo-the-last-commit
[R2]: https://help.github.com/articles/syncing-a-fork/
[R3]: https://github.com/Esri/developer-support/wiki/Setting-the-upstream-for-a-fork
[R4]: http://stackoverflow.com/questions/61212/how-do-i-remove-local-untracked-files-from-my-current-git-branch
[R5]: http://stackoverflow.com/questions/52704/how-do-you-discard-unstaged-changes-in-git

