# Python tips and gotchas

* In python everything is an object
```python
dir(5.2)        #[ '__abs__', '__add__', '__bool__', '__class__', '__delattr__', '__dir__', \
                # '__divmod__', '__doc__', '__eq__', '__float__', '__floordiv__', '__format__',\
                # '__ge__', '__getattribute__', '__getformat__', '__getnewargs__', '__gt__', \
                # '__hash__', '__init__', '__init_subclass__', '__int__', '__le__', '__lt__', \
                # '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__pos__', '__pow__', \
                # '__radd__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', \
                # '__rfloordiv__', '__rmod__', '__rmul__', '__round__', '__rpow__', '__rsub__', \
                # '__rtruediv__', '__setattr__', '__setformat__', '__sizeof__', '__str__', \
                # '__sub__', '__subclasshook__', '__truediv__', '__trunc__', 'as_integer_ratio', \ 
                # 'conjugate', 'fromhex', 'hex', 'imag', 'is_integer', 'real']

5.2.__trunc__()             # 5
5.2.__pow__(3)              # 140.608
(-6).__sub__(5)             # -11
True.__mul__(3).__add__(1)  # 4

```

[David Beazley - Modules and Packages: Live and Let Die!](http://www.dabeaz.com/modulepackage/index.html) [[Presentation Slides](http://www.dabeaz.com/modulepackage/ModulePackage.pdf)] [[Video](https://www.youtube.com/watch?v=0oTh1CXRaQ0)]  

* Explicit relative imports (slide #40)
```python
from . import foo         # Loads ./foo.py (Import from same level)
from .. import foo        # Loads ../foo.py
from .bar import foo      # Loads ./bar/foo.py
from ..grok import foo    # Loads ../grok/foo.py
```
* stitching your submodules together (slide #53)
```python
# __init__.py
from .foo import *
from .bar import *
__all__ = (foo.__all__ + bar.__all__) 
```
* defining an export decorator (slide #56)
```python
def export(defn):
    globals()[defn.__name__] = defn
    __all__.append(defn.__name__)
    return defn
```
* defining a self upgrading package on import (slide #62)
```python
# xml/__init__.py
try:
    import _xmlplus
    import sys
    sys.modules[__name__] = _xmlplus
except ImportError:
    pass
```
* `__main__.py` makes a package / submodule / raw directory executable via `python -m directory` (slide #68)
```python
spam/
     __init__.py
     __main__.py                 # Main program
     foo.py
     bar.py

bash % python3 -m spam           # Run package as main
```
* executable zipfiles (slide # 72)
```python
bash % python3 -m zipfile -c spam.zip spam/*
bash % python3 spam.zip
```
* make a zipfile executable by prepending it with a shebang (slide # 73)
```python
bash % python3 -m zipfile -c spam.zip spam/*
bash % echo -e '#!/usr/bin/env python3\n' > spamapp
bash % cat spam.zip >>spamapp
bash % chmod +x spamapp
bash % ./spamapp
```

* `_` references to the last result in the REPL. `_1, _2, _3, ..., _n` can be used in Jupyter notebooks.
```python
In [1]: 1+2
Out[1]: 3

In [2]: _1
Out[2]: 3
```
* [context managers](https://docs.python.org/3/library/contextlib.html)
  `__enter__` and `__exit__` methods
  [[pysheeet](https://www.pythonsheets.com/notes/python-basic.html#context-manager-with-statement)]
  [[Intermediate Python](http://book.pythontips.com/en/latest/context_managers.html)]
* Descriptors (properties)
[Reddit](https://www.reddit.com/r/Python/comments/1zfee5/python_property_how_and_why/)
[[pysheet](https://www.pythonsheets.com/notes/python-basic.html#property-managed-attributes)]

* If you define a class with a `__len__()` and a `__getitem__()` method, it automatically becomes iterable
```python
class Foo(object):
    def __len__(self):
        return 10
    def __getitem__(self, i):
        if i > len(self):
            raise IndexError
        return i

for i in Foo():
    print(i) 
```
* let it return an `float('inf')` infinite length and you get an infinite iteratable
```python
class TwoEach(object):
    def __len__(self):
        return float('inf')
    def __getitem__(self, x):
        return x // 2
for x in TwoEach():
    print(x)

0
0
1
1
2
2
3
3
4
4
...
```
[[PyTrick](https://github.com/brennerm/PyTricks/blob/master/deck_as_list.py)]  
[`__iter__` - Delegating Iteration](https://www.pythonsheets.com/notes/python-basic.html#iter-delegating-iteration)  
[Emulating a list](https://www.pythonsheets.com/notes/python-basic.html#emulating-a-list)  
[Emulating a dictionary](https://www.pythonsheets.com/notes/python-basic.html#emulating-a-dictionary)  
[Emulating a matrix multiplication](https://www.pythonsheets.com/notes/python-basic.html#emulating-a-matrix-multiplication)  
[Magic methods to emulate](https://www.pythonsheets.com/notes/python-basic.html#common-use-magic)  

* Callable object (`__call__`)
[[pysheet](https://www.pythonsheets.com/notes/python-basic.html#callable-object)]
[[pysheet](https://www.pythonsheets.com/notes/python-basic.html#check-callable)]


* You can use `[~i]` for reverse indexing rather than `[-i-1]` [source](https://www.reddit.com/r/Python/comments/5x374h/you_can_use_i_for_reverse_indexing_rather_than_i1/?st=j549lzkl&sh=43437e53)

* `enumerate` takes a facultative "start" argument
```python
for i, v in enumerate(['a','b','c', 'd'], 1):
    print(i, v)                                 # prints:  1 a  
                                                #          2 b  
                                                #          3 c  
                                                #          4 d  
```



* [30 Python Language Features and Tricks You May Not Know About](http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html)   
* [30 Essential Python Tips and Tricks for Programmers](http://www.techbeamers.com/essential-python-tips-tricks-programmers/) [[Reddit thread](https://www.reddit.com/r/Python/comments/53c3wi/some_cool_python_tips_and_tricks_that_are_worth)] 
* [Buggy Python Code: The 10 Most Common Mistakes That Python Developers Make](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make)  
* [Intermediate Python](http://book.pythontips.com/en/latest/)  
* [PyTricks](https://github.com/brennerm/PyTricks)  
* [Top 10 Python idioms I wish I'd learned earlier](http://prooffreaderplus.blogspot.com.by/2014/11/top-10-python-idioms-i-wished-id.html) [[HN thread](https://news.ycombinator.com/item?id=8665820)]  
* [Python Tips and Traps](https://www.airpair.com/python/posts/python-tips-and-traps)
* [10 awesome features of Python that you can't use because you refuse to upgrade to Python 3](ww.asmeurer.com/python3-presentation/slides.html#1) [[Reddit](https://www.reddit.com/r/Python/comments/69ws4c/10_awesome_features_of_python_that_you_cant_use/?st=j52n4s4e&sh=c913b7bc)]  
* [Transforming code into Beautiful, Idiomatic Python by Raymond Hettinger](https://speakerdeck.com/pyconslides/transforming-code-into-beautiful-idiomatic-python-by-raymond-hettinger-1) [[PyCon talk video](https://youtu.be/OSGv2VnC0go)]  
* [pysheeet - Python cheatsheet!](https://www.pythonsheets.com/) [[GitHub](https://github.com/crazyguitar/pysheeet)]
* [Boso](http://boso.herokuapp.com/python)
  
* [kirang89/pycrumbs](https://github.com/kirang89/pycrumbs)  
* [vinta/awesome-python](https://github.com/vinta/awesome-python)  
* [svaksha/pythonidae](https://github.com/svaksha/pythonidae) 
* [Python 3 Module of the Week](https://pymotw.com/3/)

* [What is the difference between old style and new style classes in Python?](https://stackoverflow.com/questions/54867/what-is-the-difference-between-old-style-and-new-style-classes-in-python)  
* [Hidden features of Python](https://stackoverflow.com/questions/101268/hidden-features-of-python)



## Topics

* Mixins

* Underscores  
`__variable` 
[The Meaning of Underscores in Python](https://dbader.org/blog/meaning-of-underscores-in-python)
[Python's Class Development Toolkit](https://youtu.be/HTLu2DFOdTg?t=33m25s)

* `@classmethod` vs `@staticmethod` [[StackOverflow](https://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python)]
[[Python's Class Development Toolkit](https://youtu.be/HTLu2DFOdTg?t=23m44s)]
* static variables / class variables vs instance variables [[StackOverflow](https://stackoverflow.com/questions/68645/static-class-variables-in-python)]
* Understanding `super()`  [[StackOverflow](https://stackoverflow.com/questions/576169/understanding-python-super-with-init-methods)]  
  * Raymond Hettinger - Super considered super! [[Blog article](https://rhettinger.wordpress.com/2011/05/26/super-considered-super/)] [[PyCon talk](https://youtu.be/EiOglTERPEo)]
* Class multiple inheritance: the Method Resolution Order (MRO)
[[docs](https://www.python.org/download/releases/2.3/mro/)]
[[StackOverflow](https://stackoverflow.com/questions/2010692/what-does-mro-do-in-python)]
* `__mro__` and `mro()`, `inspect.getmro()` methods
```python
>>> class Foo():
...     pass
>>> Foo.__mro__                 # (<class '__main__.Foo'>, <class 'object'>)
>>> Foo.mro()                   # [<class '__main__.Foo'>, <class 'object'>]
>>> type(Foo)                   # <class 'type'>
>>> type(Foo).__mro__           # (<class 'type'>, <class 'object'>)
>>> type(Foo).mro()             # TypeError: descriptor 'mro' of 'type' object needs an argument

# lets create an instance
>>> foo = Foo()                 
>>> foo.__mro__                 # AttributeError: 'Foo' object has no attribute '__mro__'
>>> foo.mro()                   # AttributeError: 'Foo' object has no attribute 'mro'
>>> type(foo)                   # <class '__main__.Foo'>
>>> type(foo).__mro__           # (<class '__main__.Foo'>, <class 'object'>)
>>> type(foo).mro()             # [<class '__main__.Foo'>, <class 'object'>]

# lets try another example
>>> bool.__mro__                # (<class 'bool'>, <class 'int'>, <class 'object'>)
>>> bool.mro()                  # [<class 'bool'>, <class 'int'>, <class 'object'>]
>>> type(bool)                  # <class 'type'>
>>> type(bool).__mro__          # (<class 'type'>, <class 'object'>)
>>> type(bool).mro()            # TypeError: descriptor 'mro' of 'type' object needs an argument

>>> True.__mro__                # AttributeError: 'bool' object has no attribute '__mro__'
>>> True.mro()                  # AttributeError: 'bool' object has no attribute 'mro'
>>> type(True)                  # <class 'bool'>
>>> type(True).__mro__          # (<class 'bool'>, <class 'int'>, <class 'object'>)
>>> type(True).mro()            # [<class 'bool'>, <class 'int'>, <class 'object'>]

>>> import inspect
>>> inspect.getmro(type(True))  # (<class 'bool'>, <class 'int'>, <class 'object'>)

>>> type(bool.__mro__)          # `__mro__` returns a tuple
<class 'tuple'>
>>> type(bool.mro())            # the `mro` method returns a list
<class 'list'>
```

* type [[StackOverflow](https://stackoverflow.com/questions/101268/hidden-features-of-python#108297)] 
[[pysheeet](https://www.pythonsheets.com/notes/python-basic.html#type-declare-create-a-class)]
[[docs](https://docs.python.org/3/library/functions.html#type)]
```python
X = type('X', (object,), dict(a=1))

>>> Cat = type('Cat', (), {"miaow": lambda self: print("Miaow!")})
>>> c = Cat()
>>> c.miaow()
Miaow!
```


* Metaclasses [[StackOverflow](https://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)]  
  [Understanding Python metaclasses](https://blog.ionelmc.ro/2015/02/09/understanding-python-metaclasses/)

* monkey patching  
* duck typing  

* generators
* `yield from`
* `send(value)` to a generator [[StackOverflow](https://stackoverflow.com/questions/101268/hidden-features-of-python#101739)
* lambda
* decorators
* list / dictionary / set comprehensions
* generator expressions [example](http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html#generator-expressions)
* @abstractmethod [[pysheet](https://www.pythonsheets.com/notes/python-basic.html#abstract-method-metaclass)]
```python
from abc import ABCMeta, abstractmethod
class BaseClass(object, metaclass=ABCMeta):
    @abstractmethod
    def absmethod(self):
        """ Abstract method """
        
        
class TestClass(BaseClass):
    pass

t = TestClass()
t.absmethod() # TypeError: Can't instantiate abstract class TestClass with abstract methods absmethod
```
* magic methods / dunder methods  
  [A Guide to Python's Magic Methods](https://rszalski.github.io/magicmethods/)
  [Common Use Magic](https://www.pythonsheets.com/notes/python-basic.html#common-use-magic)
* `__new__` vs `__init__` magic method [[pysheeet](https://www.pythonsheets.com/notes/python-basic.html#new-init)]
* `__repr__` vs `__str__` [[StackOverflow](https://stackoverflow.com/questions/1436703/difference-between-str-and-repr-in-python)] [[pysheeet](https://www.pythonsheets.com/notes/python-basic.html#representations-of-your-class-behave)]
* `__getattr__` vs `__getattribute__` [[StackOverflow](https://stackoverflow.com/questions/3278077/difference-between-getattr-vs-getattribute)]
* `__slots__` 
    * [Saving 9 GB of RAM with Python’s `__slots__`](http://tech.oyster.com/save-ram-with-python-slots/)  
* list of magic names [[StackOverflow](https://stackoverflow.com/a/8920403)] [[docs](https://docs.python.org/3/reference/datamodel.html)] 
* switch statement [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/calculator.py)] [[StackOverflow](https://stackoverflow.com/questions/60208/replacements-for-switch-statement-in-python)]  
* properties [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/cacheproperty.py)]
* dictionary with default values [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/dictdefaultvalue.py)]
```python
d = {}
d.setdefault('a', []).append(1)   # {'a': [1]}

d2 = {}
d2['b'] = d2.get('b', 0) + 1      # {'b': 1}
```
* defaultdict with callable
```python
from collections import defaultdict

d = defaultdict(list)
d['a'].append(1)                # defaultdict(list, {'a': [1]})

d2 = defaultdict(int)
d2['b'] += 1                    # defaultdict(int, {'b': 1})

d3 = defaultdict(lambda: 5)     
d3['c']                         # 5
d3                              # defaultdict(<function __main__.<lambda>>, {'c': 5})
```
https://github.com/brennerm/PyTricks/blob/master/tree.py
https://github.com/brennerm/PyTricks/blob/master/metatable.py
https://github.com/brennerm/PyTricks/blob/master/keydefaultdict.py
* dict sort by value [[PyTricks ](https://github.com/brennerm/PyTricks/blob/master/dictsortbyvalue.py)] 
[[StackOverflow](https://stackoverflow.com/questions/72899/how-do-i-sort-a-list-of-dictionaries-by-values-of-the-dictionary-in-python)]  
* unpacking [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/extendediterableunpacking.py)]
```python
[(c, *d, [*e]), f, *g] = [[1, 2, 3, 4, [5, 5, 5]], 6, 7, 8]
print(c, d, e, f, g)
```
* `for ... else ...` [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/forelse.py)]
* `while ... else ...` [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/whileelse.py]
* `try ... else ...` (`try ... except ... else ... finally ...`) [[docs](https://docs.python.org/3/tutorial/errors.html#defining-clean-up-actions)] [[pysheeet](https://www.pythonsheets.com/notes/python-basic.html#try-exp-else-exp)]  
* loop over overlapping dicts [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/loopoverlappingdicts.py]
* merge dicts [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/merge_dict.py)

https://github.com/brennerm/PyTricks/blob/master/minmaxindex.py  
* set global variables from dict [[PyTricks](https://github.com/brennerm/PyTricks/blob/master/setglobalvariables.py)]
```python
d = {'a': 1, 'b': 'var2', 'c': [1, 2, 3]}
globals().update(d)
```
* functools partial
[[PyTricks](https://github.com/brennerm/PyTricks/blob/master/socketmsghandling.py)]
```python
from functools import partial
blocks = []
for block in iter(partial(f.read, 32), ''):
    blocks.append(block)
```
* `iter(object, sentinel)` with sentinel
[[docs](https://docs.python.org/3/library/functions.html#iter)]
[[pysheeet](https://www.pythonsheets.com/notes/python-basic.html#reading-file-chunk)]
* named tupes [[1](http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html#named-tuples-collections-namedtuple)] [[2](http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html#inheriting-from-named-tuples)]  
* `collections.Counter`
```python
from collections import Counter

counter = Counter([1, 1, 1, 2, 2, 2, 4, 4, 4, 4, 4])

print(counter.most_common(1))       # [(4, 5)]
print(counter.most_common(2))       # [(4, 5), (1, 3)]
print(counter.most_common(3))       # [(4, 5), (1, 3), (2, 3)]
print(counter.most_common(4))       # [(4, 5), (1, 3), (2, 3)] # no exception thrown 
print(counter.most_common(5))       # [(4, 5), (1, 3), (2, 3)] # no exception thrown 
print(len(counter.most_common(6)))  # 3
```

```python
from collections import Counter
from random import randrange

mycounter = Counter(randrange(10) for _ in range(100))

mycounter  # Counter({1: 15, 5: 14, 3: 11, 4: 11, 6: 11, 7: 11, 9: 8, 8: 7, 0: 6, 2: 6})

```
* functions with keyword only arguments
```python
def describe(name, *, size, age):
  print(name, size, age)

describe("me")                    # TypeError: describe() missing 2 required keyword-only arguments: 'size' and 'age'
describe("me", 180, 30)           # TypeError: describe() takes 1 positional argument but 3 were given
describe("me", age=30, size=180)  # me 180 30

```
* `...` Ellipsis operator  
  * Elipsis Slicing [[StackOverflow](https://stackoverflow.com/questions/101268/hidden-features-of-python/112316#112316)] [[StackOverflow](https://stackoverflow.com/questions/118370/how-do-you-use-the-ellipsis-slicing-syntax-in-python)]
  * other uses [[StackOverflow](https://stackoverflow.com/questions/772124/what-does-the-python-ellipsis-object-do/6189281)]  
    ```python
    def do_something():
      ...
    ```
* `NotImplemented` constant [[docs](https://docs.python.org/3/library/constants.html)]  

* iter [[StackOverflow](https://stackoverflow.com/questions/101268/hidden-features-of-python#102202)]
* Multiline regex [[StackOverflow](https://stackoverflow.com/questions/101268/hidden-features-of-python#101537)]
* iterator vs iterable [[StackOverflow](https://stackoverflow.com/questions/9884132/what-exactly-are-pythons-iterator-iterable-and-iteration-protocols)] 
[[StackOverflow](https://stackoverflow.com/questions/2776829/difference-between-pythons-generators-and-iterators)]
[[Quora](https://www.quora.com/What-is-the-difference-between-Python-Iterator-and-Iterable)]
[[excess.org](https://excess.org/article/2013/02/itergen1/)]
* raise Error on `iter(foo) is iter(foo)` for writing functions that can accept iterables but not consumable generators [[PyCon 2015 Talk](https://youtu.be/WjJUPxKB164?t=18m26s)]
* coroutines  
[[Intermediate Python](http://book.pythontips.com/en/latest/coroutines.html)]   
* async
* `@functools.lru_cache` decorator [[docs](https://docs.python.org/3/library/functools.html#functools.lru_cache)]
* get the nth item of a generator [[StackOverflow](https://stackoverflow.com/questions/2300756/get-the-nth-item-of-a-generator-in-python)]
```python
import itertools
nth = 5
next(itertools.islice((_ for _ in "my test"), nth, nth+1)) # returns 's'
```
* create file if file doesn't exist, else raises FileExistsError [[docs](https://docs.python.org/3/library/functions.html#open)]
```python
fh = open("filename", "x") # "x" flag: open for exclusive creation, failing if the file already exists
```
* `bool` is a subclass of `int` and `True`, `False` are instances of `int`
```python
>>> True + True + True
3
>>> (True + True + True) * (True + True)
6
>>> issubclass(bool, int)
True
>>> isinstance(True, int)
True
>>> int(True)
1
>>> int(False)
0
>>> True == 1
True
>>> False == 0
True
```
* list comprehension FizzBuzz
```python
['Fizz'*(not i%3) + 'Buzz'*(not i%5) or i for i in range(1, 100)]
```
* using `or` to assign the first value that evaluates to `True` 
```python
a = None or "" or [] or False or 'Hi there'
print(a)   # Hi there
```
  - using this property of `or`, we  can handle gracefully the gotchas when defining functions with mutable default arguments
```python
def foo(my_list = []):
    my_list.append(1)
    
    return my_list

print(foo())    # [1]
print(foo([2])) # [2, 1]
print(foo([3])) # [3, 1]
print(foo())    # [1, 1]
print(foo())    # [1, 1, 1]
print(foo())    # [1, 1, 1, 1]

def bar(my_list = None):
    my_list = my_list or []
    my_list.append(1)
    
    return my_list

print(bar())    # [1]
print(bar([2])) # [2, 1]
print(bar([3])) # [3, 1]
print(bar())    # [1]
print(bar())    # [1]
print(bar())    # [1]
```
* early vs late binding 
```python
def foo():
    l = []
    for i in range(5):
        l.append(i)
    return l

def bar():
    l = []
    for i in range(5):
        l.append(lambda: i)
    return [f() for f in l]

def baz():
    l = []
    i = 0
    l.append(lambda: i)
    i = 1
    l.append(lambda: i)
    i = 2
    l.append(lambda: i)
    i = 3
    l.append(lambda: i)
    i = 4
    l.append(lambda: i)
    return [f() for f in l]

print(foo()) # [0, 1, 2, 3, 4]
print(bar()) # [4, 4, 4, 4, 4]
print(baz()) # [4, 4, 4, 4, 4]
```
* solution to solve late binding issue
```python
def spam():
    l = []
    for i in range(5):
        l.append(lambda i=i: i)  # pass `i` as a keyword argument to lambda 
    return [f() for f in l] 
    
print(spam()) # [0, 1, 2, 3, 4]
```

* the `__class__` attribute of an instance can be reassigned
```python
class A():
    def func(self):
        print("A")
    
class B():
    def func(self):
        print("B")
    
    def func_two(self):
        print("B2")
 
a = A()
a.func()  # prints A

a.__class__ = B
a.func()      # prints B
a.func_two()  # prints B2
```
* `__class__` can be used as a descriptor
```python
class C: 
    pass

class D:
    @property
    def __class__(self): 
        return C

d = D()
print(type(d))                 # <class '__main__.D'>
print(d.__class__)             # <class '__main__.C'>
print(isinstance(d, D))        # True
print(isinstance(d, C))        # True
print(D.__instancecheck__(d))  # True
print(C.__instancecheck__(d))  # True
```

* the `__code__` attribute of a function can be modified
```python
def foo(x):
    return x*2

foo.__code__ = (lambda x: x*3).__code__
print(foo(5)) # prints 15
```
* using `locals()` to format a string
```python
person = "my sister"
what = "apples"
print("{person} likes {what}".format(**locals()))  # my sister likes apples
print(f"{person} likes {what}")                    # my sister likes apples
```
* return a dictionary containing local variables
```python
def foo():
    a = 1
    b = 2
    c = 3
    return locals()

a = foo()
print(a)  # {'c': 3, 'b': 2, 'a': 1}
type(a)   # <class 'dict'>
```
* `__getitem__` from class and from instance
```python
class Meta(type):
    def __getitem__(self, key):
        print("From class", key)

class Thing(metaclass=Meta):
    def __getitem__(self, key):
        print("From instance", key)

Thing[1] # From class 1
Thing()[1] # From instance 1
```
* 
```python
a = [[0, 0]]*3    # [[0, 0], [0, 0], [0, 0]]
a[0][0] = 5       # [[5, 0], [5, 0], [5, 0]]

a = [[0, 0] for _ in range(3)]   # [[0, 0], [0, 0], [0, 0]]
a[0][0] = 5                      # [[5, 0], [0, 0], [0, 0]]
```
* shallow copy via `[:]`
```python
a = [1, 2, [3]]       
b = a             
a[:] = [4, 5, [6]]   # a == [4, 5, [6]]
                     # b == [4, 5, [6]]

c = a[:]             # shallow copy a
a[0] = 10            # a == [10, 5, [6]]
                     # c == [4, 5, [6]]

a[2].append(7)       # a == [10, 5, [6, 7]]
                     # c == [4, 5, [6, 7]]

a[:] = [7, 8, [9]]   # a == [7, 8, [9]]
                     # c == [4, 5, [6, 7]]
```

* advanced destructuring
```python
(a, (b, c)) = (1, (2, 3))

first, *middles, last = range(5)
```

* automatic string concatenation as a potential source of bugs
```python
>>> "hello" "word"
'helloword'
>>> a = ["foo",
...      "bar",
...      "baz"      # check you didn't miss a comma
...      "spam",
...      "egg"]
>>> a
['foo', 'bar', 'bazspam', 'egg']
```

* chaining comparison `a COMP_OP1 b COMP_OP2 c` is translated to `(a COMP_OP1 b) and (b COMP_OP2 c)`
```python
>>> False is False is False          # syntactic sugar for `(False is False) and (False is False)`
True
>>> False == False in [False]        # syntactic sugar for `(False == False) and (False in [False])`
True
>>> -1 < (3 < 2)                     # translates to `-1 < 0` since `False == 0`             
True

```
* debugging
  *  `import pdb; pdb.set_trace()` in your script where you want to set your breakpoint 
    
  * _or from the command line_:  
    `python -i foo.py` to launch the interactive console, then  
    `import pdb; pdb.pm()` in the console for post mortem debugging 
    
  * _or alternatively_:  
    `python -m pdb foo.py` to launch pdb then `c` to continue until the next exception  

* profiling
    * `python -m cProfile my_script.py`
    
* disambler for Python byte code [[docs](https://docs.python.org/3/library/dis.html)]
   ```python
   import dis
   dis.dis(some_function)
   ```
* `pip install --editable` your own package vs `python setup.py develop` [[Reddit](https://www.reddit.com/r/Python/comments/5f6ev8/what_one_thing_took_your_python_to_the_next_level/dai0glj/)] [[StackOverflow](https://stackoverflow.com/questions/30306099/pip-install-editable-vs-python-setup-py-develop)]
```python
# creates .egg-info directory relative to the project path ; uses wheel
pip install -e .

# creates .egg-info directory relative to the current working directory ; doesn't use wheel
python setup.py develop
```

# Gotchas

* [Amy Hanlon - Investigating Python Wats - PyCon 2015 ](https://www.youtube.com/watch?v=sH4XF6pKKmk)

* Gotcha works only in the interactive console
```python
>>> a = 256
>>> b = 256
>>> a is b
True
>>> a = 257
>>> b = 257
>>> a is b
False
>>> a = 257; b = 257
>>> a is b
True
>>> 

>>> a = -5
>>> b = -5
>>> a is b
True
>>> a = -6
>>> b = -6
>>> a is b
False
>>> a = -6; b = -6
>>> a is b
False
```

* late binding in closures
```python
funcs = [lambda x: x*i for i in range(10)]
print([f(2) for f in funcs])                    #  [18, 18, 18, 18, 18, 18, 18, 18, 18, 18]

funcs = [lambda x, i=i: x*i for i in range(10)]
print([f(2) for f in funcs])                    #  [0, 2, 4, 6, 8, 10, 12, 14, 16, 18] 
```
* this behavior is not specific to lambdas
```python
funcs = []
for i in range(10):
    def foo(x):
        return x*i
    funcs.append(foo)
    
print([f(2) for f in funcs]) # [18, 18, 18, 18, 18, 18, 18, 18, 18, 18]


funcs = []
for i in range(10):
    def foo(x, i=i):
        return x*i
    funcs.append(foo)
    
print([f(2) for f in funcs]) # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```
* `__add__` vs `__iadd__` on lists
```python
a = [1, 2, 3]
b = a
a = a + [4, 5, 6]   # a = a.__add__([4, 5, 6])
print(a)            # [1, 2, 3, 4, 5, 6]
print(b)            # [1, 2, 3]



a = [1, 2, 3]
b = a
a += [4, 5, 6]      # a = a.__iadd__([4, 5, 6])
print(a)            # [1, 2, 3, 4, 5, 6]
print(b)            # [1, 2, 3, 4, 5, 6]
```

* tuples are defined by commas, not by parentheses
```python
a = (1)
b = 1,
c = (1,)
print(type(a))   # <class 'int'>
print(type(b))   # <class 'tuple'>
print(type(c))   # <class 'tuple'>
```
*  functions with default values
```python
def foo(val=[]):
    val.append(1)
    return val

print(foo())      # [1]
print(foo())      # [1, 1]
print(foo())      # [1, 1, 1]
print(foo())      # [1, 1, 1, 1]

def bar(val=None):
    if not val:
        val = []
    val.append(1)
    return val

print(bar())      # [1]
print(bar())      # [1]
print(bar())      # [1]
print(bar())      # [1]
```