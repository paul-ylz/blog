---
title: "Squash & Merge"
date: 2019-04-16T22:29:28+08:00
draft: false
---

At work we do TBD (Trunk Based Development), which means that we always push code to the master branch. Sometimes, pushing to master is not convenient because we are in the middle of figuring out a solution and some wild trial and error is the only way we can figure out how to move forward. We often want others in our team to be able to join forces with us and help to figure out the problem, and so the most convenient thing to do is to push the existing state of our code. However, when master branch is connected to a build pipeline, we find ourselves wanting to create an experimental branch to work on. Typically this branch is going to have several commits, with terrible commit messages which tell the story of our desperate attempts to fix a tricky task.

In this case, all is not lost and we can still maintain some level of dignity by squashing these commits into a single commit and then merging our experimental branch to master so that it looks like a single, tidy commit. Squash and merge is the strategy!

## Example

Let's say you're on the `apple` branch where you have made 4 commits you wish would disappear.

```bash
% git log --graph --decorate --oneline

ed6944 (HEAD -> apple, origin/apple) running out of good commit messages
* e9340ff the commit messages tend to get worse on the experimental branch
* 00b9de0 updated readme again
* d77388a first experimental change
* 7add34f ABC-1 | Updated project summary to readme  # the last "good" commit
* a1519c5 Initial commit
~
~
~
```

We want to be checked out on the apple branch and do an interactive rebase with the SHA of the last good commit.
```bash
% git checkout apple
% git rebase -i 7add34f
```

which pops us into vim:

```VimL
pick d77388a first experimental change
pick 00b9de0 updated readme again
pick e9340ff the commit messages tend to get worse on the experimental branch
pick aed6944 running out of good commit messages

# Rebase 7add34f..aed6944 onto 7add34f (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

We want to squash these 4 commits into one and then have an opportunity to write a quality commit message. We'll leave the first as "pick" and change the latter 3 to squash. By the way, Vim lets you change multiple lines in one go... use Cntrl-V for block visual, "j" to go down as many rows as you need to change, "c" to change and then type the characters you want, say "squash". Only the first row will reflect changes at first, but when you escape out of insert mode, the other lines will update with the same changes.

So we now have:

```VimL
pick d77388a first experimental change
squash 00b9de0 updated readme again
squash e9340ff the commit messages tend to get worse on the experimental branch
squash aed6944 running out of good commit messages

# Rebase 7add34f..aed6944 onto 7add34f (4 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

`:wq` to write and quit. 

Now git gives us a change to rewrite our commit messages. We'll condense our hasty messages into an official and officious commit message that exudes quality.

BEFORE:
```VimL
# This is a combination of 4 commits.
# This is the 1st commit message:

first experimental change

# This is the commit message #2:

updated readme again

# This is the commit message #3:

the commit messages tend to get worse on the experimental branch

# This is the commit message #4:

running out of good commit messages

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Tue Apr 16 22:11:55 2019 +0800
#
# interactive rebase in progress; onto 7add34f
# Last commands done (4 commands done):
#    squash e9340ff the commit messages tend to get worse on the experimental branch
#    squash aed6944 running out of good commit messages
# No commands remaining.
# You are currently rebasing branch 'apple' on '7add34f'.
#
# Changes to be committed:
#       modified:   README.md
#
```

AFTER:
```VimL
ABC-2 | Added sample lines to readme for example

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Tue Apr 16 22:11:55 2019 +0800
#
# interactive rebase in progress; onto 7add34f
# Last commands done (4 commands done):
#    squash e9340ff the commit messages tend to get worse on the experimental branch
#    squash aed6944 running out of good commit messages
# No commands remaining.
# You are currently rebasing branch 'apple' on '7add34f'.
#
# Changes to be committed:
#       modified:   README.md
#
```

`:wq` and we are done!

Now we can just checkout master, get up to sync with the rest of the team with `git pull -r` and then merge our apple branch back in, vowing to practice good TBD and keep our commits small and sweet.

```bash
% git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

% git pull -r
Already up to date.
Current branch master is up to date.

% git merge apple
```
