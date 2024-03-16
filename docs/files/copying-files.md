# Copying Files Within The Cluster

## TL;DR
If you need to manage a lot of files, it is well worth your time to learn how to use the `rsync` command. This program is available on UNIX and Mac computers by default. It is increasingly available on Windows as the Windows Subsystem for Linux (WSL) becomes more common.

## Introduction

JHPCE users store petabytes of data on our storage servers. Some of it accumulates as work is done. Much is imported from outside the cluster as source material for research.  Moving around large numbers of files can be done with a variety of methods.  Many users default to `cp` or `mv`

All tools have limitations, most of which we, thankfully, don't encounter in day-to-day use. However, when manipulating large numbers of files and files in directory trees many levels deep, these issues can begin to surface. Some of them give you more confidence in the results than others. Some are misleadingly quiet despite failing or producing a result that isn't the same as the source location.

The `cp` command is a good example of a familiar tool that can produce unexpected results. It supports recursion through the `-R` flag. Depending on the UNIX version, `cp` may or may not treat symbolic links or hard-linked files the way you expect.

What is the state of your copy if `cp` or `mv` attempts fail mid-stream? In the middle of a file?

For example, consider what happens if a program has a limitation where it cannot handle file paths longer than 1024 characters. You would be unlikely to experience a problem using that program to copy files around unless they are `/stored/in/directory/trees/with/many/entries/YYYY-MM-DD-MM-SS/long-filename-with-details-embedded-in-its-name.dat`  Such paths are common in the sciences and become more common as time passes and data accumulates.

## Rsync

Rsync is a powerful command for copying large numbers of files between a SOURCE (aka SRC) and a DESTINATION (aka DEST), within a single computer or between computers. 

!!! Note
    A key benefit of `rsync` is that it will copy **only** the material needed to make DEST match SRC. Most other programs, such as `cp` or `scp`, will **always** copy over **all** of SRC to DEST.

There are MANY arguments which control how the copying will occur. So you can do things like specify files to exclude, create a log of the actions taken, and provide statistics when done. Rsync uses a variety of tests to compare files and directories. We wrote this page because using rsync properly for complex situations takes some skill and explanation.

The most common flag used with `rsync` is `-a` for "archive". This provides a recursive copy, preserving symbolic links, timestamps, file permissions, user & group ownership. It does NOT preserve ACLs or hard links.

## Use the appropriate resources
When transferring files **within** the cluster, please use compute nodes when copying more than a few files.

When transferring files **into and out of** the cluster, please use the transfer node. The various tools for that work are described in this [document](../access/file-transfer.md). The rsync information on this page can be useful for such transfers.

### Example rsync SLURM batch job

Is shown [here](../slurm/crafting-jobs.md#copying-data-within-cluster).

### SOURCE and DESTINATION

Multiple items can be listed as SRC material. Of course there can only be one DEST.

Rsync never updates the SRC location. Changes, if any, only occur in the DEST.

Files in DEST but not SRC will not be deleted unless you specify one of the delete flags.

SRC and DEST can be paths or hostnames with colons and paths or a combination. rsync will use ssh to send files between hosts, so you can even specify a different username for one of the SRC and DEST.

**Here are the different scenarios and their core syntax:**

Local copying:
: `rsync [OPTION...] SRC... DEST`

Access via remote shell:
: Pull:
                 `rsync [OPTION...] [USER@]HOST:SRC... [DEST]`
: Push:
                 `rsync [OPTION...] SRC... [USER@]HOST:DEST`

### Mind the trailing slash!

If the SOURCE represents a directory then adding a trailing forward slash to it will cause the contents of the directory to be copied into DESTINATION. If there is no trailing forward slash, then the SOURCE  directory itself will be copied into DESTINATION.

Example
: If directory /some/source/place contains files a, b, and c
: `rsync -a /some/source/place/  /a/destination/location/`
: will result in the contents of /a/destination/location/ being a, b, and c.
: If the command was instead 
: `rsync -a /some/source/place  /a/destination/location/`
: will result in the contents being instead /a/destination/location/place

This behavior is also found with the old standard `cp` program when using the `-R` recursive flag!!!

### Rsync Examples

It pays to be cautious when running commands which can cause many changes in short order.

Rsync can be used to compare two directory trees without updating anything. If both possibly have unique data, then you want to be careful and run preliminary `--dryrun` commands with a variety of informational flags. 

Show what would be done by make no changes:
```
rsync -a --verbose --dryrun --stats /local1 /other2
```

Only copy files ending in ".txt"
```
rsync -avz --include='*.txt' /src /dest
```

A good combination.
```
rsync -avhAXH --progress --numeric-ids --sparse --one-file-system --stats --delete-after
```


### Rsync Flags You May Want To Use

Rsync is _very_ flexible. The manual page is long. Here are some useful arguments to know about, organized somewhat by purpose. 

Most flags have two forms you can choose between, a short one consisting of a single character, or a readable form preceeded by two hyphens, and typically followed by an equals sign and a value.
```
--archive, -a            archive mode; equals -rlptgoD (no -H,-A,-X)

--acls, -A               preserve ACLs (implies --perms)
--xattrs, -X             preserve extended attributes
--hard-links, -H                       preserve hard links

--exclude-from={'list.txt'}.        there is a corresponding --include-from
--exclude={'*.txt','dir3','dir4'}   there is a corresponding --include

--dry-run, -n            perform a trial run with no changes made
--list-only              list the files instead of copying them
--ignore-existing        don't overwrite existing files, no matter what
--max-delete=NUM         don't delete more than NUM files (SET TO 0 OR LOW NUMBER FOR SAFETY)
--one-file-system, -x    ensure that you stay inside the SRC file system 

--atimes -U              preserve access (use) times
--crtimes, -N            preserve create times (newness)
--times, -t              preserve modification times

--numeric-ids            don't map uid/gid values by user/group name (more efficient)
--delete-after           wait until end to process deletes (much more efficient than deleting during)
--partial                allows you to resume an interrupted transfer

--progress               show which file is being copied (not stored in log file!!)
--itemize-changes, -i    output a change-summary for all updates 
                         has complex output. (see below for a link)
--log-file=FILE          useful when using --itemize
--verbose, -v            increase verbosity
--info=FLAGS             fine-grained informational verbosity
--human-readable, -h     output numbers in a human-readable format
--stats                  provide statistics at the end
  
--size-only              skip files that match in size (know what you're doing)
--ignore-times, -I       don't skip files that match size and time (when in doubt...)
--checksum -c            skip based on checksums, not mod-time & size

--sparse                 turn sequences of nulls into sparse blocks
--bwlimit                limit the impact of the rsync on the network (in kb/sec)

--chmod=CHMOD            affect file and/or directory permissions
--usermap=STRING         custom username mapping 
--groupmap=STRING        custom groupname mapping (STRING is not simply
--chown=USER:GROUP       simple username/groupname mapping 

```

The `--itemize-changes` flag is especially helpful when trying to compare two directory trees. However, it produces a cryptic string for each file or directory. We have copied a useful chart from the Internet that you can consult. See [this page](../files/rsync-itemize-table.md)

## Archive tools moving data through a pipe

Rsync is usually the best method. But rsync wasn't always available in the past, or it didn't support copying one or another attribute that more basic tools did.  Different kinds of file permissions, data forks, etc.

One method that can be quick and effective is to use an archive program like tar to create an archive file which is passed through an input/output pipe to the same program running in another directory that extracts files into that location. This technique can use available system memory as buffer space, leading to smoother flows of data as disk reads and writes occur with optimal amounts of bytes.

This example copies the named directory some-directory from the current working directory to another location. Because tar by default works with blocks of data 512 bytes in size, a higher efficiency is achieved by telling it to create larger blocks to reduce the overhead of doing input/output requests in such small sizes.

```
tar -cbf 20480 - some-directory | (cd /destination/place; tar -xbf 20480 -)
```
This example extends the technique to copying the data off of your current host to another host:
```
tar -cbf 20480 - some-directory | ssh myaccount@another-host (cd /destination/place; tar -xbf 20480 -)"
```
