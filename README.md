# GitHub-Reference
An entry level reference for setting up and learning how to use GitHub for software development.

# Table of Contents

* [1. Introduction](#1-introduction)
    * [So What is GitHub?](#11-so-what-is-github)
    * [What is Git?](#02-what-is-git)
    * [Why Not Just Use GitHub Desktop?](#13-why-not-just-use-github-desktop)
* [2. Getting Started With GitHub](#2-getting-started-with-github)
    * [Downloading Git](#21-downloading-git)
    * [SSH Keys](#22-ssh-keys)
    * [Signing Keys and Signing Commits](#23-signing-keys-and-signing-commits)
    * [Adding Git to PATH](#24-adding-git-to-path)
    * [Running Git Commands in the Terminal](#25-running-git-commands-in-the-terminal)
    * [Visual Studio Code Setup](#26-visual-studio-code-setup)
* [Git Basics](#3-git-basics)
    * [Repositories](#31-repositories)
    * [Tracking Changes](#32-tracking-changes)
    * [Commits](#33-commits)
    * [Pushing Changes](#34-pushing-changes)
    * [Pulling Changes](#35-pulling-changes)
* [4. Branches](#4-branches)
    * [Switching Branches](#41-switching-branches)
    * [Creating a New Branch](#42-creating-a-new-branch)
    * [Committing a New Branch to Remote](#43-committing-a-new-branch-to-remote)
    * [Deleting a Branch](#44-deleting-a-branch)
    * [Merging Branches](#45-merging-branches)
    * [Merge Conflicts](#46-merge-conflicts-yay)


# 1. Introduction
Hello and welcome to the GitHub Reference! I made this document as one easy place to get started working with GitHub for those without experience (I promise it will be *relatively* fast). I'll also provide resources at the end if you want to learn more. 

Just a disclaimer, I'm not an expert (I'm not even a CS major), so if something is confusing or flat out wrong (hopefully not), please feel free to reach out to me and let me know, and I'll do my best to fix it. 

## 1.1 So What is GitHub?

GitHub (and other sites like it) is first and foremost an online service for storing and sharing code. It facilitates collaboration on software development using version control, which is allows us to manage and track changes in software over time. 

## 1.2 What is Git?

Git is the open source  "version control" services like GitHub, GitLab, and BitBucket use. It's a collection of commands and processes used to track and log changes across multiple files, which it does via a bunch of information stored in a `.git` folder on your computer. Git stores the entire project history locally, so you don't need an internet connection to work on the project or look at code history.

Git works on any file type, but it's primarily used for source code. It works best with plain text files (like code, `.md`, and `.txt` files) because it can track changes line-by-line. It doesn't work as well with binary files (like images, videos, and compiled code) because it can't track changes within those files. We'll cover `.gitignore`'s in a [later section](#321-gitignores).

## 1.3 Why Not Just Use GitHub Desktop?

Well, downloading GitHub Desktop and simply logging in there would definitely work for most uses, **BUT** hear me out first.

Doing Git commands from the command line interface (CLI) is often more flexible and precise than the desktop app. Certain actions will be very difficult or even impossible from the GUI, which ends with you opening the terminal anyway. Using the CLI helps you understand the inner workings of Git much better. The commands are also identical across operating system or service, so you can always be productive without needing extra software. However, there is one gigantic caveat to this. 

\**sigh*\* This is where I disagree with some (not all) CS people. There's absolutely nothing wrong with using whatever interface you prefer. **If the desktop app GUI speeds up your development, use it!** 

My personal preference is the CLI, so that's what this guide covers. In the end, we'll be using VSCode, so most of the git commands can be done by pushing buttons there anyway. Most people end up using a combination of CLI and GUI. Plus, using the command line makes you feel like a wizard.

> Side note: I have used GitHub Desktop, but found it kinda clunky so didn't put a lot of time into learning it. If you have issues with it, you're on your own! Sorry lol.

## 1.4 Setup for the Setup


# 2. Getting Started With GitHub

To start using GitHub, there are a couple things we need to setup to actually get things running.

## 2.1 Downloading Git

Go to https://git-scm.com/downloads and download the latest version of Git for your operating system. Then:  
1. Run the installer
2. Follow the installation options. A few notes:
    * On the **Choosing the default editor used by Git** screen, you may want to select something (probably VSCode) other than Vim since it's a little hard to use, but we'll cover Vim basics later. 
        * This can also be changed later using the `code.editor` variable in git. 
    * On the **Adjusting the name of the initial branch in new repoisitories** screen, I chose to Override the default and used the more current `main` for default primary branch name. 
    * On the **Adjusting your PATH environment** make sure to select the option for "Git from the command line and also from 3rd-party software".
        * This saves us some steps later. 
    * Keep the default behavior of 'git pull' set to "Fast-forward or merge".
3. Hit finish.

## 2.2 Authentication

### 2.2.1 Method 2: SSH Keys
Secure Shell (SSH) keys are a way to identify trusted computers without involving passwords. They are a pair (1 public and 1 private) of crytographic keys that can be used to authenticate a secure connection between your computer and GitHub.

To set up your SSH keys: 
1. Open a terminal (in your OS or in VSCode) and run `ssh-keygen -t ed25519`
    * You can change the file location or file name if you'd like. 
    * You don't need a password, but it increases security. If you add one it keeps pushing and pulling locally secure, so no one can use the key without the password if they somehow gain access to your computer.
        * Adding one will make us go through a few additional steps later. ----- 
    * ed25519 is the -----
2. Either open the `.pub` file you just created or run `cat /path/to/.ssh/id_ed25519.pub` (or whatever you named your ssh key) so you can copy your public key.
3. Go to [https://github.com/settings/keys](https://github.com/settings/keys) 
4. Click the green `New SSH key` button. Title it whatever you'd like, and make sure "Authentication Key" is selected from the dropdown. Paste the SSH public key you copied and click `Add SSH Key`.
5. Repeat step 4. with the same key but select "Signing Key" from the dropdown instead.
6. Run `ssh git@github.com` and enter yes when prompted to verify the connection with github workds.

## 2.3 Signing Keys and Signing Commits
We'll cover what commits are later in Section [3.2](#22-commits), but for now, know that signing commits allows GitHub to verify that it was actually you who did the action. The image below shows what a verified commit looks like on GitHub, which you can see by going to the Commit History right under the green `<> Code ▾` button on the main repository page. 

<div style="text-align: center;">

![pic:verified-commit](pictures/verified_commit.png)
</div>

To just get things up and running, we'll use the SSH key we just generated for commit signing. You can also use a GPG key instead, but it requires an additional download and some different steps. More information on GPG keys can be found [here](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits).

1. The first step is what we did in step 5 of section 2.2.1. 
2. Run `git config --global gpg.format ssh`
3. Run `git config --global user.signingkey /path/to/.ssh/key.pub`

## 2.4 Adding Git to PATH

## 2.5 Running Git Commands in the Terminal

```powershell
git config --global user.email "you@example.com"
```

````powershell
git config --global user.name "Your Name" 
````

## 2.6 Visual Studio Code Setup

[don't forget to talk about path to git bin to vscode preferences]: #


# 3. Git Basics

## 3.1 Repositories

### 3.1.1 Local vs. Remote Repository

## 3.2 Tracking Changes

### 3.2.1 .gitignore's
This is something that will generally already be set up for you in a repository, but it's helpful to know what it is. A `.gitignore` file is a simple text file that tells Git which files or directories to ignore changes for in a project. This is a useful way to avoid committing files that aren't necessary to the project, such as build files, temporary files, or sensitive information like passwordsd or API keys. 

The included `.gitignore` file in this repository is a good starting point for C++ projects, and it covers syntax as well. You can find more information about `.gitignore` files and how to create them [here](https://git-scm.com/docs/gitignore).

## 3.3 Commits

## 3.4 Pushing Changes

## 3.5 Pulling Changes

# 4. Branches

Branches are one of the most useful features of Git. They are what allows multiple people to work on the same codebase without interfering with each other's work. Each branch has its own history of changes, which can all be merged when development is complete. 

Everthing starts on the `main` branch (also called the `master` or `default` branch in some repositories). The `main` branch should always contain the most up-to-date and stable (i.e. working) version of the code.

<div style="text-align: center;">

![pic:branches](pictures/branches.png)
</div>

However, since each branch has its own history, you can run into issues when someone else makes changes to either the main branch or code you were also working on. 

## 4.1 Switching Branches
To see all current branches, run `git branch`. It will show you all the branches in the repository (that you have pulled from remote). It will show your current branch with an asterisk (*) next to it. 

You can switch between branches with `git checkout <branch-name>`. If you have any changes to the current branch that you don't want to commit yet, you can run `git stash` to save the changes, just remember to run `git stash pop` to get the changes back before you start editing the current branch again. 

## 4.2 Creating a New Branch 
To create a new branch without switching to it, run `git checkout <name-of-your-branch>`. To create and switch to your new branch, run `git checkout -b <name-of-your-branch>`. 

If you accidentally created a branch, refer to section

## 4.3 Committing a New Branch to Remote

git push -u <new-branch-name>
* -u is short for --set-upstream

## 4.4 Deleting a Branch
You cannot delete the branch you are currently on. 

`git branch -d <branch-name>`


`git branch -D <branch-name>`


## 4.5 Merging Branches

### 4.5.1 Types of Merges

## 4.6 Merge Conflicts (Yay!)

# 5. Git Workflow

## 5.1 Pull Requests

## 5.2 Automation
