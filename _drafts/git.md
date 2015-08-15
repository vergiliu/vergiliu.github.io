---
layout: post
title: "Git"
date: 2015-12-31 22:22:22
categories: [books, reviews]
tags: [books, reviews, 2015]
---
Local git commands:
- git branch newBranchName
- git checkout moveToThisBranch
- one can go "up the tree" w/ git checkout HEAD^ repeatedly
    - git checkout HEAD~NUMBER, where NUMBER is the number of commits to go back
    - same applies to the HEAD's parent ^ (caret), ^ or ^1 is previous HEAD where ^2 is the branch
- git merge otherBranch, creates one merged branch from 2 different branches
    - git merge SOURCE, for example if there are 2 branches br1, br2 and we're on br1 you do git merge br2
- git rebase otherBranch, copies all commits from the other branch onto current branch
    - git rebase DESTINATION, for example if there are 2 branches br1, br2 and we want to rebase to br2 we go to br1 and do git rebase br2
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
    - same refspec <source>:<destination> works for both fetch and push, only in reverse
        - for fetch the source will be remote branch and destination will be local branch, they will get created if not present
        - for push the source will be the remote branch...
    - fetching with empty origin will just create the local branch git fetch origin :newBranch, from master
- in order to have git fetch and git merge/rebase/cherry-pick git provides a special command
- git pull
    - "is essentially shorthand for a git fetch followed by a merge of whatever branch was just fetched"
    - git pull --rebase does a fetch and then a rebase, as compared to default fetch & merge
- git push
    - share our awesome work with the world, or at least the ones fetching from the remote repo
    - specific settings depending on push.default and git version
    - git push <remote> <place>, where git push origin master - origin is the name of the remote and master the place where the commits come from
        - then the local branch is not important
    - git push origin <source>:<destination> , refspec identifying where to push from and to on the `origin`
    - git push origin :foo , we can delete the local branch remotely as no branch was present locally (empty origin)
- we can update the default tracking branch either remote or local
    - -b local_name, -u remote branch name (can be omitted if on that branch currently)

Other commands:
- git bisect, searching through history
