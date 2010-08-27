This thing happens to me pretty often: i start a story, work on it
for a while then something urgent comes up.[^customer-integration]  The urgent thing needs to
be fixed right away but i have a lot of changes in my working
directory.  Unfortunately, the changes i have made are incomplete and
non-functional.

[customer-integration]: I do a lot of customer integration.  Once a
customer starts testing it is important to keep the turn around on
their blocking issues to a minimum.  If you don't they get distracted
and it's no telling how long you'll have to wait before they start
testing again.

The usually suggested way to handle this is with
[`git-stash`][git-manual-interrupted-work].  For a long time, i used
the technique myself.  However, i often found myself lost in the stash
queue.  If you use stash to store unfinished work your stash queue can
become quite long.

For example, if the above scenario occurs again once you have started
working on the urgent issue.  Or if you switch tasks not because of an
urgent issue, but because the business deprioritized the feature.  In
those sorts of situations it can often be quite a while before you get
back to your stashed changes.

It recently occurred to me that git provides a much more elegant way
to deal with unfinished work.

The steps
------

First, always work in a feature branch.  You should be doing this
anyway but it is required for this technique to work.

So you are working away in your feature branch and you need to switch
tasks.  Do a `git add -A` followed by a `git commit -m 'WIP'`.  Your
in progress work is saved as the HEAD commit of the feature branch.

Now `git checkout <whatever>` and go fix the urgent issue.

Once you want to come back to you in progress work do a `git checkout
<feature-branch>`.  Now you have all you work you did checked out but
you have still have that the 'WIP' commit hanging around.  That is bad
because you know the code in that branch is broken.

We want to pretend like that 'WIP' commit never happened. It turns out
this is easy with git.  Just run `git reset HEAD~1`.  This will reset
the HEAD pointer of your feature branch back to the commit immediately
before the 'WIP' commit, but it leaves your working directory intact.

Now make that feature work, commit, merge into master and push.  The
commit will have the current HEAD as it's parent so the 'WIP' commit
will be excluded from the history.  The next time you do a `git gc`
that 'WIP' commit will be removed because it is no longer reachable.

How it works
-----


[git-manual-interrupted-work]: http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#interrupted-work
