---
tags:
  - in-progress
  - jeffrey
---

# Copying Files Within The Cluster

When transferring files into and out of the cluster, please use the transfer node. The various tools for that work are described in this [document](../access/file-transfer.md).

Please use compute nodes when copying more than a few files inside the cluster.

## Rsync

Rsync is a powerful command for copying large numbers of files between a SOURCE (aka SRC) and a DESTINATION (aka DEST), within a single computer or between computers. 

!!! Note
    A key benefit of `rsync` is that it will copy only the material needed to make DEST match SRC. Most other programs, such as `cp` or `scp`, will always copy over all of SRC to DEST.

There are MANY arguments which control how the copying will occur. So you can do things like specify files to exclude, create a log of the actions taken, and provide statistics when done.

The most common flag used with `rsync` is `-a` for "archive"

### SOURCE and DESTINATION

Rsync is very flexible.

SRC and DEST can be paths or hostnames with colons and paths or a combination. rsync will use ssh to send files between hosts, so you can even specify a different username for one of the SRC and DEST.

Multiple items can be listed as SRC material. Of course there can only be one DESTINATION

Local copying:
: `rsync [OPTION...] SRC... DEST`

Access via remote shell:
: Pull:
                 `rsync [OPTION...] [USER@]HOST:SRC... [DEST]`
: Push:
                 `rsync [OPTION...] SRC... [USER@]HOST:DEST`




### Mind the trailing slash

rsync ultimately needs you to specify a SOURCE location and a DESTINATION location.

If the SOURCE represents a directory then adding a forward slash to it will cause the contents of the directory to be copied into DESTINATION. If there is no trailing forward slash, then the SOURCE  directory will be copied into DESTINATION.

Example
: If directory /some/source/place contains files a, b, and c
: `rsync -a /some/source/place/  /a/destination/location/`
: will result in the contents of /a/destination/location/ being a, b, and c.
: If the command was instead 
: `rsync -a /some/source/place  /a/destination/location/`
: will result in the contents being instead /a/destination/location/place

### Rsync Examples

### Rsync Flags You May Want To Use

Most flags have two forms you can choose between, a short one consisting of a single character, or a readable form preceeded by two hyphens, and typically followed by an equals sign and a value.
```
-H preserve hard links
-x don't cross filesystem boundaries
--archive, -a            archive mode; equals -rlptgoD (no -H,-A,-X)
--dry-run, -n            perform a trial run with no changes made
--atimes -U preserve access (use) times
--crtimes, -N preserve create times (newness)
--times, -t preserve modification times
--acls, -A preserve ACLs (implies --perms)
--xattrs, -X preserve extended attributes
--numeric-ids don't map uid/gid values by user/group name
--human-readable, -h     output numbers in a human-readable format
--ignore-existing  (don't overwrite existing)
--delete-after  (wait until end to process deletes)
--max-delete=NUM         don't delete more than NUM files (FOR SAFETY) 
--itemize-changes, -i    output a change-summary for all updates 
--itemize-changes has complex output. see man page string "has a cryptic output" which I've saved as shell script rsync-itemize
--list-only              list the files instead of copying them
--log-file=FILE
--size-only              skip files that match in size 
--sparse                 turn sequences of nulls into sparse blocks
--ignore-times, -I       don't skip files that match size and time
--info=FLAGS             fine-grained informational verbosity
--chmod=CHMOD            affect file and/or directory permissions
--usermap=STRING         custom username mapping 
--groupmap=STRING        custom groupname mapping (STRING is not simply
--chown=USER:GROUP       simple username/groupname mapping 
--exclude-from={'list.txt'}
--exclude={'*.txt','dir3','dir4'}
--checksum -c           compare checksums
```

### Example batch job

Is shown [here](../slurm/crafting-jobs.md#copying-data-within-cluster).
