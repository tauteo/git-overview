# An overview of Git and how to use it

This document serves as a reference for those who would like to understand how git works, and how they can use it. The content is based on what is available in the [Pro Git book][1], as well as some other sources, although it doesn't cover the topics in nearly as much depth as the book. Please refer to the book if something is unclear.

Comparisons with Microsoft Visual SourceSafe are made throughout this document, as that is what is currently used in Columbus.

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


[1]: <https://git-scm.com/book/en/v2>
[2]: <https://nvie.com/posts/a-successful-git-branching-model/>