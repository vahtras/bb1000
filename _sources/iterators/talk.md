# Iterators

BB1000 Programming in Python

---

layout: false

## Iterables, iterators and generators

## Examples

* objects that can be used in for loops


### A list

~~~python
>>> li = [0, 1, 2]
>>> for i in li:
...     print(i, end=" ")
0 1 2 

~~~


### Dictionary

* The loop variable is the key of the key-value pair

```python
>>> dict = {'a':1, 'b':2}
>>> for k in dict:
...     print(k, dict[k])
a 1
b 2

```
---

### String

```
>>> str = 'abc'
>>> for c in str:
...     print(c)
a
b
c

```

### File objects


<!--
>>> import subprocess
>>> n = subprocess.call("/bin/echo 'one\ntwo\nthree' > 123", shell=True)

-->

    #123.txt
    one
    two
    three

```
>>> for row in open('123.txt'):
...     print(row, end="")
one
two
three

```
---

## Iterable vs iterator


```
>>> li = [0, 1, 2]
>>> print(li)
[0, 1, 2]

```

If you can call `iter` with an object that object it is *iterable*

```
>>> li_iter = iter(li)
>>> type(li_iter)
<class 'list_iterator'>

```

`iter` will return an object that supports `next` - an *iterator*


```
>>> next(li_iter)
0
>>> next(li_iter)
1
>>> next(li_iter)
2
>>> next(li_iter)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration

```

Illustrates what happens behind the scenes in a for loop

---

### Objects supporting iteration

What does iter vs next (built-in function) do? Roughly it delegates

~~~
#What the builtin iter roughly does
def iter(object):
    return object.__iter__()
~~~

~~~
#What the builtin next roughly does
def next(object):
    return object.__next__()
~~~

* iterables are defined by classes with an `__iter__` method, which which
  should return an iterator
* iterators are defined by classes with a `__next__` method, producing the
  next value of the sequence
* the for loop will stop when a `StopIteration` exception is encountered

---

### Defining your own iterator

~~~python
class Counter:
    def __init__(self, size):
        print("__init__:", size)
        self.size = size
        self.start = 0

    def __iter__(self):
        print("__iter__:", self.size)
        return CounterIter(self.start, self.size)

class CounterIter:

    def __init__(self, start, size):
        self.start = start
        self.size = size

    def __next__(self):
        if self.start < self.size:
            self.start = self.start + 1
            return self.start
        raise StopIteration
~~~
```
>>> c = Counter(3)
__init__: 3
>>> for num in c:
...     print(num, end=" ")
__iter__: 3
1 2 3 
```

---

## Generators


* Functions that contain the  yield statment
* Support iteration protocol - they return an iterator
* Resumable
* Generators are not executed when invoked, they are iterated over

### function vs. generator

```
>>> def f(n):
...    return n
>>> type(f)
<class 'function'>
>>> type(f(1))
<class 'int'>

```
```
>>> def g(n):
...    yield n
>>> type(g)
<class 'function'>
>>> type(g(1))
<class 'generator'>

```

---

### Example

```
>>> def g(n):
...     print('enter g with',n)
...     yield n
...     print('after yield')

```
```
>>> g2=g(2)
>>> next(g2)
enter g with 2
2
>>> next(g2)
after yield
Traceback (most recent call last):
...
StopIteration

```

* So: this function appears to pause at the yield statement after returning the value and continue from there the next time the next() method is called...

* When the function exits a StopIteration exception is raised

---

### Example

```
>>> def g(n):
...     print('enter g with ',n)
...     i=0
...     while i < n:
...         yield i
...         print('after yield')
...         i += 1
...     print('after while')

```

```
>>> g2=g(2)
>>> next(g2)
enter g with  2
0
>>> next(g2)
after yield
1
>>> g2.next()
after yield
after while
Traceback (most recent call last):
  File "gen.py", line 42, in <module>
    print(g2.next())
StopIteration
```

---

### Iterator in for loop

```
>>> for  i in g(5):
...     print(i, end=" ")
enter g with  5
0 after yield
1 after yield
2 after yield
3 after yield
4 after yield
after while

```

### Example: fibonacci


```
>>> def fib(n):
...     a = 1
...     b = 2
...     while a < n:
...         yield a
...         a, b = b, a + b
>>> for i in fib(5):
...     print(i, end=" ")
1 2 3 

```

---

### Convert to list

It is always possible to convert a generator to a list


```
>>> list(fib(100))
[1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

```

---

## Summary

* Several common types support iteration (list, dict, file, str)
* Objects that support iteration have an `__iter__` method returning an
  iterator
* The iterators have a `[__next__](__next__)` method that steps through some sequence
* Generators are functions with a `yield` statement and work like iterators
