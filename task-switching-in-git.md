This thing happens to me pretty often: i start a story, work on it
for a while then something urgent comes up.[^customer-integration]  The urgent thing needs to
be fixed right away but i have a lot of changes in my working
directory.  Unfortunately, the changes i have made are incomplete and
non-functional.

[^customer-integration]: I do a lot of customer integration.  Once a
customer starts testing it is important to keep the turn around on
their blocking issues to a minimum.  If you don't they get distracted
and it's no telling how long you'll have to wait before they start
testing again.

The usually suggested way to handle this is with
[`git-stash`][git-manual-interrupted-work].  For a long time, i used
stash in this situation myself.  However, i often found myself lost in
the stash queue.  If you use stash to store unfinished work your stash
queue can become quite long.  It is easy to forget you have stashed
work.  It is also easy to do a `git stash clear` and lose that work.

There are lots of situations in which it can be quite a while before
you get back to your stashed changes.  For example, if you switch
tasks because the business deprioritized the feature.  Or if the
urgent issue gets interrupted by an emergency issue.

It recently occurred to me that git provides a much more elegant way
to deal with unfinished work.

The steps
------

First, always work in a feature branch.  You should be doing this
anyway but it is required for this technique to work.  

1. `git add -A` (on the feature branch)
2. `git commit -m 'WIP'`
3. Switch branches and fix that urgent issue.  Using git like you always do.
5. `git checkout <feature-branch>`
6. `git reset HEAD~1`
7. Continue where you left off.  Once you are ready, commit.

This approach commits you in-progress work on the branch to which it
belongs, keeping it safe.

How it works
-----

Once you do your WIP commit your history will look something like:

<img src="http://barelyenough.org/blog/uploads/task-switching-in-git/git-commits-wip.png"/>

That is great for temporarily storing your in-progress work.  We
definitely don't want that nasty "WIP" commit in our history long
term, though.  The `git reset HEAD~1` command changes the HEAD pointer
of the feature branch back to the commit immediately before the "WIP"
commit.  That leave a commit graph something like:

<img src="http://barelyenough.org/blog/uploads/task-switching-in-git/git-commits-reset.png"/>

Once you have completed your changes and committed the HEAD pointer of
the feature branch will be updated to point the new commits.  This
leaves the "WIP" commit out of the commit history of the branch
forever. 

<img src="http://barelyenough.org/blog/uploads/task-switching-in-git/git-commits-final.png"/>

The "WIP" commit is now "unreachable" because no objects or references
in the system point to it.  It will be removed the next time you do a
`git gc`.

`git stash` definitely has it place but i reserve it for
situations where i am going to pop the stash very quickly (eg, i
stash, the checkout a different branch, then pop).

[git-manual-interrupted-work]: http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#interrupted-work


