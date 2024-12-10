---
title: FCM vs Git Cheat Sheet
---

## FCM vs Git

If you are familiar with FCM then this cheat sheet is for you!
Adapted from Tomek Trzeciak's (Met Office) comparison page.

### fcm branch-create BRANCH_NAME (fcm bc)

```bash
git checkout -b BRANCH_NAME
```

This also switches you to the new branch.

```bash
git checkout -m -b BRANCH_NAME
```

Switches you to the new branch and also keeps local changes.

### fcm branch-create --switch BRANCH_NAME (fcm bc --switch)

```bash
git checkout -b BRANCH_NAME main
```

### fcm branch-list (fcm bls)

```bash
git branch
```

### fcm checkout BRANCH_URL WORKING_COPY_PATH

```bash
git clone REPO_URL WORKING_COPY_PATH
cd WORKING_COPY_PATH
git checkout BRANCH_NAME
```

### fcm switch BRANCH_NAME

```bash
git checkout BRANCH_NAME
```

or the newer command:

```bash
git switch BRANCH_NAME
```

### fcm status

```bash
git status
```

### fcm info

```bash
git status
```

### fcm log

```bash
git log
```

### fcm merge BRANCH_NAME

```bash
git pull REPO BRANCH_NAME
```

or:

```bash
git fetch REPO
git merge REPO/BRANCH_NAME
```

### fcm conflicts

```bash
git mergetool
```

### fcm resolve --accept=working FILE

```bash
git add FILE
```

### fcm revert FILE

```bash
git checkout -- FILE
```

or the newer command:

```bash
git restore FILE
```

### fcm commit

```bash
git add FILE
git commit
git push
```

### fcm update

```bash
git pull REPO BRANCH_NAME
```

If `BRANCH_NAME` is not specified the default branch will be pulled.

### fcm branch-delete BRANCH_NAME (fcm brm BRANCH_NAME)

```bash
git branch -d BRANCH_NAME
```
