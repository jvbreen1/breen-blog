take 3 commits: abc, def, ghi
let's say production is at abc, and master has abc -> def -> ghi
squash + merge combines def and ghi into a new commit, let's call it jkl, and then merges that.
so youd have:
master: abc -> def -> ghi
production: abc -> jkl
the trees for master and production, even though they have the same code, no longer have the same commits in them
with rebase + merge, merging the PR replays all of the new commits (def and ghi) onto production, so abc, then abc -> def, then abc -> def -> ghi
so master and production would look exactly the same
so those conflicts came from trying to merge def and ghi from master onto production, even though they had been squashed into jkl
