## The Basics of Git

Version control systems start with a base version of the document and
then record changes you make each step of the way. You can
think of it as a recording of your progress: you can rewind to start at the base
document and play back each change you made, eventually arriving at your
more recent version.

![Changes Are Saved Sequentially](./fig/play-changes.svg)

Once you think of changes as separate from the document itself, you
can then think about "playing back" different sets of changes on the base document, ultimately
resulting in different versions of that document. For example, two users can make independent
sets of changes on the same document.

![Different Versions Can be Saved](./fig/versions.svg)

Unless there are conflicts, you can even incorporate two sets of changes into the same base document.

![Multiple Versions Can be Merged](./fig/merge.svg)

A version control system is a tool that keeps track of these changes for us,
effectively creating different versions of our files. It allows us to
decide which changes will be made to the next version (each record of these changes is called a
"commit", and keeps useful metadata about them. The
complete history of commits for a particular project and their metadata make up
a "repository". Repositories can be kept in sync
across different computers, facilitating collaboration among different people.


## Setting up Git

Log on to Stampede2, load the Git module, check which version of Git is in your `PATH`.

```
$ ssh username@stampede2.tacc.utexas.edu
(enter password)
(enter token)

$ module list

Currently Loaded Modules:
  1) intel/17.0.4   2) impi/17.0.3   3) git/2.9.0   4) autotools/1.1   5) python/2.7.13   6) xalt/2.0.7   7) TACC

$ git --version
git version 2.9.0
```

When we use Git on a new computer for the first time,
we need to configure a few things. Below are a few examples
of configurations we will set as we get started with Git:

 * our name and email address,
 * and that we want to use these settings globally (i.e. for every project).

On a command line, Git commands are written as `git verb`,
where `verb` is what we actually want to do. So here is how
we set up our environment on Stampede2:

```
$ git config --global user.name "Joe Allen"
$ git config --global user.email "wallen@tacc.utexas.edu"
```

Please use your own name and email address. This user name and email will be associated with your subsequent Git activity,
which means that any changes pushed to
[GitHub](https://github.com/),
[BitBucket](https://bitbucket.org/),
[GitLab](https://gitlab.com/) or
another Git host server
in the future will include this information.


## Local Versions of Git

A key benefit of Git is that it is platform agnostic. You can use it equally to interact with the same files from your laptop, from a lab computer, or from a cluster. Although we will not be using Git on our laptops in this workshop, instructions to install Git locally can be found [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

Previous: [Version Control](reproducibility_git_01.md) | Next: [Create a New Repository](reproducibility_git_03.md) | Top: [Course Overview](../../index.md)
