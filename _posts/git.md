---
layout: post
title: Deliver Better Software, Faster
tags: best-practice ci/cd
---

# Git Commands

## 📋 Logging

### What did I just do?

```
git log
git log --oneline # more succinct output
git log --graph # with a visual graph of branches
```

### View your "undo" history

```
git reflog
```

Because sometimes `git log` doesn't cut it, especially for commands that don't show up in your commit history.

`reflog` is basically your safety net after running "scary" commands like `git rebase`. You'll be able to see not only the commits you made, but each of the actions that led you there. See [this Atlassian article](https://www.atlassian.com/git/tutorials/refs-and-the-reflog) to learn more about how refs work.

### View your current state + any merge conflicts

```
git status
```

While `git status` is a pretty basic command we all learn early on, its importance as a learning tool for internalizing git fundamentals bears repeating. It can also help you navigate through a complicated rebase or merge.

### See the differences in your staged (or unstaged) changes

```
git diff --staged # for staged changes
git diff # for unstaged changes
```

### See the differences between two branches

```
git diff branch1..branch2
```

## 🧭 Navigation

### I want to see what I did before

```
git reset <commit-sha>
```

This will uncommit and unstage those changes but leave those files in the working directory.

### I want to switch to another branch

```
git switch branch-name   # new syntax (as of Git 2.23)
git checkout branch-name # old syntax
```

`git checkout` can be a bit confusing because it can work on the file **and** branch level. As of [Git 2.23](https://www.infoq.com/news/2019/08/git-2-23-switch-restore/), we now have `git restore` (checkout file) and `git switch` (checkout branch), which would be my suggestion if you're just starting out to avoid `checkout` confusion.

### I want to go back to the branch I was on

```
git switch -
```

## 📝 Modifications

### I dug myself into a rabbit hole, let's start over

```
git reset --hard HEAD
```

This will reset your local directory to match the latest commit and discard unstaged changes

### I want to reset a file back to how it was

```
git restore <filename>     # new syntax (as of Git 2.23)
git checkout -- <filename> # old syntax
```

### I want to undo the last commit and rewrite history

```
git reset --hard HEAD~1
```

### I want to rewind back n commits

```
git reset --hard HEAD~n        # n is the last n commits
git reset --hard <commit-sha>  # or to a specific commit
```

There is an important distinction between soft, mixed, and hard resets. Basically:

1. `--soft`: Uncommit changes but leave those changes staged
2. `--mixed` (the default): Uncommit and unstage changes, but changes are left in the working directory
3. `--hard`: Uncommit, unstage, and delete changes

### I've rewritten history and now want to push those changes to the remote repository

```
git push -f
```

This is necessary anytime your local and remote repository diverge.

*WARNING*: Force pushing should be done with **great care**. In general, on shared branches you should avoid doing any force pushes. Limit force pushing to your own branches, before opening a pull request, so that you don't inadvertently mess up your teammates' git history.

### I want to add a few more changes to the last commit

```
git commit --amend
```

### I want to rewrite a bunch of commits locally

```
git rebase -i <commit hash> # where the commit hash is the one *before* all the changes you want to make
```

This will open up an interactive prompt where you can select which commits to keep, squash, or delete. You can also change commit messages here. This is very useful when cleaning up typo or linting commits, for example.

I found rebasing to be one of the more confusing topics when learning Git in depth. See the section on [rebasing](https://dev.to/g_abud/advanced-git-reference-1o9j#rebase-vs-merge) for more.

### This rebase is a mess, let's scrap it

```
git rebase --abort
```

You can do this mid rebase. I often find that a rebase is way more trouble than it's worth, especially when rebasing two branches with a lot of similar changes. Until the rebase is complete, you can always abort it.

### I want to bring in a commit from a different branch

```
# From the branch you want to bring the commit *into*
git cherry-pick <commit-sha>
```

### I want to bring in a specific file from a different branch

```
git checkout <branch-name> <file-name>
```

### I want to stop tracking a file in version control

```
git rm --cached <file-name>
```

### I need to switch branches but my current state is broken

```
git stash # saves your changes to the top of the stash stack
git stash save "message to go along with changes"
git stash -u # stash untracked files as well
```

### I want to see what's in my stash

```
git stash list
```

### I want to bring back something in my stash

```
git stash pop # "pops" the most recent item added to the stash stack
git stash apply stash@{stash_index} # apply a given item in the stash (from git stash list)
```

### I want to undo a commit without rewriting history

```
git revert HEAD # undo the last commit
git revert <commit hash> # for a specific commit
```

This will replay the inverse of the commit specified as a new commit, thereby undoing your changes without undoing history. This is a much safer way to undo a commit on shared branches, where rewriting history has much bigger consequences.

### I want to find which commit caused a bug

```
git bisect start
git bisect bad           # Current version is bad
git bisect good v1.1     # v1.1 is known to be good

git help bisect          # For more
```

This one is a bit tricky so see [the documentation](https://git-scm.com/docs/git-bisect) for more.

## 🧹 Cleanup

### Oh my god how do I have so many branches?

```
git branch --no-color --merged | command grep -vE "^(\+|\*|\s*(master|develop|dev)\s*$)" | command xargs -n 1 git branch -d
```

This will delete all merged branches that you have locally except for master, developer or dev. If you have different names for your main and dev branches, you can change the `grep` regex accordingly.

This is a long command to remember, however you can set it to an alias like so:

```
alias gbda='git branch --no-color --merged | command grep -vE "^(\+|\*|\s*(master|develop|dev)\s*$)" | command xargs -n 1 git branch -d'
```

If you use Oh My Zosh this is already done for you. See the section on [aliases](https://dev.to/g_abud/advanced-git-reference-1o9j#aliases) for more.

### Let's garbage collect old branches/detached commits

```
git fetch --all --prune
```

This is also a really useful command if you've [setup your remote repository](https://dev.to/g_abud/advanced-git-reference-1o9j#remote-repository-settings) to delete branches on merge.

## ⌨️ Aliases

Git commands can be long and really hard to remember. We don't want to type them out each time or spend days memorizing them, so I strongly recommend you make git aliases for them.

Better yet, install a tool like [Oh My Zosh](https://ohmyz.sh/) for Z shell (Zsh) and you will get a bunch of the [most commonly used git commands as aliases](https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet#git) by default + tab completion for these. I'm lazy about configuring my shell exactly how I want it so I love open source tools like Oh My Zosh that do this for me. Not to mention it comes with a sweet looking shell.

Some of my favorites I use almost every day:

```
gst - git status
gc  - git commit
gaa - git add --all
gco - git checkout
gp  - git push
gl  - git pull
gcb - git checkout -b
gm  - git merge
grb - git rebase
gpsup - git push --set-upstream origin $(current_branch)
gbda  - git branch --no-color --merged | command grep -vE "^(\+|\*|\s*(master|develop|dev)\s*$)" | command xargs -n 1 git branch -d
gfa - git fetch --all --prune
```

If you forget what these aliases or any others you have set yourself stand for, you can run simply run:

```
alias
```

Or to search for a given alias:

```
alias grep <alias-name>
```

# Other Git Tricks

## Ignoring Files

Many files do not belong in version control. You should utilize your [global gitignore](https://egorsmirnov.me/2015/05/04/global-gitignore-file.html) for this. Examples of things that do not belong in version control are `node_modules` directories, `.vscode` or other IDE files, and Python virtual environments.

For any sensitive information, you can use environment files and add these to your local `.gitignore` at the root of your project.

## Special Files

You may need to mark certain files as binary files so that git knows to ignore them and doesn't produce lengthy diffs. Git has a `.gitattributes` file for just this purpose. In a Javascript project, you may want to add your `yarn-lock.json` or `package-lock.json` so that Git doesn't try to diff them every time you make a change.

```
# .gitattributes
package-lock.json binary
```

# Git Workflows

## Rebase vs Merge

Your team may choose to adopt either a rebase or merge workflow. There are pros and cons to both, and I've seen both be used effectively. For the most part, unless you *really* know what you're doing, you should probably opt for a merge workflow.

You can still use rebase effectively even when you're primarily using merge to bring in your changes into production. The most common situation that would call for this, is if you're working on a feature while another developer pulls in a feature into master. You could certainly use `git merge` to bring those changes in, but now you have an extra commit for the simple change your teammate made. What you really want is to *replay* your commits on top of the new master branch.

```
git rebase master
```

This should give you a much cleaner commit history now.

To explain the difference in depth would take a whole article (blog post pending), so I suggest you check out [the Atlassian docs](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) on the difference instead.

## Remote Repository Settings

I am most familiar with Github and Gitlab, however these settings should be supported by most other remote repository managers.

### 1. Delete branches on merge

Once things are merged, you should not care about the branch anymore since the history should be reflected on your master/dev branch. This significantly cleans up the number of branches you have to manage. It also makes `git fetch --all --prune` much more effective in keeping your local repository clean.

### 2. Prevent pushes directly to master

Without this, it's a very understandable mistake to accidentally forget you're on master and do a `git push`, potentially braking your production build. Not good.

### 3. Require at least one approval before merging

Depending on the size of your team, you may want to require even *more* than one approval before merging. The bare minimum should be one though, even if you're on a team of 2 people. You don't have to spend hours nit picking every single line, but in general your code should have two sets of eyes on it. [Feedback](https://dev.to/g_abud/effective-learning-and-feedback-loops-1110) is key to learning and personal growth.

### 4. Require passing CI tests to merge

Broken changes should not be merged into production. Reviewers will not be able to catch broken changes 100% of the time, so automate these checks instead. Enough said.

## Pull Requests

Keep pull requests small and concise, no more than a couple hundred lines ideally. Small and frequent pull requests will make the review process faster, lead to more bug-free code, and make your teammates' life easier. It will also lead to increased productivity and more shared learning. Make a commitment with your team to spend a certain amount of time reviewing open pull requests, every day.

We all love reviewing these:
[![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--7vLB7w1a--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/gdcwrvn006gryk0xiox0.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--7vLB7w1a--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/gdcwrvn006gryk0xiox0.png)

If you're working on a feature that will be in a broken state for a while, use [feature flags](https://www.martinfowler.com/articles/feature-toggles.html) to disable it in production. This will prevent your feature branch from diverging too much from your dev/master branch and allow you to do more frequent, small pull requests. The longer you go without merging code in, the harder it will be to do so later.

Finally, include a detailed description in your pull request, with images and/or gifs if necessary. If you use a tool like Jira to manage tickets, include the ticket number the pull request addresses. The more descriptive and visual you make your pull request, the more likely your teammates will want to review it and not drag their feet.

## Branch Naming

Your team may want to come up with branch naming conventions for easier navigation. I like to start each branch with the first letter of your first name + last name, followed by a forward slash, and the branch description separated by hyphens.

This may seem insignificant but with tab completion and tools like grep, this can really facilitate finding and making sense of all the branches you may have.

For example, when I create a new branch:

```
git checkout -b gabud/implement-important-feature
```

A week later when I forget what I called my branch, I can start typing `git checkout gabud`, press TAB, and my Z shell then shows me all of my local branches to choose from without seeing any of my teammates' branches.

## Commit Messages

Language is important. In general I find it best to not commit things in broken states, and each commit should have a succinct message that explains what the change should do. Following the official [Git recommendation](https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project), I find it best to use the present, imperative sense for commit messages. Think of each commit message as being a command to the computer/git, which you can add to the end of this sentence:

**If this commit were applied, it would...**

An example of a good commit message in the present, imperative sense would then be:

```
git commit -m "Add name field to checkout form"
```

Which now reads: "If this commit were applied, it would add name field to checkout form". Nice and clear.

## Final Thoughts

This is by no means all there is to learn about Git. I suggest you checkout the [official documentation](https://git-scm.com/doc) and `git help` for more. Don't be afraid to ask for help with Git from your teammates, you'd be surprised to learn that most of your teammates have many of the same questions.

What about you? Which Git commands or concepts have been the most instrumental in your workflow?