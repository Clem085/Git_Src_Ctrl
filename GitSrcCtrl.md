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