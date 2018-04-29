---
layout: post
title:  "Using Git-squash and Merge Your Coworker's Branch into Your Branch"
date:   2017-09-30 00:00:00 +0900
categories: jekyll update
type: Git/Github
excerpt_separator: <!--more-->
---

Using Git-squash and Merge Your Coworker's Branch into Your Branch
---

Let's have an exercise! You will learn:
- How to put multiple commits into one commit (This is called "squash")
- How to merge a branch into the branch you are working on

### Make the branch name show up in the terminal

#### 1. Add

```
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
get_git_user_name() {
    git config user.name
}
export PS1="\u@\h \[\033[32m\]\w\[\033[38;5;215m\] \$(get_git_user_name)\[\033[00m\] -\[\033[33m\]\$(parse_git_branch)\[\033[00m\] "
```
in `~/.bash_profile`.

#### 2. kick command `source ~/.bash_profile`

You see the branch name show up in the terminal.

### Create git-squash test environment like this:

*master*

--> first commit --> add a.txt

*elsa-branch*

--> first commit --> add a.txt --> fix a.txt --> add commit 1 --> add commit 2 --> add commit 3

*john-branch*

--> first commit --> add a.txt --> edit a.txt


### Squash all commits that are modified on *elsa-branch* to only one commit

So your goal is to change

*elsa-branch*

--> first commit --> add a.txt --> (fix a.txt --> add commit 1 --> add commit 2 --> add commit 3)

to this:

*elsa-branch*

--> first commit --> add a.txt --> (one commit)

Type:

```
git rebase -i [commit id of `add a.txt`]
```

You see:
```
pick 9054d31 fix a.txt
pick 6ad48a1 add commit 1
pick ee5b98e add commit 2
pick cc01df1 add commit 3
```

If you change all `pick` to `squash`, like
```
squash 9054d31 fix a.txt
squash 6ad48a1 add commit 1
squash ee5b98e add commit 2
squash cc01df1 add commit 3
```

You get an error `Could not execute editor`. In fact, you should keep the first one, which is the oldest one to be `pick`.
Since squash means base on the previous commit and merger into it. So, you are supposed to modify like:

```
pick 9054d31 fix a.txt
squash 6ad48a1 add commit 1
squash ee5b98e add commit 2
squash cc01df1 add commit 3
```

You see
```
# This is a combination of 4 commits.
# This is the 1st commit message:
fix a.txt

# This is the commit message #2:
commit 1

# This is the commit message #3:
commit 2

# This is the commit message #4:
commit 3
```

So just clear all of it and changes to the commit message you want:

```
just only one commit.
```

then `wq` save and quit.

You see
*elsa-branch* changes to

--> first commit --> add a.txt --> just only one commit

That's it!

### Merge `john-branch` into `elsa-branch`

You now have the git environment like this:

*elsa-branch*

--> first commit --> add a.txt --> just only one commit

*john-branch*

--> first commit --> add a.txt --> edit a.txt

#### 1. Make sure you are on `elsa-branch`, then
```
git merge john-branch
```

You see
```
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
Automatic merge failed; fix conflicts and then commit the result.
```

#### 2. Modify the conflict files right, or to the way you want!

After fixing the conflict, you `git s` and see

```
On branch elsa-branch
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

#### 3. You add the file `git add a.txt`

`git s` and you see

```
On branch elsa-branch
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   a.txt
```

#### 4. You conclude the merge `git commit`

You see
```
Merge branch 'john-branch' into elsa-branch
```

### Reference:
- [Change commit messages of past Git commits][R1]

[R1]: https://makandracards.com/makandra/868-change-commit-messages-of-past-git-commits
