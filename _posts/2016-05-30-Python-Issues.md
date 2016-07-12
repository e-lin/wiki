---
layout: post
title:  "Python Issues"
date:   2016-05-30 00:00:00 +0900
categories: jekyll update
type: Python
excerpt_separator: <!--more-->
---
If you encounter...
<!--more-->

[IndentationError: unindent does not match any outer indentation level][R1]
---
Solution: There might be spaces mixed in with your tabs. Try replacing all tabs with a few spaces.


[Recursive - NameError: global name 'xxx' is not defined][R2]
---
You run a recursive function like:
``` python
class Solution(object):
    def addDigits(self, num):
        """
        :type num: int
        :rtype: int
        """
        s = str(num)
        l = list(s)
        sum = 0
        for digit in l:
            sum += int(digit)

        if len(list(str(sum))) == 1:
            return sum
        else:
            return addDigits(sum)
```

If you try a test case of input 19, you will get `NameError: global name 'addDigits' is not defined`.

Solution: change the last line like:

``` python
return self.addDigits(sum)
```

[R1]: http://stackoverflow.com/questions/492387/indentationerror-unindent-does-not-match-any-outer-indentation-level