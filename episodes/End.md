---
title: End
teaching: 0
exercises: 0
---

This marks the end of the GitHub section and the workshop.
Please remember to fill out your post-workshop feedback.
This feedback is vital for us to keep improving the lesson
for other learners.

## Where to next?

The Git working practices lesson teaches you how to work
collaboratively with others using git and GitHub.

There are also a number of optional episodes after this page
which focus on open science and code which you can read
in your own time.

You can revisit this training anytime.
The Glossary, [Discussion](../learners/discuss.md),
and [FCM to Git](../learners/fcm-git_cheat_sheet.md),
and [Git cheatsheets](../learners/reference.md)
are also available on this site.

## Summary

You've now created a repository both locally on your computer
and remotely on GitHub.
You've developed changes on a feature branch,
reviewed the changes on GitHub and merged them into `main`.
Your local and remote repositories look something like this:

```mermaid
---
config:
  gitGraph:
    showCommitLabel: false
---
    gitGraph
        accDescr {A git graph showing the root-commit on the main branch and a new forecast branch with five commits. The forecast branch has been merged into main using a merge commit via a GitHub Pull Request.}
        commit id: 'Initial commit'
        branch forecast
        commit id: 'Create a md file with the forecast'
        commit id: 'Add tomorrows forecast to forecast.md'
        commit id: 'Modify the forecast to add a chance of Sun'
        commit id: 'Add in the temperature to the forecast and create the weather atlas file'
        commit id: 'Ignore png files and the data folder'
        checkout main
        merge forecast
```

A summary page outlining the steps we've taken to create
a new repository locally and connect it to a GitHub remote
can be found in the extra [Quick Start Repository Guide](../learners/repo-quick-start.md).
