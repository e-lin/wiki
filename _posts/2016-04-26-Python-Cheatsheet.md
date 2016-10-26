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


### [Accessing the index for loops][R3]

``` pyton
for idx, val in enumerate(ints):
    print(idx, val)
```


### [Two-dimensional array][R4]

``` python
# Creates a list containing 5 lists, each of 8 items, all set to 0
lists, items = 5, 8
M = [[0 for y in range(items)] for x in range(lists)]
M[4][7] = 9
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


### [Add binary numbers][R14]

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


### [if A vs if A is not None:][R20]

``` python
if A:
```
will call either `A.__nonzero__()` or `A.__len__()`. If a class defines neither __len__() nor __nonzero__(), all its instances are considered true.

``` python
if A is not None:
```
compares _only_ the reference `A` with `None` to see whether it is the same or not.

#### _Further Reading: [Python `if x is not None` or `if not x is None`?][R21]_

There's no performance difference, as they compile to the same bytecode:

``` python
Python 2.6.2 (r262:71600, Apr 15 2009, 07:20:39)
>>> import dis
>>> def f(x):
...    return x is not None
...
>>> dis.dis(f)
  2           0 LOAD_FAST                0 (x)
              3 LOAD_CONST               0 (None)
              6 COMPARE_OP               9 (is not)
              9 RETURN_VALUE
>>> def g(x):
...   return not x is None
...
>>> dis.dis(g)
  2           0 LOAD_FAST                0 (x)
              3 LOAD_CONST               0 (None)
              6 COMPARE_OP               9 (is not)
              9 RETURN_VALUE
```

Stylistically, I try to avoid `not x is y`. Although the compiler will always treat it as `not (x is y)`, a human reader might misunderstand the construct as `(not x) is y`. If I write `x is not y` then there is no ambiguity.


### [Reset global variable][R21]

If you don't need the GLOBAL_VARIABLE anymore you could use:

``` python
del GLOBAL_VARIABLE
```

If you want a empty list:

``` python
del GLOBAL_VARIABLE[:]
```


String
---

### [Comparing strings either '==' or 'is' sometimes produce a different result?][R5]

``` python
>>> a = 'pub'
>>> b = ''.join(['p', 'u', 'b'])
>>> a == b
True
>>> a is b
False
```
Since a and b has a different address in memory.

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


### [Trim whitespace (including tabs)][R11]

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


### [Remove all whitespace in a string][R12]

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

### [Convert list to string][R13]
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


### [Delete a character from a string][R19]

In Python, strings are immutable, so you have to create a new string. You have a few options of how to create the new string. If you want to remove the 'M' wherever it appears:

```
newstr = oldstr.replace("M", "")
```


Dictionary
---

### [Iterate dictionary by index][R15]

You can iterate over keys and get values by keys:

``` python
for key in dict.iterkeys():
    print key, dict[key]
```

You can iterate over keys and corresponding values:

``` python
for key, value in dict.iteritems():
    print key, value
```

You can use enumerate if you want indexes (remember that dictionaries don't have an order):

``` python
>>> for index, key in enumerate(dict):
...     print index, key
...
0 orange
1 mango
2 apple
>>>
```


### [Index a dictionary][R16]

Dictionaries are unordered in Python. If you do not care about the order of the entries and want to access the keys or values by index anyway, you can use `d.keys()[i]` and `d.values()[i]` or `d.items()[i]`. (Note that these methods create a list of all keys, values or items, respectively. So if you need them more then once, store the list in a variable to improve performance.)

``` python
>>> dict
{'blue': 5, 'yellow': 2, 'red': 3}
>>> dict.keys()[0]
'blue'
>>> dict.values()[2]
3
>>> dict.items()[1]
('yellow', 2)
```

If you do care about the order of the entries, starting with Python 2.7 you can use [collections.orderdDict][R17]. Or use a list of pairs

``` python
l = [("blue", "5"), ("red", "6"), ("yellow", "8")]
```


### [Filter a dictionary][R18]

``` python
d = dict((k, v) for k, v in d.iteritems() if v > 0)
```

In Python 2.7 and up, there's nicer syntax for this:

``` python
d = {k: v for k, v in d.items() if v > 0}
```
Note that this is not strictly a filter because it does create a new dictionary.



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
[R15]: http://stackoverflow.com/questions/17793364/python-iterate-dictionary-by-index
[R16]: http://stackoverflow.com/questions/4326658/python-index-a-dictionary
[R17]: http://docs.python.org/library/collections.html#collections.OrderedDict
[R18]: http://stackoverflow.com/questions/8425046/the-best-way-to-filter-a-dictionary-in-python
[R19]: http://stackoverflow.com/questions/3559559/how-to-delete-a-character-from-a-string-using-python
[R20]: http://stackoverflow.com/questions/7816363/if-a-vs-if-a-is-not-none
[R21]: http://stackoverflow.com/questions/3657163/how-to-reset-global-variable-in-python
