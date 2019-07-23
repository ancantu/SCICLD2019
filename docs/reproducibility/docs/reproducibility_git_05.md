## Exploring History

As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one or two lines at a time to `notes.txt`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `notes.txt`.

```
$ echo "Part 5 will link your local repository with github" >> notes.txt
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control

Part 2: Create a new repository from the command line
* use git init ./ to initialize a new repository

Part 3: Tracking changes with git
* this is what we are working on now

Part 4: Exploring history
Part 5 will link your local repository with github
```

Now, let's see what we get.

```
$ git diff HEAD notes.txt
diff --git a/notes.txt b/notes.txt
index c876e8d..a4a265a 100644
--- a/notes.txt
+++ b/notes.txt
@@ -8,3 +8,4 @@ Part 3: Tracking changes with git
 * this is what we are working on now

 Part 4: Exploring history
+Part 5 will link your local repository with github
```

Which is the same as what you would get if you leave out `HEAD` (try it).  The
real benefit in all this is when you can refer to previous commits.  We do
that by adding `~1` to refer to the commit one before `HEAD`.

```
$ git diff HEAD~1 notes.txt
diff --git a/notes.txt b/notes.txt
index 61f7805..a4a265a 100644
--- a/notes.txt
+++ b/notes.txt
@@ -6,3 +6,6 @@ Part 2: Create a new repository from the command line

 Part 3: Tracking changes with git
 * this is what we are working on now
+
+Part 4: Exploring history
+Part 5 will link your local repository with github
```

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:

```
$ git diff HEAD~2 notes.txt
diff --git a/notes.txt b/notes.txt
index fe7c565..a4a265a 100644
--- a/notes.txt
+++ b/notes.txt
@@ -3,3 +3,9 @@ Part 1: The Basics of Git

 Part 2: Create a new repository from the command line
 * use git init ./ to initialize a new repository
+
+Part 3: Tracking changes with git
+* this is what we are working on now
+
+Part 4: Exploring history
+Part 5 will link your local repository with github
```

### More Detailed Histories

We could also use `git show` which shows us what changes we made at an older commit as well as the commit message, rather than the _differences_ between a commit and our working directory that we see by using `git diff`.

```
$ git show HEAD~2 notes.txt
commit cfe53067828d2e7232503e4dfec43d9ac20e6cfb
Author: Joe Allen <wallen@tacc.utexas.edu>
Date:   Fri Jul 13 10:59:46 2018 -0500

    Added part 2 to version control notes

diff --git a/notes.txt b/notes.txt
index 0495f06..fe7c565 100644
--- a/notes.txt
+++ b/notes.txt
@@ -1,2 +1,5 @@
 Part 1: The Basics of Git
 * Git is used for version control
+
+Part 2: Create a new repository from the command line
+* use git init ./ to initialize a new repository
```

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1`
means "the previous commit",
while `HEAD~100` goes back 100 commits from where we are now.

### Look at Changes Given Commit ID

We can also refer to commits using
those long strings of digits and letters
that `git log` displays.
These are unique IDs for the changes,
and "unique" really does mean unique:
every change to any set of files on any computer
has a unique 40-character identifier.
Our first commit was given the ID
`cfe53067828d2e7232503e4dfec43d9ac20e6cfb`,
so let's try this:

```
$ git diff cfe53067828d2e7232503e4dfec43d9ac20e6cfb notes.txt
diff --git a/notes.txt b/notes.txt
index fe7c565..a4a265a 100644
--- a/notes.txt
+++ b/notes.txt
@@ -3,3 +3,9 @@ Part 1: The Basics of Git

 Part 2: Create a new repository from the command line
 * use git init ./ to initialize a new repository
+
+Part 3: Tracking changes with git
+* this is what we are working on now
+
+Part 4: Exploring history
+Part 5 will link your local repository with github
```

That's the right answer,
but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters:

```
$ git diff cfe5306 notes.txt
diff --git a/notes.txt b/notes.txt
index fe7c565..a4a265a 100644
--- a/notes.txt
+++ b/notes.txt
@@ -3,3 +3,9 @@ Part 1: The Basics of Git

 Part 2: Create a new repository from the command line
 * use git init ./ to initialize a new repository
+
+Part 3: Tracking changes with git
+* this is what we are working on now
+
+Part 4: Exploring history
+Part 5 will link your local repository with github
```

### Restoring Old Versions of Files

We can save changes to files and see what we've changed—now how
can we restore older versions of things?
Let's suppose we accidentally overwrite our file:

```
$ echo "" > notes.txt
$ cat notes.txt
```

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   notes.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

We can put things back the way they were
by using `git checkout`:

```
$ git checkout HEAD notes.txt
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control

Part 2: Create a new repository from the command line
* use git init ./ to initialize a new repository

Part 3: Tracking changes with git
* this is what we are working on now

Part 4: Exploring history
```

As you might guess from its name,
`git checkout` checks out (i.e., restores) an old version of a file.
In this case,
we're telling Git that we want to recover the version of the file recorded in `HEAD`,
which is the last saved commit.
If we want to go back even further,
we can use a commit identifier instead:

```
$ git checkout cfe5306 notes.txt
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control
```

Again, we can put things back the way they were
by using `git checkout`:

```
$ git checkout HEAD notes.txt
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control

Part 2: Create a new repository from the command line
* use git init ./ to initialize a new repository

Part 3: Tracking changes with git
* this is what we are working on now

Part 4: Exploring history
```

It looks like we recovered everything except for the very last change ("Part 5...") because we never added and committed that entry.
It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to get rid of.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `cfe5306`:

![Git Checkout](./fig/git-checkout.svg)


The fact that files can be reverted one by one
tends to change the way people organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction
without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.





### Exercise

Joe has made changes to the Python script that he has been working on for weeks, and the
modifications he made this morning "broke" the script and it no longer runs. He has spent
~ 1hr trying to fix it, with no luck...

Luckily, he has been keeping track of his project's versions using Git. Which commands below will
let him recover the last committed version of her Python script called
`data_cruncher.py`?

1. `$ git checkout HEAD`
2. `$ git checkout HEAD data_cruncher.py`
3. `$ git checkout HEAD~1 data_cruncher.py`
4. `$ git checkout <unique ID of last commit> data_cruncher.py`


### Summarize Histories

Exploring history is an important part of git, often it is a challenge to find
the right commit ID, especially if the commit is from several months ago.

Imagine the `my_first_repo` project has more than 50 files.
You would like to find a commit with specific text in `notes.txt`.
When you type `git log`, a very long list appeared,
How can you narrow down the search?

Recall that the `git diff` command allow us to explore one specific file,
e.g. `git diff notes.txt`. We can apply a similar idea here.

```
$ git log notes.txt
```

Unfortunately, some of these commit messages may be very ambiguous e.g. `update files`.
How can you search through these files?

Both `git diff` and `git log` are very useful and they summarize a different part of the history for you.
Is it possible to combine both? Let's try the following:

```
$ git log --patch notes.txt
```

You should get a long list of output, and you should be able to see both commit messages and the difference between each commit.


### Exercise

1. What does the following command do? `$ git log --patch HEAD~3 *.txt`
2. Create a new Git repo in a new folder. Create or copy in two files with text in them (any text is fine). Separately, add and commit those files to the new repo.


Previous: [Tracking Changes](reproducibility_git_04.md) | Next: [Link a Local Repository to GitHub](reproducibility_git_06.md) | Top: [Course Overview](../reproducibility.md)
