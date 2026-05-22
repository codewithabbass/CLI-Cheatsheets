<div align="center">

# 🌿 Git Commands

</div>

[← Back to Home](../README.md)

---

## Table of Contents

- [Setup & Configuration](#-setup--configuration)
- [Initialize & Clone](#-initialize--clone)
- [Staging & Committing](#-staging--committing)
- [Branching](#-branching)
- [Remote & Sync](#-remote--sync)
- [Undoing Changes](#-undoing-changes)
- [Stashing](#-stashing)
- [Tagging](#-tagging)
- [Log & History](#-log--history)
- [Diff & Comparison](#-diff--comparison)
- [Submodules](#-submodules)

---

## ⚙️ Setup & Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"    # VS Code as editor
git config --global init.defaultBranch main

git config --list                                 # View all settings
git config --global --list                        # Global settings only
git config user.name                              # Read single value
```

---

## 🚀 Initialize & Clone

```bash
git init                                    # Init new repo in current dir
git init my-project                         # Init in new directory

git clone https://github.com/user/repo.git  # Clone remote repo
git clone https://github.com/user/repo.git my-folder  # Clone into named folder
git clone --depth 1 https://github.com/user/repo.git  # Shallow clone (latest only)
git clone --branch dev https://github.com/user/repo.git  # Clone specific branch
```

---

## 📝 Staging & Committing

### Stage changes

```bash
git add file.txt              # Stage specific file
git add .                     # Stage all changes
git add -p                    # Interactively stage chunks
git add *.js                  # Stage by pattern
```

---

### Commit changes

```bash
git commit -m "Your message"          # Commit with message
git commit -am "Message"              # Stage tracked files + commit
git commit --amend -m "New message"   # Amend last commit message
git commit --amend --no-edit          # Add to last commit, keep message
```

> ⚠️ `--amend` rewrites history. Only amend commits that haven't been pushed.

---

### View status

```bash
git status                    # Show working tree status
git status -s                 # Short/compact format
```

---

## 🌿 Branching

### Create and switch branches

```bash
git branch                              # List local branches
git branch -a                           # List all branches (incl. remote)
git branch feature/login                # Create branch
git checkout feature/login              # Switch to branch
git checkout -b feature/login           # Create and switch (classic)
git switch feature/login                # Switch (modern)
git switch -c feature/login             # Create and switch (modern)
```

---

### Merge branches

```bash
git merge feature/login                 # Merge into current branch
git merge --no-ff feature/login         # Merge with merge commit always
git merge --squash feature/login        # Squash all commits into one
```

---

### Rebase

```bash
git rebase main                         # Rebase current branch onto main
git rebase -i HEAD~3                    # Interactive rebase (last 3 commits)
git rebase --abort                      # Abort rebase
git rebase --continue                   # Continue after resolving conflicts
```

---

### Delete branches

```bash
git branch -d feature/login             # Delete merged branch
git branch -D feature/login             # Force delete
git push origin --delete feature/login  # Delete remote branch
```

---

## 🔄 Remote & Sync

### Manage remotes

```bash
git remote -v                            # List remotes
git remote add origin https://...        # Add remote
git remote remove origin                 # Remove remote
git remote rename origin upstream        # Rename remote
git remote set-url origin https://...    # Change URL
```

---

### Fetch, pull, push

```bash
git fetch                                # Download changes (no merge)
git fetch origin                         # Fetch specific remote
git pull                                 # Fetch + merge
git pull --rebase                        # Fetch + rebase instead of merge
git pull origin main                     # Pull specific branch

git push                                 # Push current branch
git push origin main                     # Push to specific remote/branch
git push -u origin feature/login         # Push + set upstream tracking
git push --force-with-lease              # Safe force push
git push --tags                          # Push all tags
```

> ⚠️ Avoid `git push --force` on shared branches — use `--force-with-lease` instead.

---

## ↩️ Undoing Changes

### Discard unstaged changes

```bash
git restore file.txt                     # Discard changes in working dir
git restore .                            # Discard all unstaged changes
git checkout -- file.txt                 # (classic equivalent)
```

---

### Unstage files

```bash
git restore --staged file.txt            # Unstage specific file
git reset HEAD file.txt                  # (classic equivalent)
```

---

### Undo commits

```bash
git revert <commit-hash>                 # Create new commit that undoes changes (safe)
git reset --soft HEAD~1                  # Undo last commit, keep changes staged
git reset --mixed HEAD~1                 # Undo last commit, keep changes unstaged
git reset --hard HEAD~1                  # Undo last commit, discard all changes
```

> ⚠️ `git reset --hard` is destructive. Changes cannot be recovered without reflog.

---

### Use reflog (safety net)

```bash
git reflog                               # Show history of HEAD movements
git checkout HEAD@{2}                    # Go back to a previous HEAD state
git reset --hard HEAD@{2}               # Reset to a previous state
```

---

## 📦 Stashing

```bash
git stash                                # Stash current changes
git stash push -m "work in progress"     # Stash with description
git stash list                           # List all stashes
git stash apply                          # Apply most recent stash (keep it)
git stash apply stash@{2}               # Apply specific stash
git stash pop                            # Apply and remove most recent stash
git stash drop stash@{1}                # Delete specific stash
git stash clear                          # Delete all stashes
git stash branch feature/new stash@{0}  # Create branch from stash
```

---

## 🏷️ Tagging

```bash
git tag                                  # List tags
git tag v1.0.0                           # Create lightweight tag
git tag -a v1.0.0 -m "Release v1.0.0"   # Annotated tag
git tag -a v1.0.0 <commit-hash>          # Tag specific commit
git push origin v1.0.0                   # Push single tag
git push origin --tags                   # Push all tags
git tag -d v1.0.0                        # Delete local tag
git push origin --delete v1.0.0          # Delete remote tag
git show v1.0.0                          # Show tag details
```

---

## 📜 Log & History

```bash
git log                                  # Full commit history
git log --oneline                        # One line per commit
git log --oneline --graph --all          # Visual branch graph
git log --oneline -10                    # Last 10 commits
git log --author="Alice"                 # Filter by author
git log --since="2024-01-01"             # Filter by date
git log --grep="fix"                     # Filter by commit message
git log -- file.txt                      # History of specific file
git log -p file.txt                      # Show diff for each commit
git log --stat                           # Show changed file stats
git log --follow file.txt                # Follow renames
git shortlog -sn                         # Commits per author
```

---

## 🔍 Diff & Comparison

```bash
git diff                                 # Unstaged changes
git diff --staged                        # Staged changes (vs last commit)
git diff HEAD                            # All changes vs last commit
git diff main..feature/login             # Compare branches
git diff <hash1>..<hash2>                # Compare commits
git diff --name-only                     # Only filenames
git show <commit-hash>                   # Show commit changes
git show HEAD                            # Show last commit
```

---

## 🔗 Submodules

```bash
git submodule add https://github.com/user/repo.git path/to/sub
git submodule init                       # Initialize after clone
git submodule update                     # Pull latest submodule content
git submodule update --init --recursive  # Init + update nested
git submodule foreach git pull           # Update all submodules
```

---

### Useful aliases (add to `~/.gitconfig`)

```ini
[alias]
  lg = log --oneline --graph --all --decorate
  st = status -s
  co = checkout
  sw = switch
  cm = commit -m
  undo = reset --soft HEAD~1
  wip = !git add -A && git commit -m "WIP"
```

---

[← Linux](linux.md) | [Back to Home](../README.md) | [☸️ Kubernetes →](kubernetes.md)
