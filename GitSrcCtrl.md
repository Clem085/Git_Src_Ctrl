# Git Source Control
## Introduction
The goal of this repository is to become more comforatable with using Git on larger projects that require advanced verson control techmics. (No more sacred timelines, prepare for a multiverse of madness)

# List of Commands
## Setup SSH Key Signing
Note: This assumes an SSH key has already beeen setup to be used for commits. All we have to do is tell Git to use the same key for tag signing

```bash
git config --global gpg.format ssh
```

```bash
git config --global user.signingkey ~/.ssh/id_ed25519.pub
```


## Create Tag
```bash
git tag -sa v1.0 -m "Initial Version Release"
```


## Typical Hotfix Flow
### 1. Merge hotfix into master
```bash
git checkout master
git merge hotfixes
```

### 2. Tag the release on master
```bash
git tag -a v1.0.1 -m "Hotfix release v1.0.1"
```

### 3. Push master and the tag
```bash
git push
```
git push origin v1.0.1

### 4. Also merge hotfix into develop if needed
```bash
git checkout develop
git merge hotfixes
git push
```

## List Branches
To List all Git Branches for a repo, enter the follwoing command 
```bash
git branch
```

## Create Development Branch
Note: Ensure your current branch is set to main before running this command
<!-- Add the command here to set current branch to main regardless -->

```bash
git checkout -b "develop"
```

## Create new File within Dev Branch
Created this markdown file.

## Push to Dev BRanch
```bash
git push --set-upstream origin develop
```


### Merging feature into develop
## 1. Make sure you're on develop and up to date
git checkout develop
git pull origin develop

## 2. Merge your feature branch with --no-ff to preserve history
git merge --no-ff feature/my-feature -m "Merge feature/my-feature: Add user authentication"

## 3. Push the result
git push origin develop


## 🔀 Git: Merge vs Pull vs Rebase

Understanding how Git integrates code from different branches is essential for managing a clean and collaborative workflow. Here's a breakdown of how `merge`, `pull`, and `rebase` differ:

| Command         | What It Does                                                                 | When to Use                                                                 |
|-----------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `git merge`     | Combines changes from another branch into the current branch via a new merge commit. | Use when integrating features, releases, or hotfixes without rewriting history. |
| `git pull`      | Equivalent to `git fetch` + `git merge` — pulls changes from the remote tracking branch into the current branch. | Use to stay up to date with the remote version of your current branch.      |
| `git rebase`    | Reapplies your commits on top of another branch’s HEAD, creating a new, linear history. | Use to clean up history or replay your changes on the latest version of `develop` or `main`. |

---

### ❓ Does `git pull` only pull the remote version of the current branch?

✅ **Yes** — by default, `git pull` only affects the branch you currently have checked out, and it only pulls from the **upstream tracking branch** (e.g., `origin/develop` if you're on `develop` and it’s tracking `origin/develop`).

To check your current upstream tracking branch:

```bash
git status
```

It will show something like:

```
Your branch is up to date with 'origin/feature/mybranch'.
```

To pull changes from a different branch, use:

```bash
git pull origin branch-name
```

---

### 🔧 Examples

```bash
# Merge a completed feature branch into develop
git checkout develop
git merge feature/login

# Pull latest remote changes into the current branch
git checkout develop
git pull

# Rebase your feature branch onto the latest develop
git checkout feature/login
git fetch
git rebase origin/develop
```

---

### 🧠 Tips

- ✅ **Use merge** to preserve all branch history (good for teams).
- ✅ **Use rebase** to make your history linear and easier to follow.
- ✅ **Use pull** frequently to stay up to date — just know it's merging behind the scenes.

> ⚠️ Avoid rebasing shared branches — it rewrites history and can confuse collaborators.


# Secret Message
> Added to Dev Branch, not main
```bash
echo "Hello World"
``` 