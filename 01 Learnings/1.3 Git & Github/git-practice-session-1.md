# Git Practice Notes

These notes guide a beginner through setting up Git, configuring it, authenticating with GitHub, creating a local repository, connecting to a remote, branching, merging, and pushing changes.

## Prerequisites

- Install Git: [https://git-scm.com/downloads](https://git-scm.com/downloads)
- (Optional) Install GitHub CLI: [https://cli.github.com/](https://cli.github.com/)
- (Optional) Install Python and Node.js if your project requires them.

---

## 1. Verify Git Installation

```bash
git --version
```

Ensure you see a version number (e.g., `git version 2.34.1`).

---

## 2. Configure Git User Information

Set your name and email for commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Verify the settings:

```bash
git config --global --list
```

---

## 3. Configure Credential Helper

To cache your credentials (Windows):

```bash
git config --global credential.helper manager
```

---
## 4. Create a New Local Repository

Navigate to your desired directory and create a project folder:

```bash
cd ~/Downloads
mkdir my-test-repo
cd my-test-repo
```

Initialize the Git repository:

```bash
git init
```

---

## 5. Add a README File

Create a README:

```bash
echo "# My Test Repo" > README.md
```

---

## 6. Stage and Commit Changes

```bash
git add .
git commit -m "Initial commit"
```

---

## 7. Create a Remote Repository on GitHub

Use GitHub CLI to create and push in one step:

```bash
gh repo create Practice-Repo --public --source=. --remote=origin --push
```

Alternatively, create the repository on GitHub's website and note the URL.

---

## 8. Add Remote and Push (Manual Method)

If you created the repo manually, add the remote:

```bash
git remote add origin https://github.com/USERNAME/Practice-Repo.git
```

Verify the remote:

```bash
git remote -v
```

Push to the default branch (often `main`):

```bash
git push -u origin main
```

> **Note:** Some repositories default to `master`. Replace `main` with `master` if needed.

---

## 9. Working with Branches

List branches:

```bash
git branch
```

Create and switch to a new branch:

```bash
git checkout -b first-branch
```

Make changes, then stage, commit, and push:

```bash
# Example: create an HTML file
echo "<!DOCTYPE html><html><head><title>Test</title></head><body><h1>Hello</h1></body></html>" > index.html
git add .
git commit -m "Add HTML file"
git push -u origin first-branch
```

---

## 10. Merging Branches

Switch back to the main branch:

```bash
git checkout main
```

Merge changes from `first-branch`:

```bash
git merge first-branch
```

Push merged changes:

```bash
git push
```

---

## 11. Additional Tips and Commands

- **Change a remote URL:**
    
    ```bash
    git remote set-url origin https://github.com/USERNAME/Practice-Repo.git
    ```
    
- **View commit history:**
    
    ```bash
    git log --oneline
    ```
    
- **Delete a local branch:**
    
    ```bash
    git branch -d first-branch
    ```
    
- **Check Python and Node versions:**
    
    ```bash
    python --version
    node --version
    ```
    
- **Clear the terminal (macOS/Linux):**
    
    ```bash
    clear
    ```
    
- **List directory contents:**
    
    ```bash
    ls
    ```
    

---

These steps should provide a clear, error-free workflow for beginners working with Git and GitHub.