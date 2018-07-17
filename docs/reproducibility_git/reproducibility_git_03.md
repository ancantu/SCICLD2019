## Create a New Repository on the Command Line


First, let's create a new folder in our `$HOME` directory on Stampede2 to organize our work:

```
$ cd ~/
$ mkdir my_first_repo/
$ cd my_first_repo/       # currently empty
```

Then we will use a Git command to initialize this directory as a new Git repository - or a place where Git can start to organize versions of our files.
```
$ git init
Initialized empty Git repository in /home1/03439/wallen/my_first_repo/.git/
```


If we use `ls -la`, we can see that Git has created a hidden directory within `my_first_repo` called `.git`:

```
$ ls -a
./  ../  .git/
```

Use the `tree` command to get a clearer view of the contents of the `.git/` directory:

```
$ tree .git/
.git/
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

9 directories, 13 files
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

Previous: [The Basics of Git](reproducibility_git_02.md) | Next: [Tracking Changes](reproducibility_git_04.md) | Top: [Course Overview](../../index.md)
