---
tags:
  - topic-overview
  - needs-major-revision
---

# STORAGE OVERVIEW
!!! Warning
    Document under heavy-duty construction

Previous [stg page](storage.md) has some text that should move into this one.

## Types of storage
There are three basic categories of data storage you can write to.


|Type|Example Path|Quota?|Use|
|-------|-------------|:----:|---|
|Home directory|/users/*yourusername*|Yes|Store your environment|
|Project space|/dcs07/*grpname*/data|Yes|Research data|
|Scratch space|/fastscratch/|No|Application temporary files|


Characteristics, best uses of each.

Speed differences between SSD (dcs06, fastscratch) and spinning hard drives (dcs04, dsc05, dcs06).

## Specific File Systems You Need To Know About
Make a table for each of the three types of storage. Instead of one large table. Be consistent in dealing with the three types of stg.

In each table, include

* Name or form (/dcs0N/group/data)
* the environment variable you can use to refer to it, e.g. $HOME $FASTSCRATCH (JRT doesn't know of a SLURM variable for where your job starts. There's SLURM_SUBMIT_DIR but it doesn't reflect any usage of --chdir)

## Don't fill up /tmp
What about compute node's /tmp? Users need to know about it, because they may not know that software often writes there, and they need to understand that it is a shared limited resource. if they are explicitly writing to /tmp they need to know to limit their usage and clean up after themselves

## Home directories
How to learn how much you're using. How often updated?
Disk quotas - only hard, no soft (so no warning)
Issue with snapshots preventing you from being able to immediately reduce usage. Ask for increase in disk quota while overhang is being deleted?
See [this document](buying-in.md) to request increases in /users quota?

## Buying additional storage space
Periodic offerings.
How paid for (and therefore length of committment)

## Backing up storage
You need to ensure that you have copies of your most vital files located somewhere else.
See [this document](backups-restores.md) for more information.

## Getting information about storage
* du -sh
* df -h
* df -h .

Consider potential impact of running certain commands that will cause lots of I/O. Not crossing file system boundaries is an option found on a number of UNIX commands that is good to use.

## JUST YANKED OUT OF KNOWLEDGEBASE DOC - PUT SOMEWHERE

## Disk Storage Space on the JHPCE Cluster
+ There are several types of storage on the JHPCE cluster. 
  + Some space is for permanent storage of files, and other spaces can be used for short term storage of data.
  + For long term storage of files, most users make use of their 100GB of space in their home directory. 
  + For those groups needing more, we have large storage arrays and sell allocations on these large arrays. [Storage docs](https://jhpce-jhu.github.io/jhpce_mkdocs/storage/).
  + We build a new storage array about every 18 months; so if you are interested in purchasing an allocation please email bitsupport@lists.jhu.edu. 
  + In addition to these long-term storage offerings, there are scratch space areas for short-term data storage. 
    + Scratch space tends to be faster. Using scratch space avoids taking up space in your home or project storage space.
  + The best area used for scratch is the `fastscratch` array on the cluster. The fastscratch array provides 22TB of space that is built on faster SSD drives, vs traditional hard drives used for project space and home directories. All users have a 1TB quota for scratch space, and data older than 30 days is purged.
  + Traditionally in Unix/Linux, `/tmp` or `/var/tmp` directories are used for storing temporary files. On the JHPCE cluster, this is strongly discouraged as these directories are small. 
  + In R, the `tmpdir()` setting will dictate where temporary files are stored. If you are generating 10s of GB of temporary files, change `tmpdir()` to `fastscratch`.
  + In SAS, the default `WORK` directory will be located under your `fastscratch` directory.
  + In Stata, the default `tempfile` location is under `/tmp`. This can be changed by setting the `STATATMP` environment variable.

## Encrypted filesystem

!!! Hopefully
    We can just delete this info because we're getting rid of Lustre ASAP
    
+ The JHPCE Cluster currently supports the following mechanisms for
using encrypted filesystems.
+ Encrypted filesystem are used to provide “Encryption At Rest”, meaning that the data on disk will be safely
stored in an encrypted format, and only available in an unencrypted
state when the data is accessed by an approved user.
+ Userspace encrypted filesystems using `encfs` `ZFS/Lustre` encrypted
filesystems.
+ The `DCL02` storage array is built on encrypted disk
devices.
