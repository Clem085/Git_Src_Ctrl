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


## 🔀 Git: Merge vs Pull vs Rebase

Understanding how Git integrates code from different branches is essential for managing a clean history. Here's a quick breakdown:

| Command         | What It Does                                                                 | When to Use                                               |
|-----------------|------------------------------------------------------------------------------|------------------------------------------------------------|
| `git merge`     | Combines changes from one branch into another via a merge commit.            | Safe and easy. Use when integrating feature, release, or hotfix branches. |
| `git pull`      | Fetches latest changes from remote and merges them into your current branch. | Use frequently to sync your branch with the remote (origin). |
| `git rebase`    | Rewrites your commits to appear as if they were made after the latest base.  | Use to create a linear history before merging to `develop` or `main`. |

### 🔧 Examples

```bash
# Merge 'feature/login' into 'develop'
git checkout develop
git merge feature/login

# Pull latest remote changes into 'develop'
git checkout develop
git pull

# Rebase feature branch onto latest develop
git checkout feature/login
git rebase develop
```

### 🧠 Tips

- Use **merge** for clarity when working with a team — preserves context and branch history.
- Use **rebase** to clean up your history *before* merging — makes it easier to follow linear progress.
- Use **pull** regularly to stay up to date with remote changes.

> 🔥 Avoid rebasing public branches that others are using — it rewrites history!

