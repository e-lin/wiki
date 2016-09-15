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

### zip():

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

### Read input from STDIN. Print output to STDOUT
(ongoing)

### [Accessing the index in Python for loops][R3]

``` pyton
for idx, val in enumerate(ints):
    print(idx, val)
```

### [Two-dimensional array in python][R4]

``` python
# Creates a list containing 5 lists, each of 8 items, all set to 0
lists, items = 5, 8
M = [[0 for y in range(items)] for x in range(lists)]
M[4][7] = 9
```


Reference
---
[How do you read from stdin in Python?][R1]<br />
[Python 标准输出 sys.stdout 重定向][R2]


[R1]: https://developers.google.com/edu/python/
[R2]: http://www.cnblogs.com/turtle-fly/p/3280519.html
[R3]: http://stackoverflow.com/questions/522563/accessing-the-index-in-python-for-loops
[R4]: http://stackoverflow.com/questions/6667201/how-to-define-two-dimensional-array-in-python
