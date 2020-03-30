# Git Cheat Sheet

[TOC]

## Notation

- a commit can be referenced by its full checksum, the first 7 characters of its checksum, or its relative position to the current head, e.g. `HEAD~1` is previous commit



## Configuration

#### `git config`

- shorthand to edit config file in either ??? if local or ??? if global
- `--edit` opens config file ??
- `--global`, `--local`, `--system` ???

```bash
git config --global user.name "Max Mustermann"
git config --global user.email "musterman@musterdomain.com"
git config --global credential.helper osxkeychain
git config --global core.editor "nano -w"
git config --global alias.glog "log --oneline --graph --decorate"
```



## Repository set up

#### `git init`

- creates new Git repository (.git folder and master branch)
- needs to still set up remote repository via `git remote add <remote_name> <remote_repo_url>`

#### `git remote`

- set shortcut name for URL to remote repository
- just interface for editing `./.git/config` file
- can `add` / `rm` / `rename <name> <url>`

#### `git clone <URL>`

- downloads complete copy of existing Git repository
- automatically sets up remote connection to cloned repository as „origin“

#### .gitignore

- file containing patterns to match files in subfolders to be ignored by Git
- e.g. folder names, wildcard \*,
- is itself versioned like any other file in the repository
- use only one, put in root of project folder



## Workflow (within a branch)

####  `git status`

- state of the working directory and the staging area
- lists which files are staged, unstaged, or untracked

#### `git diff`

- show changes between working area and staging area, any uncommitted changes since the last commit
- can select specific files, commits, tags, branches
- `--staged` shows changes between staging area and most recent commit
- `--cached` shows changes between staging area and most recent commit ????

#### `git log`

- show history of commits, committed snapshots, walks graph from HEAD back, i.e. doesn’t show commits on an unmerged branch
- doesn’t show unreferenced commits, for this needs `git reflog`
- e.g. `--oneline --graph --decorate`

#### `git checkout -- <file>`

- discard changes in working area (?)
- replace working area with latest saved commit (?) staging area (?)
- not working directory safe, could loose data if working directory has unstaged changes
- `checkout HEAD -- <files>` restores from repo to stage and working area (?)

#### `git add`

- adds changes in the working directory to the staging area
- can be specific files or folders
- can use wildcards, e.g. `.` for all
- `--all` all changed files in repo (?)
- if wants to update already staged changes, just re add again, previous not yet committed stages will stay as unreferenced blobs, will eventually be garbage collected
- `-p` interactively add only specific parts of a file

#### `git reset <option> <commit>`

- undo staging or commit
- option `soft`: only moves the HEAD pointer to <commit>, keeps staging and working area untouched, doesn’t delete blobs and trees, can still find via `reflog`, ready to commit again
- option `mixed`: (default ?!) moves the HEAD pointer to <commit> and updates staging area with <commit>, keeps working area untouched, doesn’t delete blobs and trees, can still find via `reflog`, ready to stage again
- option `hard`: moves the HEAD pointer to <commit> and updates staging and working area with <commit>, ready to start over again, doesn’t delete blobs and trees, can still find via `reflog` for 30 days, i.e. can get back any commit, but not changes that were never committed ⚠️
- not working directory safe, could loose data if working directory has unstaged changes and is reset because there exists no blobs and trees yet, but danger only in case of `hard` option
- ?? `HEAD -- <file>` deletes recent commit ? reverts most recent commit
- ?? difference to checkout — ?
- ?? can be files or folders
- ?? can use wildcards, e.g. `.` for all

#### `git rm`

- remove file from working and staging area
- ?? `--cached` remove from repository but keep in working directory as ignored file (i.e. adds entry to `.gitignore`)

#### `git commit`

- create snapshot of the staged changes
- needs to provide description, multi-line possible
- `-m "Descriptive message“` inline message
- `-a` commit all changes of already tracked files without need to stage
- `--amend` add to previous commit instead of new commit, creates entirely new commit which references same parent commit, with no pointer pointing to previous commit it will eventually be garbage collected, beware: if already pushed previous commit, will fail to push amended commit because the commit history now diverges, needs to force push ⚠️



## Branching

#### `git branch`

- list all local branches
- `-a` list all remote branches
- `<branch>` create new branch
- `<branch> -m` rename current branch ??

#### `git checkout <branch>`

- switch branches
- updates working directory and staging area (?) with files from last commit in branch
- `-b` shorthand to create branch and switch to it

// do workflow (within all branches)

#### `git checkout master`

(git pull origin master)

- (?) merges locally, avoids conflicts remotely, then just fast-forward merge

#### `git branch --merged` (?)

#### `git merge <branch>`

- merges branch into current branch
- only works if there are no uncommitted changes in working area (?)
- if fast-forward merge, actually no merge commit, just forwards current branch, can demand commit with `--no-ff` e.g. for documentation
- if 3-way merge, needs to make a merge commit, can avoid by rebasing then fast-forward merge
  (git push origin master)

#### `git branch --merged` (?)

#### `git branch -d <branch>`

- deletes branch, efficient since only needs to delete pointer
- only works if there are no unmerged commits, is safe
- use `-D` to force delete even if there are unmerged commits, not safe
- beware: doesn’t delete remote copy of branch ❗️

#### `git push origin --delete <branch>` ??????

- deletes branch remotely

#### `git branch -a` (?)

#### Cherrypicking

- ???

##### `git revert`

- undo a cherrypick ???

##### `git rebase`

- replays series of commits on top of another commit, e.g. move branch forward on master branch as if it was created later, doesn't need to merge in master to get latest changes
- beware: creates completely new commits, leaves old for garbage collection, like `commit --amend`, now history diverges for others, needs to force push ⚠️
- while creating these new commits can also modify them, e.g. squash into one, drop a change, rename the commit message, reorder the commits, basically can rewrite history freely



## Resolve merge conflict

1. `git status` shows conflicting files

2. Git edits affected files, 7 „<„ mark beginning of conflict, content of receiving branch follows, 7 „=„ mark content of merging branch follows, 7 „>“ mark end of conflict

3. `git add` the resolved files

4. `git commit` to do the merging commit




## Remote repository

- all only get readable commits, i.e. not the unreferenced ones (?)

#### `git fetch <remote>`

- fetches changes from remote repository into local repository
- doesn’t merge the changes into local repository, needs to manually merge `<remote>/master` into `master`
- `<branch>` to fetch only for specific branch

#### `git pull <remote> <branch>`

- fetch & merge combined, shorthand
- merges the changes into local repository, potential conflicts

// workflow (within a branch)

#### `git push <remote> <branch>`

- pushes `<branch>` to `<remote>`, creates it if doesn't exist there yet

- `-u origin <branch>` to use push alone later (?)
- works only if its a fast-forward merge, i.e. it does not do merges on the server, must be done locally by pulling before



## Stash

#### `git stash`

- saves uncommitted changes (both staged and unstaged) into stash
- `-u` to include also untracked files
- `-a` to include also untracked and ignored files
- `-p` to stash only a particular file
- `save "descriptive message"` include message

#### `git stash list`

- lists current stashes

#### `git stash show`

- show summary of stash

#### `git stash pop`

- reapplies changes from stash into working copy
- removes changes from stash

- by default latest, change with `stash@\{index\}`

#### `git stash apply`

- reapplies previously stashed changes into working copy
- keeps changes in stash

#### `git stash drop`

- delete a stash
- by default latest, change with `stash@\{index\}`

#### `git stash clear`

- delete all stashes