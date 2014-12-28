Local git commands:
- git branch newBranchName
- git checkout moveToThisBranch
- one can go "up the tree" w/ git checkout HEAD^ repeatedly
    - git checkout HEAD~NUMBER, where NUMBER is the number of commits to go back
    - same applies to the HEAD's parent ^ (caret), ^ or ^1 is previous HEAD where ^2 is the branch
- git merge otherBranch, creates one merged branch from 2 different branches
- git rebase otherBranch, copies all commits from the other branch onto current branch
- detached HEAD, when doing git checkout hash1235, as we are no longer in a specific "place"
    - HEAD is current place in the tree where we are
- force assignment master branch to 3 commits up using relative refs ~ operator: git branch -f master HEAD~3
- git reset HEAD^ rewrite history as there were no commits, only works on local commits
- git revert HEAD for remote/pushed changes
- git cherry-pick commit1/hash1 commit2/hash2 (...) selects only the commits desired
- git rebase -i , for interactive rebasing, squash commits means merging them into 1, pick means dropping the commits
- git commit --amend to update the commit message
- git tag tagName hash/branch, to freeze in time the git repo state
- git describe <reference>, to see information about the commits state between tags, output will be
    - tagName_numberOfCommits_gHashOfDescribedCommit

Remote git commands:
- git clone
- git fetch, gets remote changes and updates where the local repo points to, but it will not update the local repo
    - it will not change anything in the local
- in order to have git fetch and git merge/rebase/cherry-pick git provides a special command
- git pull
    - "is essentially shorthand for a git fetch followed by a merge of whatever branch was just fetched"

Other commands:
- git bisect,
