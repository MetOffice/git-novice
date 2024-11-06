---
title: Exploring History
teaching: 25
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what the HEAD of a repository is and how to use it.
- Identify and use Git commit numbers.
- Compare various versions of tracked files.
- Restore old versions of files.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I identify old versions of files?
- How do I review my changes?
- How can I recover old versions of files?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Viewing a Repositories History

If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

```bash
$ git log
```

```output
commit cdb7fa654c3f5aee731a655e57f2ba74d9c74582 (HEAD -> main)
Author: Joanne Simpson <j.simpson@weather.org>
Date:   Mon Nov 4 18:35:21 2024 +0000

    Add in the temperature to the forecast and create the weather atlas file
```

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created.

::: spoiler

### FCM Comparison

`git log` is equivalent to:

```bash
$ fcm log
```

:::

:::::::::::::::::::::::::::::::::::::::::  callout

## Paging the Log

When the output of `git log` is too long to fit in your screen,
`git` uses a program to split it into pages of the size of your screen.
When this "pager" is called, you will notice that the last line in your
screen is a `:`, instead of your usual prompt.

- To get out of the pager, press <kbd>Q</kbd>.
- To move to the next page, press <kbd>Spacebar</kbd>.
- To search for `some_word` in all pages,
  press <kbd>/</kbd>
  and type `some_word`.
  Navigate through matches pressing <kbd>N</kbd>.
  

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Limit Log Size

To avoid having `git log` cover your entire terminal screen, you can limit the
number of commits that Git lists by using `-N`, where `N` is the number of
commits that you want to view. For example, if you only want information from
the last commit you can use:

```bash
$ git log -1
```

```output
commit cdb7fa654c3f5aee731a655e57f2ba74d9c74582 (HEAD -> main)
Author: Joanne Simpson <j.simpson@weather.org>
Date:   Mon Nov 4 18:35:21 2024 +0000

    Add in the temperature to the forecast and create the weather atlas file
```

You can also reduce the quantity of information using the
`--oneline` option:

```bash
$ git log --oneline
```

```output
cdb7fa6 (HEAD -> main) Add in the temperature to the forecast and create the weather atlas file
62a9457 Modify the forecast to add a chance of Sun
d3e4637 Add tomorrows forecast to forecast.md
590c40c Create a md file with the forecast
```

You can also combine the `--oneline` option with others. One useful
combination adds `--graph` to display the commit history as a text-based
graph and to indicate which commits are associated with the
current `HEAD`, the current branch `main`, or
[other Git references][git-references]:

```bash
$ git log --oneline --graph
```

```output
* cdb7fa6 (HEAD -> main) Add in the temperature to the forecast and create the weather atlas file
* 62a9457 Modify the forecast to add a chance of Sun
* d3e4637 Add tomorrows forecast to forecast.md
* 590c40c Create a md file with the forecast
```

::::::::::::::::::::::::::::::::::::::::::::::::::

## Identifying Commits

As we saw in the previous episode, we can refer to commits by their
identifiers.  You can refer to the *most recent commit* of the working
directory by using the identifier `HEAD`.

We've been adding small changes at a time to `forecast.md`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `forecast.md`, adding yet another line.

```bash
$ nano forecast.md
$ cat forecast.md
```

```output
# Forecast
## Today
Cloudy with a chance of sun.
Mild temperatures around 16 °C.
## Tomorrow
Morning rainbows followed by light showers.
An ill-considered change.
```

Now, let's see what we get.

```bash
$ git diff HEAD forecast.md
```

```output
diff --git a/forecast.md b/forecast.md
index b36abfd..0848c8d 100644
--- a/forecast.md
+++ b/forecast.md
@@ -4,3 +4,4 @@ Cloudy with a chance of sun.
 Mild temperatures around 16 °C.
 ## Tomorrow
 Morning rainbows followed by light showers.
+An ill-considered change.
```

which is the same as what you would get if you leave out `HEAD` (try it).  The
real goodness in all this is when you can refer to previous commits.  We do
that by adding `~1`
(where "~" is "tilde", pronounced [**til**\-d*uh*])
to refer to the commit one before `HEAD`.

```bash
$ git diff HEAD~1 forecast.md
```

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:

```bash
$ git diff HEAD~2 forecast.md
```

```output
diff --git a/forecast.md b/forecast.md
index df0654a..b36abfd 100644
--- a/forecast.md
+++ b/forecast.md
@@ -1,5 +1,7 @@
 # Forecast
 ## Today
-Cloudy with a chance of pizza.
+Cloudy with a chance of sun.
+Mild temperatures around 16 °C.
 ## Tomorrow
 Morning rainbows followed by light showers.
+An ill-considered change.
```

We could also use `git show` which shows us what changes we made at an older commit as
well as the commit message, rather than the *differences* between a commit and our
working directory that we see by using `git diff`.

```bash
$ git show HEAD~2 forecast.md
```

```output
Author: Joanne Simpson <j.simpson@weather.org>
Date:   Mon Nov 4 18:16:29 2024 +0000

    Add tomorrows forecast to forecast.md

diff --git a/forecast.md b/forecast.md
index d8bc6ce..5b5d97e 100644
--- a/forecast.md
+++ b/forecast.md
@@ -1,3 +1,5 @@
 # Forecast
 ## Today
 Cloudy with a chance of pizza.
+## Tomorrow
+Morning rainbows followed by light showers.
```

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1`
means "the previous commit",
while `HEAD~123` goes back 123 commits from where we are now.

We can also refer to commits using
those long strings of digits and letters
that both `git log` and `git show` display.
These are unique IDs for the changes,
and "unique" really does mean unique:
every change to any set of files on any computer
has a unique 40-character identifier.
Our first commit was given the ID
`f22b25e3233b4645dabd0d81e651fe074bd8e73b`,
so let's try this:

```bash
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b forecast.md
```

```output
diff --git a/forecast.md b/forecast.md
index df0654a..93a3e13 100644
--- a/forecast.md
+++ b/forecast.md
@@ -1,3 +1,7 @@
 # Forecast
 ## Today
-Cloudy with a chance of pizza.
+Cloudy with a chance of sun.
+Mild temperatures around 16 °C.
+## Tomorrow
+Morning rainbows followed by light showers.
+An ill-considered change.
```

That's the right answer,
but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters (typically seven for normal size projects):

```bash
$ git diff f22b25e forecast.md
```

```output
diff --git a/forecast.md b/forecast.md
index df0654a..93a3e13 100644
--- a/forecast.md
+++ b/forecast.md
@@ -1,3 +1,7 @@
 # Forecast
 ## Today
-Cloudy with a chance of pizza.
+Cloudy with a chance of sun.
+Mild temperatures around 16 °C.
+## Tomorrow
+Morning rainbows followed by light showers.
+An ill-considered change.
```

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

:::::::::::::::::::::::::::::::::::::::  challenge

## Understanding Workflow and History

What is the output of the last command in

```bash
$ cd weather
$ echo "Global Climate Data" > CMIP7.md
$ git add CMIP7.md
$ echo "Data from the 7th model intercomparison project" >> CMIP7.md
$ git commit -m "Adds in CMIP7 data file"
$ git restore CMIP7.md
$ cat CMIP7.md  # this will print the content of CMIP7.md on screen
```

1. ```output
  Data from the 7th model intercomparison project
  ```
2. ```output
  Global Climate Data
  ```
3. ```output
  Global Climate Data
  Data from the 7th model intercomparison project
  ```
4. ```output
  Error because you have changed CMIP7.md without committing the changes
  ```

:::::::::::::::  solution

## Solution

The answer is 2.

The changes to the file from the second `echo` command are only applied to the working copy,
not the version in the staging area.

So, when `git commit -m "Adds in CMIP7 data file"` is executed,
the version of `CMIP7.md` committed to the repository is the one from the staging area and
only has one line, `Global Climate Data`.

At this time, the working copy still has the second line (and
`git status` will show that the file is modified).
However, `git restore CMIP7.md`
removes all unstaged modifications to the `CMIP7.md` file, so the second line is removed.
So, `cat CMIP7.md` will output

```output
Global Climate Data
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Checking Understanding of `git diff`

Consider this command: `git diff HEAD~9 forecast.md`. What do you predict this command
will do if you execute it? What happens when you do execute it? Why?

Try another command, `git diff [ID] forecast.md`, where [ID] is replaced with
the unique identifier for your most recent commit. What do you think will happen,
and what does happen?


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Explore and Summarize Histories

Exploring history is an important part of Git, and often it is a challenge to find
the right commit ID, especially if the commit is from several months ago.

Imagine the `weather` project has more than 50 files.
You would like to find a commit that modifies some specific text in `forecast.md`.
When you type `git log`, a very long list appeared.
How can you narrow down the search?

Recall that the `git diff` command allows us to explore one specific file,
e.g., `git diff forecast.md`. We can apply a similar idea here.

```bash
$ git log forecast.md
```

Unfortunately some of these commit messages are very ambiguous, e.g., `update files`.
How can you search through these files?

Both `git diff` and `git log` are very useful and they summarize a different part of the history
for you.
Is it possible to combine both? Let's try the following:

```bash
$ git log --patch forecast.md
```

You should get a long list of output, and you should be able to see both commit messages and
the difference between each commit.

Question: What does the following command do?

```bash
$ git log --patch HEAD~9 *.md
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git diff` displays differences between commits.
- `git restore` recovers old versions of files.
- `git reset` undoes staged changes.
- `git revert` reverses a commit.

::::::::::::::::::::::::::::::::::::::::::::::::::
