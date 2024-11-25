---
title: Reverting Changes
teaching: 15
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Restore old versions of files.
- Undo commits.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I recover old versions of files?

::::::::::::::::::::::::::::::::::::::::::::::::::

All right! So
we can save changes to files and see what we've changed. Now, how
can we restore older versions of things?
Let's suppose we change our mind about the last update to
`forecast.md` (the "ill-considered change").

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   forecast.md

no changes added to commit (use "git add" and/or "git commit -a")
```

We can put things back the way they were
by using `git restore`:

```bash
$ git restore forecast.md
$ cat forecast.md
```

```output
# Forecast
## Today
Cloudy with a chance of sun.
Mild temperatures around 16 °C.
## Tomorrow
Morning rainbows followed by light showers.
```

As you might guess from its name,
`git restore` restores an old version of a file.
By default,
it recovers the version of the file recorded in `HEAD`,
which is the last saved commit.

::: spoiler

### FCM Comparison

`git restore` is equivalent to:

```bash
$ fcm revert FILE
```

:::

## Restoring a file from further back

If we want to go back even further,
we can use a commit identifier instead, using `-s` option:

```bash
$ git restore -s f22b25e forecast.md
```

```bash
$ cat forecast.md
```

```output
# Forecast
## Today
Cloudy with a chance of pizza.
```

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   forecast.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Notice that the changes are not currently in the staging area, and have not been committed. 
If we wished, we can put things back the way they were at the last commit by using `git restore` to overwrite
the working copy with the last committed version:

```bash
$ git restore forecast.md
$ cat forecast.md
```

```output
# Forecast
## Today
Cloudy with a chance of sun.
Mild temperatures around 16 °C.
## Tomorrow
Morning rainbows followed by light showers.
```

It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to discard.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `f22b25e`. We use the `.` to mean all files:

![](fig/git-restore.svg){alt='A diagram showing how git restore can be used to restore the previous version of two files'}

The fact that files can be reverted one by one
tends to change the way people organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction
without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.

## Reverting Changes

Generally it is best to spot and revert mistakes before the commit stage.
The table below summarises how to revert a change depending on where in the 
commit process you are:

| **To revert files you have ...** |     **git command**     |
|:--------------------------------:|:-----------------------:|
| modified                         | `$ git restore <files>` |
| staged                           | `$ git reset <files>`   |
| committed                        | `$ git revert <commit>` |

:::::::::::::::::::::::::::::::::::::::  challenge

## Recovering Older Versions of a File

Jennifer has made changes to the Python script that she has been working on for weeks, and the
modifications she made this morning "broke" the script and it no longer runs. She has spent
\~ 1hr trying to fix it, with no luck...

Luckily, she has been keeping track of her project's versions using Git! Which commands below will
let her recover the last committed version of her Python script called
`data_cruncher.py`?

1. `$ git restore`

2. `$ git restore data_cruncher.py`

3. `$ git restore -s HEAD~1 data_cruncher.py`

4. `$ git restore -s <unique ID of last commit> data_cruncher.py`

5. Both 2 and 4

:::::::::::::::  solution

## Solution

The answer is (5)-Both 2 and 4.

The `restore` command restores files from the repository, overwriting the files in your working
directory. Answers 2 and 4 both restore the *latest* version *in the repository* of the file
`data_cruncher.py`. Answer 2 uses `HEAD` to indicate the *latest*, whereas answer 4 uses the
unique ID of the last commit, which is what `HEAD` means.

Answer 3 gets the version of `data_cruncher.py` from the commit *before* `HEAD`, which is NOT
what we wanted.

Answer 1 results in an error. You need to specify a file to restore. If you want to restore all files
you should use `git restore .`



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Reverting a Commit

Ahmed is collaborating with colleagues on a Python script. He
realizes his last commit to the project's repository contained an error, and
wants to undo it. Ahmed wants to undo it correctly so everyone in the project's
repository gets the correct change. The command `git revert [erroneous commit ID]` will create a
new commit that reverses the erroneous commit.

The command `git revert` is
different from `git restore -s [commit ID] .`.
`git restore` restores files within the local repository to a previous state, 
whereas `git revert` restores the files to a previous state **and**
adds then commits these changes to the local repository.
So `git revert` here is the same as `git restore -s [commit ID]`  
followed by `git commit -am Reverts: [commit]`.

`git revert` undoes a whole commit whereas
`git restore -s` can be used restore individual files.

Below are the right steps and explanations for Ahmed to use `git revert`,
what is the missing command?

1. `________ # Look at the git history of the project to find the commit ID`

2. Copy the ID (the first few characters of the ID, e.g. 0b1d055).

3. `git revert [commit ID]`

4. Type in the new commit message.

5. Save and close.

:::::::::::::::  solution

## Solution

The command `git log` lists project history with commit IDs.

The command `git show HEAD` shows changes made at the latest commit, and lists
the commit ID; however, Ahmed should double-check it is the correct commit, 
and no one else has committed changes to the repository.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git restore` recovers old versions of files.
- `git reset` undoes staged changes.
- `git revert` reverses a commit.

::::::::::::::::::::::::::::::::::::::::::::::::::
