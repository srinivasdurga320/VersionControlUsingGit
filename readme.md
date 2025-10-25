# Git Commands Complete Reference

A comprehensive guide to Git commands with practical examples for developers of all levels.

## 📋 Table of Contents

- [Setup & Configuration](#setup--configuration)
- [Starting a Repository](#starting-a-repository)
- [Basic Commands](#basic-commands)
- [Viewing History](#viewing-history)
- [Branching](#branching)
- [Merging & Rebasing](#merging--rebasing)
- [Remote Repositories](#remote-repositories)
- [Undoing Changes](#undoing-changes)
- [Stashing](#stashing)
- [Tags](#tags)
- [Inspecting & Comparing](#inspecting--comparing)
- [Cleaning Up](#cleaning-up)
- [Advanced Commands](#advanced-commands)
- [Collaboration Workflows](#collaboration-workflows)
- [Git Aliases](#git-aliases)
- [Emergency Commands](#emergency-commands)
- [Best Practices](#best-practices)
- [Quick Reference](#quick-reference)

---

## Setup & Configuration

### Initial Setup

Set up your Git identity (required for commits):

```bash
# Set your name
git config --global user.name "John Doe"

# Set your email
git config --global user.email "john.doe@example.com"
```

**Example Output:**
```
# No output, but verify with:
git config user.name
# Output: John Doe
```

### Configure Default Branch

```bash
# Set default branch name to 'main'
git config --global init.defaultBranch main
```

### View Configuration

```bash
# View all settings
git config --list

# View specific setting
git config user.name

# View with origin (which file the setting comes from)
git config --list --show-origin
```

**Example Output:**
```
user.name=John Doe
user.email=john.doe@example.com
core.editor=code --wait
color.ui=auto
init.defaultbranch=main
```

### Edit Configuration File

```bash
# Open global config in editor
git config --global --edit

# Open local config (repository-specific)
git config --edit
```

### Line Ending Configuration

```bash
# Windows (convert LF to CRLF on checkout)
git config --global core.autocrlf true

# Mac/Linux (convert CRLF to LF on commit)
git config --global core.autocrlf input
```

---

## Starting a Repository

### Initialize a New Repository

```bash
# Initialize in current directory
git init

# Initialize in specific directory
git init my-project
```

**Example:**
```bash
mkdir my-website
cd my-website
git init
```

**Output:**
```
Initialized empty Git repository in /home/user/my-website/.git/
```

### Clone an Existing Repository

```bash
# Clone via HTTPS
git clone https://github.com/username/repository.git

# Clone via SSH
git clone git@github.com:username/repository.git

# Clone into specific folder
git clone https://github.com/username/repository.git my-folder

# Clone specific branch
git clone -b develop https://github.com/username/repository.git
```

**Example:**
```bash
git clone https://github.com/torvalds/linux.git
```

**Output:**
```
Cloning into 'linux'...
remote: Enumerating objects: 9000000, done.
remote: Counting objects: 100% (9000000/9000000), done.
Receiving objects: 100% (9000000/9000000), 2.50 GiB | 5.00 MiB/s, done.
```

### Shallow Clone (Faster)

```bash
# Clone only recent history (last commit)
git clone --depth 1 https://github.com/username/repository.git

# Clone with limited history (last 50 commits)
git clone --depth 50 https://github.com/username/repository.git
```

---

## Basic Commands

### Check Status

```bash
# Full status
git status

# Short status
git status -s
```

**Example:**
```bash
git status
```

**Output:**
```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        style.css

no changes added to commit (use "git add" and/or "git commit -a")
```

**Short Status Output:**
```bash
git status -s
```
```
 M index.html
?? style.css
```

### Adding Files to Staging

```bash
# Add specific file
git add filename.txt

# Add multiple files
git add file1.txt file2.txt

# Add all files in current directory
git add .

# Add all files in repository
git add -A

# Add all JavaScript files
git add *.js

# Add all files in a folder
git add src/

# Interactive staging
git add -p
```

**Example:**
```bash
# Create and add a file
echo "Hello World" > hello.txt
git add hello.txt
git status
```

**Output:**
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   hello.txt
```

### Committing Changes

```bash
# Commit with message
git commit -m "Add hello.txt file"

# Add all modified files and commit
git commit -am "Update existing files"

# Commit with multi-line message (opens editor)
git commit

# Amend last commit
git commit --amend -m "Corrected commit message"

# Amend without changing message
git commit --amend --no-edit
```

**Example:**
```bash
git commit -m "Add user authentication feature"
```

**Output:**
```
[main 3a7f8d2] Add user authentication feature
 3 files changed, 45 insertions(+), 2 deletions(-)
 create mode 100644 auth.js
```

### View Changes

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged

# Show changes in specific file
git diff index.html

# Word-level diff
git diff --word-diff
```

**Example:**
```bash
# Modify a file
echo "New line" >> hello.txt
git diff
```

**Output:**
```diff
diff --git a/hello.txt b/hello.txt
index 557db03..980a0d5 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 Hello World
+New line
```

### Remove and Move Files

```bash
# Remove file from Git and filesystem
git rm filename.txt

# Remove from Git but keep in filesystem
git rm --cached filename.txt

# Remove directory
git rm -r folder-name/

# Rename/move file
git mv old-name.txt new-name.txt
```

**Example:**
```bash
git mv hello.txt greeting.txt
git status
```

**Output:**
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        renamed:    hello.txt -> greeting.txt
```

---

## Viewing History

### Basic Log Commands

```bash
# View commit history
git log

# Compact one-line format
git log --oneline

# View last 5 commits
git log -5

# View with graph
git log --graph --oneline --all

# View with file changes
git log --stat

# View with actual code changes
git log -p
```

**Example:**
```bash
git log --oneline -5
```

**Output:**
```
3a7f8d2 (HEAD -> main) Add user authentication feature
b4e9c1a Fix navigation bug
7d2a5f3 Update README
e8f3b2c Initial commit
```

### Advanced Log Filtering

```bash
# View commits by author
git log --author="John Doe"

# View commits in date range
git log --since="2023-01-01" --until="2023-12-31"

# View commits from last week
git log --since="1 week ago"

# View commits that modified specific file
git log -- index.html

# Search commit messages
git log --grep="bug fix"

# View only merge commits
git log --merges

# View non-merge commits only
git log --no-merges
```

**Example:**
```bash
git log --author="John" --since="1 month ago" --oneline
```

**Output:**
```
3a7f8d2 Add user authentication feature
b4e9c1a Fix navigation bug
7d2a5f3 Update README
```

### Pretty Log Formats

```bash
# Custom format
git log --pretty=format:"%h - %an, %ar : %s"

# Detailed graph
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'
```

**Example Output:**
```
3a7f8d2 - John Doe, 2 hours ago : Add user authentication feature
b4e9c1a - Jane Smith, 1 day ago : Fix navigation bug
7d2a5f3 - John Doe, 3 days ago : Update README
```

### Show Specific Commit

```bash
# Show commit details
git show 3a7f8d2

# Show specific file from commit
git show 3a7f8d2:index.html

# Show commit statistics
git show --stat 3a7f8d2
```

**Example:**
```bash
git show HEAD
```

**Output:**
```
commit 3a7f8d2f1c9b8e7a6d5c4b3a2f1e0d9c8b7a6d5c
Author: John Doe <john.doe@example.com>
Date:   Mon Jan 15 14:30:00 2024 -0500

    Add user authentication feature

diff --git a/auth.js b/auth.js
new file mode 100644
index 0000000..e69de29
```

### Blame (Line-by-line History)

```bash
# Show who changed each line
git blame filename.txt

# Blame with email
git blame -e filename.txt

# Blame specific lines
git blame -L 10,20 filename.txt
```

**Example:**
```bash
git blame index.html
```

**Output:**
```
3a7f8d2f (John Doe  2024-01-15 14:30:00 -0500  1) <!DOCTYPE html>
b4e9c1a3 (Jane Smith 2024-01-14 10:15:00 -0500  2) <html>
3a7f8d2f (John Doe  2024-01-15 14:30:00 -0500  3)   <head>
```

### Reference Log

```bash
# View all actions (including deleted commits)
git reflog

# View reflog for specific branch
git reflog show main

# View last 10 reflog entries
git reflog -10
```

**Example Output:**
```
3a7f8d2 HEAD@{0}: commit: Add user authentication feature
b4e9c1a HEAD@{1}: commit: Fix navigation bug
7d2a5f3 HEAD@{2}: commit: Update README
```

---

## Branching

### List Branches

```bash
# List local branches
git branch

# List all branches (local + remote)
git branch -a

# List remote branches only
git branch -r

# List branches with last commit
git branch -v

# List merged branches
git branch --merged

# List unmerged branches
git branch --no-merged
```

**Example:**
```bash
git branch -v
```

**Output:**
```
* main                3a7f8d2 Add user authentication feature
  feature/new-design  b4e9c1a Update styles
  bugfix/navbar       7d2a5f3 Fix navigation
```

### Create Branches

```bash
# Create new branch
git branch feature-login

# Create and switch to new branch (old method)
git checkout -b feature-login

# Create and switch to new branch (new method, Git 2.23+)
git switch -c feature-login

# Create branch from specific commit
git branch feature-login 3a7f8d2

# Create branch from remote branch
git branch feature-login origin/feature-login
```

**Example:**
```bash
git checkout -b feature/user-profile
```

**Output:**
```
Switched to a new branch 'feature/user-profile'
```

### Switch Branches

```bash
# Switch to branch (old method)
git checkout main

# Switch to branch (new method, Git 2.23+)
git switch main

# Switch to previous branch
git checkout -
git switch -

# Force switch (discard local changes)
git checkout -f branch-name
```

**Example:**
```bash
git switch main
```

**Output:**
```
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

### Delete Branches

```bash
# Delete local branch (safe)
git branch -d feature-login

# Force delete local branch
git branch -D feature-login

# Delete remote branch
git push origin --delete feature-login
```

**Example:**
```bash
git branch -d feature/old-feature
```

**Output:**
```
Deleted branch feature/old-feature (was 3a7f8d2).
```

### Rename Branches

```bash
# Rename current branch
git branch -m new-branch-name

# Rename specific branch
git branch -m old-name new-name
```

**Example:**
```bash
git branch -m feature/login feature/authentication
```

---

## Merging & Rebasing

### Merging

```bash
# Merge branch into current branch
git merge feature-branch

# Merge without fast-forward
git merge --no-ff feature-branch

# Merge with fast-forward only
git merge --ff-only feature-branch

# Squash merge (combine all commits)
git merge --squash feature-branch

# Abort merge
git merge --abort
```

**Example:**
```bash
git checkout main
git merge feature/user-profile
```

**Output (successful merge):**
```
Updating 3a7f8d2..b4e9c1a
Fast-forward
 profile.html | 45 +++++++++++++++++++++++++++++++++++++++++++++
 profile.css  | 23 +++++++++++++++++++++++
 2 files changed, 68 insertions(+)
 create mode 100644 profile.html
 create mode 100644 profile.css
```

**Output (with conflicts):**
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

### Handling Merge Conflicts

```bash
# View files with conflicts
git status

# Use "ours" version
git checkout --ours index.html

# Use "theirs" version
git checkout --theirs index.html

# After resolving conflicts
git add index.html
git commit

# Use merge tool
git mergetool
```

**Example Conflict in File:**
```html
<!DOCTYPE html>
<html>
<<<<<<< HEAD
  <title>My Website - Home</title>
=======
  <title>My Amazing Website</title>
>>>>>>> feature/new-title
</html>
```

**After Resolving:**
```html
<!DOCTYPE html>
<html>
  <title>My Amazing Website - Home</title>
</html>
```

### Rebasing

```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git add .
git rebase --continue

# Skip current commit
git rebase --skip

# Abort rebase
git rebase --abort
```

**Example:**
```bash
git checkout feature/user-profile
git rebase main
```

**Output:**
```
First, rewinding head to replay your work on top of it...
Applying: Add user profile page
Applying: Add profile styles
```

### Interactive Rebase

```bash
git rebase -i HEAD~3
```

**Interactive Editor Opens:**
```
pick 3a7f8d2 Add user profile page
pick b4e9c1a Add profile styles
pick 7d2a5f3 Fix profile bug

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# d, drop = remove commit
```

**Example - Squash commits:**
```
pick 3a7f8d2 Add user profile page
squash b4e9c1a Add profile styles
squash 7d2a5f3 Fix profile bug
```

---

## Remote Repositories

### Managing Remotes

```bash
# List remotes
git remote

# List remotes with URLs
git remote -v

# Add remote
git remote add origin https://github.com/username/repo.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream

# Show remote details
git remote show origin
```

**Example:**
```bash
git remote -v
```

**Output:**
```
origin  https://github.com/username/repo.git (fetch)
origin  https://github.com/username/repo.git (push)
upstream  https://github.com/original/repo.git (fetch)
upstream  https://github.com/original/repo.git (push)
```

### Fetching

```bash
# Fetch from origin
git fetch origin

# Fetch from all remotes
git fetch --all

# Fetch and prune deleted branches
git fetch --prune
git fetch -p

# Fetch specific branch
git fetch origin main

# Fetch tags
git fetch --tags
```

**Example:**
```bash
git fetch origin
```

**Output:**
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/username/repo
   3a7f8d2..b4e9c1a  main       -> origin/main
```

### Pulling

```bash
# Pull from origin/main
git pull origin main

# Pull with rebase
git pull --rebase

# Pull all branches
git pull --all

# Pull with autostash
git pull --autostash
```

**Example:**
```bash
git pull origin main
```

**Output:**
```
From https://github.com/username/repo
 * branch            main       -> FETCH_HEAD
Updating 3a7f8d2..b4e9c1a
Fast-forward
 README.md | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
```

### Pushing

```bash
# Push to origin/main
git push origin main

# Push and set upstream (first time)
git push -u origin main

# Push all branches
git push --all

# Push tags
git push --tags

# Force push (dangerous!)
git push --force

# Safer force push
git push --force-with-lease

# Delete remote branch
git push origin --delete feature-branch

# Push to different remote branch
git push origin local-branch:remote-branch
```

**Example:**
```bash
git push -u origin main
```

**Output:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 356 bytes | 356.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
To https://github.com/username/repo.git
   3a7f8d2..b4e9c1a  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

---

## Undoing Changes

### Discard Changes in Working Directory

```bash
# Discard changes in file (old method)
git checkout -- filename.txt

# Discard all changes
git checkout -- .

# Restore file (new method, Git 2.23+)
git restore filename.txt

# Restore all files
git restore .

# Restore from specific commit
git restore --source=HEAD~2 filename.txt
```

**Example:**
```bash
# Make unwanted changes
echo "Mistake" >> index.html

# Discard changes
git restore index.html

# Verify
git status
```

**Output:**
```
On branch main
nothing to commit, working tree clean
```

### Unstage Files

```bash
# Unstage file (old method)
git reset HEAD filename.txt

# Unstage file (new method)
git restore --staged filename.txt

# Unstage all files
git restore --staged .
```

**Example:**
```bash
git add mistake.txt
git restore --staged mistake.txt
git status
```

**Output:**
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        mistake.txt
```

### Undo Commits

```bash
# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes unstaged
git reset HEAD~1

# Undo last commit, discard changes (DANGEROUS!)
git reset --hard HEAD~1

# Undo multiple commits
git reset --hard HEAD~3

# Reset to specific commit
git reset --hard 3a7f8d2

# Reset to remote state
git reset --hard origin/main
```

**Example:**
```bash
# Create a commit
echo "Test" > test.txt
git add test.txt
git commit -m "Test commit"

# Undo it (keep changes)
git reset HEAD~1

git status
```

**Output:**
```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt
```

### Revert Commits

```bash
# Create new commit that undoes previous commit
git revert HEAD

# Revert specific commit
git revert 3a7f8d2

# Revert without committing
git revert -n 3a7f8d2

# Revert merge commit
git revert -m 1 merge-commit-hash
```

**Example:**
```bash
git revert HEAD
```

**Output:**
```
[main b4e9c1a] Revert "Add test feature"
 1 file changed, 0 insertions(+), 15 deletions(-)
 delete mode 100644 test.js
```

### Recover Lost Commits

```bash
# View reflog
git reflog

# Recover commit
git checkout 3a7f8d2

# Create branch from lost commit
git checkout -b recovery 3a7f8d2

# Cherry-pick lost commit
git cherry-pick 3a7f8d2
```

**Example:**
```bash
git reflog
```

**Output:**
```
b4e9c1a HEAD@{0}: reset: moving to HEAD~1
3a7f8d2 HEAD@{1}: commit: Add test feature
7d2a5f3 HEAD@{2}: commit: Update README
```

```bash
# Recover the commit
git cherry-pick 3a7f8d2
```

---

## Stashing

### Basic Stashing

```bash
# Stash current changes
git stash

# Stash with message
git stash save "Work in progress on login feature"

# Stash including untracked files
git stash -u

# Stash including untracked and ignored files
git stash -a
```

**Example:**
```bash
# Make some changes
echo "WIP" >> index.html

# Stash them
git stash

git status
```

**Output:**
```
On branch main
nothing to commit, working tree clean
```

### List and View Stashes

```bash
# List all stashes
git stash list

# Show stash contents (summary)
git stash show

# Show stash contents (detailed)
git stash show -p

# Show specific stash
git stash show stash@{1} -p
```

**Example:**
```bash
git stash list
```

**Output:**
```
stash@{0}: WIP on main: 3a7f8d2 Add authentication
stash@{1}: On feature: Work in progress on login
stash@{2}: WIP on main: b4e9c1a Fix bug
```

### Apply and Pop Stashes

```bash
# Apply most recent stash (keeps stash)
git stash apply

# Apply specific stash
git stash apply stash@{1}

# Apply and remove most recent stash
git stash pop

# Apply specific stash and remove it
git stash pop stash@{1}
```

**Example:**
```bash
git stash pop
```

**Output:**
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
        modified:   index.html

Dropped refs/stash@{0} (3a7f8d2f1c9b8e7a6d5c4b3a2f1e0d9c8b7a6d5c)
```

### Manage Stashes

```bash
# Create branch from stash
git stash branch feature-branch

# Create branch from specific stash
git stash branch feature-branch stash@{1}

# Delete specific stash
git stash drop stash@{1}

# Delete all stashes
git stash clear
```

**Example:**
```bash
git stash branch feature/quick-fix
```

**Output:**
```
Switched to a new branch 'feature/quick-fix'
On branch feature/quick-fix
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html

Dropped refs/stash@{0} (3a7f8d2f1c9b8e7a6d5c4b3a2f1e0d9c8b7a6d5c)
```

---

## Tags

### Create Tags

```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag (recommended)
git tag -a v1.0.0 -m "Version 1.0.0 release"

# Tag specific commit
git tag -a v1.0.0 3a7f8d2 -m "Version 1.0.0"

# Tag with detailed message
git tag -a v1.0.0
# Opens editor for detailed message
```

**Example:**
```bash
git tag -a v1.0.0 -m "First stable release"

git tag
```

**Output:**
```
v0.1.0
v0.2.0
v1.0.0
```

### List Tags

```bash
# List all tags
git tag

# List tags matching pattern
git tag -l "v1.*"

# List tags with messages
git tag -n
```

**Example:**
```bash
git tag -l "v1.*"
```

**Output:**
```
v1.0.0
v1.0.1
v1.1.0
```

### Show Tag Information

```bash
# Show tag details
git show v1.0.0

# Show tag commit
git rev-list -n 1 v1.0.0
```

**Example:**
```bash
git show v1.0.0
```

**Output:**
```
tag v1.0.0
Tagger: John Doe <john.doe@example.com>
Date:   Mon Jan 15 14:30:00 2024 -0500

First stable release

commit 3a7f8d2f1c9b8e7a6d5c4b3a2f1e0d9c8b7a6d5c
Author: John Doe <john.doe@example.com>
Date:   Mon Jan 15 14:30:00 2024 -0500

    Add final features for v1.0.0
```

### Push and Delete Tags

```bash
# Push specific tag
git push origin v1.0.0

# Push all tags
git push --tags

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0

# Fetch all tags
git fetch --tags
```

**Example:**
```bash
git push origin v1.0.0
```

**Output:**
```
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/username/repo.git
 * [new tag]         v1.0.0 -> v1.0.0
```

### Checkout Tags

```bash
# Checkout tag (detached HEAD)
git checkout v1.0.0

# Create branch from tag
git checkout -b version1 v1.0.0
```

**Example:**
```bash
git checkout v1.0.0
```

**Output:**
```
Note: switching to 'v1.0.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

---

## Inspecting & Comparing

### Diff Commands

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged

# Show all changes (staged + unstaged)
git diff HEAD

# Compare specific file
git diff index.html

# Compare two commits
git diff commit1 commit2

# Compare two branches
git diff main..feature-branch

# Show only filenames
git diff --name-only

# Show filenames with status
git diff --name-status

# Word-level diff
git diff --word-diff
```

**Example:**
```bash
git diff main..feature/new-design
```

**Output:**
```diff
diff --git a/style.css b/style.css
index 3a7f8d2..b4e9c1a 100644
--- a/style.css
+++ b/style.css
@@ -1,3 +1,7 @@
 body {
-  color: black;
+  color: #333;
+  font-family: Arial, sans-serif;
 }
+
+.header {
+  background: blue;
+}
```

### Compare Specific Files

```bash
# Compare file between branches
git diff main feature-branch -- index.html

# Show file from specific commit
git show HEAD:index.html

# Show file from specific branch
git show main:index.html
```

**Example:**
```bash
git diff main feature/redesign -- style.css
```

### Diff Statistics

```bash
# Show diff statistics
git diff --stat

# Show summary
git diff --summary

# Show compact stat
git diff --shortstat
```

**Example:**
```bash
git diff --stat main..feature/new-feature
```

**Output:**
```
 index.html |  15 +++++++++---
 style.css  |  42 ++++++++++++++++++++++++++++++++++
 app.js     |   8 ++++---
 3 files changed, 59 insertions(+), 6 deletions(-)
```

### Search in Commits

```bash
# Find commits that changed specific string
git log -S "function_name"

# Find commits with string in message
git log --grep="bug fix"

# Search in all commits
git log --all --grep="search term"
```

**Example:**
```bash
git log -S "loginUser" --oneline
```

**Output:**
```
3a7f8d2 Add login functionality
b4e9c1a Refactor authentication
```

---

## Cleaning Up

### Clean Untracked Files

```bash
# Dry run (see what would be deleted)
git clean -n

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd

# Remove ignored files too
git clean -fdx

# Remove only ignored files
git clean -fdX

# Interactive clean
git clean -i
```

**Example:**
```bash
git clean -n
```

**Output:**
```
Would remove temp.txt
Would remove cache/
Would remove build/
```

```bash
git clean -fd
```

**Output:**
```
Removing temp.txt
Removing cache/
Removing build/
```

### Prune Remote Branches

```bash
# Remove local references to deleted remote branches
git remote prune origin

# Fetch and prune in one command
git fetch --prune
```

**Example:**
```bash
git remote prune origin
```

**Output:**
```
Pruning origin
URL: https://github.com/username/repo.git
 * [pruned] origin/feature/old-feature
 * [pruned] origin/feature/obsolete
```

### Optimize Repository

```bash
# Garbage collection
git gc

# Aggressive garbage collection
git gc --aggressive

# Prune unreachable objects
git prune

# Show repository size
git count-objects -vH
```

**Example:**
```bash
git count-objects -vH
```

**Output:**
```
count: 234
size: 1.25 MiB
in-pack: 1567
packs: 1
size-pack: 15.67 MiB
prune-packable: 0
garbage: 0
size-garbage: 0 bytes
```

---

## Advanced Commands

### Cherry-Pick

```bash
# Apply specific commit to current branch
git cherry-pick 3a7f8d2

# Cherry-pick multiple commits
git cherry-pick 3a7f8d2 b4e9c1a

# Cherry-pick without committing
git cherry-pick -n 3a7f8d2

# Continue after conflict
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort
```

**Example:**
```bash
git cherry-pick 3a7f8d2
```

**Output:**
```
[main b4e9c1a] Add login feature
 Date: Mon Jan 15 14:30:00 2024 -0500
 1 file changed, 25 insertions(+)
 create mode 100644 login.js
```

### Bisect (Find Bugs)

```bash
# Start bisect
git bisect start

# Mark current as bad
git bisect bad

# Mark known good commit
git bisect good 3a7f8d2

# Mark current as good
git bisect good

# Mark current as bad
git bisect bad

# Skip commit
git bisect skip

# End bisect
git bisect reset
```

**Example:**
```bash
git bisect start
git bisect bad
git bisect good v1.0.0
```

**Output:**
```
Bisecting: 5 revisions left to test after this (roughly 3 steps)
[3a7f8d2f1c9b8e7a6d5c4b3a2f1e0d9c8b7a6d5c] Add feature X
```

### Submodules

```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone with submodules
git clone --recursive https://github.com/user/repo.git

# Update all submodules
git submodule update --remote

# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
```

**Example:**
```bash
git submodule add https://github.com/user/library.git lib/library
```

**Output:**
```
Cloning into '/home/user/project/lib/library'...
remote: Enumerating objects: 150, done.
remote: Counting objects: 100% (150/150), done.
remote: Compressing objects: 100% (100/100), done.
Receiving objects: 100% (150/150), 50.00 KiB | 1.00 MiB/s, done.
```

### Archive

```bash
# Create ZIP archive
git archive --format=zip HEAD > project.zip

# Create tar.gz archive
git archive --format=tar.gz HEAD > project.tar.gz

# Archive specific branch
git archive --format=zip feature-branch > feature.zip

# Archive with prefix
git archive --format=zip --prefix=project/ HEAD > project.zip
```

**Example:**
```bash
git archive --format=zip HEAD > myproject-v1.0.zip
```

### Other Advanced Commands

```bash
# List all tracked files
git ls-files

# List ignored files
git ls-files --ignored --exclude-standard

# Count commits
git rev-list --count HEAD

# Count commits by author
git shortlog -sn

# Verify repository
git fsck

# Ignore local changes (assume unchanged)
git update-index --assume-unchanged filename.txt

# Undo assume-unchanged
git update-index --no-assume-unchanged filename.txt

# List assume-unchanged files
git ls-files -v | grep '^h'
```

**Example:**
```bash
git shortlog -sn
```

**Output:**
```
    45  John Doe
    32  Jane Smith
    18  Bob Johnson
     5  Alice Williams
```

---

## Collaboration Workflows

### Feature Branch Workflow

```bash
# 1. Update main branch
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/user-authentication

# 3. Work on feature
git add .
git commit -m "Add login functionality"

# 4. Push feature branch
git push -u origin feature/user-authentication

# 5. After PR approval, update main
git checkout main
git pull origin main

# 6. Delete feature branch
git branch -d feature/user-authentication
git push origin --delete feature/user-authentication
```

**Example - Complete Workflow:**
```bash
# Start new feature
git checkout main
git pull origin main
git checkout -b feature/password-reset

# Work on feature
echo "Reset password code" > reset.js
git add reset.js
git commit -m "Add password reset functionality"

# Push to remote
git push -u origin feature/password-reset

# After merge, cleanup
git checkout main
git pull origin main
git branch -d feature/password-reset
```

### Fork and Pull Request Workflow

```bash
# 1. Fork repository on GitHub, then clone
git clone https://github.com/YOUR-USERNAME/repo.git
cd repo

# 2. Add upstream remote
git remote add upstream https://github.com/ORIGINAL-OWNER/repo.git

# 3. Create feature branch
git checkout -b fix/typo-in-readme

# 4. Make changes and commit
git add README.md
git commit -m "Fix typo in README"

# 5. Push to your fork
git push origin fix/typo-in-readme

# 6. Create Pull Request on GitHub

# 7. Keep fork updated
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

**Example - Syncing Fork:**
```bash
# Add upstream (only once)
git remote add upstream https://github.com/original/repo.git

# Fetch upstream changes
git fetch upstream
```

**Output:**
```
remote: Enumerating objects: 25, done.
remote: Counting objects: 100% (25/25), done.
remote: Compressing objects: 100% (15/15), done.
Unpacking objects: 100% (25/25), done.
From https://github.com/original/repo
 * [new branch]      main       -> upstream/main
```

```bash
# Merge upstream changes
git checkout main
git merge upstream/main
git push origin main
```

### Gitflow Workflow

```bash
# Initialize gitflow (if using git-flow extension)
git flow init

# Start new feature
git flow feature start user-profile

# Finish feature (merges to develop)
git flow feature finish user-profile

# Start release
git flow release start 1.0.0

# Finish release (merges to main and develop)
git flow release finish 1.0.0

# Start hotfix
git flow hotfix start critical-bug

# Finish hotfix
git flow hotfix finish critical-bug
```

**Manual Gitflow (without extension):**
```bash
# Create develop branch
git checkout -b develop main

# Create feature
git checkout -b feature/new-feature develop

# Finish feature
git checkout develop
git merge --no-ff feature/new-feature
git branch -d feature/new-feature

# Create release
git checkout -b release/1.0.0 develop

# Finish release
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"
git checkout develop
git merge --no-ff release/1.0.0
git branch -d release/1.0.0
```

---

## Git Aliases

### Create Aliases

```bash
# Basic aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit

# Useful aliases
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'
git config --global alias.undo 'reset --soft HEAD~1'
git config --global alias.amend 'commit --amend --no-edit'
```

**Example:**
```bash
# Set alias
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Use alias
git lg
```

**Output:**
```
* 3a7f8d2 - (HEAD -> main, origin/main) Add authentication (2 hours ago) <John Doe>
* b4e9c1a - Fix navbar bug (1 day ago) <Jane Smith>
* 7d2a5f3 - Update README (3 days ago) <John Doe>
```

### Popular Aliases Collection

```bash
# Status and log
git config --global alias.s 'status -s'
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
git config --global alias.hist 'log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short'

# Branch management
git config --global alias.bra 'branch -a'
git config --global alias.bdel 'branch -d'

# Commit shortcuts
git config --global alias.cm 'commit -m'
git config --global alias.ca 'commit -am'
git config --global alias.amend 'commit --amend --no-edit'

# Quick fixes
git config --global alias.unstage 'reset HEAD --'
git config --global alias.undo 'reset --soft HEAD~1'
git config --global alias.discard 'checkout --'

# Information
git config --global alias.contributors 'shortlog -sn'
git config --global alias.ls 'log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate'
git config --global alias.ll 'log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat'

# Sync
git config --global alias.sync '!git pull && git push'
git config --global alias.update 'pull --rebase --autostash'
```

### View and Remove Aliases

```bash
# View all aliases
git config --global --get-regexp alias

# Remove alias
git config --global --unset alias.cm
```

---

## Emergency Commands

### Scenario: Committed to Wrong Branch

**Problem:** You committed to `main` but should have used a feature branch.

```bash
# Create branch with current commits
git branch feature/accidental-work

# Reset main to match remote
git reset --hard origin/main

# Switch to new branch
git checkout feature/accidental-work
```

**Example:**
```bash
# You're on main with new commits
git branch feature/new-feature      # Create branch
git reset --hard origin/main        # Reset main
git checkout feature/new-feature    # Switch to feature branch
```

### Scenario: Need to Undo Last Push

**Problem:** You pushed something you shouldn't have.

```bash
# Safe way - revert the commit
git revert HEAD
git push

# Or reset and force push (if no one pulled yet)
git reset --hard HEAD~1
git push --force-with-lease
```

**Example:**
```bash
# Create revert commit
git revert HEAD
```

**Output:**
```
[main b4e9c1a] Revert "Accidentally committed secrets"
 1 file changed, 0 insertions(+), 5 deletions(-)
```

### Scenario: Accidentally Deleted Branch

**Problem:** You deleted a branch but need it back.

```bash
# Find the commit hash
git reflog

# Recreate branch
git checkout -b recovered-branch commit-hash
```

**Example:**
```bash
git reflog
```

**Output:**
```
b4e9c1a HEAD@{0}: checkout: moving from feature/deleted to main
3a7f8d2 HEAD@{1}: commit: Last commit on deleted branch
```

```bash
# Recover the branch
git checkout -b feature/recovered 3a7f8d2
```

### Scenario: Merge Conflict Panic

**Problem:** Merge created conflicts and you're not sure what to do.

```bash
# See conflicted files
git status

# Option 1: Abort the merge
git merge --abort

# Option 2: Use one version completely
git checkout --ours filename.txt    # Keep your version
git checkout --theirs filename.txt  # Keep their version
git add filename.txt
git commit

# Option 3: Manual resolution
# Edit file, remove conflict markers, then:
git add filename.txt
git commit
```

**Example:**
```bash
git merge feature/conflicting
```

**Output:**
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
# Abort if needed
git merge --abort
```

### Scenario: Committed Sensitive Data

**Problem:** You accidentally committed API keys or passwords.

```bash
# Remove from last commit
git rm --cached .env
echo ".env" >> .gitignore
git add .gitignore
git commit --amend -m "Remove sensitive file"
git push --force-with-lease

# Remove from entire history (use BFG Repo Cleaner)
# Download from: https://rtyley.github.io/bfg-repo-cleaner/
java -jar bfg.jar --delete-files .env
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

### Scenario: Detached HEAD State

**Problem:** You checked out a commit and now you're in detached HEAD.

```bash
# Option 1: Create branch from current state
git checkout -b new-branch-name

# Option 2: Return to previous branch
git checkout -

# Option 3: Go to main
git checkout main
```

**Example:**
```bash
git checkout 3a7f8d2
```

**Output:**
```
You are in 'detached HEAD' state...
```

```bash
# Create branch to save work
git checkout -b temp-work
```

### Scenario: Lost Commits After Reset

**Problem:** You did `git reset --hard` and lost commits.

```bash
# View reflog
git reflog

# Find lost commit
git checkout <commit-hash>

# Create branch from it
git checkout -b recovery <commit-hash>

# Or cherry-pick specific commits
git cherry-pick <commit-hash>
```

**Example:**
```bash
git reflog
```

**Output:**
```
3a7f8d2 HEAD@{0}: reset: moving to HEAD~3
b4e9c1a HEAD@{1}: commit: Important work (LOST)
7d2a5f3 HEAD@{2}: commit: More important work (LOST)
```

```bash
# Recover the work
git checkout -b recovery b4e9c1a
```

### Scenario: Want to Start Over Completely

**Problem:** Everything is messed up, need clean slate.

```bash
# Discard all local changes
git reset --hard HEAD
git clean -fd

# Reset to remote state
git fetch origin
git reset --hard origin/main

# Nuclear option - delete and reclone
cd ..
rm -rf project-folder
git clone https://github.com/username/repo.git
```

---

## Best Practices

### Commit Messages

**Good Commit Message Format:**
```
[type]: [short summary in present tense]

[optional detailed description]

[optional footer with references]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code formatting (no logic change)
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**

✅ **Good:**
```
feat: Add user authentication with JWT

Implemented login, logout, and token refresh endpoints.
Added middleware for protected routes.
Integrated bcrypt for password hashing.

Closes #123
```

✅ **Good:**
```
fix: Resolve null pointer exception in payment processing

Added null check before accessing user.payment object.
Added unit tests to prevent regression.

Fixes #456
```

❌ **Bad:**
```
Update stuff
```

❌ **Bad:**
```
Fixed bug
```

❌ **Bad:**
```
asdfasdf
```

### Branching Strategy

**Branch Naming Convention:**
```
feature/description     - New features
bugfix/description      - Bug fixes
hotfix/description      - Urgent production fixes
release/version         - Release preparation
docs/description        - Documentation updates
```

**Examples:**
```
feature/user-authentication
feature/payment-integration
bugfix/navbar-mobile-responsive
hotfix/security-patch
release/v1.2.0
docs/api-documentation
```

### .gitignore Best Practices

**Example .gitignore:**
```gitignore
# Dependencies
node_modules/
vendor/
bower_components/

# Environment variables
.env
.env.local
.env.*.local
*.env

# IDE and Editor files
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store

# Build outputs
dist/
build/
out/
target/

# Logs
*.log
logs/
npm-debug.log*

# Testing
coverage/
.nyc_output/

# OS files
Thumbs.db
desktop.ini

# Temporary files
*.tmp
*.temp
*.cache
.sass-cache/

# Package manager
package-lock.json  # if using yarn
yarn.lock         # if using npm
```

### Workflow Best Practices

**Daily Workflow:**
```bash
# Morning: Update your local repository
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/my-feature

# Work and commit frequently
git add .
git commit -m "feat: Add initial structure"

# More work
git add .
git commit -m "feat: Implement core functionality"

# Push to remote regularly
git push -u origin feature/my-feature

# Before final push: update with main
git checkout main
git pull origin main
git checkout feature/my-feature
git rebase main  # or merge main

# Final push
git push origin feature/my-feature
```

**Code Review Workflow:**
```bash
# Create PR from feature branch

# After review feedback
git checkout feature/my-feature
# Make changes
git add .
git commit -m "fix: Address review comments"
git push origin feature/my-feature

# After PR approval
git checkout main
git pull origin main
git branch -d feature/my-feature
git push origin --delete feature/my-feature
```

### Security Best Practices

**Never Commit:**
- Passwords
- API keys
- Private keys
- Access tokens
- Database credentials
- Environment files (.env)

**Use:**
- Environment variables
- Secret management tools
- .gitignore for sensitive files

**If you accidentally commit secrets:**
```bash
# Immediate action
git rm --cached .env
git commit --amend -m "Remove sensitive data"
git push --force-with-lease

# Rotate the exposed credentials immediately
# Change passwords, regenerate API keys, etc.
```

### Performance Best Practices

```bash
# Keep repository size manageable
# Don't commit large binaries
# Use Git LFS for large files
git lfs install
git lfs track "*.psd"

# Regular maintenance
git gc --aggressive
git prune

# Shallow clone for CI/CD
git clone --depth 1 https://github.com/user/repo.git
```

---

## Quick Reference

### Essential Commands

| Command | Description |
|---------|-------------|
| `git init` | Initialize repository |
| `git clone [url]` | Clone repository |
| `git status` | Check status |
| `git add [file]` | Stage file |
| `git commit -m "[msg]"` | Commit changes |
| `git push` | Push to remote |
| `git pull` | Pull from remote |
| `git branch` | List branches |
| `git checkout [branch]` | Switch branch |
| `git merge [branch]` | Merge branch |

### Quick Workflow

```bash
# Start new project
git init
git add .
git commit -m "Initial commit"
git remote add origin [url]
git push -u origin main

# Daily work
git pull
git checkout -b feature/new-feature
# ... make changes ...
git add .
git commit -m "Add new feature"
git push -u origin feature/new-feature

# Sync with main
git checkout main
git pull
git checkout feature/new-feature
git merge main

# After PR merged
git checkout main
git pull
git branch -d feature/new-feature
```

### Common Scenarios

**Undo last commit (keep changes):**
```bash
git reset HEAD~1
```

**Undo last commit (discard changes):**
```bash
git reset --hard HEAD~1
```

**Discard local changes:**
```bash
git restore .
```

**Update from remote:**
```bash
git pull --rebase
```

**Create and switch branch:**
```bash
git checkout -b feature/new-branch
```

**Delete branch:**
```bash
git branch -d feature/old-branch
```

**View commit history:**
```bash
git log --oneline --graph
```

**Stash changes:**
```bash
git stash
git stash pop
```

---

## Getting Help

### Command Help

```bash
# Get help for any command
git help [command]
git [command] --help
git [command] -h

# Examples
git help commit
git log --help
git push -h
```

### Useful Resources

- **Official Documentation:** https://git-scm.com/doc
- **Pro Git Book (Free):** https://git-scm.com/book
- **GitHub Guides:** https://guides.github.com
- **Atlassian Tutorials:** https://www.atlassian.com/git/tutorials
- **Git Cheat Sheet:** https://education.github.com/git-cheat-sheet-education.pdf
- **Interactive Learning:** https://learngitbranching.js.org

### Git GUI Tools

- **GitHub Desktop** - https://desktop.github.com
- **GitKraken** - https://www.gitkraken.com
- **SourceTree** - https://www.sourcetreeapp.com
- **Tower** - https://www.git-tower.com

---

## Contributing

If you find any errors or want to add more examples, feel free to contribute!

## License

This guide is free to use and distribute.

---

**Last Updated:** January 2024  
**Version:** 1.0