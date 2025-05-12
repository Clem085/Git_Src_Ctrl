# Git Source Control

## 📚 Table of Contents

* [🧭 Project Overview](#-project-overview)
* [📁 Branch Structure Summary (from `git_graph.txt`)](#-branch-structure-summary-from-git_graphtxt)
* [⚙️ Git Configuration](#️-git-configuration-recommended-for-team)
* [🛠️ Core Workflow Commands](#️-core-workflow-commands-fully-explained)
* [⚔️ Handling Merge Conflicts in VS Code](#️-handling-merge-conflicts-in-vs-code)
* [🔀 Merge vs Pull vs Rebase Summary](#-merge-vs-pull-vs-rebase-summary)
* [📊 Visualizing Git History](#-visualizing-git-history)
* [🧠 Best Practices & Lessons Learned](#-best-practices--lessons-learned)
* [🧪 Practice Resources](#-practice-resources)
* [📌 Reference Materials](#-reference-materials)

## 🧭 Project Overview

This project documents a full hands-on implementation of Git Flow, based on the article [A Successful Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/). It is designed as a comprehensive guide and training resource for engineers and developers with little to no Git experience.

The objective is to teach:

* How to use version control for collaborative development
* How to organize work using `main`, `develop`, `feature`, `release`, and `hotfix` branches
* How to manage merges, rebase, tagging, and upstream tracking
* How to resolve merge conflicts and preserve project history

---

## 📁 Branch Structure Summary (from `git_graph.txt`)

Following Git Flow:

* `main` → Stable, production-ready code (only updated from `release/*` or `hotfix/*`)
* `develop` → Integration branch for all features
* `feature/*` → Short-lived branches for individual features (e.g., `feature/login-ui`)
* `release/*` → Pre-release stabilization branches (e.g., `release/2.0.0`)
* `hotfix/*` → Emergency fixes branched from `main` (e.g., `hotfix/2.0.1`)

Tags like `v1.0.0`, `v1.1.0`, and `v2.0.0` mark official release points.

> 📝 **Note:** Each feature should be developed in a **separate branch** from `develop`. Feature branches follow the naming convention `feature/*`, where `*` is a brief, dash-separated description (e.g., `feature/export-csv`).

---

## ⚙️ Git Configuration (Recommended for Team)

### SSH Tag Signing Setup

```bash
git config --global gpg.format ssh
```

```bash
git config --global user.signingkey ~/.ssh/id_ed25519.pub
```

### Automatically Set Upstream for New Branches

```bash
git config --global push.autoSetupRemote true
```

### Git Line Ending Configuration (Cross-Platform)

**Add this `.gitattributes` file:**

```bash
# Use LF (Unix-style) endings for source code
*.c     text eol=lf
*.cpp   text eol=lf
*.h     text eol=lf
*.py    text eol=lf
*.sh    text eol=lf
*.md    text eol=lf
*.txt   text eol=lf

# Treat .git* files as text with LF
.gitignore text eol=lf
.gitattributes text eol=lf

# Use CRLF for Windows batch files (optional, for compatibility)
*.bat   text eol=crlf

# Prevent Git from touching line endings for binary files
*.jpg   binary
*.jpeg  binary
*.png   binary
*.gif   binary
*.pdf   binary
*.zip   binary
*.exe   binary
```

Re-normalize line endings:

```bash
git add --renormalize .
```

```bash
git commit -m "Normalize line endings using .gitattributes"
```

---

## 🛠️ Core Workflow Commands (Fully Explained)

### Create Tag

```bash
git tag -sa v1.0.0 -m "Initial Version Release"
```

```bash
git tag -sa v<version> -m "Release Description"
```

### Create `develop` From `main`

```bash
git checkout main
```

```bash
git pull origin main
```

```bash
git checkout -b develop
```

```bash
git push -u origin develop
```

### Create a Feature Branch

```bash
git checkout develop
```

```bash
git pull origin develop
```

```bash
git checkout -b feature/my-feature
```

### Merge a Feature into `develop`

```bash
git checkout develop
```

```bash
git pull origin develop
```

```bash
git merge --no-ff feature/my-feature -m "Merge feature/my-feature"
```

```bash
git push
```

### Start a Release Branch

```bash
git checkout develop
```

```bash
git pull origin develop
```

```bash
git checkout -b release/2.0.0
```

### Finish the Release

```bash
git checkout main
```

```bash
git merge --no-ff release/2.0.0 -m "Release v2.0.0"
```

```bash
git tag -sa v2.0.0 -m "Tagging release v2.0.0"
```

```bash
git push
```

```bash
git push origin v2.0.0
```

```bash
git checkout develop
```

```bash
git merge --no-ff release/2.0.0 -m "Merge release v2.0.0"
```

```bash
git push
```

```bash
git branch -d release/2.0.0
```

```bash
git push origin --delete release/2.0.0
```

### Creating a Hotfix Branch

```bash
git checkout main
```

```bash
git pull origin main
```

```bash
git checkout -b hotfix/2.0.1
```

### Merge Hotfix into `main`, Tag It

```bash
git checkout main
```

```bash
git merge --no-ff hotfix/2.0.1 -m "Apply hotfix v2.0.1"
```

```bash
git tag -sa v2.0.1 -m "Hotfix v2.0.1"
```

```bash
git push
```

```bash
git push origin v2.0.1
```

### Merge Hotfix into `develop`

```bash
git checkout develop
```

```bash
git pull origin develop
```

```bash
git merge --no-ff hotfix/2.0.1 -m "Backport hotfix v2.0.1"
```

```bash
git push
```

### Clean Up Hotfix

```bash
git branch -d hotfix/2.0.1
```

```bash
git push origin --delete hotfix/2.0.1
```

### Syncing Hotfix with Latest Main Code

```bash
git checkout hotfix/2.0.1
```

```bash
git fetch origin
```

```bash
git merge origin/main --no-ff -m "Sync hotfix with main"
```

```bash
git push
```

---

## ⚔️ Handling Merge Conflicts in VS Code

1. Start the merge:

```bash
git merge feature/my-feature
```

2. VS Code highlights conflicted files
3. Open files and resolve blocks manually
4. Use resolution buttons in VS Code
5. Save, stage, and commit:

```bash
git add .
```

```bash
git commit -m "Commit Message"
```

6. Push the result

---

## 🔀 Merge vs Pull vs Rebase Summary

| Command  | What It Does                                          | When to Use                                     |
| -------- | ----------------------------------------------------- | ----------------------------------------------- |
| `merge`  | Combines branches with a merge commit                 | Always use for features, releases, and hotfixes |
| `pull`   | Shortcut for fetch + merge of current upstream branch | Keep local branch up to date                    |
| `rebase` | Rewrites commits onto a new base (linear history)     | Use only on local/private feature branches      |

---

## 📊 Visualizing Git History

```bash
git log --oneline --graph --all --decorate
```

```bash
git log --oneline --graph --all --decorate > git_graph.txt
```

---

## 🧠 Best Practices & Lessons Learned

* Use `--no-ff` to preserve branch history
* One feature per `feature/*` branch from `develop`
* Create `release/*` for each version
* Hotfix from `main`, merge into `main` and `develop`
* Tag releases **after** merging into `main`
* Clean up merged branches
* Avoid rebase on shared branches

---

## 🧪 Practice Resources

* Simulate merge conflicts
* Try reverting a merge commit:

```bash
git revert -m 1 <merge-commit>
```

* Use `git stash` for managing WIP
* Use reflog to recover deleted branches

---

## 📌 Reference Materials

* [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
* `cmd_hist`: full command history
* `git_graph.txt`: visual history output
* This document: internal Git Flow tutorial
