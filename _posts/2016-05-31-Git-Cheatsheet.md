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
(ongoing)

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

[Create a new branch with git and manage branches][R10]
---
In your github fork, you need to keep your master branch clean, by clean I mean without any changes, like that you can create at any time a branch from your master. Each time, that you want to commit a bug or a feature, you need to create a branch for it, which will be a copy of your master branch.

When you do a pull request on a branch, you can continue to work on another branch and make another pull request on this other branch.

Before creating a new branch, pull the changes from upstream. Your master needs to be up to date.

Create the branch on your local machine and switch in this branch :

```
$ git checkout -b [name_of_your_new_branch]
```

Push the branch on github :

```
$ git push origin [name_of_your_new_branch]
```

When you want to commit something in your branch, be sure to be in your branch.

You can see all branches created by using :

```
$ git branch -a
```

Add a new remote for your branch :

```
$ git remote add [name_of_your_remote]
```

Push changes from your commit into your branch :

```
$ git push [name_of_your_new_remote] [name_of_your_branch]
```

Update your branch when the original branch from official repository has been updated :

```
$ git fetch [name_of_your_remote]
```

Then you need to apply to merge changes, if your branch is derivated from develop you need to do :

```
$ git merge [name_of_your_remote]/develop
```

Delete a branch on your local filesystem :

```
$ git branch -d [name_of_your_new_branch]
```

To force the deletion of local branch on your filesystem :

```
$ git branch -D [name_of_your_new_branch]
```

Delete the branch on github :

```
$ git push origin :[name_of_your_new_branch]
```

The only difference is the : to say delete, you can do it too by using github interface to remove branch : [https://help.github.com/articles/deleting-unused-branches][R10].


[Get changes from master into branch in Git][R13]
---
In my repository I have a branch called `aq` which I'm working on. I then committed new work and bugs in `master`.

What is the best way to get those commits into the `aq` branch? Create another new branch out of `master` and merge it with `aq`?

Solution: Check out the `aq` branch, and rebase from `master`.

```
$ git checkout aq
$ git rebase master
```


Read More
---

- [How to handle conflicts with git][R11]
- [Useful git commands][R12]


[R1]: http://stackoverflow.com/questions/927358/how-do-you-undo-the-last-commit
[R2]: https://help.github.com/articles/syncing-a-fork/
[R3]: https://github.com/Esri/developer-support/wiki/Setting-the-upstream-for-a-fork
[R4]: http://stackoverflow.com/questions/61212/how-do-i-remove-local-untracked-files-from-my-current-git-branch
[R5]: http://stackoverflow.com/questions/52704/how-do-you-discard-unstaged-changes-in-git
[R6]: http://stackoverflow.com/questions/7103631/how-to-unstage-large-number-of-files-without-deleting-the-content
[R7]: http://stackoverflow.com/questions/7744049/git-how-to-rebase-to-a-specific-commit
[R8]: https://www.atlassian.com/git/tutorials/merging-vs-rebasing
[R9]: http://stackoverflow.com/questions/24290358/remove-a-folder-from-git-tracking
[R10]: https://help.github.com/articles/deleting-unused-branches
[R11]: https://github.com/Kunena/Kunena-Forum/wiki/How-to-handle-conflicts-with-git
[R12]: https://github.com/Kunena/Kunena-Forum/wiki/Useful-git-commands
[R13]: http://stackoverflow.com/questions/5340724/get-changes-from-master-into-branch-in-git
