---
title: Creating a Repository
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Create a local Git repository.
- Describe the purpose of the `.git` directory.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Where does Git store information?

::::::::::::::::::::::::::::::::::::::::::::::::::

Now that Git is configured, we can start using it.

First, let's create a new directory in the `Desktop` folder for our work 
and then change the current working directory to the newly created one:

```bash
$ cd ~/Desktop
$ mkdir weather
$ cd weather
```

Then we tell Git to make `weather` a [repository](../learners/reference.md#repository)
\-- a place where Git can store versions of our files:

```bash
$ git init
```

It is important to note that `git init` will create a repository that
can include subdirectories and their files---there is no need to create
separate repositories nested within the `weather` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `weather` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```bash
$ ls
```

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `weather` called `.git`:

```bash
$ ls -a
```

```output
.	..	.git
```

Git uses this special subdirectory to store all the information about the project,
including the tracked files and sub-directories located within the project's directory.
If we ever delete the `.git` subdirectory,
we will lose the project's history.

::: spoiler

### FCM Comparison

FCM, which wraps SVN, is a [centralised version control](https://about.gitlab.com/topics/version-control/what-is-centralized-version-control-system/) system.
There is one central repository stored on a server that we work from.

Git is an example of [distributed version control](https://about.gitlab.com/blog/2020/11/19/move-to-distributed-vcs/).
The `.git` directory contains the entire history of the repository.
Each colleague working on the same repository will have a backup of the
whole repository.
We recommend reading the GitLab links in this callout for more
benefits of Git and distributed version control systems over FCM/SVN.

:::

We can now start using one of the most important git commands, which is particularly helpful to beginners. `git status` tells us the status of our project, and better, a list of changes in the project and options on what to do with those changes.
We can use it as often as we want, whenever we want to understand what is going on.

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

::: spoiler

### FCM Comparison

`git status` is equivalent to:

```bash
$ fcm status
```

or

```bash
$ fcm info
```

:::

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

## Initial Commit

As soon as you initialise your repository
you should make an initial commit.
All repositories should have a `README` file
which outlines the purpose of the repository
and other useful information.
For now we will create the file with just
the repository name, **Weather** as the title:

```bash
$ echo "# Weather" > README.md
$ cat README.md
```

```output
# Weather
```

Now add and commit the `README.md` file
using the `git add` and `git commit` commands:

```bash
$ git add README.md
$ git commit -m "Initial commit"
```

```output
[main (root-commit) 6f12a47] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

You've just added your first file to be version controlled with Git!
This first commit is the special **root-commit**.
It is the start of your version control history and
like all commits has been given a unique alphanumeric hash (`6f12a47`).
In the next few episodes you will explore
tracking changes with `git add` and `git commit` in detail,
and learn how to inspect your repositories history.

::: callout

### README Files

All repositories should have a `README` file.
The `README` file describes what is in your repository.
The [makeareadme](https://www.makeareadme.com/) website is a great
resource for `README` templates and inspiration.

The `README.md` file we added is a [Markdown](https://www.markdownguide.org/basic-syntax/)
file.
Markdown is a simple markup language and
GitHub can render Markdown files natively.
The GitHub documentation pages on [Writing on GitHub](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
have more info on writing in Markdown for GitHub.

:::

:::::::::::::::::::::::::::::::::::::::  challenge

## Places to Create Git Repositories

Along with tracking information about weather (the project we have already created),
you might also want to track information about clouds specifically.
Imagine you create a `clouds` project inside your `weather`
project with the following sequence of commands:

```bash
$ cd ~/Desktop    # return to Desktop directory
$ cd weather      # go into weather directory, which is already a Git repository
$ ls -a           # ensure the .git subdirectory is still present in the weather directory
$ mkdir clouds    # make a sub-directory weather/clouds
$ cd clouds       # go into clouds subdirectory
$ git init        # make the clouds subdirectory a Git repository
$ ls -a           # ensure the .git subdirectory is present indicating we have created a new Git repository
```

Is the `git init` command, run inside the `clouds` subdirectory, required for
tracking files stored in the `clouds` subdirectory?

:::::::::::::::  solution

## Solution

No. You do not need to make the `clouds` subdirectory a Git repository
because the `weather` repository will track all files, sub-directories, and
subdirectory files under the `weather` directory.  Thus, in order to track
all information about clouds, you only needed to add the `clouds` subdirectory
to the `weather` directory.

Additionally, Git repositories can interfere with each other if they are "nested":
the outer repository will try to version-control
the inner repository. Therefore, it's best to create each new Git
repository in a separate directory. To be sure that there is no conflicting
repository in the directory, check the output of `git status`. If it looks
like the following, you are good to go to create a new repository as shown
above:

```bash
$ git status
```

```output
fatal: Not a git repository (or any of the parent directories): .git
```

:::::::::::::::::::::::::

## Correcting `git init` Mistakes

A colleague explains to you how a nested repository is redundant and may cause confusion
down the road. You would like to go back to a single git repository.
How can you undo the last `git init` in the `clouds` subdirectory?

:::::::::::::::  solution

## Solution -- USE WITH CAUTION!

### Background

Removing files from a Git repository needs to be done with caution. But we have not learned
yet how to tell Git to track a particular file; we will learn this in the next episode. Files
that are not tracked by Git can easily be removed like any other "ordinary" files with

```bash
$ rm filename
```

Similarly a directory can be removed using `rm -r dirname`.
If the files or folder being removed in this fashion are tracked by Git, then their removal
becomes another change that we will need to track, as we will see in the next episode.

### Solution

Git keeps all of its files in the `.git` directory.
To recover from this little mistake, you can remove the `.git`
folder in the clouds subdirectory by running the following command from inside the `weather` directory:

```bash
$ rm -rf clouds/.git
```

But be careful! Running this command in the wrong directory will remove
the entire Git history of a project you might want to keep.
In general, deleting files and directories using `rm` from the command line cannot be reversed.
Therefore, always check your current directory using the command `pwd`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git init` initializes a repository.
- Git stores all of its repository data in the `.git` directory.

::::::::::::::::::::::::::::::::::::::::::::::::::
