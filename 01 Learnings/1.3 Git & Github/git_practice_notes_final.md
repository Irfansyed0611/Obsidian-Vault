
# Git Basics: A Beginner's Guide with Step-by-Step Notes

## 1. Git Installation and Configuration
To begin using Git, make sure you have it installed and configured with your username and email. This helps associate your commits with your identity.

```bash
git --version                       # Check if Git is installed and verify the version.
git config --global user.name "Syed Irfan"   # Set your global Git username.
git config --global user.email "syedirfan0611@gmail.com"   # Set your global email address.
git config --global credential.helper manager   # Set the credential helper to store credentials securely.
git config --global --list             # Verify the configuration settings.
```

## 2. Setting Up a New Git Repository

### Create a New Local Directory for Your Project

```bash
mkdir my-test-repo                # Create a new directory for your repository.
cd my-test-repo/                  # Navigate into the newly created directory.
git init                          # Initialize a new Git repository in the directory.
```

### Add a `README.md` file and Make Your First Commit

```bash
echo "# My test repo" > README.md  # Create a README.md file with your repo title.
git add .                         # Stage the new README file for commit.
git commit -m "Initial commit"     # Commit the staged files with a message.
```

## 3. Connecting to a Remote Repository on GitHub

### Add a Remote Repository

```bash
git remote add origin https://github.com/Irfansyed0611/Practice-Repo.git  # Add the remote GitHub repository.
```

### Push the Local Repository to GitHub

```bash
git push -u origin master  # Push your local changes to the master branch on GitHub.
```

## 4. Creating and Managing Branches

### Create and Switch to a New Branch

```bash
git checkout -b first-branch  # Create and switch to a new branch called first-branch.
```

### Make Changes to the New Branch

Let’s say you edit the README file or add new files, then:

```bash
git add .                # Stage the changes for commit.
git commit -m "Add changes in first-branch"  # Commit the changes with a message.
git push -u origin first-branch  # Push the first-branch to GitHub.
```

### Switch Back to the Master Branch

```bash
git checkout master       # Switch back to the master branch.
```

## 5. Merging Changes from Another Branch

### Merge `first-branch` into `master`

To bring the changes from `first-branch` into `master`:

```bash
git merge first-branch     # Merge the changes from first-branch into master.
```

### Push the Merged Changes to GitHub

```bash
git push -u origin master   # Push the merged master branch back to GitHub.
```

## 6. Handling Remote URLs

If you need to change the remote URL for the repository, you can use:

```bash
git remote set-url origin http://github.com/Irfansyed0611/Practice-repo.git  # Update the remote URL.
```

You can verify the remote URL by:

```bash
git remote -v    # Check the current remote URL associated with the repo.
```

## 7. Final Git Workflow Recap

Here’s the typical flow that you followed:

1. **Set Up Git**: Installed and configured Git with your username and email.
2. **Create a Repo**: Created a new repository and made the first commit with a README file.
3. **Push to GitHub**: Connected to GitHub and pushed your local repository to GitHub.
4. **Branching**: Created a new branch, made changes, committed, and pushed it to GitHub.
5. **Merging**: Merged changes from the feature branch (`first-branch`) into the `master` branch and pushed those changes back to GitHub.

## Conclusion

You’ve successfully learned the basic Git workflow! Here's what you can now do:

- **Create repositories**: Both locally and on GitHub.
- **Work with branches**: Isolate features and work on them independently.
- **Merge branches**: Bring changes from different branches together.
- **Push and pull**: Sync changes between your local machine and GitHub.
