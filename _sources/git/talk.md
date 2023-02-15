<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Introduction to git

BB1000 Programming in Python
KTH

---

layout: false

## Learning objectives

* What version control is

* Why we should use version control

* Why we choose use git

* Know about the basic components in git
    - work directory
    - staging area
    - repository (local, remote)
    - branches

* Know commands for basic git workflow
    - How to save history
    - How to undo mistakes
    - How to collaborate with others (github)

---

## What is version control:

* A Version Control System (VCS) is a frameorks that tracks the history of a project

* The history of a project is a sequence of versions

* At any point in time we can go back to a previous version

* A VCS allows you to compare different versions

* A VCS allows several users to work on the same project files simultaneously

---

## Why use version control

You may already have used some manual version...

```
$ cp -r Project Project.save
```

... some work ...

```
$ cp -r Project Project.save.v2
```

... some work ...

```
$ cp -r Project Project.save.v2.new
```

---

<center>
<img src="http://www.phdcomics.com/comics/archive/phd101212s.gif" height="500"/>
</center>

---

## Introducing git

<center>
<img src="http://www.linux.com/images/stories/714/Linus-Torvalds-LinuxCon-Europe-2014.jpg" height="250" />
</center>


* Written by Linus Thorvalds, originally for the Linux kernel
* A distributed Version Control System
* Several servers have all information
* Any one can be chosen as the reference version
* One of the most popular frameworks today (others: bazaar, mercurial)

* Historical frameworks (subversion, CVS, RCS)

---

## When to use

### Scenario

* For source code development
* For manuscripts
* In single-user projects
* In collaborative projects

### Benefits

* No history is lost
* All versions of your documents are preserved
* Easy to backup to other sites

---

## Concepts

* Work directory: local directory where you work
* Cache: temporary area for files you intend to keep
* Commit: a snapshot of the project files at a point in time
* Repository: sequence/tree of commits (history of the project)

---

## Concepts

* Work directory: local directory where you work
* Cache: temporary area for files you intend to keep
* Commit: a snapshot of the project files at a point in time
* Repository: sequence/tree of commits (history of the project)

### 11 basic commands

<div class="col-md-6">
    <ul>
        <li>Initialize</li>
        <ul>
        <li>clone</li>
        <li>init</li>
        </ul>
        <li>Queries</li>
        <ul>
        <li>status</li>
        <li>log </li>
        <li>diff</li>
        </ul>
    </ul>
</div>
<div class="col-md-6">
    <ul>
        <li>Changing locally</li>
        <ul>
        <li>add </li>
        <li>commit</li>
        <li>merge</li>
        </ul>
        <li>Remote interaction</li>
        <ul>
        <li>push</li>
        <li>fetch</li>
        <li>pull</li>
        </ul>
    </ul>
</div>

---

## Setup

### The first time around

```
    $ git config --global user.name "First Last"
    $ git config --global user.email "first.last@isp.com"
```

Creates a configuration file ``~/.gitconfig``

```
    [user]
	name = First Last
	email = first.last@isp.com
```

*Note*:    You can create and edit the file directly

---

## Initializing a repository

* Use an existing directory or create a new project directory

```
    $ mkdir proj
```

<pre>
    proj/
</pre>

* Go to the directory and initialize

```
    $ cd proj
    $ git init
    Initialized empty Git repository in (...)proj/.git/
```

<pre>
    proj/
    └── .git
</pre>

---

<pre>
proj/
└── .git
</pre>

## Check status

* Check repository status

```
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

---

<pre>
proj
├── .git
└── hello.py
</pre>

## Create a new file `hello.py`

```python
#hello.py
print("Hello world!")
```

* Recheck status

```
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    hello.py

nothing added to commit but untracked files present (use "git add" to track)
```

* Git warns about **untracked** files

---

<pre>
proj
├── .git
└── hello.py
</pre>

## Add file to Git

```
$ git add hello.py

$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   hello.py
```

### The staging area/cache

* After `git add` a file is in the staging area (cache)
* This is an intermediate level between the work directory and repository

---

<pre>
proj
├── .git
└── hello.py
</pre>

## Save to repository

* Save the latest changes in the local repository (in `.git` directory)

```
    $ git commit -m "First hello"
    [master (root-commit) cf06b48] First hello
     1 file changed, 1 insertion(+)
     create mode 100644 hello.py
```

* Check the status again

```
    $ git status
    On branch master
    nothing to commit, working directory clean
```

---

## The work cycle

There are three levels

* The work directory
* The staging area
* The repository

```
repository (.git)
    ^
    |   commit

staging area (cache)
    ^
    |   add

work directory     <- init
```

---

## The work cycle

There are three levels

* The work directory
* The staging area
* The repository

```
repository (.git)
    ^
    |   commit

staging area (cache)
    ^
    |   add

work directory     <- init
```

The basic work cycle is edit-add-commit

```
    $ <edit> <file>   # edit your file
    $ git add <file>  # cache your changes
    $ git commit -m <message> <file>  # save your changes in repository
```

---

## Review history

To see the commit history of the project files

```
$ git log
commit cf06b48ceb6f9c301867373845bd59e6620fb72f (HEAD -> master)
Author: First Last <first.last@isp.com>
Date:   Tue Mar 19 13:25:10 2019 +0100

    First hello
```

---

## Viewing changes

* Consider a modified file

```
$ cat << EOF > hello.py
print("Hello there world!")
EOF
```

* git now recognizes this tracked file as modified

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   hello.py

no changes added to commit (use "git add" and/or "git commit -a")
```

---

## Viewing changes (continued)

* `git diff` shows the changes

```
$ git diff
diff --git a/hello.py b/hello.py
index f1a1813..086befc 100644
--- a/hello.py
+++ b/hello.py
@@ -1 +1 @@
-print("Hello world!")
+print("Hello there world!")
```

---

## Update repository

```bash
$ git add hello.py
```

```bash
$ git commit -m "Change greeting"
[master 04064bc] Change greeting
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```bash
$ git log
commit 04064bc84ecaee64e3fb4c3dd54b73da33ec6986 (HEAD -> master)
Author: First Last <first.last@isp.com>
Date:   Tue Mar 19 13:33:10 2019 +0100

    Change greeting

commit cf06b48ceb6f9c301867373845bd59e6620fb72f
Author: First Last <first.last@isp.com>
Date:   Tue Mar 19 13:25:10 2019 +0100

    First hello
```

---

## Recovering old work

* To retreive old verions, use checkout with the commit string

```
$ git checkout cf06b4
Note: checking out 'cf06b4'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at cf06b48 First hello
```

```
$ cat hello.py
print("Hello world!")
```

---

## Switch back to latest version

```
$ git status
HEAD detached at cf06b48
nothing to commit, working tree clean
```

```
$ git checkout master
Previous HEAD position was cf06b48 First hello
Switched to branch 'master'
```

```
$ git status
On branch master
nothing to commit, working directory clean
```

```
$ cat hello.py
print("Hello there world!")
```

---

## Remote repositories

* Necessary for collaborative projects
* Useful for single-user projects
* Web-services, github, bitbucket
* A shared directory (NFS, AFS, Dropbox....)
* git pull from remote
* git push to remote

```
repository (.git)  <->   remote
                pull,push

    ^
    |   commit

staging area (cache)

    ^
    |   add

work directory     <- init, clone
```

---

### A shared directory repository

* Create an empty remote repository
```
    $ git init --bare ~/Dropbox/proj.git
    Initialized empty Git repository in /home/olav/Dropbox/proj.git/
```
--

* Create an alias for the remote repository
```
    $ git remote add dropbox ~/Dropbox/hello.git
    $ git remote -v
    dropbox	/home/olav/Dropbox/proj.git (fetch)
    dropbox	/home/olav/Dropbox/proj.git (push)
```
--

* Let the local branch track the remote repository
```
    $ git push -u dropbox master
    Counting objects: 6, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (6/6), 473 bytes | 0 bytes/s, done.
    Total 6 (delta 0), reused 0 (delta 0)
    To /home/olav/Dropbox/proj.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from dropbox.
```

---


* Now your local is in sync with your remote
* Make another local change

```
    $ cat << EOF > hello.py
    print "HELLO THERE WORLD!"
    EOF
```

```
    $ git add hello.py
```

```
    $ git commit -m "Capitalize"
    [master cb05b3f] Capitalize
     1 file changed, 1 insertion(+), 1 deletion(-)
```

---

### Backup to remote

* View local changes
```
    $ git status
    On branch master
    Your branch is ahead of 'origin/master' by 1 commit.
      (use "git push" to publish your local commits)

    nothing to commit, working directory clean
```

* Backup to remote repository
```
    $ git push
    Counting objects: 5, done.
    Writing objects: 100% (3/3), 269 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To /home/olav/Dropbox/proj.git
       f7efe62..cb05b3f  master -> master
```

---

### Continue work on another computer

* With access to the same shared file system
```
    |Other> git clone ~/Dropbox/proj.git
    Cloning into 'proj'...
    done.
    |Other> cd proj
```
* work on remote
    - edit -> add -> commit -> push

---

### Back on local

* Retrieve changes that was made on another system

* By you or another developer

```
    $ git pull
    remote: Counting objects: 5, done.
    remote: Total 3 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From /home/olav/Dropbox/proj
       cb05b3f..d5fe073  master     -> origin/master
    Updating cb05b3f..d5fe073
    Fast-forward
     hello.py | 2 +-
     1 file changed, 1 insertion(+), 1 deletion(-)

```

---

### Summary of work cycle

* Start a new project
```
    $ git init
```

* Get a copy of existing project
```
    $ git clone
```

* Locally: edit-add-commit
```
    $ vim ...
    $ git add...
    $ git commit...
```
* Sync with remote: pull-push
```
    $ git pull
    $ git push
```

---

### Use a remote server (service)

* github.com (free for public projects)
* gitlab.com  (free for public and private projects)

### Links

* http://git-scm.com/book
* http://swcarpentry.github.io/git-novice/
* http://www.linux.com/news/featured-blogs/185-jennifer-cloer/821541-10-years-of-git-an-interview-with-git-creator-linus-torvalds
* https://gun.io/blog/how-to-github-fork-branch-and-pull-request/
* https://www.linux.com/learn/fixing-mistakes-git
* http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html
* http://git-man-page-generator.lokaltog.net/
