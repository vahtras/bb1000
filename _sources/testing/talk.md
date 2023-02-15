<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Testing

## BB1000 Programming in Python
KTH

---

layout: false

## Learning goals

As a result of taking this module you will be able to

* Know main reasons for testing
* Develop code with a test-driven development appraoch
* Use one of Python's main libraries for testing (pytest)
* Know how to prepare tests with fixtures
* Know how to measure how code is covered by tests

---

## Why testing?

* Make sure the code is correct.
* Testing is a design tool
* Testing is a documentation tool
* It makes for a better design
* Code will never be written that is difficult to test
* Code tends to get better structure
* It's what professionals do
* Because it saves time and money

---

## What is a test?

All testing is based on the `assert` statement

```python
>>> assert ...                                                                            #doctest: +SKIP
```

* The `assert` command
* If `...` is true, do nothing
* If `...` is false, stop the program
* Can be used to check stuff in production code, e.g. consistent input


<!--
Run in terminal
```
assert True
assert False
assert 1
assert 0
assert "Hello"
assert ""
assert [1,2,3]
assert []
assert {'a:', 1}
assert {}
```

Simple demo test
-->

Different types of tests

* unit test: testing of small units of code (programmers view)
* integration test: testing how units work together (clients view)

Same tools are used for both cases

---
## What is a test in practice?

* An single function that uses assert
    - to compare actual vs expected result

```python
>>> def test_slash_join():
...    assert slash_join_strings('abc', 'def') == 'abc/def'

```

```python
>>> def slash_join_strings(s1, s2):
...    return "/".join((s1, s2))

```



<!--
```
>>> test_slash_join()

```
-->

<!--
note explicit naming

in terminal solve
e.g.

execute by hand first
then with pytest
-->

* Several test functions may be collected in a file
* A test *suite* is a collection of test files.
* A test *runner* executes all tests it can find

---

## What is test-driven development (TDD)?

To add new functionality, most often involves writing a function

* Make up in your mind what the function should do
* What input is required
* What output is expected
* However, do not code the function just yet...

In brief

* Write a test which contains some `assert` statement
* Write code such the test can pass

---


## The TDD work cycle

Testing a function

### Initialize

* Write a test that 
    - calls the function
    - `assert` that the actual output and expected output are the same



### Repeat

* Run the test 
* Did it fail?
    - The first time: yes 
    - Add code to the function with the purpose of passing the test
    - Start over
* Did it pass?
    - Stop
    - Do not write more code. You are done.

---

## Tools

### Tools for testing python code

* external
    - pytest: a commonly used third-party tool for running tests
    - nose: same but older

* builtin
    - unittest: a module of the standard python library to provide advanced testing
    - doctest: a  simple way of including tests in a doc-string of a function

---

## Pytest

* Understands many styles of tests
* The most modern testing framework
* Easy to get started
* Advanced features
* It supports test coverage
* It couples to the python debugger

---

## Example


<!--
```
>>> from my import *

```
-->

Consider

```python
#my_math.py
def test_my_add():
   assert my_add(1, 1) == 2

def my_add(x, y):
    return x - y

```
Execute the  file with pytest

* Collects everything that looks like a test
* Executes them one by one

---
```python
#my_math.py
def test_my_add():
   assert my_add(1, 1) == 2

def my_add(x, y):
    return x - y

```
<hr>
```shell
$ pytest my_math.py
============================= test session starts ==============================
platform linux -- Python 3.6.3, pytest-3.5.0, py-1.5.3, pluggy-0.6.0
rootdir: /home/olav, inifile: pytest.ini
collected 1 item

my_math.py F                                                             [100%]

=================================== FAILURES ===================================
_________________________________ test_my_add __________________________________

    def test_my_add():
>      assert my_add(1, 1) == 2
E      assert 0 == 2
E       +  where 0 = my_add(1, 1)

my_math.py:3: AssertionError
=========================== 1 failed in 0.02 seconds ===========================
```


---
```python
#my_math.py
def test_my_add():
   assert my_add(1, 1) == 2

def my_add(x, y):
    return x + y

```
<hr>
```shell
$ pytest my_math.py
============================= test session starts ==============================
platform linux -- Python 3.6.3, pytest-3.5.0, py-1.5.3, pluggy-0.6.0
rootdir: /home/olav, inifile: pytest.ini
collected 1 item

my_math.py .                                                             [100%]

=========================== 1 passed in 0.00 seconds ===========================
```


---

## Exercise

```
>>> def test_leap_year():
...     assert is_leap_year(2000) == True
...     assert is_leap_year(1999) == False
...     assert is_leap_year(1998) == False
...     assert is_leap_year(1996) == True
...     assert is_leap_year(1900) == False
...     assert is_leap_year(1800) == False
...     assert is_leap_year(1600) == True

```

* Save in a file `test_leap.py`
* Write code in `leap.py`
* Verify with pytest

---

## Test coverage

* Coverage is a measure how much of your code is executed by tests
* During a test run a coverage library keeps track of executed lines
* Guides developer where bugs may hide

How it works (pytest)
* Summary

```
C:\Users\...> pytest test_my.py --cov my
```

* Line info
```
C:\Users\...> pytest test_my.py --cov my --cov-report=term-missing
```
---

* HTML report

```
C:\Users\...> pytest test_my.py --cov my --cov-report=html
```
Open in browser


<img src="cov1.png" height="250">
<img src="cov2.png" height="250">

---

## Doctest

* Simple test cases can be provided as part of the documentation string

* Cut and paste an interactive session known to give true result

* The doctest module executes the example from the interactive session as a test case
* Test on string output and not values - small changes in formatting will case tests to fail

---

## Example
```
    #calculate.py
    def add(a, b):
        """Return sum of two arguments
        >>> add(1, 1) #doctest: +SKIP
        2
        >>>
        """
        return a + b

    def sub(a, b):
        """Return difference of two arguments
        >>> sub(1, 1) #doctest: +SKIP
        0
        """
        return a + b

```

can you see the bug?

---

## Running doctest

On the command line

```
    $ python -m doctest calculate.py
```

* All code in the file is executed
* Functions are defined
* Test are run

---
### The output
``` 
    $ python calculate.py
    **********************************************************************
    File "calculate.py", line 14, in __main__.sub
    Failed example:
        sub(1, 1)
    Expected:
        0
    Got:
        2
    **********************************************************************
    1 items had failures:
       1 of   1 in __main__.sub
    ***Test Failed*** 1 failures.
```
---

Correct the bug
```
        return a + b -> return a - b
```
Rerun

```
    $ python -m doctest calculate.py
    $
```
silent - all ok

---

## Conclusion - doctests

* Very easy to include testning into your code

* The test serves as documentation as well 

* Typically tests only one aspect of the function

* But could clutter your code and may not be the best for extensive testing

* Extensive testing is best separated from production code

* More information on http://docs.python.org/library/doctest

---


## The unittest module



* A unit test module exist for this purpose: `unittest`

* The tests can be written in a separate file

* One defines a class which is a subclass of `unittest.TestCase`

* The unittest framework executes and checks everything that begins with test

* Part of the standard library and provides very portable testing

---

### Howto

* Define a class with a name beginning with ``Test`` as a subclass of ``unittest.TestCase``

* Define class methods that begin with ``test`` using the test functions of the ``unittest`` module

* Optionally one may define a ``setUp`` and a ``tearDown`` method which are run before and after every test.

* In the main section run ``unittest.main()``

```
    class TestSomething(unittest.TestCase):
        ...
        def test_this(self):
            ...
        def test_that(self):
            ...
    if __name__ == "__main__":
        unittest.main()
```

---

### Example

```python
    #test_calculate.py
    import unittest
    import calculate

    class TestCalculate(unittest.TestCase):
        
        def testadd(self):
            res = calculate.add(1, 1)
            self.assertEqual(res, 2)

        def testsub(self):
            res = calculate.sub(1, 1)
            self.assertEqual(res, 0)

    if __name__ == "__main__":
        unittest.main()

```
compare  with
```python
#my_math.py
def test_add():
    assert calculate.add(1, 1) == 1

def test_sub():
    assert calculate.sub(1, 1) == 0

```
---

### Run test
```bash
    $ python test_calculate.py
    .F
    ======================================================================
    FAIL: testsub (__main__.TestCalculate)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "test_calculate.py", line 18, in testsub
        self.assertEqual(res, 0)
    AssertionError: 2 != 0

    ----------------------------------------------------------------------
    Ran 2 tests in 0.001s

    FAILED (failures=1)
```
---

### Run verbose test

```
    $ python test_calculate.py -v
    testadd (__main__.TestCalculate) ... ok
    testsub (__main__.TestCalculate) ... FAIL

    ======================================================================
    FAIL: testsub (__main__.TestCalculate)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "test_calculate.py", line 18, in testsub
        self.assertEqual(res, 0)
    AssertionError: 2 != 0

    ----------------------------------------------------------------------
    Ran 2 tests in 0.001s

    FAILED (failures=1)
```

---


### Fix the bug
```
    return a + b -> return a - b
```
--

### Rerun test
```
    $ python test_calculate.py -v
    testadd (__main__.TestCalculate) ... ok
    testsub (__main__.TestCalculate) ... ok

----------------------------------------------------------------------
Ran 2 tests in 0.000s

OK
```

---
## Other helper functions tests

* ``assertNotEqual``

* ``assertTrue``

* ``assertFalse``

* ``assertAlmostEqual``
    - Most numerical testing is within a threshold, e.g.

```
     def testdiv(self):
         res = calculate.div(1., 3)
         self.assertAlmostEqual(res, 0.333333, 6)
```

* see also http://docs.python.org/3/library/unittest.html 

---

### Summary

* The `pytest` library is the most modern testing framework for Python
    - Easy to get started with simple test functions
* `nose` is a similar older library
* The standard library contains `unittest` library
    - Requires knowledge of classes to code your tests, 
* Fine to ``doctest`` for small illustrations

---

### Final tip

* Embrace the TDD philosphy, write test before code.
* Document code and modules - be kind to your future self.
* For good programming style, consider PEP 8, http://www.python.org/dev/peps/pep-0008/
* Be obsessive about testing
* If your test code is larger that your production code, you are on the right track
* This takes initially a little more time but the rewards in the long run are huge

### Links

* http://pythontesting.net
* http://mathieu.agopian.info/presentations/2015_06_djangocon_europe
* http://katyhuff.github.io/python-testing
* <a href="https://www.amazon.com/s/ref=nb_sb_noss_1?url=search-alias%3Dstripbooks&field-keywords=tdd&rh=n%3A283155%2Ck%3Atdd">Amazon search:"TDD"</a> > 100 books
* http://pyvideo.org: <tt>About 80 results</tt>

