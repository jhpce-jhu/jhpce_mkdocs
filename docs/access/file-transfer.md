---
tags:
  - topic-overview
  - needs-major-revision
title: File Transfer - Overview
---

!!! Warning "Authoring Note"
    This document was copied over from the old web site and is being slowly updated.

# File Transfer - Overview
A number of options exist for transfering files to-and-fro between
JHPCE and your local host. Which solution you chose depends on your
use case.

Copying files around inside the cluster, between JHPCE file systems, is a different activity.
We have a document about [using rsync](../files/copying-files.md) to copy files. This tool can be used for both internal copies and for moving files into or out of the cluster.

## Use the transfer node and partition

For transferring files to and from the cluster, you should use
`jhpce-transfer01.jhsph.edu` rather than a login node.
This is both significantly faster, as the transfer node has a 40G Ethernet connection to the outside world while the login nodes have 10G connections. In any case, EVERYONE depends on the login nodes, and you should not run ANYTHING on them that occupies them.

### Interactive use of the transfer node
You can ssh directly to `jhpce-transfer01.jhsph.edu` and do your work.

We have a `transfer` SLURM [partition](../slurm/partitions.md) which uses the same node that you can use for interactive or batch sessions.

``` title="Interactive job"
jhpce01% srun --pty --x11 -p transfer bash
```

### Batch use of the transfer node/partition

[Here](../slurm/crafting-jobs.md/#copying-data-within-cluster) is a sample SLURM batch job that you can use as a model to do an internal transfer on a compute node using good rsync arguments. You can adapt it to run on the `transfer` partition to perform transfers into or out of the cluster. You can also change it to use some other transfer programs. Usually such batch jobs require care when providing authentication information.

## Common Tools

Many of these transfer protocols have both command-line and graphic user interface programs available.

+ scp or sftp — file transfer via command line
+ [rsync](../files/copying-files.md) - a **very** useful uni-directional mirroring utility program
+ GUI for sftp — file transfer by drag and drop from your desktop
+ GlobusOnline — fast file transfers between GlobusOnline endpoints
+ Aspera  —  very fast file transfer to and from Aspera servers
+ OneDrive access with rclone  —  Use “rclone” to access your OneDrive directory (as well as other network drives (AWS buckets, Google Drive…)
+ Unison — keep directories synced (can be configured to be bi-directional)
+ Mount remote filesystems — directories at JHPCE mounted on your local host. **IS THIS MATERIAL STILL ACCURATE IN 2024?** Is this [example SSHFS doc](https://hpc-docs.cubi.bihealth.org/connecting/configure-ssh/linux/#file-system-mount-via-sshfs) worth re-using?
+ ftp (kind of…)


## `scp` and `sftp`
The `scp` and `sftp` command-line tools are the most common tools used for
transferring data to and from the cluster.  The basic tradeoff is
between speed (`scp` is faster) and flexibility (`sftp` is more
flexible).  The `scp` and `sftp` commands are available from the Terminal on a
MacOS or Linux based laptop/desktop, or from a `CMD` or `Powershell`
prompt on recent Windows systems.

Although both SCP and SFTP utilize the same SSH encryption during file
transfer with the same general level of overhead, SCP is usually
faster than SFTP at transferring files, especially on high latency
networks.  SFTP should be used when you may need an interactive
session on the cluster to navigate to a directory before transferring
the files, whereas SCP should be used when you know the exact path of
the file you want to transfer.

### `scp`
The `scp` command can be though of as a `network cp` command.  The command to transfer a file called `data.txt` from your local system to your home directory on the cluster would be:

```
scp LOCAL_PATH/data.txt USERID@jhpce-transfer01.jhsph.edu:REMOTE_PATH/REMOTE_TARGET_FILENAME
```

where the paths default to your current local directory and home
directory on the remote. The target filename if omitted will be the
local filename.


If you want to copy a file from the cluster to your local laptop/desktop, you would reverse the arguments. For example to copy `data2.txt` from the cluster to a local file:

```
scp USERID@jhpce-transfer01.jhsph.edu:REMOTE_PATH/data2.txt LOCAL_PATH/LOCAL_TARGET_FILENAME
```
### `sftp`

The `sftp` command is another means of transfering data to and from the cluster.  To use `sftp`, you would run the command:

```
sftp USERID@jhpce-transfer01.jhsph.edu
```

Once you’ve connected, you’ll be shown an `sftp>` prompt.  From here you can use
the shell command `ls` to get a directory listing, and and `cd` to
change directories.  In addition to `ls` and `cd` you can use the
`get` command to transfer a file from the cluster it to your local
system, or the `put` command to transfer a file from your local system
to the cluster.  Once you are done with `sftp`, you would type `exit` to
end the session.


## GUIs
If you prefer drag and drop interface rather than using shell
commands, then an application that presents a window for drag and drop
is what you want. Depending on which OS you are using, we can
recommend the following applications:

macOS users might consider [Filezilla](https://en.wikipedia.org/wiki/FileZilla). It 
 is an outstanding application that not only
provides a GUI browser for FTP, SFTP, but it also allows you to browse
WebDav, Amazon S3, and OpenStack Swift file systems. It is free to
download and install.


Our recommended application for Windows users accessing the JHPCE cluster is [MobaXterm](http://mobaxterm.mobatek.net/download-home-edition.html).
You can also use https://en.wikipedia.org/wiki/WinSCP or [Putty](https://en.wikipedia.org/wiki/PuTTY) if you are already familiar
with them.

### Rclone
[Rclone](https://en.wikipedia.org/wiki/Rclone) can be used to access network file resources, such as OneDrive,
Google Drive, and AWS. See [here](../access/onedrive.md) for instructions on using it to connect to Hopkins OneDrive storage.

### Aspera

[Aspera](https://www.ibm.com/products/aspera) is a commercial product from IBM that allows file transfers that are
reportedly 20 times faster than `ftp`.  Documentation is available for macOS, Linux and Windows.

Aspera is required to download data from the
[NCBI Aspera](https://www.ncbi.nlm.nih.gov/home/download/) server or download/upload data from/to JHU CIDR on the
Bayview campus.

The Aspera license does not allow us to install the client for our users. You must install it
yourself. You may either download the linux client from the [Aspera Download site](https://www.ibm.com/aspera/connect/) or else use the client that we already downloaded. (It may be out of date.) If you prefer the latter, simply copy the installation script from here:

```
/jhpce/shared/jhpce/core/JHPCE_tools/3.0/packages/ibm-aspera-connect_4.2.10.749_linux_x86_64.sh
```

into your home directory, and then run the script:

```
bash ibm-aspera-connect_4.2.10.749_linux_x86_64.sh
```

This will install the `ascp` command under your home directory at  `~/.aspera/connect/bin`.  You can either add `~/.aspera/connect/bin` to your `PATH`, or use the full path to the `ascp` command to run it.

You may also need to do other steps, such as install an extension to your web browser. Instructions on how to do that for Linux, as an example, are available from IBM [here](https://www.ibm.com/docs/en/aspera-connect/4.2?topic=suc-installation).


### Unison

!!! Warning
    We no longer have unison installed in the cluster. You can install your own copy of it.
    
Using [Unison](https://en.wikipedia.org/wiki/Unison_(software)), you can keep data synchronized between directories, including ones on a single computer or between the cluster and on your local system. Both CLI and GUI versions are available. Unison needs to be installed on both computers if used across a network.

Unison is a synchronization tool. It can be told to update files in both SOURCE and DESTINATION locations according to some rules.

Unison home page is [here](https://github.com/bcpierce00/unison?tab=readme-ov-file#readme) with a [wiki](https://github.com/bcpierce00/unison/wiki) that provides access to documentation and some binaries.

An extensive [tutorial](https://ostechnix.com/how-to-synchronize-files-with-unison-on-linux/) at ostechnix.

A [wiki](https://wiki.archlinux.org/title/unison) about using it from ArchLinux.

[This](https://github.com/jfiksel/cluster-example#how-to-use-unison-for-file-transfer-and-syncing) document written by a previous JHPCE user (Jacob Fiksel) might still be useful.

### Globus
We have a Globus endpoint. Please see [this document](globus.md).


## Mounting virtual file systems

!!! Obsolete
    This is probably obsolete information as of 20240220 - OSXFUSE is now named macFUSE and is [hosted](https://osxfuse.github.io) at a different location than what is described below.

A common use case occurs when a user has a pipeline that is
periodicially emiting tab delimited files and the user wants to plot
these files with a favorite plotting or analysis application that runs
on their local host. In this case it is common to mount the remote
file system on the local host via NFS or SMB.

Unfortunately, given the size and hetergeneity of our user base (which
spans the entire medical campus), this is not practical. Instead, we
recommend that users create a virtual file system on their OSX machine
with the MACFusion application. MacFusion is free and allows you to
create a mount point on your local host that looks like just another
directory in your local file system. So any applications and scripts
on your local host can access the data in that mount point. From the
user perspective, it acts just like an SMB or NSF mount point. Data is
transferred back and forth via an encrypted link.

MacFusion requires the installation of an OSX kernel extenstion and
some associated tools. OSXFuse provides the needed extension. OSXFuse
implements a so called ” FileSystem in USErspace”. This technology is
described here. There exist FUSE kernel modules for most flavors of
unix and linux.  The procedure for installing OSXFuse and MACFusion is
described below.

+ Downloaded OSXFUSE from sourceforge repository
+ Install OSXFUSE
+ Launch the OSXFUSE installer and perform a custom install.  Be sure to select “MacFuse Compatibility Layer” in the Custom Install screen. 
+ After installing the kernel extension it may, or may not, be necessary to reboot your mac.Screen Shot
+ Download and install the Macfusion app from: [http://macfusionapp.org](http://macfusionapp.org).
+ Startup MacFusion, and create an entry for `jhpce-transfer01.jhsph.edu` — enter your login and password.
+ Select a mount point, e.g. `~/jhpce/myhome/`
+ Once the drive is mounted, you can cd to the directory in the shell or view it  in a window on your desktop. To do this you need to “Reveal” the drive by pressing Command-R.  Once the directory is revealed, you can drag and drop files into the director in the usual way you drag and drop files into any directory on your mac.

## ftp
We don’t have the ftp client installed on the cluster.  It is an
older, less secure, unencrypted channel for transferring files.
However if you are downloading files from an older site that does not
support SFTP or one of the other more modern mechanisms, you have a
couple of options for ftp.

If you want to be able to interactively browse through the ftp site you can use the lynx text based browser command:

```
lynx ftp://USER@ftp.site.gov
```

Once connected, you can then use the arrow keys to move around the
site, and to select a file to download or a directory to descend into.

If you know the exact path to the file you want, you can use the “wget” command:

```
wget ftp://USER:PASSWORD@ftp.site.gov/path/to/file
```

All of these should be done from the `transfer` queue to make use of our high speed ScienceDMZ network connection.
