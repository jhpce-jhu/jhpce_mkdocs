# MANAGING FILES TOPIC - outline page

This is not an overview page yet. JRT didn't break out subtopics in the nav bar.
 
We have previously addressed these issues as if they were independent. I believe it would help users increase their efficiency if they were seen as a set of related issues under this topic: managing files.

## Overview
General info about where files of different types live (should live). Pointers to existing info over in the Storage topic. Reminder about what is backed up and what is not.

## Sharing files with other users
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
Existing document goes here, although that needs revision. Among other things, to be more clear about how commands to provide inherited are needed in addition to the initial commands providing the desired access.

## Transferring files into and out of the cluster
Our existing information gets folded in here.

## Accessing data from compute nodes
Should you stage your data to fastscratch?

Should you stage the data locally? Space considerations. (DO NOT FILL UP /TMP). Mention [sbcast](https://slurm.schedmd.com/sbcast.html)?

There's probably other things we could usefully say about this subject.

## Copying Data Around **Within** Cluster
Alternatives to copying (sharing via ACL, using symbolic links).

Do it on compute nodes.

Example batch jobs for doing so.

Rsync argument recommendations.

## Best Practices
Should users bother with compressing? No, because of underlying ZFS compression.

But archiving files can be a good idea. Speeds up directory access (less metadata in directories). Simplifies transferring. Mention checksumming with md5sum.

Check beforehand that there will be sufficient space in destination before trying to import data or copy it around.

Suggestions about backing up your critical files, such as using versioning systems.

## Data Protection Policies
What do users need to know about cluster and university policies to protect certain classes of information? Within their own files and with respect to files copied out of databases like marketscan.
