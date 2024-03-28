---
tags:
  - done
---

# Fastscratch

A 22TB file system created from fast Solid State Disk is available for your use. This provides a fast place to store input or output files for your compute jobs. There is no cost for using your personal scratch space.

!!! Note
    *This scratch space is meant to be a short-term storage location; it is not a long-term storage solution. Please remove data from your personal scratch space once you have finished using it.*

You can access your scratch space by using the `$MYSCRATCH` environment variable from an interactive cluster node session, or within a submitted job.

The actual absolute path to your personal scratch space is `/fastscratch/myscratch/$USER`.

## Key details

Because this limited resource is shared by all users, there are some very important restrictions for using it.

+ There is a generous 1TB quota set on the personal scratch space. (See [this document](quotas.md) for more information about disk quotas.)
+ All files older than 30 days will be removed without exception.  
+ Even though there is a 30 day automatic deletion of data, {==we ask that you please remove data from your personal scratch space once you have finished using it.==} If only 22 users used their full quotas and left their files behind counting on automatic deletion then the file system would fill up and many jobs would fail.
+ The personal scratch space is not backed up. Therefore if you delete a file it cannot be recovered.
+ The fastscratch file system has NO redundancy, so scratch space drive failures result in data loss. Thus, move important files needed to be kept from scratch space ASAP.
+ Abuse of this space may result in files being deleted on an as needed basis.

!!! Danger
    If you `untar` or `unzip` an archive file, and the extracted files have a timestamp older than 30 days from the original bundle, they will be removed when the daily purge begins. To work around this, you can use the `touch` command to update the timestamp on the extracted files.
    
## Use chains of jobs to stage data

By using the dependent jobs feature of SLURM, you can arrange a series of jobs to stage data into and out of /fastscratch.

This document has information about dependent jobs https://jhpce.jhu.edu/slurm/crafting-jobs/#dependent-jobs

You can use a batch job to copy files from project storage space (e.g. /dcs07/something) or your home directory as they are needed. Or even download a file from the internet via a job submitted to the `transfer` partition.
 
Then you can create a second batch job that starts automatically when the copy batch job completes successfully by making the second job dependent on the first one using the right condition tag.

Third or later jobs can be run to clean out your finished files, whether by deleting them or creating archive files and copying them to a permanent location.

