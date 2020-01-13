# An overview of Git and how to use it

This document serves as a reference for those who would like to understand how git works, and how they can use it. The content is based on what is available in the [Pro Git book][1], as well as some other sources, although it doesn't cover the topics in nearly as much depth as the book. Please refer to the book if something is unclear.

Comparisons with Microsoft Visual SourceSafe are made throughout this document, as that is what is currently used in Columbus.

The document provides some background on how Git works, which is then followed by a series of step-by-step examples of how to use Git as a version control system. This should serve as practical guidance on how to use Git and what the results of common operations are. Concepts are introduced as needed, and comparisons are made for clarification when necessary. The document does not attempt to champion a specific workflow, although it does use the [GitFlow][2] workflow to illustrate concepts around branching and merging.

## What is Git and what does it do differently

Most of us are used to some form of centralised version control system [CVCS], for example Subversion, Perforce, or SourceSafe.

![central-vcs][i1]

In a CVCS, all of the versioned files are stored on a server. This server runs a program that does things like user management, network communications, file transfer, and most importantly: version management of the files that it tracks.

Having a server like this allows users to retrieve the tracked files (usually via some client program, or integration with an IDE). The users can then edit these files, although the same file cannot be edited simultaneously with another user. When a changed file is submitted to the server, it creates a new version of that file and this is what is transferred the next time this file is requested.

It is important to note that, except for temporary local copies, the actual files **and their entire history** are only stored on the server. A failure on the server thus risks a loss of all of this information, with recovery being difficult to impossible.

### So how is Git different from this

Git is a Distributed Version Control System (DCVS). This means that checking out the repository not only retrieves the latest snapshot of the files, but the **entire history** of the repository. The local repository is in fact an **exact mirror** of the remote repository.

![distributed-vcs][i2]

There is absolutely no difference in the version control program running on the "server" vs. the version control program running locally. This allows you to use Git as a version control system even though no server (remote) has been defined.

In fact, it is common to create a local repository first, then commit many changes that are tracked by Git, and only much later on add a remote and mirror (clone) the repository to the remote.

As a result of how Git works, it is usually much faster and more reliable than a CVCS.

### How does Git actually work

#### Data model

In most other version control system (central or distributed), information is stored as a list of file-based changes. This means that the version control systems think of their stored information as a set of files and *changes made to these files* over time (in other words an initial file plus changes). Such a model is called **delta-based** version control.

This means that in order to get version-x of a file, we download the initial file and **apply all of the deltas** until we get to the requested version.

![delta-vc][i3]

Git does not work this way!

Instead of thinking of information as a set of deltas from an initial file, Git thinks of information as a series of file-system snapshots. Every commit you create generates a new snapshot and stores the snapshot using a reference (also called a commit id, this reference is a 40-character SHA-1 hash).

The snapshots store a complete image of what the tracked file system looked like at the time of the commit, but importantly, unchanged files are just stored as a soft link to their equivalent in the previous snapshot.

![snapshot-vc][i4]

This provides some significant benefits, as each commit is simply a pointer to a snapshot. As an example, branching is made easier and much cheaper in this model, as a branch is simply a named pointer to a specific commit (and not a deep copy with a common ancestor). All of the commits on the branch after the initial operation occur on a parallel series of snapshots.

#### Change model

As a consequence of the snapshot model, Git can store individual changes *inside* a file in a commit. This means that conceptually unrelated changes can be made in the same file, at the same time, but do not need to form part of the same commit. This is simply not possible in delta-based version control, as the entire file and all of its contents form part of the check-in (i.e. there are no check-ins of partial content).

In order to achieve this, Git has a separate logical area before a change is committed to the repository. The three stages are thus the *Working Directory*, the *Staging Area*, and the *Repository*.

![git-areas][i5]

Git tracks all changes (current file-system status is different to latest snapshot) in the *Working Directory*, which shows a file as **modified**. These changes can then be **staged** (into the *Staging Area*) either as a wholesale file, or as individual changes. The changes in the *Staging Area* can then be **committed** to the repository.

Changes in the *Working Directory* that were not in the *Staging Area* at the time of the commit are not added to the repository, but remain waiting to be staged and committed at a later time.

## Git basics

This section covers the basic of how to use Git. It provides examples of how to perform common operations, both from the command line, as well as from the [Sourctree][3] GUI. Most of the actions discussed in this section only affect the local repository.

The following actions are covered:

1. How to create a new repository
2. How clone an existing repository
3. How to record changes to the repository
4. How to view the commit history
5. How to undo
6. How to tag commits

### Creating a new repository

#### Creating a new repository on the command line

It is very simple to create a new repository on the command line. Simply change to the directory you want the repository to be in and type the command `git init`. This adds the `.git` directory that contains all of the necessary repository files. At this point, the repository is *empty*.

![git init from command line][i6]

#### Creating a new repository in Sourcetree

In Sourcetree, you can create a new repository by either clicking on `File->Clone/New`, or by choosing `Local` from the repository tab and then clicking on `Create`. This will bring up a "Create a repository" dialog like the following:

![git init from sourctree][i7]

Choose your directory and a name for the repository (the default is the same as the directory name), and click on `Create`. This will create a Git repository in the directory and will automatically open the repository in Sourcetree.

### Cloning a repository

In order to make a copy of a repository that is linked to the source repository, we need to make a "clone". As mentioned previously, this makes an exact copy (all files and complete file history) of the remote repository in the local destination.

The remote repository can be any repository for which you have a valid, reachable url. This means that the repository could be on a server that exposes the repo via `http` or `https`, or it could be a directory on a shared network drive (perhaps accessible using SSH), or it could simply be another directory on the same machine. As long as the url is valid, Git will be able to make a copy. Although there are many options, servers like Bitbucket or GitHub usually allow interfacing via `http`, `https`, or `ssh` only.

When cloning a repo, it is worth noting that the cloning process will create a directory for the repo. There is thus no need to create the directory manually.

#### Cloning a repository from the command line

Simply change directory to the directory in which you want the repo directory to be located. Then execute the command `git clone <url>`.

![git clone from cmd line][i8]

#### Cloning a repository from Sourcetree

Cloning from Sourctree is also simple. Either click on `File->Clone/New`, or click on `Clone` from the repository tab. This will open up the "Clone" dialog, where you need to specify the remote url and the local destination directory. The name is inferred from the remote repo name.

![git clone from sourctree][i9]

### Making and storing changes

When you first add a file to a directory that contains a Git repo, Git does not automatically keep a history of the file. A file's status can be either `tracked` (meaning that Git keeps a history of the file) or `untracked` (meaning that Git does not keep a history of the file). Files that are `tracked` can have status `unmodified`, `modified`, or `staged`. The typical lifecycle of a file in a Git repo is *Untracked*->*Staged*->*Unmodified*->*Modified*->*Staged* and so on.

![git-lifecycle][i10]

There are several commands that you use to interact with files and the repository. These are

* `git status` (shows the current status of the repository)
* `git add <pattern> (stage all files matching the pattern to be tracked in the repo)
* `git diff` (shows the difference between the working directory and the repo)
* `git commit` (commits the staged changes to the repo)
* `git rm` (remove the file from the repo, file becomes untracked)
* `git mv` (move the file to a different location, but keeps the history)




[1]: <https://git-scm.com/book/en/v2>
[2]: <https://nvie.com/posts/a-successful-git-branching-model/>
[3]: <https://www.sourcetreeapp.com/>

[i1]: <https://git-scm.com/book/en/v2/images/centralized.png>
[i2]: <https://git-scm.com/book/en/v2/images/distributed.png>
[i3]: <https://git-scm.com/book/en/v2/images/deltas.png>
[i4]: <https://git-scm.com/book/en/v2/images/snapshots.png>
[i5]: <https://git-scm.com/book/en/v2/images/areas.png>
[i6]: ./images/cmd-init.png
[i7]: ./images/srctree-init.png
[i8]: ./images/cmd-clone.png
[i9]: ./images/srctree-clone.png
[i10]: <https://git-scm.com/book/en/v2/images/lifecycle.png>
