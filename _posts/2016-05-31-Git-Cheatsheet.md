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

[Unstage multiple files][R6]
---
If all you want is to undo an overzealous "git add" run:

```
$ git reset
```
Your changes will be unstanged and ready for you to re-add as you please.

[Rebase a specific commit][R7]
---
To rebase to a specific commit, not to a HEAD of the other branch:

```
A --- B --- C          master
 \
  \-- D                topic
```
to

```
A --- B --- C          master
       \
        \-- D          topic
```
instead of

```
A --- B --- C          master
             \
              \-- D    topic
```

```
$ git branch temp master^
$ git checkout topic
$ git rebase temp
$ git branch -d temp
```

[Merging vs. Rebasing][R8]
---

[Remove a folder from git tracking][R9]
---
#1. Add the folder path to your repo's root .gitignore file.

```
path_to_your_folder/
```

#2. Remove the folder from your local git tracking, but keep it on your disk.

```
$ git rm -r --cached path_to_your_folder/
```
#3. Push your changes to your git repo.

The folder will be considered "deleted" from Git's point of view (i.e. they are in past history, but not in the latest commit, and people pulling from this repo will get the files removed from their trees), but stay on your working directory because you've used `--cached`.


[R1]: http://stackoverflow.com/questions/927358/how-do-you-undo-the-last-commit
[R2]: https://help.github.com/articles/syncing-a-fork/
[R3]: https://github.com/Esri/developer-support/wiki/Setting-the-upstream-for-a-fork
[R4]: http://stackoverflow.com/questions/61212/how-do-i-remove-local-untracked-files-from-my-current-git-branch
[R5]: http://stackoverflow.com/questions/52704/how-do-you-discard-unstaged-changes-in-git
[R6]: http://stackoverflow.com/questions/7103631/how-to-unstage-large-number-of-files-without-deleting-the-content
[R7]: http://stackoverflow.com/questions/7744049/git-how-to-rebase-to-a-specific-commit
[R8]: https://www.atlassian.com/git/tutorials/merging-vs-rebasing
[R9]: http://stackoverflow.com/questions/24290358/remove-a-folder-from-git-tracking


