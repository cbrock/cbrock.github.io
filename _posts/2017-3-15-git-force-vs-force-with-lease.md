---
layout: post
title: Prefer git's `--force-with-lease` over `--force`
---

A new lease on life... Or, at least, on how you interact with remote git repositories. I've been meaning to write this down for a while, as it's an easy change you can make to an existing git workflow to partially protect from destructive and probably embarrassing scenarios.

## The problem
When interacting with remote repositories, particularly in a team environment, it's inevitable that you'll need to overwrite changes at some point - for example, when squashing commits after incorporating feedback from a peer or cleaning up commits in preparation for a pull request.

The problem arises because these examples are destructive actions. That is, they essentially rewrite history. In this case, your git commit history. The examples above are all valid reasons to do so, but what is probably the most obvious method to achieve this - git push's `--force` option - is not the best.

That's because `--force` basically does it what it says - when used, it completely ignores any other upstream changes to the remote repository and rewrites its history to point to yours (Atlassian's developer blog has some [concrete examples that illustrate](https://developer.atlassian.com/blog/2015/04/force-with-lease/)). In other words, it's possible (maybe even easy if you or a coworker are new to version control or git) to accidentally overwrite someone else's changes that have been pushed to the remote. Hopefully, those changes would still exist in a local repository, but why risk it in the first place?

## The (partial) solution
There's a simple way to protect againt this and, aliased on the command line, doesn't need to be any longer character-wise: [`git push --force-with-lease`](https://git-scm.com/docs/git-push#git-push---force-with-leaseltrefnamegt). Using `--force-with-lease` provides guard rails, so-to-speak, by refusing to overwrite the remote repository branch if someone else has pushed changes. Simple, and protects against a use case that is annoying, and potentially impossible, recover from.

*tl;dr* Prefer `git push --force-with-lease` over `git push --force` when you need to overwrite your commit history.

## More info
I mentioned aliasing the `git push --force-with-lease` command to make it less verbose. If you use [Oh-My-Zsh](http://ohmyz.sh/), for example, you might consider modifying or copying its [git plugin's `ggf` function](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh#L105-L108):

```bash
ggf() {
  [[ "$#" != 1 ]] && local b="$(git_current_branch)"
  git push --force origin "${b:=$1}"
}
```

Or, if you don't, port that over to whatever works for you.
