Pull from github: git pull -or- git pull --rebase (to put your local changes ahead)

Create new branch:  git checkout -b new_branch
Push new branch to github: git push origin new_branch

Go back to Master branch:  git checkout master
Merge branch to master:  git merge new_branch

switch back to working on master files:  ls

View all branches:  git branch

Push to github:  git push

Show all past commit activity: git reflog

Delete branch (after merge): git branch -d old_branch
Delete branch without merging it: git branch -D old_branch

Review each diff and add to index or not: git add -p

undo uncommitted changes easily by having Git check out the previous commit with the checkout command (and a -f flag to force overwriting the current changes):

# Review remote (github) changes:
git fetch origin
git diff master origin/master
git merge origin/master

# Review diffs between branches
git diff one-branch two-branch

git merge new_branch

Pull from github to local - override local changes:
git reset --hard remotes/origin/HEAD

To avoid entering password for every push:
git config remote.origin.url git@github.com:the_repository_username/your_project.git
