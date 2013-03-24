git-branch-tools
================

A set of git tools to help manage branches better.

Archiving branches 
------------------
Some branches are not actively developed anymore but should still be
preserved for posterity. These branches are clogging up the branch view.

    git archive-branch mybranch

moves `mybranch` to `archive/2013/mybranch` and tags the current top commit
with a message about the branch history.


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


