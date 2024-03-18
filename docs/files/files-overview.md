---
tags:
  - topic-overview
  - in-progress
  - jeffrey
---

# MANAGING FILES

We spend a lot of our time manipulating files.  This document attempts to provide an overview of, and pointers to, useful knowledge.

## Overview

General info about where files of different types live (should live). Pointers to existing info over in the Storage topic. Reminder about what is backed up and what is not.

## Sharing files with other users

Collaborating with others is a daily activity on the cluster. There are better -- and worse -- ways to do that.

### Normal UNIX file ownership and permissions
Brief overview of how things work and how to inspect.

UNIX groups.

If nothing else, a pointer to other web pages (ours or on the Internet).

### Home Directory
What are the default settings and why are they important?

### Project storage
What are the default settings and why are they important?

What are common variations?

### Using ACL's to allow increased access
Traditional Unix file ownership and permissions can be used to share access to files and directories. However, there can be times when more fine-grained control of access is needed. To accomplish this, Access Control Lists (ACLs) can be used. They add to or extend the normal permissions.

We have a good document about how to use ACLs [here](../files/acl.md).

## Transferring files into and out of the cluster

A variety of tools exist for this task. We have documented many of them in [this document](../files/copying-files.md).

Before moving large amounts of data in or out, you may want to consolidate it into fewer files using archive tools. We have documented some of the relevant considerations in [this document](../files/archive-files.md).

For transferring files to and from the cluster, you should use
`jhpce-transfer01.jhsph.edu` rather than a login node.
This is both significantly faster, as the transfer node has a 40G Ethernet connection to the outside world while the login nodes have 10G connections. In any case, EVERYONE depends on the login nodes, and you should not run ANYTHING on them that occupies them.

## Accessing data from compute nodes

Consider your input/output needs for computations. Which storage location and type provides the best combination of speed, capacity and proximity to CPUs?

There may well be different answers for different kinds of files used during your work.

Should you stage your data to the [fastscratch](../storage/fastscratch.md) file system?

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

We protect home directories and project space with redundant arrays of disks. Some of our file systems are backed up daily to other locations. But ultimately **you** need to ensure that you have backups elsewhere of precious files. Consider creating a GitHub repo to hold programming code -- it represents a lot of effort.

Don't bother compressing files in most cases because we have enabled compression on our ZFS file systems. 

Archiving old or infrequently-used files can be a good idea. It speeds up directory access (less metadata in directories). It simplifies transferring bulk data and checking on the success or failure.

## Backups and Restores

Home directories are backed up once a day. Users can access the last fourteen days of data themselves. Primary Investigators can request that we back up project spaces. Only a few have done so.

Further information about this topic can be found in [this document](../storage/backups-restores.md)

## Data Protection Policies

We will be publishing information  about cluster and university policies for the protection of certain classes of information in [this document](../files/data-security.md). 
