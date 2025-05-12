---
# 🧠 Git Flow Cheat Sheet

## 🔧 Initial Setup
```bash
git config --global push.autoSetupRemote true
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
```

---

## 📁 Branching Model
- `main` → Production-ready code
- `develop` → Integration branch
- `feature/*` → Feature branches (from `develop`)
- `hotfixes` → Emergency fixes (from `main`)

---

## 🚀 Common Commands

### ✅ Start a Feature
```bash
git checkout develop
git pull origin develop
git checkout -b feature/your-feature
```

### 🧩 Merge Feature into Develop
```bash
git checkout develop
git pull origin develop
git merge --no-ff feature/your-feature -m "Merge feature"
git push
```

### 🔥 Start a Hotfix
```bash
git checkout main
git pull origin main
git checkout -b hotfixes
```

### 🩹 Merge Hotfix into Main and Tag
```bash
git checkout main
git merge --no-ff hotfixes -m "Apply hotfix"
git tag -sa vX.Y.Z -m "Hotfix release"
git push
git push origin vX.Y.Z
```

### ♻️ Merge Hotfix into Develop
```bash
git checkout develop
git pull origin develop
git merge --no-ff hotfixes -m "Backport hotfix"
git push
```

### 🔁 Sync Hotfix with Main
```bash
git checkout hotfixes
git fetch origin
git merge origin/main --no-ff -m "Sync with main"
git push
```

---

## 🛠 Merge Conflict Workflow (VS Code)
1. Run merge (e.g. `git merge feature/foo`)
2. Open conflicted files in VS Code
3. Use buttons: Accept Current / Incoming / Both
4. Save, stage, commit:
```bash
git add .
git commit
git push
```

---

## 📊 View Git History
```bash
git log --oneline --graph --all --decorate
```

---

## 🧠 Best Practices
- ✅ Use `--no-ff` to preserve branch context
- 🏷️ Tag only after merging into `main`
- ⚠️ Don’t rebase shared branches
- 📥 Always `pull` before starting merges
- 📌 Push tags explicitly: `git push origin vX.Y.Z`

---

## 📚 Learn More
- Full guide: [nvie Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
