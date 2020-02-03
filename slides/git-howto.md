---
title: 'How to use Git: a practical guide'
author: Otto Taute
date: 2019-01-14
theme: solarized
transition: fade
slideNumber: false
previewLinks: true
fig-width: 5
fig-caption: false
embedded: true
width: 1920
height: 1080
center: false
dummy: 'pandoc -s -t revealjs --slide-level=2 <sourcefile> -o <destfile>'
---

# What is Git?

## Git overview

Git is a *Distributed Version Control System*. This differs significantly from the more traditional *Centralised Version Control Systems* (such as SourceSafe).

Git also models data differently to most version control systems. Where most version control system model data as a set of changes to files, Git models data as a series of file-system snapshots.

---

## Centralised VCS {data-transition="slide"}

* Traditional VCS are centralised
  * All files are kept on the server
  * History is only kept on the server
* Interaction with the server and managed files is through a client or IDE integration
* The same file cannot be edited by different people at the same time
* Server failure risks the loss of the files and the *file history*

---

![centralised version control](../images/centralized.png)

## Distributed VCS {data-background=#ff0000 data-background-transition="slide"}

* Each repo contains all of the files
* Each repo contains the **entire history** of the files
* Local repo is an **exact mirror** of the remote
* There is no difference between the version control program running on the server vs. locally
* Can be used without a remote being defined
* Much faster and more reliable than a CVCS

---

![distributed version control](../images/distributed.png)

## Git's data model

* Almost all other version control systems use a *delta-based* model
  * A set of files
  * Changes made to these files over time (deltas)
  * Latest version is the initial version **with all of the deltas applied**
* Git models information as a series of file-system **snapshots**
* Every commit generates a new snapshot
  * Stored using a 40-character SHA1 hash as an id
* This forms a complete image of how the file-system looked at the time of the commit
* Unchanged files are stored as a soft link to their equivalent in the previous snapshot

---

![delta-based version control](../images/deltas.png)

![snapshot based version control](../images/snapshots.png)

## Git's change model

* Changes *inside* a file can form a separate commit
* Conceptually unrelated changes can be made at the same time
  * Will not necessarily form part of the same commit
* Delta-base version control checks in the entire file (with all changes)
  * Cannot check in partial changes
* In order to be able to commit different file changes as different commits, Git introduces a separate area called the *Staging Area*

---

## Git areas

![git areas](../images/areas.png)

* Changes in the *Working Directory* are tracked (the file shows as **modified**)
* Individual changes can be **staged** into the *Staging Area*
* Staged changes can be **committed** to the repo
* Changes that are in the *Working Directory*, but not the *Staging Area* are not committed in the current commit

# Git basics

## Basics overview

This section provides examples of the most basic operations in Git. Examples are provided from the command line, as well as in the [Sourcetree](<https://www.sourcetreeapp.com/>) GUI.

The following examples are provided:

1. How to create a new repo
2. How to clone an existing repo
3. How to make and store changes

---

## Creating a new repo

* The command for creating a new (empty) repo is `git init`.
* This command is executed from the directory in which you want a repo to be created.
* A `.git` directory is created, which contains all of the necessary repo files.

```bash
~/git-demo$ git init
Initialized empty Git repository in ~/git-demo/.git/

~/git-demo$
```

---

The same can be achieved in Sourctree

* Click on "`File->Clone/New`" or choose "`Local`" from the repo tab and click "`Create`"
* Fill in the directory and repo name (defaults to directory name)
* Repo is created and opened in Sourctree

![creating a new repo in Sourctree](../images/compressed/srctree-init.png)

## Cloning a repo

* A clone is a copy with a link to the source repository
* It is an exact copy
  * All files
  * All file history
* Can be any repo for which you have a valid, reachable url
  * Usually exposed by a server (such as Bitbucket or GitHub) as an `http` or `https` endpoint
  * Sometimes exposed through `ssh`
  * Can also use the `git` protocol
* Creates a directory for the repo automatically

```bash
~$ git clone http://user@server/git-demo.git
Cloning into 'git-demo'...

~$
```

---

In Sourcetree, click on "`File->Clone/New`" or click on "`Clone`" from the repo tab. Then specify the remote url and local destination directory in the dialog.

![cloning a repo in Sourcetree](../images/srctree-clone.png)

## Making and storing changes

* Git does not automatically track new files added to the repo directory
* The file has an initial status of "`untracked`"
* Need to instruct Git to track the file (status changes to "`tracked`")
* Tracked files can have a status of
  * "`unmodified`" (no changes between working directory and repo)
  * "`modified`" (working directory is different to repo)
  * "`staged`" (file is in staging area, ready to be committed)
* Typical lifecycle of a file is *untracked*->*staged*->*unmodified*->*modified*->*staged*->*...*

---

![git file lifecycle](../images/lifecycle.png)

---

### Commands to interact with files and the repo

* `git status` (shows the current status of files and the repo)
* `git add <pattern>` (stage all files matching the pattern)
* `git diff` (shows the difference between the working directory and the repo)
  * `git diff --staged` (shows the difference between the staged file(s) and the repo)
* `git commit` (commits the staged changes to the repo)
* `git rm` (untrack the file and delete from working directory)
* `git mv` (move the file to a different directory, history is kept)

---

### Repo status

![git status on empty repo](../images/cmd-emptystatus.png)

![git status on untracked file](../images/cmd-untrackedstatus.png)

---

### Tracking a file

* Start tracking a file using "`git add <pattern>`" e.g.
  * "`git add .`" to add all files in the current directory.
  * "`git add hello.txt`" to add only the specific file
* File is moved to staging area, ready to be committed

![adding a file](../images/cmd-addfilestatus.png)

---

![adding a file in Sourctree](../images/srctree-addnewfile.gif)

---

### Committing a change

* Use the command "`git commit`"
* Every commit **must** have a commit message
  * A short message describing the change
  * Written in the imperative tense (i.e. "Add", not "Added" or "Adding")
  * Aims to complete the sentence: *"When applied, this commit will..."*
  * Read about good commit messages in [A note about git commit messages](<http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html>)
* Git opens the default text editor to allow you to edit the commit message
* Can commit with a short message by using the `-m` option
  * "`git commit -m "Add file hello.txt"`"

---

![committing a change](../images/cmd-commit.png)

---

![making and committing a change in Sourctree](../images/srctree-changeandcommit.gif)

---

### Seeing what will change

* Once a file is tracked, Git picks up any modifications made to the file
* See which files have been modified by using "`git status`"
* See the difference between the working directory and the repo by using "`git diff`"
  * Shows what has been added
  * Shows what has been removed
  * Shows what has been modified
* Once changes have been staged, use "`git diff --staged`" to see the difference between the staging area and the repo

![seeing what has been changed](../images/cmd-modifyanddiff.png)

---

### Ignoring files

* Git can be instructed to ignore a file
  * Will not show up as untracked
* First create a `.gitignore` file in the project directory (not in the hidden `.git` directory)
* File name patterns in this file are ignored by Git
  * Standard glob patterns are applied recursively throughout the working tree
  * Comment lines start with `#`
  * Patterns starting with `/` are not applied recursively
  * Patterns ending with `/` specify entire directories
  * Patterns starting with `!` invert the pattern
  * Common to ignore compiled files (e.g. `*.dll, *.exe`)

---

Here is an example:

```text
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

---

### Removing files

* Sometimes it is necessary to delete a file and instruct Git to no longer track the file
* Use the "`git rm <pattern>`" command to achieve this
* The file is not deleted from the history
* Restoring a previous snapshot will also restore the file as it was at the time of that commit

---

### Moving files

* Sometimes it is necessary to move a file to a different directory in the project
* Moving a file in the OS is equivalent (from Git's perspective) to deleting the file and creating and identically named file elsewhere
* Rather use the "`git mv <source> <destination>`" command
  * Git interprets this as a rename
  * The file history is preserved

This is equivalent to the following:

```bash
# first move the file without Git's help
~/git-demo$ mv hello.txt hello.md

# then remove the old file and track the new file
~/git-demo$ git rm hello.txt
~/git-demo$ git add hello.md
```

# Viewing the commit history

## Using `git log`

* The basic command for viewing the commit history is "`git log`"
  * Shows a summary of changes to the repo
  * Commit ID, author, date, and message are shown for every commit
  * Commits are shown in reverse chronological order (i.e. most recent first)
* There are many options that change the output of `git log`
* The commit history in graph format is automatically shown in Sourctree when a branch is selected
  * This history is interactive
  * The message, date, author, and commit ID are shown

```bash
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

## Patches only

* `git log -p` shows the difference introduced with each commit
* `git log -p -2` only shows the diff of the last two commits

```bash
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
```

## Commit statistics

* `git log --stat` shows the statistics for each commit
  * lists modified files in each commit
  * shows how many files were changed
  * shows how many lines in each file were added/removed
  * shows a summary at the end

```bash
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit

 README           |  6 ++++++
 Rakefile         | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)
```

## Formatting the log output

* `git log --pretty=oneline` shows each commit ID and message on a line
* Can also use the options `short`, `full`, and `fuller` instead of `oneline`

```bash
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```

* The format for `--pretty` can be manually specified
* This is useful if you want the output to be parsed

```bash
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
```

---

Useful options for `git log --pretty=format`:

| Option | Description               |
| :----: | :------------------------ |
|  `%H`  | Commit hash               |
|  `%h`  | Abbreviated commit hash   |
|  `%T`  | Tree hash                 |
|  `%P`  | Parent hashes             |
|  `%p`  | Abbreviated parent hashes |
| `%an`  | Author name               |
| `%ae`  | Author email              |
| `%ad`  | Author date               |
| `%ar`  | Author date relative      |
| `%cn`  | Committer name            |
| `%ce`  | Committer email           |
| `%cd`  | Committer date            |
| `%cr`  | Committer date relative   |
|  `%s`  | Subject                   |

---

* Can also use the `--graph` option, which shows an ascii graph of the branch and merge history

```bash
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
```

---

Useful options for `git log`:

|      Option       | Description                                                                  |
| :---------------: | :--------------------------------------------------------------------------- |
|       `-p`        | Show the patch introduced with each commit                                   |
|     `--stat`      | Show statistics for files modified in each commit                            |
|   `--shortstat`   | Display only the changed/insertions/deletions line from the `--stat` command |
|   `--name-only`   | Show the list of files modified after the commit information                 |
| `--abbrev-commit` | Show only the first few characters of the the SHA-1 checksum                 |
| `--relative-date` | Display the date in relative format                                          |
|     `--graph`     | Display an ASCII graph of the branch and merge history                       |
|    `--pretty`     | Show commits in an alternate format                                          |
|    `--oneline`    | Shorthand for `--pretty=oneline --abbrev-commit`                             |

## Limiting log output

* Log output is shown per page by default
* There are a number of other useful limiting options that show a subset of commits

|        Option         | Description                                                                                                                        |
| :-------------------: | :--------------------------------------------------------------------------------------------------------------------------------- |
|        `-<n>`         | shows the last `n` commits                                                                                                         |
| `--since`, `--after`  | shows only commits made after the specified time, which can be absolute or relative (e.g. `--since=2019-09-22`, `--since=2.weeks`) |
| `--until`, `--before` | shows only commits made before the specified time, which can be absolute or relative                                               |
|      `--author`       | shows only commits made by the author (can specify more than one author by using `--author` multiple times)                        |
|     `--committer`     | shows only commits where the committer entry matches the specified string                                                          |
|       `--grep`        | shows only commits with the specified string in the commit message (can specify more than one string to match)                     |
|          `S`          | shows only commits adding or removing code that matches the specified string                                                       |

---

A log example:

```bash
$ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
```

# Undoing work

## Undoing overview

* There are a few basic tools in Git for undoing things
* Be careful, because you can't usually undo the undos
* You can lose work by performing certain undos

## Amending commits

* The latest commit can be amended (changed) by using `git commit --amend`
* Typically used if some changes should have been staged, but weren't, or if the commit message was wrong
* `git commit --amend` adds currently stages changes to the previous commit (i.e. redoing the previous commit)
* If no changes are staged, then only the commit message is changed
* This does not add an additional commit, but **replaces** the last commit
* **Only the latest commit can be changed in this way**

```bash
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

## Unstaging a file

* Files can be unstaged (i.e. *Staging Area*->*Working Directory*)
* Usually when changes have been accidentally committed (e.g. `git add *`, instead of `git add <filename>`)
* The important thing to remember is that the repo `HEAD` is still on the latest commit
* Resetting the file to `HEAD` will undo the stage for that file

```bash
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

## Unmodifying a file

* Changes to a file need to sometimes be discarded
* The way to do this is again to reset the file to what it looked like before the modifications
* Use the `checkout` command in this case
* This reverts the **entire** file to its content in the latest commit. **All** modifications to the file are lost and *not retrievable*!
* Committed changes can almost always be recovered. Uncommitted changes can **never** be recovered.

```bash
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

# Working with remote repositories

## Remote repos overview

* In order to collaborate with others, a shared repository is often used
* The shared repo is hosted on some commonly accessible server
* Usually through a Git server management program (like Bitbucket, Gitlab, or Github)
* Access and rights (read/write) can be specified per contributor, repo, and branch
* Data is pushed and pulled from the remote repo
* Remote repo needs to be added to local repo (i.e. local repo needs to be made aware that the remote exists)
* Local repo needs to be set up to track the remote repo (i.e. define **upstream** branches)
* Multiple remotes can be added to a local repo

## Showing remotes

* `git remote` shows all of the configured remotes for the current repo
* `git remote -v` shows the remotes and associated URLs
* default remote is called `origin`

```bash
$ git remote
origin
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
```

## Configuring a remote repo

* Cloning from a remote repo automatically sets up the `origin` remote on the local repo
* Can add a new remote to a repo using `git remote add <shortname> <url>`
* The `<shortname>` aliases the `<url>` and makes it easier to refer to a specific remote
* More info about the remote can be obtained using `git remote show <remote>`
  * Shows URL and branch tracking information
  * Shows which local branches track which remote branches
* A remote can be renamed using `git remote rename`
  * Automatically updates tracking branches with the new name
* A remote can be removed using `git remote rm <remote>`
  * All remote tracking branches are also deleted

```bash
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
pb  https://github.com/paulboone/ticgit (fetch)
pb  https://github.com/paulboone/ticgit (push)
```

## Getting data from a remote

* There are three commands that interact with data from a remote
* The `fetch` command gets data *from* the remote, but **does not merge** it with your local branches
* The `pull` command is only valid if a local branch is set up to track a remote branch
  * Gets data *from* the remote and attempts to **automatically merge** it with the local tracking branch
* The `push` command sends data *to* the remote (e.g. `git push <remote> <branch>`)
  * Only works if you have **write** access on the remote (in general), and **write** access on the specific branch
  * Cannot push if the local repo is out of date (i.e. changes made by someone else since the last fetch/pull)

# Tagging commits

## Tagging overview

* Tags are friendly names given to specific commits
* Usually used to indicate specific points in the history as being important
* Most common use case is to mark release points (e.g. `v1.1`)
* Tags can be listed using `git tag -l`
  * Can filter for specific tags using `git tag -l <pattern>` (e.g. `git tag -l "v1.8.5*"` lists all tags starting with `v1.8.5`)
* Tags can be *lightweight* or *annotated*
  * *lightweight* tags are just pointers to a specific commit
  * *annotated* tags are full objects in the database and contain tagger name, email, and date. The also have a message and are checksummed.
* It is generally recommended to create *annotated* tags
  * Use the `git tag -a <tagname> -m <tagmessage>` command
  * Without the `-m` option, the default editor is opened to edit the tag message
  * Use `git show <tagname>` to show information about the tag

---

* *lightweight* tags are created using `git tag` without any options
* Can tag any commit in the history using `git tag -a <tagname> <commitid>`
* Tags are not transferred to remote repos.
  * Need to explicitly push with `git push <remote> <tagname>`
  * Or push all (annotated and lightweight) tags with `git push <remote> --tags`
* Tags are deleted locally using `git tag -d <tagname>`
* Tags on a remote are deleted using `git push <remote> --delete <tagname>`
* A big advantage of using tags is that they can be used as *checkout* references
  * Use `git checkout <tagname>` to get the file version the tag is pointing to
  * This puts the repo in *detached HEAD* state, so be careful
  * **Must** make a branch to keep work done from a commit in this state (i.e. `git checkout -b <branchname> <tagname>`)

# Git branching

## Overview

* Branches allow work to happen without influencing the main code
* An expensive operation in delta-based version control
  * Need to make a copy of the directory
  * Can take long for large projects
* Git branches are *lightweight* and branching happens almost instantaneously
* It is easy to switch between branches
* Using a branching workflow is encouraged

## How Git branches work

* Remember that Git stores data as a series of snapshots
* Commits are objects that contain a *pointer to a snapshot* (as well as other info such as author, date, commit message, etc.)
* Commits also contain pointers to *parent commits* (immediate ancestor)
  * Initial commit has zero parents
  * A normal commit has one parent
  * A merge commit has multiple parents
* Branches are *lightweight*, movable pointers to a commit

![git commits with parents](<https://git-scm.com/book/en/v2/images/commits-and-parents.png>)

## Basics of branching and merging

* The default initial branch is called *master*
  * Just like any other branch
  * The name can be changed (although mostly we don't bother doing so)
* New branches can be created by using `git branch <branchname>`
  * This simply creates a new pointer to the current commit (also referred to as the `HEAD` pointer)
  * Although the branch is created, work continues on the current branch until you switch to the new branch

![creating a new branch](<https://git-scm.com/book/en/v2/images/head-to-master.png>)

---

* Switching to a new branch requires the `checkout` command, e.g. `git checkout <branchname>`
  * Can create and switch to a branch using `git checkout -b <branchname>`
* This moves the `HEAD` pointer to the checked out branch
* New commits will increment the current branch and `HEAD` branch pointers
* Can move back to a different branch using `checkout`, e.g. `git checkout master`
  * This **changes** the files in the working directory to match the snapshot at the commit this branch is pointing to
  * Work on the other branch is **not** lost! Switching back to that branch will restore the changes made
  * This makes it easy to separate work

---

* Committing on the parent branch (e.g. `master`) will cause the commit history to *diverge*
* This is displayed in the Sourctree UI, as well as with the command `git log --oneline --decorate --graph --all`
* **Please create and use branches often!**

![divergent history](<https://git-scm.com/book/en/v2/images/advance-master.png>)

## A simple branching example

* Demonstrate branching concepts with a simple example
* Will follow the following outline:
  * Do some work in a file
  * Create a branch for a new feature
  * Do some work in the new branch
  * Switch to a production branch
  * Create a branch to fix a bug
  * Merge the bugfix branch
  * Continue working on the new feature branch

---

* The `master` branch is initially checked out, with some commits preceding the current `HEAD`

![initial commit history](<https://git-scm.com/book/en/v2/images/basic-branching-1.png>)

---

* You want to work on issue #53
* Use the `git checkout -b iss53` command to create a new branch `iss53` and switch to it
  * Equivalent to `git branch iss53`, followed by `git checkout iss53`
* This repository is now in the following state:

![repo with branch iss53](<https://git-scm.com/book/en/v2/images/basic-branching-2.png>)

---

* Do some work on the new branch and create a commit
* This moves the `iss53` branch ahead, while leaving the `master` branch still pointing to the same snapshot

![new commit on branch iss53](<https://git-scm.com/book/en/v2/images/basic-branching-3.png>)

---

* There is now an urgent bugfix that needs to happen
  * This can be done independently of the feature work that is in progress
  * In progress feature work does not need to be deployed together with the bugfix
  * Feature work does not need to be reverted before working on the bugfix
* Simply switch back to the `master` branch
  * Pending work in the staging area will need to be committed or unstaged first
* Check out a new branch with `git checkout -b hotfix`
* Fix the bug and commit on the `hotfix` branch.
* There is now a diverging history

![the hotfix branch off of master](<https://git-scm.com/book/en/v2/images/basic-branching-4.png>)

---

* The `hotfix` branch can now be merged back into `master`
* Follow the following process
  * Check out the destination branch, i.e. `git checkout master`
  * Merge the source branch, i.e. `git merge hotfix`
* This creates a *"fast forward"* merge, as there is a linear path for the `master` pointer to reach the `hotfix` pointer
  * history is simply replayed in *fast forward*, as if the commit happened on master itself
* The `hotfix` branch should be deleted, as it is no longer needed. Delete the branch using `git branch -d hotfix`

![repo after the merge](<https://git-scm.com/book/en/v2/images/basic-branching-5.png>)

---

* Work can now continue on the feature, checkout the branch using `git checkout iss53`
* Do another commit on the feature branch to finish the feature
* The work done in the `hotfix` branch **is not present** in the `iss53` branch
  * Can pull the work from `master` into `iss53` by doing `git merge master`
  * Or can merge `iss53` into `master`

![more work on the iss53 branch](<https://git-scm.com/book/en/v2/images/basic-branching-6.png>)

---

* Merging from `iss53` to `master` is similar to how we merged `hotfix` into `master`
  * First, check out `master` (`git checkout master`)
  * Then merge the feature branch (`git merge iss53`)
* Fast forward cannot be done this time, as there is no linear forward path between the branch pointers
* A recursive merge will be performed
  * This is possible if two branches share a common ancestor
  * Will result in a merge commit that has multiple parents
* Can now delete the feature branch `git branch -d iss53`

![repo state showing common ancestor](<https://git-scm.com/book/en/v2/images/basic-merging-1.png>)
![merge commit](<https://git-scm.com/book/en/v2/images/basic-merging-2.png>)

## Merge conflicts

* Merging two branches often creates a conflict
  * The same part of the file has changed in two different branches
  * Git doesn't know which version of the file is correct
* The merge will fail if there is a conflict
* The conflict needs to be resolved before the merge can continue
  * Check which files are unmerged with `git status`

```bash
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

---

* Conflict resolution *markers* are added to the file with the conflict
* The file needs to be opened and edited manually to resolve the conflict
* Markers indicate what the current version (that exists on your current branch) the incoming version (on the branch you are merging from) look like
* Simply delete the section that you do not want to keep. Also delete the resolution markers
* Run `git add` on the file to mark it as *resolved*
* Run `git commit` to finally create the merge commit
* Merge conflicts **should always be resolved in the local repo**. Do not push a version to a remote repo that will cause a conflict there, as it is much harder to solve.
  * The usual procedure is to *pull* the latest changes from the remote repo
  * Then resolve any merge conflicts locally
  * Once all conflicts are resolved, the result can be pushed to the remote without conflicts

```text
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
<div id="footer">
please contact us at email.support@github.com
</div>
```

## Managing branches

* Some common commands for managing branches are

|         Command          | Description                                                       |
| :----------------------: | :---------------------------------------------------------------- |
|       `git branch`       | Lists all the current branches ('*' indicates the current branch) |
|     `git branch -v`      | Lists the latest commit on each branch                            |
|  `git branch --merged`   | Lists branches that have been merged into the current branch      |
| `git branch --no-merged` | Lists branches that have not been merged into the current branch  |
| `git branch -d <branch>` | Deletes the branch. Will fail if branch has not been merged       |
| `git branch -D <branch>` | Forces deletion of the branch (even if unmerged)                  |

## Workflows

* There are several workflows that are made possible due to Git's inexpensive branching
* Choosing a workflow depends on several things
  * The size of your team
  * How close together you are as a team
  * The amount of people working on the repo simultaneously
  * Whether you are working predominantly local or remote
  * ...and many other considerations...
* We will look at two workflows and suggest a third workflow in [Distributed Git](#distributed-git)
  * Long-running branches
  * Topic branches
  * Git-flow variant (later)

---

### Long-running branches

* It is possible to merge from one branch into another branch multiple times in the repo history
* To take advantage of this, some branches can remain open indefinitely
* These branches can mirror different stages in the development cycle
* There are usually two long-running branches that are kept open
  * `master`
  * `develop`
* All work on `master` is considered stable and production ready and has been or will be released soon
* All work is done by branching from `develop` into a feature branch and merging back into `develop`
* Once `develop` is stable enough, work from here can be merged into `master`
* Having multiple branches and merge steps is helpful when dealing with large and complex projects

![long running branches](<https://git-scm.com/book/en/v2/images/lr-branches-2.png>)

---

### Topic branches

* These are short-lived branches that are created for a specific piece of work
* They usually branch directly off of `master`
* Multiple branches are created and merged per day
* Can switch between branches easily
  * Allows experimentation
  * Allows separation of ideas and work
* Can choose which branches to merge and which to delete
* This makes code review easier, as it is easy to see which changes came from where

## Remote branches

* No conceptual difference to local branches
* Merely a branch on a remote repo
* Pointers that represent the state of a branch on a remote
* Remote branches can be "tracked" on the local repo
  * This creates a local branch
  * Sets up the local branch to track the remote branch
  * Local branch is of the form `feature#123`, remote branches take the form `<remote>/<branch>` (e.g. `origin/feature#123`)
* A good example is the `master` and `origin/master` branches that are created when cloning a repo
* Remote branches are not directly editable locally

---

![local and remote master branches on cloning](../images/remote-branches-1.png)

---

* Local and remote branches can become out of sync when work is done locally
* Can use tracking branches to stay in sync
  * create a local tracking branch by branching from a remote, e.g. `git checkout -b <branch> <remote>/<branch>` or simply `git checkout --track <remote>/<branch>`
  * tracking branches automatically `push` and `pull` to the correct remote branch
  * can instruct an existing local branch to track a remote branch by setting the upstream: `git branch -u <remote>/<branch>`
* Three commands to synchronise local and remote:
  * `git fetch <remote>` updates the `<remote>/<branch>` pointer with the current remote state of the branch (does not affect the local branch)
  * `git push <remote> <branch>` pushes the branch to the remote (i.e. creates a remote branch of the same name) and sets the local branch to track the remote branch
    * can also push to a differently named branch like so: `git push <remote> <localbranch>:<remotebranch>`
    * this is typically used to share branches with others in order to collaborate
  * `git pull` gets the latest changes from the remote and automatically merges into the local branch (**this modifies the working directory**)
    * this is the same as running `git fetch`, followed by `git merge`
* Can delete remote branches by running `git push <remote> --delete <branch>`

## The rebase command

* Rebasing is an alternative to `merge`
* Can be very useful in some circumstances
* Certain cases where it *must not be used*
* The `rebase` command takes a change snapshot and *reapplies it onto another snapshot*
* It *rewrites history* as if the changes happened on the target branch
* Allows a fast forward merge after the rebase has completed
* The main reason for its use is to maintain a cleaner repo history (all history is linear, with no diversions)
* **Do not rebase work outside of your local repo, as other people may rely on shared history staying unchanged**
  * e.g. don't push work to a remote, have others branch off of that work, then rebase your work and then push again
  * In other words, rebase before pushing to maintain a clean history, but never rebase after pushing

```bash
## first checkout a branch with changes on it
$ git checkout experiment
## rebase the current branch (experiment) onto the master branch
$ git rebase master
## move to the target branch
$ git checkout master
## merge the changed branch into master, will be fast-forward as history is shared
$ git merge experiment
```

# Distributed Git

## Distributed Workflows

* There are many different strategies for how to manage collaboration through Git. These are called workflows
* Each workflow has advantages and disadvantages
* Common workflows are:
  * [Centralised workflow](<https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow>)
  * [Feature branch workflow](<https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow>)
  * [Git Flow workflow](<https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow>)
    * Integrated into Sourctree
    * Originally proposed by [Vincent Driessen](<https://nvie.com/posts/a-successful-git-branching-model/>)
  * [Forking workflow](<https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow>)
  * [Integration-Manager workflow](<https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows#wfdiag_b>)
  * [Dictator and Lieutenants workflow](<https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows#wfdiag_c>)
* Some work best with small local teams (e.g. Centralised, Feature branch, Git Flow), and some work best with potentially large teams that are far apart (e.g. Forking, Integration-Manager, Dictator Lieutenants)
* It is proposed to use the **Git Flow** workflow, as it fits well with the current tracker/issue manager, it enforces the versioning model, and it is suitable for use in small local teams or large distributed teams
