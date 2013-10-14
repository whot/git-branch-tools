git-branch-tools
================

A set of git tools to help manage branches better. Some of the tools use
git-config hooks for local customization, have a look at the tools directly.

Archiving branches 
------------------
Sometimes branches are not actively developed anymore but need to be
preserved for posterity. These branches are clogging up the branch view.

    git archive-branch mybranch

moves `mybranch` to `archive/2013/mybranch` and tags the current top commit
with a message containing bits of the branch history.

Showing recent branches
-----------------------
Working on many branches can mean you forget which branch you worked on last
week, or the week before.

    git recent-branches

lists the various branches checked out over the history, including the date
and last commit date on that branch.  

Example:

    next                                          4 hours ago          last commit 6 days ago
    server-1.13-branch                            4 hours ago          last commit 2 weeks ago
    touch-grab-race-condition-56578-v2            3 days ago           last commit 3 days ago
    touch-grab-race-condition-56578               4 days ago           last commit 6 days ago      †
    bug/xts-segfaults                             6 days ago           last commit 6 days ago      †
    master                                        6 days ago           last commit 3 weeks ago
    for-keith                                     10 days ago          last commit 2 weeks ago
    memleak                                       13 days ago          last commit 2 weeks ago


### Dependencies

git-recent-branches requires GitPython: http://pythonhosted.org/GitPython/

Better branch descriptions
--------------------------
Can't remember what branch "fix-race-condition" was? Me neither. That's what

    git branch-description [branchname] [upstream]

will tell you. If upstream is given, it'll also show you what has been
merged into upstream already (by patch, not by commit).

And install the `git-post-checkout-nagging-hook` as your
`.git/hooks/post-checkout` to make sure you get reminded to set the branch
description.

Creating patch sets
-------------------
A wrapper around git-format-patch, that drops the patches into a directory
named after the patch set and date.

    $> git patch-set origin/master..HEAD~2
    $> ls patches
    patches/patches-2013-08-30-10:47-origin\master..HEAD~2/
    patches/latest -> patches/patches-2013-08-30-10:47-origin\master..HEAD~2/
 
patches/latest always links to the last generated patcheset.

Note: git-patch-set will run the pre-push hook (if any). Ideally, you'll
have a pre-push hook that runs make check, so you never send out patches
that haven't at least be built and tested.

    $> cat .git/hooks/pre-push
    cat ~/code/libevdev/.git/hooks/pre-push
    #!/bin/bash
    set -e
    echo "running make check"
    make check

Splitting commits
-----------------
During rebase, when you realise that a commit needs to be broken up into
two.
   $> git rebase -i
   # mark commit to split as "edit"
   $> git split-commit

The tool now calls git add -p, letting you add the hunks needed (and/or missing
files). Finish up, it will re-commit with the same message and drop you
back to the shell to finish up and continue. Alternatively,

   $> git split-commit --after
will drop you into a shell immediately, letting you git add and commit
anything you want committed _before_ the current commit. Exiting the shell
will commit the rest.

