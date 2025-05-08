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