## Python Best Practice


### global

```python
# UTF-8 Encoding:
# -*- coding: utf-8 -*-
# utf8 is one implement of unicode
# str1.decode('gb2312') str1(gb2312) to unicode(commmon)
# str2.encode('gb2312') str2(unicode(common)) to gb2312

# key word:

# and       del       from      not       while
# as        elif      global    or        with
# assert    else      if        pass      yield
# break     except    import    print
# class     exec      in        raise
# continue  finally   is        return
# def       for       lambda    try

# _* :
# useless for `from module import *`

# __*__ :
# System use

# __* :
# Class private variable

# new-style class A(object): Breadth-First-Search
# old-style class A: Depth-First-Search
```

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

### Check if variable equals a constant

 * Bad
 
 ```python
 if attr == True:
   print 'True!'

 if attr == None:
   print 'attr is None!'
 ```
 * Good
 
 ```python
 # Just check the value
 if attr:
   print 'attr is truthy!'

 # or check for the opposite
 if not attr:
   print 'attr is falsey!'

 # or, since None is considered false, explicitly check for it
 if attr is None:
   print 'attr is None!'
 ```

### Access a Dictionary Element

 * Bad
 
 ```python
 d = {'hello': 'world'}
 if d.has_key('hello'):
   print d['hello']    # prints 'world'
 else:
   print 'default_value'
 ```
 
 * Good
 
 ```python
 d = {'hello': 'world'}

 print d.get('hello', 'default_value') # prints 'world'
 print d.get('thingy', 'default_value') # prints 'default_value'

 # Or:
 if 'hello' in d:
   print d['hello']
 ```

### Short Ways to Manipulate Lists

 * Bad
 
 ```python
 # Filter elements greater than 4
 a = [3, 4, 5]
 b = []
 for i in a:
   if i > 4:
     b.append(i)
 ```
 
 * Good
 
 ```python
 a = [3, 4, 5]
 b = [i for i in a if i > 4]
 # Or:
 b = filter(lambda x: x > 4, a)
 # (lambda x: x*2)(3) 参数:语句(调用参数)
 ```

 * Bad
 
 ```python
 # Add three to all list members.
 a = [3, 4, 5]
 for i in range(len(a)):
   a[i] += 3
 ```
 
 * Good
 
 ```python
 a = [3, 4, 5]
 a = [i + 3 for i in a]
 # Or:
 a = map(lambda i: i + 3, a)
 ```
 
 ```python
 questions = ['name', 'quest', 'favorite color']
 answers = ['lancelot', 'the holy grail', 'blue']
 for q, a in zip(questions, answers):
  print 'What is your {0}?  It is {1}.'.format(q, a)

 # What is your name?  It is lancelot.
 # What is your quest?  It is the holy grail.
 # What is your favorite color?  It is blue.
 ```
 
 
### Read From a File

 * Bad
 
 ```python
 f = open('file.txt')
 a = f.read()
 print a
 f.close()
 ```
 
 * Good
 
 ```python
 with open('file.txt') as f:
   for line in f:
     print line
 ```
 
### Line Continuations

 * Bad
 
 ```python
 my_very_big_string = """For a long time I used to go to bed early. Sometimes, \
   when I had put out my candle, my eyes would close so quickly that I had not even \
   time to say “I’m going to sleep.”"""

 from some.deep.module.inside.a.module import a_nice_function, another_nice_function, \
   yet_another_nice_function
 ```
 
 * Good
 
 ```python
 my_very_big_string = (
   "For a long time I used to go to bed early. Sometimes, "
   "when I had put out my candle, my eyes would close so quickly "
   "that I had not even time to say “I’m going to sleep.”"
 )

 from some.deep.module.inside.a.module import (
   a_nice_function, another_nice_function, yet_another_nice_function)
 ```

---

## Common Gotchas

What You Wrote

```python
def append_to(element, to=[]):
  to.append(element)
  return to
```

What You Might Have Expected to Happen

```python
my_list = append_to(12)
print my_list # [12]

my_other_list = append_to(42)
print my_other_list # [42]
```

What Does Happen

```python
# [12]
# [12,42]
```

Because:Python’s default arguments are evaluated once when the function is defined, not each time the function is called (like it is in say, Ruby). This means that if you use a mutable default argument and mutate it, you will and have mutated that object for all future calls to the function as well.

What You Should Do Instead

```python
def append_to(element, to=None):
  if to is None:
    to = []
  to.append(element)
  return to
```

What You Wrote

```python
def create_multipliers():
  return [lambda x : i * x for i in range(5)]
```

What You Might Have Expected to Happen

```python
for multiplier in create_multipliers():
  print multiplier(2)
# 0 2 4 6 8
```

What Does Happen

```python
# 8 8 8 8 8
```

Because Python’s closures are late binding. This means that the values of variables used in closures are looked up at the time the inner function is called.Here, whenever any of the returned functions are called, the value of i is looked up in the surrounding scope at call time

What You Should Do Instead

```python
def create_multipliers():
  return [lambda x, i=i : i * x for i in range(5)]
    
# Or

from functools import partial
from operator import mul

def create_multipliers():
    return [partial(mul, i) for i in range(5)]
```

pip install pep8

pip install requests

pip install Pillow

[Environments](http://docs.python-guide.org/en/latest/dev/virtualenvs/)

http://docs.python-guide.org/en/latest/
