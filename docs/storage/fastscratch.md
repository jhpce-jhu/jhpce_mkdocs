---
tags:
  - in-progress
---

# Fastscratch

A 22TB file system created from fast Solid State Disk is available for your use. This provides a fast place to store input or output files for your compute jobs. There is no cost for using your personal scratch space.

*This scratch space is meant to be a short-term storage location; it is not a long-term storage solution.*

You can access your scratch space by using the `$MYSCRATCH` environment variable from an interactive cluster node session, or within a submitted job.

The actual absolute path to your personal scratch space is `/fastscratch/myscratch/$USER`.

## Key details

Because this limited resource is shared by all users, there are some very important restrictions for using it.

+ There is a 1TB quota set on the personal scratch space.
+ All files older than 30 days will be removed without exception.  
+ Even though there is a 30 day automatic deletion of data, we ask that you please remove data from your personal scratch space once you have finished using it.
+ The personal scratch space is not backed up. Therefore if you delete a file it cannot be recovered.
+ The fastscratch file system has NO redundancy, so scratch space drive failures result in data loss. Thus, move important files needed to be kept from scratch space ASAP.
+ Abuse of this space may result in files being deleted on an as needed basis.

!!! Danger
    If you `untar` or `unzip` a file, and the extracted files have a timestamp older than 30 days from the original bundle, they will be removed when the daily purge begins. To work around this, you can use the `touch` command to update the timestamp on the extracted files.

