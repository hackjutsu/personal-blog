
# A Cup of Git Latte

title: A Cup of Git Latte
date: 2016-03-12 18:00:01
tags:
- Git

----------

<img src="https://git-scm.com/book/en/v2/book/01-introduction/images/areas.png" style="max-height: 350px;"/>

Quick reference for some frequently used Git commands.
<!--more-->

>**Terminology convention:** working tree --> stage --> local repo --> remote repo

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

----
## Working with remote repositories

### Set up remote repository

```bash
# set up a new remote repo (origin)
git remote add origin <new_remote_repo>

# change URL for a remote repo (origin)
git remote set-url origin <new_remote_repo>

# list all the remote repo URLs
git remote -v

# rename a remote repo (e.g. origin -> my_source)
git remote rename origin my_source
```

### Sync with the remote repository

```bash
# To pull changes to working tree (= fetch + merge)
git pull <remote_repo> master

# Download lastest changes(objects and refs) from remote repo
git fetch <remote_repo>

# To merge the local repo and fetched changes
git merge FETCH_HEAD

# To merge the origin/feature branch to the current branch
git merge origin/feature
```
## More about fetch, merge, pull
### More about fetch
>**git fetch** ------ Download objects and refs from another repository

`git fetch` fetches *branches* and/or *tags* (collectively, *refs*) from one or more other repositories, along with the objects necessary to complete their histories. Remote-tracking branches are updated.

When no remote is specified, by default the `origin` remote will be used, unless there’s an upstream branch configured for the current branch.

The names of refs that are fetched, together with the object names they point at, are written to `.git/FETCH_HEAD`.This information may be used by scripts or other git commands, such as `git-pull`.  The `FETCH_HEAD` is just a reference to the tip of the last fetch, whether that fetch was initiated directly using the fetch command or as part of a pull.

#### branch.< name >.fetch

When we have the `branch.<name>.fetch` set as:
```
[remote "origin"] fetch = +refs/heads/*:refs/remotes/origin/*
```

This configuration is used in two ways:


##### Without specifying branches

```bash
git fetch origin
```
The above command copies all branches from the remote `refs/heads/` namespace and stores them to the local `refs/remotes/origin/` namespace, unless the `branch.<name>.fetch` option is used to specify a non-default refspec.

##### Specifying branches
```bash
git fetch origin master
```
This command will fetch only the master branch. The `remote.<repository>.fetch` values determine which remote-tracking branch, if any, is updated.

### More about merge
>**git merge** ------ Join two or more development histories together

`git merge` incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch.

Assume the following history exists and *the current branch is "master"*:

      A---B---C topic
     /
    D---E---F---G master

Then `git merge topic` will replay the changes made on the topic branch since it diverged from master (E) until its current commit (C) on top of master, and record the result in a new commit (H) along with the names of the two parent commits and a log message from the user describing the changes.

      A---B---C topic
	 /         \
    D---E---F---G---H master

#### Pre-merge checks
Before performing any merge, we should make sure our codes are in good shape and commit all local changes. `git pull` and `git merge` will stop without doing anything when local uncommitted changes overlap with files that `git pull`/`git merge` may need to update.

To avoid recording unrelated changes in the merge commit, `git pull` and `git merge` will also abort if there are any changes registered in the index relative to the `HEAD` commit.  (One exception is when the changed index entries are in the state that would result from the merge already.)

If all named commits are already ancestors of `HEAD`, `git merge` will exit early with the message "Already up-to-date."

#### Fast-forward merge
Often the current branch head is an ancestor of the named commit. This is the most common case especially when invoked from `git pull`: we are tracking an upstream repository, we have committed no local changes, and now we want to update to a newer upstream revision. In this case, a new commit is not needed to store the combined history; instead, the `HEAD` (along with the index) is updated to point at the named commit, without creating an extra merge commit.

This behavior can be suppressed with the `--no-ff` option.

#### True merge
Except in a fast-forward merge (see above), the branches to be merged must be tied together by a merge commit that has both of them as its parents.

A merged version reconciling the changes from all branches to be merged is committed, and our `HEAD`, index, and working tree are updated to it. It is possible to have modifications in the working tree as long as they do not overlap; the update will preserve them.

When it is not obvious how to reconcile the changes, the following happens:

1. The `HEAD` pointer stays the same.

2. The `MERGE_HEAD` ref is set to point to the other branch head.

3. Paths that merged cleanly are updated both in the index file and in our working tree.

4. For conflicting paths, the index file records up to three versions: stage 1 stores the version from the common ancestor, stage 2 from `HEAD`, and stage 3 from `MERGE_HEAD` (we can inspect the stages with `git ls-files -u`). The working tree files contain the result of the "merge" program; i.e. 3-way merge results with familiar conflict markers `<<<` `===` `>>>`.

5. No other changes are made. In particular, the local modifications we had before we started merge will stay the same and the index entries for them stay as they were, i.e. matching `HEAD`.

If we tried a merge which resulted in complex conflicts and want to start over, we can recover with `git merge --abort`.

#### Resolve conflicts
After seeing a conflict, we can do two things:

- **Decide not to merge.** The only clean-ups we need are to reset the index file to the `HEAD` commit to reverse 2. and to clean up working tree changes made by 2. and 3.; `git merge --abort` can be used for this.

- **Resolve the conflicts.** Git will mark the conflicts in the working tree. Edit the files into shape and git add them to the index. Use `git commit` to seal the deal.

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


----
## Fix the previous commit

```bash
git commit --amend
```

----
## Undo changes

```bash
# unstage changes but keep the local changes in the working tree
git reset HEAD

# discard all the changes in the stage and working tree
# and match the working tree the commit B
git reset --hard B

# move the HEAD to commit B, no modification will be made
# to the stage and the working tree
git reset --soft B
```

----
## Untrack files

```bash
git rm --cached <file_name>
```

----
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

# delete a branch localy
git branch -d <branch_name>

# delete a branch remotely
git push origin :<branch_name>

# push local branch A to remote branch B
git push origin <local branch>:<remote branch>
```

----
## About diff

```bash
# inspect all the modifications between working tree and stage
git diff

# inspect all the modifications between stage and local repo
git diff --cached
```

----

## About configs

```bash
# configs for all repositories
git config --global user.name # check current global user.name
git config --global user.name <My username> # set global user.name
git config --global user.email # check current global user.email
git config --gloabl user.email myemail@example.com # set global user.name

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

----
## View history for a file
```bash
# Show commit history of a specific file
git log --follow -p <file>

# Show details of a specific commit
git show <commit id>

# Show what revision and author last modified each line of a file
git blame <file>
```

----
## Files info about index/working tree
```bash
git ls-files
    (--[cached|deleted|others|ignored|stage|unmerged|killed|modified])
    (-[c|d|o|i|s|u|k|m])
```

----
## Advanced topics

```bash
# get the SHA-1 key for a specific contents
echo 'test content' | git hash-object -w --stdin

# get the content of a SHA-1 key
git cat-file -p <key>

# get a object type for a SHA-1 key
git cat-file -t <key>

# list the contents of a tree object
git ls-tree <tree_key>

```

----
## Resource
https://git-scm.com/
http://www.open-open.com/lib/view/open1328069889514.html

>**Disclaimer:** This is my personal notes for Git reference. Some of the notes come from my daily experience and some come from other existing Git tutorials or documentations.
