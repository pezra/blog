Note to self: If you need to revert commits that have already been
pushed, or otherwised merged with any other repo or branch, use `git
revert`.  

`git reset` is exclusively for undoing commits in your local working
tree that have not seen the light of day.  Attempting to use `git
reset` to undo changes that exist in multiple trees (changes that have
been pushed or merged in to another branch or repo) will result in
pain and suffering.
