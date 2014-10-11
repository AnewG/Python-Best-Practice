# Python Best Practice

---------

easy_insall & pip like composer in php

### Module

 * Very bad

 ```python
 [...]
 from modu import *
 [...]
 x = sqrt(4)  # Is sqrt part of modu? A builtin? Defined above?
 ```
 
 * Better

 ```python
 from modu import sqrt
 [...]
 x = sqrt(4)  # sqrt may be part of modu, if not redefined in between
 ```
 
 * Best

 ```python
 import modu
 [...]
 x = modu.sqrt(4)  # direct import module , sqrt is visibly part of modu's namespace
 ```

### Decorators

 ```python
 def foo():
   # do something

 def decorator(func):
   # manipulate func
   return func

 foo = decorator(foo)  # Manually decorate

 @decorator
 def bar():
   # Do something
 # bar() is decorated auto
 ```

### Create a concatenated string

 * Bad

 ```python
 # create a concatenated string from 0 to 19 (e.g. "012..1819")
 nums = ""
 for n in range(20):
  nums += str(n)   # slow and inefficient , because string is immutable types
 print nums
 ```
 
 * Good

 ```python
 # create a concatenated string from 0 to 19 (e.g. "012..1819")
 nums = []
 for n in range(20):
  nums.append(str(n))
 print "".join(nums)  # much more efficient
 ```
 
 * Best

 ```python
 # create a concatenated string from 0 to 19 (e.g. "012..1819")
 nums = [str(n) for n in range(20)] # more beauty
 print "".join(nums)
 ```
 
 * Note:
 
 ```python
 foobar = '%s%s' % (foo, bar) # It is OK
 foobar = '{0}{1}'.format(foo, bar) # It is better
 foobar = '{foo}{bar}'.format(foo=foo, bar=bar) # It is best
 ```
 
### Explicit code

 * Bad

 ```python
 def make_complex(*args):
   x, y = args
   return dict(**locals()) # local variable
 ```
 
 * Good

 ```python
 def make_complex(x, y):
   return {'x': x, 'y': y}
 ```
 
### Unpacking

 ```python
 for index, item in enumerate(some_list):
   # do something with index and item
 ```
 
 * In Py3
 
 ```python
 a, *rest = [1, 2, 3]
 # a = 1, rest = [2, 3]
 a, *middle, c = [1, 2, 3, 4]
 # a = 1, middle = [2, 3], c = 4
 ```
 
 * Create an ignored variable
 
 ```python
 filename = 'foobar.txt'
 basename, __, ext = filename.rpartition('.')
 ```
