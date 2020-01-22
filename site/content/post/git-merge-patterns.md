---
title: "All About that Rebase"
date: 2020-01-22T20:20:00Z
description: >-
  Squashing some common Git merge patterns
---

## Background

Take 3 commits: `abc`, `def`, `ghi`
And let's suppose you have two git branches: `master` and `production`.

Production's history currently looks like this:
`abc`

Master is two commits ahead:
`abc` -> `def` -> `ghi`

You open a Pull Request to get `def` and `ghi` into `production`.

## Merge types

You have a few options for how to merge the Pull Request. All of these methods serve the purpose of getting the code from `def` and `ghi` into the `production` branch - the main differences center around the commit histories of the two branches afterwards.

### Merge Commit

I very rarely use this option in my work. As I understand it, it creates a sepearate commit, let's call it `jkl`, that describes the merging of `ghi` into `production`.

### Squash Merge
Squash merge combines `def` and `ghi` into a new commit, let's call it `mno`, and then merges that.

If you squash merge your PR, you'd end up with:

master: `abc` -> `def` -> `ghi`
production: `abc` -> `mno`

The commit histories for `master` and `production`, even though they have the same code, no longer have the same commits in them.

### Rebase Merge
Rebase merge replays all of the new commits (def and ghi) onto production: 
`abc`
`abc` -> `def`
`abc` -> `def` -> `ghi`

At the end, `master` and `production` would look exactly the same.

## Conclusion

If I can help it, I prefer **Rebase merge**. It avoids creating new commits, so it often results in a cleaner commit history between branches. Sometimes I use squash merge if my branch has a bunch of commits like:
```
ahioh: feat: do something
inkcl: lint error
apsdq: fix test
apsap: please work
pwowq: Finally working
```
That way, the commit history doesn't show my stream of consciousness as I fight with linters, continuous integration, and test suites.

As I mentioned, I don't often use merge commits - I'm sure they have a use case, but I don't find them helpful to me a majority of the time
