# Copying Files Within The Cluster

## TL;DR

1. Please do your file copying on compute nodes, not login nodes.
2. If you need to manage a lot of files, it is well worth your time to learn how to use the `rsync` command. This program is available on UNIX and Mac computers by default. It is increasingly available on Windows as the Windows Subsystem for Linux (WSL) becomes more common.

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

We have written an extensive document about using rsync, which we invite you to read [here](../files/rsync.md).

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
