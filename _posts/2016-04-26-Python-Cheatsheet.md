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


### [Comparing strings either '==' or 'is' sometimes produce a different result?][R5]

``` python
>>> a = 'pub'
>>> b = ''.join(['p', 'u', 'b'])
>>> a == b
True
>>> a is b
False
```
`is` is identity testing, `==` is equality testing.

In other words: `is` is the `id(a) == id(b)`


### [Sorting list based on the length of the string][R6]

``` python
xs.sort(key = len)
```
You can also use the built-in [sorted][R9] function rather than the `list.sort` method, which creates a new list rather than sorting the existing one in-place:

``` python
print sorted(xs, key=len)
```


### [Finding the index of an item in a list][R7]

``` python
>>> ["foo", "bar", "baz"].index("bar")
1
```


### [ASCII value of a character][R8]

``` python
>>> ord('a')
97
>>> chr(97)
'a'
>>> chr(ord('a') + 3)
'd'
>>>
```


### [Scope of variable in for loop][R10]

The for loop iterates over all the numbers in range(10), that is, [0,1,2,3,4,5,6,7,8,9].
That you change the current value of i has no effect on the next value in the range.

You can change the value of `i` with a while loop.

``` python
i = 0
while i < 10:
    # do stuff and manipulate `i` as much as you like
    if i==5:
        i+=3

    print i

    # don't forget to increment `i` manually
    i += 1
```


### [trim whitespace (including tabs)][R11]

Whitespace on the both sides:

``` python
s = "  \t a string example\t  "
s = s.strip()
s = s.rstrip() # right side
s = s.lstrip() # left side

You can provide an argument to strip arbitrary characters to any of these functions like this:

``` python
s = s.strip(' \t\n\r')
```
This will strip any space, `\t`, `\n`, or `\r` characters from the left-hand side, right-hand side, or both sides of the string.

If you want to also remove characters from the middle of a string, try `re.sub`:

``` python
import re
print re.sub('[\s+]', '', s)
```
That should print out:

```
astringexample
```


### [remove all whitespace in a string][R12]

If you want to remove leading and ending spaces, use `str.strip()`:

``` python
sentence = ' hello  apple'
sentence.strip()
>>> 'hello  apple'
```

If you want to remove all spaces, use `str.replace()`:

``` python
sentence = ' hello  apple'
sentence.replace(" ", "")
>>> 'helloapple'
```

If you want to remove duplicated spaces, use `str.split()`:

``` python
sentence = ' hello  apple'
" ".join(sentence.split())
>>> 'hello apple'
```

### [convert list to string][R13]
By using `''.join`

``` python
list1 = ['1', '2', '3']
str1 = ''.join(list1)
```
Or if the list is of integers, convert the elements before joining them.

``` python
list1 = [1, 2, 3]
str1 = ''.join(str(e) for e in list1)
```


### [adding binary numbers][R14]

`bin` and `int` are very useful here:

``` python
a = '001'
b = '011'

c = bin(int(a,2) + int(b,2))
# 0b100
```
`int` allows you to specify what base the first argument is in when converting from a string (in this case two), and `bin` converts a number back to a binary string.

Or definde a function:
```
def bin_add(*args): return bin(sum(int(x, 2) for x in args))[2:]
```

``` python
>>> bin_add('1', '10', '100')
'111'
```


Reference
---
[How do you read from stdin in Python?][R1]<br />
[Python 标准输出 sys.stdout 重定向][R2]


[R1]: https://developers.google.com/edu/python/
[R2]: http://www.cnblogs.com/turtle-fly/p/3280519.html
[R3]: http://stackoverflow.com/questions/522563/accessing-the-index-in-python-for-loops
[R4]: http://stackoverflow.com/questions/6667201/how-to-define-two-dimensional-array-in-python
[R5]: http://stackoverflow.com/questions/1504717/why-does-comparing-strings-in-python-using-either-or-is-sometimes-produce
[R6]: http://stackoverflow.com/questions/2587402/sorting-python-list-based-on-the-length-of-the-string
[R7]: http://stackoverflow.com/questions/176918/finding-the-index-of-an-item-given-a-list-containing-it-in-python
[R8]: http://stackoverflow.com/questions/227459/ascii-value-of-a-character-in-python
[R9]: http://docs.python.org/library/functions.html#sorted
[R10]: http://stackoverflow.com/questions/15363138/scope-of-python-variable-in-for-loop
[R11]: http://stackoverflow.com/questions/1185524/how-to-trim-whitespace-including-tabs
[R12]: http://stackoverflow.com/questions/8270092/python-remove-all-whitespace-in-a-string
[R13]: http://stackoverflow.com/questions/5618878/how-to-convert-list-to-string
[R14]: http://stackoverflow.com/questions/21420447/need-help-in-adding-binary-numbers-in-python
