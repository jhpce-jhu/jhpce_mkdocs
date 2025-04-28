---
tags:
  - topic-overview
---

# MANAGING FILES

We all spend a lot of our time manipulating files.  This document attempts to provide an overview of, and pointers to, useful knowledge.

## Where files live

We have created a Storage topic in this web site. Please visit the [overview](../storage/index.md) and any other pages of interest.

## Naming files
Files are created and manipulated with programs. Those programs are written by many different people and organizations. Some of them attempt to adhere to standards like [POSIX](https://en.wikipedia.org/wiki/POSIX). Files are stored in file systems. Both programs and file systems have rules and limitations -- and bugs. {==It is best to be conservative when naming files.==}

UNIX shells like bash, our default, interpret many characters that people have grown used to using in file names on their Windows or Mac computers as having special meanings. Ampersands, parentheses, vertical bars for example. Using other characters, such as spaces, require file names to be enclosed in single or double quotes or escaped with backslashes when provided to UNIX commands. Combining multiple commands which pass file names from one to the other either fail or require the use of special techniques.

Instead of space characters, use hyphens or underscores.

It is best to use the plain Latin alphabet. Accented characters or those which require Unicode to portray might be stored in the file system, but they may not be handled correctly by programs that try to manipulate them. If those failures are silent, then you may not understand what is happening or what failed to happen. For example, a file may not be added to an archive file, or if it is, might be hard to extract.

There are limits to the length of file names (255 characters in most _file systems_) and _program_ limits to the length of file paths (e.g. 4096 in Linux, 1024 in macOS). Again, exceeding these limits might work in some situations but not others. For example, creating the files might succeed but trying to specify one using a long absolute path might fail. Working with them in one operating system and then transferring them to another might fail.

You can look up some limits using the getconf command. Note again that these limits are not guarantees that all of the software you are using will work correctly with things that approach or exceed them.

```
getconf NAME_MAX /
getconf PATH_MAX /
```

{==It is HIGHLY RECOMMENDED that you simply DO NOT create files with such names, and that you rename ones you might already have. It will save you a lot of grief over the long run.==}



## Sharing files with other users

Collaborating with others is a daily activity on the cluster. There are better -- and worse -- ways to do that.

We have tried to document relevant information in [this document](../files/sharing-files.md).

### Using ACL's to allow increased access
Traditional Unix file ownership and permissions can be used to share access to files and directories. However, there can be times when more fine-grained control of access is needed. To accomplish this, Access Control Lists (ACLs) can be used. They add to or extend the normal permissions.

We have a good document about how to use ACLs [here](../files/acl.md).

## Transferring files into and out of the cluster

A variety of tools exist for this task. We have documented many of them in [this document](../files/copying-files.md).  `Rsync` is one such program. It has a learning curve and there are some important things to know about using it. We have put that information into the document we wrote about [copying data around within the cluster](../files/copying-files.md). 

Before moving large amounts of data in or out, you may want to consolidate it into fewer files using archive tools. We have documented some of the relevant considerations in [this document](../files/archive-files.md).

For transferring files to and from the cluster, you should use
`jhpce-transfer01.jhsph.edu` rather than a login node.
This is both significantly faster, as the transfer node has a 40G Ethernet connection to the outside world while the login nodes have 10G connections. In any case, EVERYONE depends on the login nodes, and you should not run ANYTHING on them that occupies them.

## Accessing data from compute nodes

Consider your input/output needs for computations. Which storage location and type provides the best combination of speed, capacity and proximity to CPUs?

There may well be different answers for different kinds of files used during your work.

Should you stage your data to the [fastscratch](../storage/fastscratch.md) file system? That document describes how to use chained jobs to get the data there with one batch job and then start the computation with another.

Should you stage the data locally to the compute nodes or access it over the network? Compute nodes only have local storage in their /tmp file systems. Everyone else using a node needs to share that space. The operating system of the node itself needs there to be some free space in /tmp.

There is a SLURM command which can copy data to assigned nodes. See the [sbcast](https://slurm.schedmd.com/sbcast.html) manual page.


## Copying Data Around **Within** Cluster

Alternatives to copying (sharing via ACL, using symbolic links).

Do it on compute nodes.

Example batch jobs for doing so.

Rsync argument recommendations.

## Best Practices

Think before hitting ++return++. Think again. There are few substitutes.

Check beforehand that there will be sufficient space in destination before trying to import or create data or copy it around.

We protect home directories and project space with redundant arrays of disks. Some of our file systems are backed up daily to other locations. But ultimately **you** need to ensure that you have backups **elsewhere** of precious files. Consider creating a GitHub repo to hold programming code -- such files represent a lot of effort. US Navy SEALs have a pessimistic/realistic motto about redundancy: Two is one, one is none.

Don't bother compressing files in most cases because we have enabled compression on our ZFS file systems. 

Archiving old or infrequently-used files can be a good idea. It speeds up directory access (less metadata in directories). It simplifies transferring bulk data and checking on the success or failure. We have a document exploring file archiving [here](../files/archive-files.md).

## Backups and Restores

Home directories are backed up once a day. Users can access the last fourteen days of data themselves. Primary Investigators can request that we back up project spaces. (Only a few have done so.)

Further information about this topic can be found in [this document](../storage/backups-restores.md)

## Data Protection Policies

We will be publishing information  about cluster and university policies for the protection of certain classes of information in [this document](../files/data-security.md). 

## Navigating Deep Directory Trees

Many researchers use directory and file names to create logical structures of significant complexity to organize their files.  The complexity increases over time as new projects and data sets come along. Long file or directory names are used to make things as self-obvious as possible.

It can be a hassle to move around quickly and efficiently between directories you are frequently using. 

If you have to switch between two directories with long paths, the techniques in [this document](../files/deep-paths.md) can make life better.
