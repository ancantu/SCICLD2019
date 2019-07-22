## Tracking Changes

We will use this repository to record notes from the version control module.
Create a new file called `notes.txt` in your repo:

```
$ pwd
/home/wallen/my_first_repo
$ touch notes.txt
$ ls
notes.txt
```

Open the file with VIM and enter the following text:

```
$ vim notes.txt
```

Now in VIM:
```
(press 'i' to enter insert mode)

Part 1: The Basics of Git
* Git is used for version control

(press Esc then :wq to save and quit)
```
```
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control
```

### Start Tracking a New File

If we check the status of our project again,
Git tells us that it's noticed the new file:

```
$ git status
On branch master

Initial commit

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	notes.txt

nothing added to commit but untracked files present (use "git add" to track)
```

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

```
$ git add notes.txt
```

And then check for the expected behavior:

```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   notes.txt
```
### Commit Changes to the Repo

Git now knows that it's supposed to keep track of `notes.txt`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

```
$ git commit -m "Started notes for the version control module"
[master (root-commit) 39e316e] Started notes for the version control module
 1 file changed, 1 insertion(+)
 create mode 100644 notes.txt
```


When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a "commit"
(or "revision") and its short identifier is `39e316e`.
Your commit may have another identifier.

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
Good commit messages start with a brief (<50 characters) statement about the
changes made in the commit. Generally, the message should complete the sentence "If applied, this commit will" <commit message here>.
If you want to go into more detail, add a blank line between the summary line and your additional notes. Use this additional space to explain why you made changes and/or what their impact will be.

If we run `git status` now:

```
$ git status
On branch master
nothing to commit, working directory clean
```

it tells us everything is up to date.

### Check the Project History

If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

```
$ git log
commit 39e316e4afe33957495a328750c72834551bd9f1
Author: Joe Allen <wallen@tacc.utexas.edu>
Date:   Fri Jul 13 10:49:08 2018 -0500

    Started notes for the version control module
```

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created.


### Exercise

1. Take a moment to browse the `.git/` directory to see if you can find where the changes are stored


### Making Further Changes

Now suppose we add more information to the file. Edit the file with `VIM` to add Part 2 of the notes:

```
$ vim notes.txt
```

Now in VIM:
```
(press 'i' to enter insert mode)

(add this new text at the bottom:)

Part 2: Create a new repository from the command line
* use git init to initialize a new repository

(press Esc then :wq to save and quit)
```
```
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control

Part 2: Create a new repository from the command line
* use git init ./ to initialize a new repository
```

When we run `git status` now,
it tells us that a file it already knows about has been modified:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   notes.txt

no changes added to commit (use "git add" and/or "git commit -a")
```


The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
nor have we saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review
our changes before saving them. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:

```
$ git diff
diff --git a/notes.txt b/notes.txt
index 0495f06..dc3ae88 100644
--- a/notes.txt
+++ b/notes.txt
@@ -1,2 +1,5 @@
Part 1: The Basics of Git
* Git is used for version control
+
+Part 2: Create a new repository from the command line
+* use git init ./ to initialize a new repository
```

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `0495f06` and `dc3ae88` are unique computer-generated labels for those versions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added lines.

After reviewing our change, it's time to commit it:

```
$ git add notes.txt
$ git commit -m "Added part 2 to version control notes"
[master cfe5306] Added part 2 to version control notes
 1 file changed, 3 insertion(+)
$ git status
On branch master
nothing to commit, working directory clean
```

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.
For example,
suppose we're adding a few citations to relevant research to our thesis.
We might want to commit those additions,
and the corresponding bibliography entries,
but *not* commit some of our work drafting the conclusion
(which we haven't finished yet).

To allow for this,
Git has a special *staging area*
where it keeps track of things that have been added to
the current changeset
but not yet committed.


### Staging Area

If you think of Git as taking snapshots of changes over the life of a project,
`git add` specifies *what* will go in a snapshot
(putting things in the staging area),
and `git commit` then *actually takes* the snapshot, and
makes a permanent record of it (as a commit).

![The Git Staging Area](./fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First,
we'll add another line to the file:

```
$ vim notes.txt
```

Now in VIM:
```
(press 'i' to enter insert mode)

(add this new text at the bottom:)

Part 3: Tracking changes with git
* this is what we are working on now

(press Esc then :wq to save and quit)
```
```
$ cat notes.txt
Part 1: The Basics of Git
* Git is used for version control

Part 2: Create a new repository from the command line
* use git init ./ to initialize a new repository

Part 3: Tracking changes with git
* this is what we are working on now
```

Now check the changes:
```
$ git diff
diff --git a/notes.txt b/notes.txt
index fe7c565..61f7805 100644
--- a/notes.txt
+++ b/notes.txt
@@ -3,3 +3,6 @@ Part 1: The Basics of Git

 Part 2: Create a new repository from the command line
 * use git init ./ to initialize a new repository
+
+Part 3: Tracking changes with git
+* this is what we are working on now
```

So far, so good:
we've added a few lines to the end of the file
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

```
$ git add notes.txt
$ git diff
```

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

```
$ git diff --staged
diff --git a/notes.txt b/notes.txt
index fe7c565..61f7805 100644
--- a/notes.txt
+++ b/notes.txt
@@ -3,3 +3,6 @@ Part 1: The Basics of Git

 Part 2: Create a new repository from the command line
 * use git init ./ to initialize a new repository
+
+Part 3: Tracking changes with git
+* this is what we are working on now
```

It shows us the difference between
the last committed change
and what's in the staging area.
Let's save our changes:

```
$ git commit -m "Started adding instructions for part 3"
[master 53baf60] Started adding instructions for part 3
 1 file changed, 3 insertion(+)
```

Check our status:

```
$ git status
On branch master
nothing to commit, working directory clean
```

And look at the history of what we've done so far:
```
$ git log
commit 53baf60a5ddeb490f8ed0542458abb7e850048e9
Author: William Allen <wallen@tacc.utexas.edu>
Date:   Fri Jul 13 11:15:04 2018 -0500

    Started adding instructions for part 3

commit cfe53067828d2e7232503e4dfec43d9ac20e6cfb
Author: William Allen <wallen@tacc.utexas.edu>
Date:   Fri Jul 13 10:59:46 2018 -0500

    Added part 2 to version control notes

commit 39e316e4afe33957495a328750c72834551bd9f1
Author: William Allen <wallen@tacc.utexas.edu>
Date:   Fri Jul 13 10:49:08 2018 -0500

    Started notes for the version control module
```

### Note on Directories

There are a couple important facts you should know about directories in Git. First, Git does not track directories on their own, only files within them. Try it for yourself:

```
$ mkdir directory
$ git status
$ git add directory
$ git status
```

Note, our newly created empty directory `directory` does not appear in
the list of untracked files even if we explicitly add it (_via_ `git add`) to our
repository.

Second, if you create a directory in your Git repository and populate it with files,
you can add all files in the directory at once by:

```
$ git add <directory-with-files>
```

### Exercise

Here is the next section that we will cover, add it to `notes.txt`:
```
Part 4: Exploring history
```

1. Add this next section to your text file using VIM
2. Add the modified file to the staging area
3. Commit the modifications
4. Browse the `.git/` folder to find where commits are located


Previous: [Create a New Repository](reproducibility_git_03.md) | Next: [Exploring History](reproducibility_git_05.md) | Top: [Course Overview](../reproducibility.md)
