## 🧭 Git Quick Reference (with Explanations for Mac & Windows)

---

### 🛑 Stop a Running Process (Like a Test or Server)

| Task                  | Mac/Linux | Windows    |
| --------------------- | --------- | ---------- |
| Stop a process / test | `Cmd + C` | `Ctrl + C` |

> ✨ This stops a running command in the terminal (e.g., `npm test`, `live-server`, etc.)

---

### ✅ `git commit -m "message"`

**What it does:**  
Saves the current staged changes to the project history with a commit message.

```bash
git commit -m "Add homepage layout"
```

> 💡 Always write clear, concise commit messages that explain what you changed.

---

### 🌍 Connect to GitHub

```bash
git remote add origin https://github.com/your-username/repo-name.git
git branch -M main
git push -u origin main
```

**Explanation:**

- `git remote add origin <url>` → Links your local repo to a GitHub repo.
- `git branch -M main` → Renames the default branch to `main`.
- `git push -u origin main` → Uploads your code and sets `main` to track the remote.

---

### 📋 `git status`

**What it does:**  
Shows the current state of your working directory and staging area.

> Tells you:
- Which files have been modified
- Which files are staged
- Which files are untracked

```bash
git status
```

---

### ➕ `git add`

**What it does:**  
Stages changes to be committed.

```bash
git add file.js       # Adds a specific file
git add .             # Adds all modified/untracked files
```

> 🧠 Files must be staged before they can be committed.

---

### 📜 `git log`

**What it does:**  
Displays a history of your commits (including commit hash, author, date, and message).

```bash
git log
```

---

### 🌿 `git branch`

```bash
git branch        # Lists local branches
git branch -a     # Lists all (local + remote) branches
git branch -M new-name   # Renames current branch
```

> 📌 Use `-M` instead of `-m` when overwriting a branch name.

---

### 🔁 `git checkout`

```bash
git checkout -b new-feature
```

**Creates and switches** to a new branch.

```bash
git checkout main
```

**Switches** to an existing branch.

---

### 🔄 `git pull origin main`

**What it does:**  
Downloads the latest changes from the remote repo and merges them into your local branch.

```bash
git pull origin main
```

> 🌐 Use this to keep your local project up-to-date with the team or remote repo.

---

### 📤 `git push`

```bash
git push                 # Pushes changes to remote (if tracking is set)
git push -u origin main  # Pushes and sets tracking to `origin/main`
```

> 🔗 After using `-u` once, future `git push` commands will know where to go.

---

### 🗑 Undo or Remove Files

```bash
git reset filename       # Unstages a file
git restore filename     # Restores file to last committed version
git rm filename          # Removes file and stages the deletion
```

> ⚠️ Use `restore` and `reset` to undo mistakes. Be cautious with `rm`.

---

### 🧼 `git clean`

```bash
git clean -fd
```

**What it does:**  
Deletes all untracked files and folders.

> 🔥 Use with caution — this is permanent!

---

### 🧭 Git Workflow Example

```bash
git init                              # Create a local Git repo
git add .                             # Stage all files
git commit -m "Initial commit"        # Save your changes
git remote add origin <repo-url>      # Link to GitHub
git branch -M main                    # Rename branch to main
git push -u origin main               # Push and track remote branch
```

---

## 💻 Terminal Basics (Mac vs Windows)

| Task               | Mac/Linux     | Windows (Git Bash / CMD) |
|--------------------|---------------|---------------------------|
| Navigate to folder | `cd folder`   | `cd folder`               |
| Go back a folder   | `cd ..`       | `cd ..`                   |
| List files         | `ls`          | `ls` or `dir`             |
| Clear terminal     | `clear`       | `clear` or `cls`          |

---

## 📌 Summary

- `git init` → Start a new Git repository  
- `git add` → Stage files  
- `git commit` → Save changes  
- `git push` → Upload to GitHub  
- `git pul
