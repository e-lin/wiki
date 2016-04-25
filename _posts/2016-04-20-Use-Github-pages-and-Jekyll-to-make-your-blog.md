---
layout: post
title:  "Use Github-pages and Jekyll to make your blog"
date:   2016-04-20 18:55:00 +0900
categories: jekyll update
type: Git/Github
excerpt_separator: <!--more-->
---
The advantage of using Jekyll is that:

- use markdown syntax to release a blog post
- render your blog at local side
- git control
<!--more-->

Steps
---
* Follow the instruction in [Github Pages][Github-pages], select to create `Project site` -> `Start from scratch`. Do not choose `Generate a site`, because it will offer you a web template and generate some web-related files that you do not need in Jekyll.

* Install Jekyll

```
$ gem install jekyll
```

* cd to your blog folder and initialize Jekyll.

``` sh
$ cd myblog
$ jekyll new . --force
$ jekyll serve --detach
# => Now browse to http://localhost:4000
```

* config the `_config.yml` file to fit your setting. For example, just add `port: 8080` in the _config.yml, then it changes http://localhost:4000 to http://localhost:8080. See more configuration at [Jekyll docs][Jekyll-docs]

* command `jekyll build` whenever you do modification.

Notes
---
There are some inconvenient facts that [Markdown is not rendering line spacing/block quotes properly with Jekyll][Issue]. When you do want to insert a break tag using Markdown, you end a line with two or more spaces, then type return. Which, sadly, is a part of Markdown that Redcarpet doesn't conform to. The workaround I use is adding `<br /> ` where you want to break a single line.

After doing this, I found there is another way to make my static website. Since I am much familier with python, I was wondering if I should change to use Pelican instead of Jekyll. Anyway, I will let this to be a futher try.

[Personal website powered by Bootstrap, Pelican, and GitHub Pages][R4]<br />
[Setting up a blog with Pelican and GitHub Pages][R5]


Reference
---
[使用 GitHub Pages 和 Jekyll 來建立 Blog][R1]<br />
[Using Jekyll as a static site generator with GitHub Pages][R2]<br />
[how to configure website menu in jekyll][R3]


[Github-pages]: https://pages.github.com/
[Jekyll-docs]: https://jekyllrb.com/docs/configuration/
[Issue]: https://github.com/jekyll/jekyll/issues/1368
[R1]: http://xareelee.github.io/development_notes/%E4%BD%BF%E7%94%A8-GitHub-Pages-%E5%92%8C-Jekyll-%E4%BE%86%E5%BB%BA%E7%AB%8B-Blog/
[R2]: https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/
[R3]: http://stackoverflow.com/questions/36011266/how-to-configure-website-menu-in-jekyll
[R4]: https://github.com/donnemartin/donnemartin.github.io
[R5]: http://cyrille.rossant.net/pelican-github/