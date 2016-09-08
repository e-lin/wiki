---
layout: post
title:  "Avoid Bugs by Automatically Running Your Tests"
date:   2016-09-06 00:00:00 +0900
categories: jekyll update
type: Git/Github
excerpt_separator: <!--more-->
---
<!--more-->

You Get a Message from GitHub...
---
>Continuous integration can help catch bugs by running your tests automatically.
Merge your code with confidence using one of our continuous integration providers.


[Travis CI][R1]
---
Travis CI is a automatic test and deploy tool. Travis CI helps make sure your code runs against all versions of your favorite language (supporting over 20 different languages) without breaking a sweat, giving your team the best tool to build all of their applications with confidence.

Supports your workflow like:
![Imgur](http://i.imgur.com/wIyzKDu.png)


[Getting started with Travid CI][R2]
---
#1. On GitHub, fork the [example PHP repository][R4].

#2. Sign in to [Travis CI][R5] with your GitHub account, accepting the GitHub access permissions confirmation.

#3. Once you’re signed in, and we’ve synchronized your repositories from GitHub, go to your profile page and enable Travis CI builds for your fork of the `travis-broken-example` repository.

![Imgur](http://i.imgur.com/q3BR9oc.png)

#4.Take a look at `.travis.yml`, the file which tells Travis CI what to do:

```
language: php
php:
- 5.5
- 5.4
- hhvm
script: phpunit Test.php
```
This file tells Travis CI that this project is written in PHP, and to test `Test.php` with phpunit against PHP versions 5.5, 5.4 and HHVM.

#5. Edit the empty `NewUser.txt` file by adding your name to the empty file. Add the file to git, commit and push, to trigger a Travis CI build:

```
$ git add -A
$ git commit -m 'Testing Travis CI'
$ git push
```
Wait for Travis CI to run a build on your fork of the `travis-broken-example` repository, check the build status and notice that the build fails. (Travis CI sends you an email when this happens)

#6. Fix the code by making sure that 2=1+1 in Test.php, commit and push to GitHub. This time, the build does not fail.

```
$ git add -A
$ git commit -m 'Testing Travis CI: fixing the build'
$ git push
```
Congratulations, you have added a GitHub repository to Travis and learnt the basics of configuring builds and testing code.


Problems I Met
---
If you try to `git commit --amend` to amend last commit, and try to `git push --force` to let your forked repository be forced updated. You will see the Travis CI build error under your forked repository like this:
![Imgur](http://i.imgur.com/dIkGeTm.png)

The commit of `Testing Travis CI: fixing the build` should fix the build error, and it is not expected to see the build has errors.

Job logs says that:
![Imgur](http://i.imgur.com/EoZQueQ.png)

Since you force push the branch that the Travis CI is not able to reference the last commit.

I do not know how to fix this, however, this would not be a problem with pushing a pull request (PR) to the primordial repository (the repository that you have forked). Seeing like this:
![Imgur](http://i.imgur.com/ngV6Z1l.png)


[Slack Notifications][R6]
---
On Slack, set up a [new Travis CI integration][R7]. Select a channel, and you’ll find the details to paste into your `.travis.yml`.

Just copy and paste the settings, which already include the proper token, into your `.travis.yml`, and you’re good to go.

![Imgur](http://i.imgur.com/y35G2ID.png)


Dish of the day
---
![Imgur](http://i.imgur.com/K4zdqWq.jpg)
#homemade


Reference
---
[Travis CI][R1]<br />
[Travis CI Getting started][R2]<br />
[Travis CI for Complete Beginners][R3]

[R1]: https://github.com/integrations/travis-ci
[R2]: https://docs.travis-ci.com/user/getting-started/
[R3]: https://docs.travis-ci.com/user/for-beginners
[R4]: https://github.com/plaindocs/travis-broken-example
[R5]: https://travis-ci.org/
[R6]: https://docs.travis-ci.com/user/notifications/#Slack-notifications
[R7]: https://my.slack.com/services/new/travis
