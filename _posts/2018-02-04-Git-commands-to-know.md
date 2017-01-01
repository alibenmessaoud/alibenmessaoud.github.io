---
layout: post
title: Useful git Commands You Need to Know
tags: git cli
---

In this article, we are going to go over some [Git](https://git-scm.com/) commands that may make your life easier. Some of these you may have used, some may be new to you.

## Remove file from the last commit

```sh
git rm —-cached <file-to-remove>
git commit —-amend
```

This command helps to commit by combining `rm` and `commit --amend` commands.

## Select particular files from git stash

```sh
git checkout <stash_id> -- <file_path>

Example
git checkout stash@{1} -- main.js
```

Picking particular files from git stash is very helpful.

As stated in the above command, you have to checkout to a stash with particular file paths available in the stash.

## Interactively Commit Some Parts Of a File

```bash
git add -p
```

Once this command runs, we’ll be asked about "hunks". "Hunks" are the *chunks* of code that we'll decide what to do with each tracked file.

We’ll see the question, "Stage this hunk [y, n, g, a, d, e, ?]?" at the bottom of the terminal.

The letters mean the following things:

- y: stage this hunk
- n: do not stage this hunk
- g: select a different hunk to go to
- a: stage this hunk and all the other hunks in this file
- d: do not stage this hunk or any of the other hunks in this file
- e: edit the current hunk manually
- ?: print help

## How to create an empty commit

```
git commit --allow-empty -m <commit_msg>

Example
git commit --allow-empty -m "Trigger CI"
```

The empty commit has no files to track.

You may wonder why to create an empty commit. Sometimes you may need to trigger some deployment or you need to test some integration. So pushing an empty commit can do the magic here.

## List all branches ordered by most recent commits

```
git branch --sort=committerdate
```

The above command will list branches in order of the most recent commits. So you can see branches that you are using it most in recent times.

Very helpful and it comes in handy when you are working with multiple branches.

## Switch to the Previous Branch

```sh
git checkout -
```

This command allows you to quickly switch to the previously checked out branch. On a general note `-` is an alias for the previous branch.  

## Showing diff

We have already seen this command in two of our previous issues.

```sh
git log master..develop
```

This command will help you show all the commits from develop but that are not present in the master branch. 

In this way, you can know that how many new commits are added to the develop branch that is not present in the master branch. And make sure you have the updated changes in the local before comparing.

```sh
git diff --staged # for staged changes
git diff # for unstaged changes
```

Helps to see the differences in your staged (or unstaged) changes.

## Rename Branches Locally

```sh
git branch -m old-name new-name
```

## Undo the last commit, but keep the changes!

```sh
git reset —soft HEAD~1
```

When using this command, it’s going to remove the last commit from the current branch. But the cool thing about this command is that the changes won’t disappear! All the changes will stay in the working tree!

## How to mark a commit as a fix of the previous commit

```sh
git commit --fixup <commit-hash>

Example
git commit --fixup 30b6c97
```

The fixup option helps you to create a commit which will be considered as a fix of the previous commit.

I know you can use rebase or amend to fix the previous commit. But there is a possibility that you have pushed the code and may not be in the position to force push it.

## Optimize the repository locally

```sh
git gc --prune=now --aggressive
```

## Modify The Most Recent Commit

```sh
git commit --amend
```

`—-amend` allows to append staged changes (e.g. to add a forgotten file) to the previous commit. Adding `—-no-edit` will amend the last commit without changing its commit message. 

If there are no changes, `-—amend` will allow you to reword the last commit message.

## Interactively Stash Selected Parts of Files

```sh
git stash -p
```

Similar to `git add -p`, you can use the `-p` option to interactively select parts of each tracked file to stash.

## Logging

```sh
git log
git log --oneline # more succinct output
git log --graph # with a visual graph of branches
```

## See what I did before

```
git reset <commit-sha>
```

This will uncommit and unstage those changes but leave those files in the working directory.

## Switch to another branch

```
git switch branch-name   # new syntax (as of Git 2.23)
git checkout branch-name # old syntax
```

As of [Git 2.23](https://www.infoq.com/news/2019/08/git-2-23-switch-restore/), we now have `git restore` (checkout file) and `git switch` (checkout branch) to remove confusion.

## Let's start over

```sh
git reset --hard HEAD
```

This will reset your local directory to match the latest commit and discard unstaged changes

### reset a file back to how it was

```
git restore <filename>     # new syntax (as of Git 2.23)
git checkout -- <filename> # old syntax
```

## Reset a file back to how it was

```
git restore <filename>     # new syntax (as of Git 2.23)
git checkout -- <filename> # old syntax
```

## Undo the last commit and rewrite history

```
git reset --hard HEAD~1
```

## Rewind back n commits

```
git reset --hard HEAD~n        # n is the last n commits
git reset --hard <commit-sha>  # or to a specific commit
```

`--hard`: Uncommit, unstage, and delete changes. There are also some other options like `--soft` to uncommit changes but leave those changes staged and `--mixed` (the default) to uncommit and unstage changes, but changes are left in the working directory.

### Bring in a specific file from a different branch

```
git checkout <branch-name> <file-name>
```

## Cleanup

```sh
git branch --no-color --merged | command grep -vE "^(\+|\*|\s*(master|develop|main)\s*$)" | command xargs -n 1 git branch -d
```

This command deletes all merged branches that you have locally except for master, developer or main.

```sh
git fetch --all --prune
```





*Last Update: 03/03/2021*