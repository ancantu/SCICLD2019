## Create a New Repository on the Command Line


First, let's create a new folder in our `$HOME` directory on `training01` to organize our work:

```
$ cd ~/
$ mkdir my_first_repo/
$ cd my_first_repo/       # currently empty
```

Then we will use a Git command to initialize this directory as a new Git repository - or a place where Git can start to organize versions of our files.
```
$ git init
Initialized empty Git repository in /home/wallen/my_first_repo/.git/
```


If we use `ls -a`, we can see that Git has created a hidden directory within `my_first_repo` called `.git`:

```
$ ls -a
./  ../  .git/
```

Use the `find` command to get a overview of the contents of the `.git/` directory:

```
$ find .git/
.git/
.git/refs
.git/refs/heads
.git/refs/tags
.git/branches
.git/description
.git/hooks
.git/hooks/applypatch-msg.sample
.git/hooks/commit-msg.sample
.git/hooks/post-update.sample
.git/hooks/pre-applypatch.sample
.git/hooks/pre-commit.sample
.git/hooks/pre-push.sample
.git/hooks/pre-rebase.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/update.sample
.git/info
.git/info/exclude
.git/HEAD
.git/config
.git/objects
.git/objects/pack
.git/objects/info
```

Git uses this special sub-directory to store all the information about the project,
including all files and sub-directories located within the project's directory.
If we ever delete the `.git` sub-directory, we will lose the project's history.

We can check that everything is set up correctly by asking Git to tell us the status of our project:

```
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

### Exercise

1. Explore the files and folders in the `.git/` directory
2. Can you find a file with your name and e-mail in it? How did it get there?

Next: [Tracking Changes](reproducibility_git_04.md) | Top: [Course Overview](../reproducibility.md)
