# Git Source Control

## 📚 Table of Contents

* [🧭 Project Overview](#-project-overview)
* [📁 Branch Structure Summary (from `git_graph.txt`)](#-branch-structure-summary-from-git_graphtxt)
* [⚙️ Git Configuration](#️-git-configuration-recommended-for-team)
* [🛠️ Core Workflow Commands](#️-core-workflow-commands-fully-explained)

  <!-- * [Create Tag](#create-tag)
  * [Create `develop` From `main`](#create-develop-from-main)
  * [Create a Feature Branch](#create-a-feature-branch)
  * [Merge a Feature into `develop`](#merge-a-feature-into-develop)
  * [Start a Release Branch](#start-a-release-branch)
  * [Finish the Release](#finish-the-release)
  * [Creating a Hotfix Branch](#creating-a-hotfix-branch)
  * [Merge Hotfix into `main`, Tag It](#merge-hotfix-into-main-tag-it)
  * [Merge Hotfix into `develop`](#merge-hotfix-into-develop)
  * [Clean Up Hotfix](#clean-up-hotfix)
  * [Syncing Hotfix with Latest Main Code](#syncing-hotfix-with-latest-main-code) -->
* [⚔️ Handling Merge Conflicts in VS Code](#️-handling-merge-conflicts-in-vs-code)
* [🔀 Merge vs Pull vs Rebase Summary](#-merge-vs-pull-vs-rebase-summary)
* [📊 Visualizing Git History](#-visualizing-git-history)
* [🧠 Best Practices & Lessons Learned](#-best-practices--lessons-learned)
* [🧪 Practice Resources](#-practice-resources)
* [📎 Reference Materials](#-reference-materials)


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
git config --global user.signingkey ~/.ssh/id_ed25519.pub
```

### Automatically Set Upstream for New Branches

```bash
git config --global push.autoSetupRemote true
```

This avoids `--set-upstream` every time you push a new branch.

---

## 🛠️ Core Workflow Commands (Fully Explained)

### Create Tag

Tags are used to label release versions on the `main` branch.

```bash
git tag -sa v1.0.0 -m "Initial Version Release"
```

**General Syntax:**

```bash
git tag -sa v<version> -m "Release Description"
```

### 🔴 **TODO:** Add info on our versioning semantics and rules

**Proposed Versioning Format:**
`v<major>.<minor>.<patch>`

```
Version:  MAJOR.MINOR.PATCH
            ↑     ↑     ↑
            │     │     └── Hotfix / Patch (urgent or critical fixes)
            │     └──────── Minor (new features, non-breaking updates)
            └────────────── Major (significant changes or merges from develop to main)
```

**Examples:**

* `v1.0.0` — Initial release to `main`
* `v1.2.0` — New features from `develop`
* `v1.2.1` — Hotfix applied to `main`

> 🔎 **Note:** Versions are compared numerically (not as strings), so `v3.10.2` is newer than `v3.1.2`.
> ✅ **Do not zero-pad** version numbers (e.g., use `v3.10.2`, not `v3.010.2`) — Git and SemVer treat `010` as `10`, but some tools may not.

---

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

Feature branches are short-lived and created from `develop`.

---

### Merge a Feature into `develop`

```bash
git checkout develop
git pull origin develop
git merge --no-ff feature/my-feature -m "Merge feature/my-feature"
git push
```

Use `--no-ff` to preserve a merge commit and group the feature history.

---

### Start a Release Branch

```bash
git checkout develop
git pull origin develop
git checkout -b release/2.0.0
```

Stabilize the release here (bug fixes, version bumps, documentation).

---

### Finish the Release

```bash
git checkout main
git merge --no-ff release/2.0.0 -m "Release v2.0.0"
git tag -sa v2.0.0 -m "Tagging release v2.0.0"
git push
git push origin v2.0.0

# Backport release fixes to develop
git checkout develop
git merge --no-ff release/2.0.0 -m "Merge release v2.0.0"
git push

# Clean up
git branch -d release/2.0.0
git push origin --delete release/2.0.0
```

---

### Creating a Hotfix Branch

```bash
git checkout main
git pull origin main
git checkout -b hotfix/2.0.1
```

Hotfix branches are used for urgent production fixes.

---

### Merge Hotfix into `main`, Tag It

```bash
git checkout main
git merge --no-ff hotfix/2.0.1 -m "Apply hotfix v2.0.1"
git tag -sa v2.0.1 -m "Hotfix v2.0.1"
git push
git push origin v2.0.1
```

---

### Merge Hotfix into `develop`

```bash
git checkout develop
git pull origin develop
git merge --no-ff hotfix/2.0.1 -m "Backport hotfix v2.0.1"
git push
```

---

### Clean Up Hotfix

```bash
git branch -d hotfix/2.0.1
git push origin --delete hotfix/2.0.1
```

---

### Syncing Hotfix with Latest Main Code

```bash
git checkout hotfix/2.0.1
git fetch origin
git merge origin/main --no-ff -m "Sync hotfix with main"
git push
```

---

## ⚔️ Handling Merge Conflicts in VS Code

1. Start the merge (e.g. `git merge feature/my-feature`)
2. VS Code highlights conflicted files
3. Open files to resolve blocks:

   ```
   <<<<<<< HEAD
   Your version
   =======
   Incoming version
   >>>>>>> feature/my-feature
   ```
4. Use resolution buttons
5. Save + stage:

```bash
git add .
git commit
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
* Try reverting a merge commit (`git revert -m 1 <merge-commit>`)
* Use `git stash` for managing WIP
* Use reflog to recover deleted branches

---

## 📎 Reference Materials

* [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
* `cmd_hist`: full command history
* `git_graph.txt`: visual history output
* This document: internal Git Flow tutorial