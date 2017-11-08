---
title: Getting into Git
image: https://www.thermo.io/wp-content/themes/thermo/static/images/perks-6.svg
description: Installation, basic commands and concepts.
---

# Getting into Git
Git is an essential skill for developers and IT professionals who find themselves consistently working on team projects. Whether it is writing code or collaborating on documentation for a server build, Git is the most popular distributed version control system. Git allows multiple people from all over the world to contribute to open source projects, allowing distant users to come together.

Setting up Git is fairly easy, so let’s explore the basics: how to set it up locally, and how to contribute to remote repositories.
## Installing Git
Installing on GNU/Linux is as easy as telling your package manager to install it:
* For Ubuntu:
```
sudo apt-get install git
```
* For CentOS and Fedora:
```
sudo yum install git
```
Then, use the `git config` built-in tool to set some configuration variables from the following files:
* `/etc/gitconfig` -- Stores configuration info for all system users and their repos
* `/.gitconfig`    -- Stores user-specific configuraiton files on the system
* `.gif/config`    -- Configuration file of the currently working repo

The `gif config` tool will ask for your name and email, which will be attached to any commits made from your local machine. If you already have a GitHub account, you can use that email and username.

Next, check your work by using ``git config --list``, which will display the global variables for username and email. If you need to change any information, running the following commands, replacing the angled brackets (`<>`) and everything between them with the indicated information:
```
git config --global user.name <examplename>
git config --global user.email <user@example.com>
```
You can also change your default editor with:
```
gif config --global core.editor editor-name
```
## Adding Git to current projects
It’s also possible to start using Git to track projects already in development.

To do so, change to the project’s directory and run git init , which will create a new .git subdirectory. This will be where Git stores your local configurations, and specific to your username.

Now, tell Git which files are going to be a part of the project and afterwards make a commit with a message to describe it. Always make a commit message so others know what it contains; for example:
```
git add filename
git commit -m “First commit”
```
Next, let’s look at some helpful and common Git commands.
## Basic commands
```
git add
git rm
git mv
```
The `rm` and `mv` commands that remove and move files from repos are likely already familiar to you, and the `add` command was discussed above. But what if we want to add a repo from a remote site like Github?
## Branches
Say you are working on a project with large team and each commit to the project requires rigorous testing and discussion. This is where branches shine.

Branches are what makes Git such a powerful tool for collaboration. If “too many cooks spoil the broth,” then Git is what allows every cook, or developer, to have their own kitchen, or branch.

Most Repos have a master branch, usually known simply as “master.” Branches are often named after either a feature yet to be added and tested, or an issue requiring a fix. Since Git tracks all changes, working on multiple different branches doesn’t interfere with other developers. Reverting back to a previous version of a branch is possible, and easy.
### Creating branches
To create a branch, get a local copy of the remote repository you will be working on. Replace the angled brackets (`<>`) and everything between them with the actual URL of the repository to be cloned:
```
git clone <remoteurl>
```
This creates a copy of the remote repository in your current directory. From here, you can begin to manage the files and make your needed changes to the branch.
### Basic commands
Replace the angled brackets and everything between them with the indicated information.
* `git status`: Show any changes to tracked files
* `git branch -r`: List all remote branches
* `git branch -a`: List all local and remote branches
* `git branch -m`: Rename a branch
* `git branch -d`: Delete a local branch
* `git branch -r -d`: Delete a remote branch
* `git add <remote_name> <url>`: add a remote repo
* `git pull`: Obtain and merge a remote branch into your current local branch
* `git push <remote_name> <branch_name>`: Move local data in the current branch to the remote repo
* `git remote show <remote_repo>`: Display information about the remote repo
* `git remote rename <old_name> <new_name>`: Rename a remo
* `git remote rm <name>`: Remove the specified remote repo
