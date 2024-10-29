# git vain

## Motivation

I have literally hundreds of local directories with git repositories. Most of
them have remotes at GitHub or elsewhere. Many are just clones of projects I
looked at out of curiosity. Some include one or two additional commits. Some
commits have been pushed or merged into the remote, some have not. Some
directories contain untracked files or uncommitted changes. This script helps
to find out whether a local directory can safely be removed. 

The script checks for

- whether this is a git repository at all (must be called at root level)
- untracked files  
- diffs not yet checked in  
- stashes  
- detached HEAD  
- branches not pushed to `origin`  
- tags not pushed to `origin`
- all of the above for any submodules (not implemented yet)

It returns 0 if the repository passes all checks (no untracked files, no diffs,
...), and the number of violations otherwise.

With the -a flag, take the first relevant automatic action. In every case that
is not purely informational, ask permission before doing anything. If an action
feels unsafe or inappropriate, just say no or hit <ctrl>-C to leave.

<!--
 * If there are untracked files, run `git status -s`.
 * If there are diffs, run `git diff`.
 * If there is one stash, run `git stash show -p` [i.e., print the stash diff].
 * If there are multiple stashes, run `git stash list`.
 * If there are tags not pushed to origin, push them.
 * If in a detached HEAD state, merge to master.
 * If a branch (incl. master) needs to be `git push origin`-ed, do so.
 * If there are submodules, recurse into them and run all of the above; else print nothing.
 * If everything is clean, remove the directory!

The last step is intended for people who have the work habit of not leaving copies of
repositories in their home directory if they aren't needed—effectively putting them
back on the shelf to take down later. If that isn't your habit, it is safe to remove the
last block of this script that takes that action. If you do delete the directory entirely,
you will still need to `cd` to another directory yourself (a subshell can't change
its parent shell's working directory.)

-->

## Installation

Either copy the script [`git-vain`](git-vain) to your PATH or copy it to any
location and reference in `~/.gitconfig`:

```
[alias]
    vain = !/path/to/git-vain
```

## Usage

Run `git vain` like any other git subcommand, either in the root of a working
tree directory, or by giving the directory with git option `-C`:

`git -C $dir vain`

Limitations:

- Misses empty subdirectories---including those with only dot files!
- Assumes your only remote is origin.

## License

[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), [original script by Ben Klemens](http://modelingwithdata.org/arch/00000194.htm), modifications [by Ben](https://github.com/b-k/git-isclean/tree/master), rewritten by Jakob Voß.
