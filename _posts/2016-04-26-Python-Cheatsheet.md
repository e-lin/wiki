---
layout: post
title:  "Python Cheatsheet"
date:   2016-04-26 00:00:00 +0900
categories: jekyll update
type: Python
excerpt_separator: <!--more-->
---
<!--more-->

Built-in Functions
---

zip():

``` python
>>> x = [1, 2, 3]
>>> y = [4, 5, 6]
>>> zipped = zip(x, y)
>>> zipped
[(1, 4), (2, 5), (3, 6)]
>>> x2, y2 = zip(*zipped)
>>> x = list(x2) and y = list(y2)
True
```

Read input from STDIN. Print output to STDOUT

[How do you read from stdin in Python?][R1]<br />
[Python 标准输出 sys.stdout 重定向][R2]


[R1]: https://developers.google.com/edu/python/
[R2]: http://www.cnblogs.com/turtle-fly/p/3280519.html