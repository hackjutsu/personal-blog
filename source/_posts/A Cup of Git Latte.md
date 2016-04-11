
# A Cup of Git Latte

title: A Cup of Git Latte
date: 2016-03-12 18:00:01
tags:
- Git

----------

<img src="https://git-scm.com/book/en/v2/book/01-introduction/images/areas.png" style="max-height: 350px;"/>

Quick reference for some frequently used Git commands.

<!--more-->
## Set up a new repository

```bash
git init
git pull https://github.com/registercosmo/Cheatsheet.git
git remote add origin https://github.com/registercosmo/Cheatsheet.git
git add .
git status
git commit
git commit -m "comments"
git push -u origin master
```
## Working with remote repositories

### Set up remote repository

```bash
# set up a new remote repo (origin)
git remote add origin <new_remote_repo>

# change URL for a remote repo (origin)
git remote set-url origin <new_remote_repo>

# list all the remote repositories URLs
git remote -v
```

### Sync with the remote repository

```bash
# To pull changes to local workspace (= fetch + merge)
git pull <remote_repo> master

# Download lastest changes(objects and refs) from remote repo
git fetch <remote_repo>

# To merge the local repo and fetched changes
git merge FETCH_HEAD

# To merge the origin/feature branch to the current branch
git merge origin/feature
```

### More about pull
>**git pull** ------ Fetch from and integrate with another repository or a local branch

`git pull` incorporates changes from a remote repository into the current branch. In its default mode, `git pull` is shorthand for `git fetch` followed by `git merge FETCH_HEAD`.

More precisely, `git pull` runs `git fetch` with the given parameters and calls git merge to **merge the retrieved branch heads into the current branch**. See the *More about config variables* for more details.

- Update the remote-tracking branches for the repository we cloned from, then merge one of them into our current branch:
```bash
git pull, git pull origin
```
  Normally the branch merged in is the `HEAD` of the remote repository, but the choice is determined by the `branch.<name>.remote` and `branch.<name>.merge` options.

- To merge a specific remote branch `next` into our current branch, we can run:
```bash
git pull origin next
```
  This leaves a copy of next temporarily in `FETCH_HEAD`, but does not update any remote-tracking branches. Using remote-tracking branches, the same can be done by invoking fetch and merge:
```bash
git fetch origin
git merge origin/next
```
If we tried a pull which resulted in complex conflicts and would want to start over, we can recover with `git reset`.


## Fix the previous commit

```bash
git commit --amend
```

## Undo changes

```bash
# unstage changes but keep the local changes in the working space
git reset HEAD

# discard all the changes in the stage and working space
# and match the working space the commit B
git reset --hard B

# move the HEAD to commit B, no modification will be made
# to the stage and the working space
git reset --soft B
```

## Untrack files

```bash
git rm --cached <file_name>
```

## About configs

```bash
# configs for all repositories
git config --global user.name # check current global user.name
git config --global user.name <My username> # set global user.name
git config --global user.email # check current global user.email
git config --gloabl user.email=myemail@example.com # set global user.name

# configs for the current local repository
git config user.name # check current local user.name
git config user.name <My username> # set local user.name
git config user.email # check current local user.email
git config user.email=myemail@example.com # set local user.name

# list all the configs
git config --list

# Remove the configs
git config --global --unset-all user.name # global

# Change the configs
git config --global --replace-all user.name <New User Name> # global
```

### More about config variables
- **`branch.<name>.remote`**
  When on `branch <name>`, it tells *git fetch* and *git push* which remote to fetch from/push to.

- **`branch.<name>.merge`**
  Defines, together with `branch.<name>.remote`, the upstream branch for the given branch. It tells *git fetch/git pull/git rebase* which branch to merge and can also affect *git push* (see push.default). When in `branch <name>`, it tells *git fetch* the default refspec to be marked for merging in `FETCH_HEAD`.

- **`branch.<name>.pushRemote`**
  When on `branch <name>`, it overrides `branch.<name>.remote` for pushing. It also overrides `remote.pushDefault` for pushing from `branch <name>`. When we pull from one place (e.g. our upstream) and push to another place (e.g. our own publishing repository), we would want to set `remote.pushDefault` to specify the remote to push to for all branches, and use this option to override it for a specific branch.

- **`remote.pushDefault`**
  The remote to push to by default. Overrides `branch.<name>.remote` for all branches, and is overridden by `branch.<name>.pushRemote` for specific branches.

## About branches

```bash
# fetch remote branches from Github
git remote add origin <ULR> # we have to set origin first
git pull # it will pull(fetch + merge) all available branches from repositories

# check info for all available branches
git branch

# create a new branch
git branch  <new_branch_name>

# switch to a specific branch
git checkout  <new_branch_name>

# create and switch to a new branch
git checkout -b <new_branch_name>

# check the current branch
git status

# rename a branch
git branch -m <old_branch_name> <new_branch_name>

# merge a specific branch to the current branch
git merge <another_branch_name>

# delete a branch locally
git branch -d <branch_name>

# delete a branch remotely
git push origin :<branch_name>
```
## About diff

```bash
# inspect all the modifications between workspace and stage
git diff

# inspect all the modifications between stage and local repo
git diff --cached
```

## About resolving conflicts

```bash
git add <resolved file>
```

## Advanced topics

```bash
# get the SHA-1 key for a specific contents
echo 'test content' | git hash-object -w --stdin

# get the content of a SHA-1 key
git cat-file -p <key>

# get a object type for a SHA-1 key
git cat-file -t <key>
```


