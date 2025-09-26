---
tags:
  - devops
  - git
  - github
---

# Git Practice Notes

## Git Setup and Configuration
1. `git --version` - Check the current Git version.
2. `git config --global --list` - List all the global Git configuration settings.
3. `git config --global user.name "Syed Irfan"` - Set the global user name for Git.
4. `git config --global user.email "syedirfan0611@gmail.com"` - Set the global email for Git.
5. `git config --global credential.helper manager`  - Set the credential helper to store credentials securely.`
6. `git config --global --list` - Confirm the settings after setting the user name and email.

## Working with Files and Repositories
1. `ls` - List the contents of the current directory.
2. `mkdir my-test-repo` - Create a new directory named `my-test-repo`.
3. `cd my-test-repo/` - Navigate into the newly created `my-test-repo` directory.
4. `git init` - Initialize a new Git repository in the directory.
5. `echo "# My test repo" > README.md` - Create a `README.md` file with the text "# Just a test Repo".
6. `git add .` - Stage all changes in the repository.
7. `git commit -m "Initial commit"` - Commit the staged changes with the message "Initial commit".
8. Confirm the changes in GitHub.

## Remote Repositories and Pushing Changes
1. `git remote add origin https://github.com/Irfansyed0611/Practice-Repo.git` - Add a remote repository called "origin".
2. `git push -u origin master` - Push the `master` branch to the remote repository and set up the tracking information.

## Managing Branches
1. `git branch` - List all branches in the local repository.
2. `git checkout -b first-branch` - Create and switch to a new branch called `first-branch`.
3. `git checkout master` - Switch back to the `master` branch.
4. `git push -u origin first-branch` - Push the `first-branch` to the remote repository and set up the tracking information.
5. `git merge first-branch` - Merge `first-branch` into the current branch (`master` in this case).
6. `git checkout master` - Switch to the `master` branch.
7. `git merge first-branch` - Merge the `first-branch` into the `master` branch.

## Other Git Commands
1. `git remote -v` - List the remote repositories associated with your local Git repository.
2. `git remote set-url origin http://github.com/Irfansyed0611/Practice-repo.git` - Change the remote repository URL.
3. `git commit -m "updat README file"` - Commit changes to the `README.md` file.
4. `git push -u origin master` - Push changes to the `master` branch.
5. `git add .` - Stage all changes.
6. `git commit -m "just a html file"` - Commit changes related to the HTML file.
7. `git push -u origin first-branch` - Push `first-branch` again.

## Final Merge Command Flow
1. `git checkout master` - Switch to `master` branch.
2. `git merge first-branch` - Merge changes from `first-branch` into `master`.
