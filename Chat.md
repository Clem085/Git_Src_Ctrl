# Git Source Control

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

* `main` → Stable, tagged releases
* `develop` → Integration branch
* `feature/*` → Feature development branches
* `hotfixes` → Emergency fixes to `main`

Tags like `v1.0`, `v1.1`, and `v2.0` mark release points.

---

## ⚙️ Git Configuration (Recommended for Team)

### SSH Tag Signing Setup

```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
```

### Automatically Set Upstream for New Branches

```bash
git config --global push.autoSetupRemote true
```

This avoids `--set-upstream` every time you push a new branch.

---

## 🛠️ Core Workflow Commands (Fully Explained)

### Create `develop` From `main`

```bash
git checkout main
git pull origin main
git checkout -b develop
git push -u origin develop
```

Creates `develop` from the most up-to-date `main`. `develop` is where integration happens.

---

### Create a Feature Branch

```bash
git checkout develop
git pull origin develop
git checkout -b feature/my-feature
```

Feature branches are always created from `develop`. Use one branch per feature.

---

### Merge a Feature into `develop` (Preserving History)

```bash
git checkout develop
git pull origin develop
git merge --no-ff feature/my-feature -m "Merge feature/my-feature"
git push
```

`--no-ff` guarantees a merge commit so the feature's history stays grouped. This is critical for revertability.

---

### Pull vs Fetch + Merge

```bash
git pull              # Pulls from upstream tracking branch
```

Use this when staying up to date on a branch you’re working on.

```bash
git fetch origin
git merge origin/main   # Merge another branch without switching
```

Use this when you're on a branch (like `hotfix`) and want to sync with another (`main`).

---

### Rebase (Linear History, Use Carefully)

```bash
git checkout feature/my-feature
git fetch origin
git rebase origin/develop
```

Moves your local commits on top of the latest `develop`. Makes the history linear but **rewrites commits** — only use on branches you haven’t shared.

---

### Creating a Hotfix Branch

```bash
git checkout main
git pull origin main
git checkout -b hotfixes
```

Always base your hotfix branch off the latest `main`, then fix the issue.

---

### Merge Hotfix into `main`, Tag It

```bash
git checkout main
git merge --no-ff hotfixes -m "Apply critical fix"
git tag -sa v1.0.1 -m "Hotfix v1.0.1"
git push
git push origin v1.0.1
```

Tagging **must happen after the merge**, before the push.

---

### Merge Hotfix into `develop` (Backport Fix)

```bash
git checkout develop
git pull origin develop
git merge --no-ff hotfixes -m "Backport hotfix to develop"
git push
```

Keeps your development branch up to date with what’s in production.

---

### Syncing Hotfix with Latest Main Code

```bash
git checkout hotfixes
git fetch origin
git merge origin/main --no-ff -m "Merge main into hotfix to sync"
git push
```

Ensures your hotfix includes anything that might’ve changed in production.

---

## ⚔️ Handling Merge Conflicts in VS Code

1. Start the merge normally (e.g. `git merge feature/my-feature`)
2. VS Code will highlight all conflicted files
3. Open them to see inline merge markers:

   ```
   <<<<<<< HEAD
   Your current branch's version
   =======
   Incoming branch’s version
   >>>>>>> feature/my-feature
   ```
4. Use the VS Code buttons:

   * Accept Current
   * Accept Incoming
   * Accept Both
   * Compare
5. After resolving:

```bash
git add .
git commit
```

6. Push the result.

> Merge conflicts must be resolved before Git will allow you to complete the merge.

---

## 🔀 Merge vs Pull vs Rebase Summary

| Command  | What It Does                                                   | When to Use                                  |
| -------- | -------------------------------------------------------------- | -------------------------------------------- |
| `merge`  | Merges another branch into your current one via a merge commit | Always for hotfixes, features, or releases   |
| `pull`   | Fetches + merges from tracking branch                          | Stay up to date while working                |
| `rebase` | Reapplies commits onto a new base — rewrites history           | For clean, linear history **before merging** |

---

## 📊 Visualizing Git History

View your commit tree:

```bash
git log --oneline --graph --all --decorate
```

Save it:

```bash
git log --oneline --graph --all --decorate > git_graph.txt
```

The `git_graph.txt` output shows full commit structure including tags, branches, merges.

---

## 🧠 Best Practices & Lessons Learned

* Always use `--no-ff` merges to preserve branch history
* Pull latest from `main` before creating hotfixes
* Tag only after merging to `main`
* Avoid rebasing shared branches
* Use one branch per feature
* Push all tags explicitly: `git push origin vX.Y.Z`
* Use merge commit messages to describe purpose clearly

---

## 🧪 Practice Resources

* Simulate merge conflicts intentionally
* Try recovering a deleted branch from reflog
* Test reverts on merged features (`git revert -m 1 <merge-commit>`)
* Explore `git stash` for managing WIP code

---

## 📎 Reference Materials

* [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
* `cmd_hist` file: all raw commands used
* `git_graph.txt`: visual history of this project
* This document: your internal Git Flow tutorial
