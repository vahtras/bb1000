<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# File handling

BB1000 Programming in Python

KTH

---

layout: false

## Learning objectives

+ File operations: open, close, read, write

+ Handling of input and output with Python

+ Usage of string, list

---

## The file system

### Navigation

* The *file system* organizes data into files and directories (folders)
* Directories are special files that are contains of other files
* Often called a file tree
* The beginning of the tree is called the root "/"

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRajiMVifhNZj_GfkJ53S7AfxQIm330z6hIiYi19hKt6jMWTNgyzg"> 


* Directories are branches
* Files are leaves

---
name: file-tree

### A tree view of a file system

<img src="https://docs.oracle.com/cd/B19306_01/backup.102/b14236/img/obref001.gif"> 


???

* Files names often have a dot, the latter part is the extension 
* signifies type of file
* convention not requirement

---

<section>
<h2>Where am I?</h2>
</section>

---
name: looking

### Looking around

Concept of location: current/working directory

~~~
$ pwd 
/home/jane
$ ls
file1  file1.bak  file1.tmp  tmp
$ ls /home/pablo
file1 file1.bak file2
~~~
<img src="https://docs.oracle.com/cd/B19306_01/backup.102/b14236/img/obref001.gif" height=300> 

---
name: example

~~~
$ echo ~
/home/jane
$ echo ~pablo
/home/pablo
$ ls ../pablo
file1 file1.bak file2
~~~

<img src="https://docs.oracle.com/cd/B19306_01/backup.102/b14236/img/obref001.gif" height=250> 

Directory shortcuts
- ~ (home) 
- . (current directory) 
- .. (parent directory)

---
name: moving

### Moving around

* `cd` - change directory

~~~
$ cd ~pablo
$ pwd
/home/pablo
$ ls 
file1 file1.bak file2
~~~

<img src="https://docs.oracle.com/cd/B19306_01/backup.102/b14236/img/obref001.gif" height=250> 
---
<section>
<h2>How do I view/change/save text in files?</h2>
</section>
---
name: files

## Working with files and direcories

### Files

#### Saving output in a file

* redirection (>)
* redirection with append (>>)

~~~
$ echo "Hello" >  /tmp/saved
$ ls  >>  /tmp/saved
~~~
#### What is in the file?
~~~
$ cat /tmp/saved
Hello
file1
file1.bak
file2
~~~
---
name: edit

#### Edit a file with a text editor
~~~
$ nano /tmp/saved
~~~
~~~
  GNU nano 2.9.8                      /tmp/saved                                
____________________________________________________________________________

Hello
file1
file1.bak
file2












^G Get Help  ^O Write Out ^W Where Is  ^K Cut Text  ^J Justify   ^C Cur Pos
^X Exit      ^R Read File ^\ Replace   ^U Uncut Text^T To Spell  ^_ Go To Line




~~~

#### Remove the file
~~~
$ rm /tmp/saved
~~~

---
name: directories

### Directories

#### Creating and removing dictionaries

* `mkdir` create directory
* `rmdir` remove directory

```
$ mkdir thesis
$ cd thesis
$ nano thesis.tex
...
```

~~~
$ cd ..
$ rmdir thesis
rmdir: failed to remove 'thesis': Directory not empty
$ rm -r thesis
~~~

* be careful with this command!
---


## Opening files

The built-in `open` function

+ `open` accepts a file name and a mode as arguments
+ file name: a string
+ mode: 'r' for reading; 'w' for writing
+ `open` returns a file object
+ after reading, the opened file should be closed

Getting help
~~~
Help on built-in function open in module io:                                     
                                                                                 
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, clos
efd=True, opener=None)                                                           
    Open file and return a stream.  Raise IOError upon failure.                  
                                                                                 
    file is either a text or byte string giving the name (and the path           
    if the file isn't in the current working directory) of the file to           
    be opened or an integer file descriptor of the file to be                    
    wrapped. (If a file descriptor is given, it is closed when the               
    returned I/O object is closed, unless closefd is set to False.)          
~~~
>>> help(open)

---

## Reading/writing file

Methods of file objects:

+ The `read` method
  Reads the entire file, unless a `size` argument is provided

+ The `readline` method
  Reads one line from the file

+ The `readlines` method
  Reads all lines from the file into a list

+ The `write` method
  Accepts a string as argument, and writes the string to file

---

## Example: reading

Given a text file `foo.txt`

~~~
foo
bar
baz
~~~

~~~
>>> open('foo.txt').read()
foo
'foo\nbar\nbaz\n' 
~~~

~~~
>>> open('foo.txt').readlines()
['foo\n', 'bar\n', 'baz\n']          
~~~

~~~
>>> f = open('foo.txt')                                                          
>>> f.readline()                                                                 
'foo\n'                                                                          
>>> f.readline()                                                                 
'bar\n'                                                                          
>>> f.readline()                                                                 
'baz\n'                       
~~~

---

File objects are iterables, which means you cat put them in a for loop. Text
files are designed to return one line at at time

~~~
>>> for line in open('foo.txt'):
...     print(line)
foo

bar

baz

~~~

Why do we get an extra space?

---
## Example: writing

By default `open` expects an existing file for reading. To write to a file we
need to give it a second parameter

~~~
>>> f = open('bar.txt', 'w')
>>> f.write('foo\n')
>>> f.write('bar\n')
>>> f.write('baz\n')
>>> f.close()
~~~

This will create a new file or overwrite an existing file

Note that you will not normally see any content in the file system until you have closed the file.

Alternative: you can supply the file object as an argument to print

~~~
f = open('bar.txt', 'w')
print('foo', file=f)
print('bar', file=f)
print('baz', file=f)
f.close()
~~~


---

## Closing files

It is good practice to close a file in the same program unit where opened

~~~
f = open('file.txt')
# work with file
# ...
f.close()
~~~

Howver, Python is very permissive about it so that if you forget, the file
will close when you leave the function where it happened

Alternative way is to use a context manager (a `with` block)

~~~
with open('file.txt') as f:
    # work with file
    # ...
~~~

The file will be closed when you leave the `with` block



---

Additional help:

https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files
