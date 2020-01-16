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
---

# What is Git?

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

![](../images/areas.png)

* Changes in the *Working Directory* are tracked (the file shows as **modified**)
* Individual changes can be **staged** into the *Staging Area*
* Staged changes can be **committed** to the repo
* Changes that are in the *Working Directory*, but not the *Staging Area* are not committed in the current commit

# Git basics

---

## Creating a new repo

## Cloning a repo

## Making and storing changes

---

### Commands to interact with files and the repo

---

### Repo status

---

### Tracking a file

---

### Committing a change

---

### Seeing what will change

---

### Ignoring files

---

### Removing files

---

### Moving files

## Viewing the commit history

## Undoing work

## Tagging commits