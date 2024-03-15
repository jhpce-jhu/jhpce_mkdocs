---
tags:
  - in-progress
  - jeffrey
---

# Copying Files Within The Cluster

JHPCE users store petabytes of data on our storage servers. Some of it accumulates as work is done. Much is imported from outside the cluster as source material for research. Moving around large numbers of files can be done with a variety of methods.  Some of them give you more confidence in the results than others. Some are misleadingly quiet despite failing or producing a result that isn't the same as the source location.

The `cp` command is a good example of a familiar tool that can produce unexpected results. It supports recursion through the `-R` flag. Depending on the UNIX version, `cp` may or may not treat symbolic links or hard-linked files or sparse files or device files the way you expect.

All tools have limitations, most of which we, thankfully, don't encounter in day-to-day use. However, when manipulating large numbers of files and files in directory trees many levels deep, these issues can begin to surface. For example, consider what happens if a program has a limitation where it cannot handle file paths longer than 1024 characters. You would be unlikely to experience a problem using that program to copy files around unless they are `/stored/in/directory/trees/with/many/entries/YYYY-MM-DD-MM-SS/long-filename-with-details-embedded-in-its-name.dat`  Such paths are common in the sciences and become more common as time passes and data accumulates.

When transferring files **into and out of** the cluster, please use the transfer node. The various tools for that work are described in this [document](../access/file-transfer.md).

Please use compute nodes when copying more than a few files inside the cluster.

## Archive tools moving data through a pipe

Rsync is usually the best method. But rsync wasn't always available in the past, or it didn't support copying one or another attribute that more basic tools did. Kinds of file permissions, data forks, etc.

One method that can be quick and effective is to use an archive program like tar to create an archive file which is passed through an input/output pipe to the same program running in another directory that extracts files into that location. This technique can use available system memory as buffer space, leading to smoother flows of data as disk reads and writes occur with optimal amounts of bytes.

This example copies the named directory some-directory from the current working directory to another location. Because tar by default works with blocks of data 512 bytes in size, a higher efficiency is achieved by telling it to create larger blocks to reduce the overhead of doing input/output requests in such small sizes.

```
tar cbf 20480 - some-directory | (cd /destination/place; tar -xbf 20480 -)
```
This example extends the technique to copying the data off of your current host to another host:
```
tar cbf 20480 - some-directory | ssh myaccount@another-host (cd /destination/place; tar -xbf 20480 -)"
```

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
