<style>
.centered {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
<link rel="stylesheet" href="/js/highlight/styles/gruvbox-light.css">
<script src="/js/highlight/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

# Debugging

BB1000 Programming in Python
KTH

---

layout: false

## Learning outcomes

* know how to use assertions
* know how to use exceptions
* know how to use logging
* know how to use a debugger

---

## About bugs

### Best approch to avoiding

- TDD
- Version control

### How to find bugs

- insert print statements
- use other builtin tools

---

## Assertions

```
>>> assert True
>>> assert False
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError

```
* evaluates an expression
* if it is true-ish continue
* if not stop or handle error

In tests:
* a fundamental component in testing frameworks

In code:
* a sanity check
* *I assert that this condition is true, otherwise there is a bug somewhere*
* fail fast and recover
* catch programming errors

---

```
#get_number.py
def get_number(n):
    assert type(n) == int, 'Argument should be an integer'
    print("Number is %d" % n)

get_number("hello")
```

* Disable with -O

```
> python -O program.py
```

---

##Exceptions

* exceptions are errors: the program crashes
* unless you handle them in some way and continue running

```
>>> 1/0                                                                          #doctest: +SKIP
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: integer division or modulo by zero

```
* Errors can be handled with `try...except...  `
* to detect and handle errors you can aniticipate
* potential problems are put in a `try` clause
* if an error happends the code skips to the following except clause

```
>>> try:
...     1/0
... except ZeroDivisionError:
...     print("Warning:Zero division, continuing")
Warning:Zero division, continuing
>>>


```

---

Parents' approach (ask permission first)

<!--
```
>>> n = 0

```

-->

```
>>> if n != 0:
...     print(1/n)
... else:
...     print('Warning...')
Warning...

```

Childs' approach (ask forgiveness after)
```
>>> try:
...     print(1/n)
... except ZeroDivisionError:
...     print('Warning...')
Warning...

```

*Easier to ask for forgiveness than for permission*

---
###Example

Script which emulates the Linux `cat` command:
* take filename argument
* opens and reads file
* prints contents to stdout

```
#cat.py
import sys
try:
    filename = sys.argv[1]
    lines = open(filename).read()
except IndexError:
    print("Usage: %s filename" % sys.argv[0])
    sys.exit()
except FileNotFoundError:
    print("%s: no such file, %s" % (sys.argv[0], filename))
    sys.exit()

print(lines)
```

```bash
python cat.py
Usage: %s filename
python cat.py nofile
cat.py: no such file, nofile
python cat.py somefile
<contents of somefile>
```

---

In programs

* an error condition is indicated by raising an exception
```
raise Exception('something is wrong here')
```
* exceptions can be created with the Exception() call
    - creates an instance of the Exception class
    - 
* many standard exceptions are subclasses to the Exception class
    - ZeroDivisionError (division by zero)
    - FileNotFoundError (file not found)
    - https://docs.python.org/3/library/exceptions.html

---

Define your own exceptions

<!--
```
>>> from zoo import ZooCatalog, Bird, Reptile

```
-->

```
class NoBirdError(Exception):
   pass
```
```
class ZooCatalog:
    ...
    def add_bird(self, new_bird):
        if type(new_bird) != Bird:
            raise NoBirdError('this is not a bird')
```

executing

```
>>> zoo = ZooCatalog()
>>> zoo.add_bird(Reptile())                                                      #doctest: +IGNORE_EXCEPTION_DETAIL
Traceback (most recent call last):
	...
    raise NoBirdError('this not a bird')
__main__.NoBirdError: this not a bird

```

testing

```
def test_nobird():
    zoo = ZooCatalog()
    with pytest.raises(NoBirdError):
        zoo.add_bird(Reptile())
```


---

## Logging

* a better type of print


```
>>> import logging
>>> logging.basicConfig(level=logging.DEBUG, format="%(message)")
>>> logging.debug('Hello world')                                                 #doctest: +SKIP
Hello world

```

* easy to switch between hiding/showing log messages
* intended for programmer not user
* catagorize your log messages by importance

* tracking more info

```
>>> log_format = ' %(asctime)s %(levelname)s: <%(message)s>'
>>> logging.basicConfig(level=logging.DEBUG, format=log_format)
>>> logging.debug('Hello world')
 2017-05-09 13:42:55,142 DEBUG: <Hello world>
```


---

Levels | function  |When
:------ | :--- | :---
DEBUG  | `logging.debug()` | Diagnosing
INFO   | `logging.info()` | Confirming
WARNING   | `logging.warning()` |Potential problem
ERROR | `logging.error()` | Program failure
CRITICAL | `logging.critical()` |Fatal error


```
logging.debug('Diagnosic output')
logging.info('Starting ok')
logging.error('Som error has occured')
logging.critical('Unable to recover')
```

```
 2017-05-09 14:17:26,822 DEBUG: <Diagnosic output>
 2017-05-09 14:17:26,822 INFO: <Starting ok>
 2017-05-09 14:17:26,822 ERROR: <Some error has occured>
 2017-05-09 14:17:26,822 CRITICAL: <Unable to recover>
```

* disable all logging

```
logging.disable(logging.CRITICAL)
```

* logging to a file (keep normal output clean)

```
logging.basicConfig(level=logging.DEBUG, format="%(message)", filename="my.log")

```

---

## Debuggers


* execute code line by line
* inspect variables
* set break points


---

## The pdb module
    
```
>>> import pdb
>>> dir(pdb)
['Pdb', 'Restart', 'TESTCMD', '__all__', '__builtins__', '__cached__',
'__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__',
'_rstr', '_usage', 'bdb', 'cmd', 'code', 'dis', 'find_function',
'getsourcelines', 'glob', 'help', 'inspect', 'lasti2lineno', 'line_prefix',
'linecache', 'main', 'os', 'pm', 'post_mortem', 'pprint', 're', 'run',
'runcall', 'runctx', 'runeval', 'set_trace', 'signal', 'sys', 'test',
'traceback']

```

---

## Breakpoints

```
>>> def add(a, b):
...    breakpoint()  
...    # import pdb; pdb.set_trace() for python<3.7
...    c  = a - b
...    return c

```


---


### pdb commands

Some of the most used commands are common ``gdb``

* step (s) : execute the next line (for function calls step into that function)
* next (n) : execute the next line (for function calls complete the function all and stop at the next line of current function)
* break (b): set break point
* continue (c): continue execution

---


```
(Pdb) help
Documented commands (type help <topic>):
========================================
EOF    bt         cont      enable  jump  pp       run      unt   
a      c          continue  exit    l     q        s        until 
alias  cl         d         h       list  quit     step     up    
args   clear      debug     help    n     r        tbreak   w     
b      commands   disable   ignore  next  restart  u        whatis
break  condition  down      j       p     return   unalias  where 
...

```
     
---

## Links

* http://docs.python.org/3/library/pdb.html
* https://realpython.com/python-debugging-pdb/
* https://youtu.be/5AYIe-3cD-s

---


### Summary

* `pdb`  modules  for debugging
* Useful shortcut: insert the line
```
breakpoint()
```
anywhere in your code to get a breakpoint and the run the program as normal

* pytest --pdb executes tests and stops in the debugger if there is an error
* insert "assert False" in a code under pytest works as a breakpoint


