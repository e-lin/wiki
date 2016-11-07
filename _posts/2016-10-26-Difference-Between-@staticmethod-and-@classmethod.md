---
layout: post
title:  "Difference Between @staticmethod and @classmethod"
date:   2016-10-26 00:00:00 +0900
categories: jekyll update
type: Python
excerpt_separator: <!--more-->
---
<!--more-->

A Simple Example
---

``` python
class A(object):
    def foo(self, x):
        print "executing foo(%s, %s)" % (self, x)

    @classmethod
    def class_foo(cls, x):
        print "executing class_foo(%s, %s)" % (cls, x)

    @staticmethod
    def static_foo(x):
        print "executing static_foo(%s)" % (x)
```

The usual way an object instance calls a method.

``` python
a = A()
a.foo(1)
```
```
→　executing foo(<__main__.A object at 0x108262b50>, 1)
```

**With classmethods,**

``` python
a.class_foo(2)
A.class_foo(2)
```
```
→　executing class_foo(<class '__main__.A'>, 2)
→　executing class_foo(<class '__main__.A'>, 2)
```

**With staticmethods,**

``` python
a.static_foo(3)
A.static_foo(3)
```
```
→　executing static_foo(3)
→　executing static_foo(3)
```

Function Arguments
---

`foo` is just a function, but when you call `a.foo`, you don't just get the function, you get a "partially applied" version of the function with the object instance `a` bound as the first argument to the function.
`foo` expects 2 arguments, while `a.foo` only expects 1 argument.

``` python
print a.foo
<bound method A.foo of <__main__.A object at 0x108262b50>>
```
→　The class instance `a` is bound to the `foo` method.

``` python
print a.class_foo
<bound method type.class_foo of <class '__main__.A'>>
```
→　The class `A` is bound to the `calss_foo` method.

``` python
print a.static_foo
<function static_foo at 0x108270050>
```
→　just returns a method with no arguments bound


Purpose for What?
---

Some say that:

> Use classmethods to create [inheritable alternative constructors][R4].

> Use staticmethods to group functions which have some logical connection with a class to the class.

While some say that:

> __A staticmethod__ is a method that knows nothing about the class or instance it was called on. It just gets the arguments that were passed, no implicit first argument. It is basically useless in Python -- you can just use a module function instead of a staticmethod.

> __A classmethod__, on the other hand, is a method that gets passed the class it was called on, or the class of the instance it was called on, as first argument. This is useful when you want the method to be a factory for the class: since it gets the actual class it was called on as first argument, you can always instantiate the right class, even when subclasses are involved. Observe for instance how dict.fromkeys(), a classmethod, returns an instance of the subclass when called on a subclass:

``` python
>>> class DictSubclass(dict):
...     def __repr__(self):
...         return "DictSubclass"
...
>>> dict.fromkeys("abc")
{'a': None, 'c': None, 'b': None}
>>> DictSubclass.fromkeys("abc")
DictSubclass
>>>
```


Dish of the Day
---

![Imgur](http://i.imgur.com/rqwvWjm.jpg)
#homemade #甘塩たら #asparagus #bacon #pumpkin #carrot

Reference
---
[What is the difference between @staticmethod and @classmethod in Python?][R1]<br />
[Difference between @staticmethod and @classmethod in Python][R2]<br />
[[翻译]PYTHON中STATICMETHOD和CLASSMETHOD的差异][R3]


[R1]: http://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python
[R2]: http://pythoncentral.io/difference-between-staticmethod-and-classmethod-in-python/
[R3]: http://www.wklken.me/posts/2013/12/22/difference-between-staticmethod-and-classmethod-in-python.html
[R4]: http://stackoverflow.com/a/1950927/190597