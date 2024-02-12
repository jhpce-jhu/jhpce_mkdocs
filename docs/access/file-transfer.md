---
tags:
  - topic-overview
  - needs-revision
---


# THIS PREVIOUS DOC IS TOO LONG AND NEEDS TO BE BROKEN UP INTO AN OVERVIEW AND SOME SUBORDINATE DOCUMENTS

Maybe file transfer needs to be a whole topic, meaning it has its own subdirectory inside docs to hold the markdown files, the include images files (from their own docs/filetransfer/images/ subdirectory), etc.

There is already a [Globus document](globus.md).

(JRT thinks that this doc should include a pointer to a document about INTERNAL file transfers. How to do that on a node, in a batch job, using good rsync arguments.)

# File Transfer
A number of options exist for transfering files to-and-fro between
JHPCE and your local host. Which solution you chose, depends on your
use case.

+ scp or sftp — file transfer via command line
+ GUI for sftp — file transfer by drag and drop from your desktop
+ GlobusOnline — fast file transfers between GlobusOnline endpoints
+ Aspera  —  very fast file transfer to and from Aspera servers
+ OneDrive access with rclone  —  Use “rclone” to access your OneDrive directory (as well as other network drives (AWS buckets, Google Drive…)
+ Unison — keep directories synced between the cluster and your local computer
+ Mount remote filesystems — directories at JHPCE mounted on your local host. **IS THIS MATERIAL STILL ACCURATE IN 2024?** Is this [example SSHFS doc](https://hpc-docs.cubi.bihealth.org/connecting/configure-ssh/linux/#file-system-mount-via-sshfs) worth re-using?
+ ftp (kind of…)

For transferring files to the cluster, you should use
`jhpce-transfer01.jhsph.edu` rather than a login node when
connecting. This is both significantly faster and more considerate to
other users.

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

**Apple OSX Filezilla** is an outstanding application that not only
provides a GUI browser for FTP, SFTP, but it also allows you to browse
WebDav, Amazon S3, and OpenStack Swift file systems. It is free to
download and install.


Microsoft Windows **MobaXterm** is our recommended application for
accessing the JHPCE cluster, or transferring files to and from the
cluster.  You can also use WinSCP or Putty if you are already familiar
with them.

## Rclone
Rclone can be used to access network file resources, such as OneDrive,
Google Drive, and AWS. See here for an example of connecting to
OneDrive.

## Aspera
Aspera is a commercial product that allows file transfers that are
reportedly 20 times faster than `ftp`. If you download data from the
NCBI Aspera server or download/upload data from/to JHU CIDR on the
Bayview campus, then you will use Aspera. The Aspera license does not
allow us to install the client for our users. You must install it
yourself. You may either download the linux client from the aspera
site or else use the client that we already downloaded. If you prefer
the latter, simply copy the installation script from here:

```
/jhpce/shared/jhpce/core/JHPCE_tools/1.0/packages/aspera-connect-3.6.2.117442-linux-64.sh
```

into your home directory, and then run the script:

```
bash aspera-connect-3.6.2.117442-linux-64.sh
```

This will install the `ascp` command under your home directory at  `~/.aspera/connect/bin`.  You can either add `~/.aspera/connect/bin` to your `PATH`, or use the full path to the `ascp` command to run it.


## Unison
Using Unison, you can keep data synchronized between a directory on
the cluster and a directory on your local system.  Please see this
excellent document written by Jacob Fiksel, one of our expert JHPCE
cluster users:
[https://github.com/jfiksel/cluster-example#how-to-use-unison-for-file-transfer-and-syncing]
(https://github.com/jfiksel/cluster-example#how-to-use-unison-for-file-transfer-and-syncing)

## Globus
See [https://jhpce-jhu.github.io/jhpce_mkdocs/globus/](https://jhpce-jhu.github.io/jhpce_mkdocs/globus/).


## Mounting virtual file systems
A common use case occurs when a user has a pipeline that is
periodicially emiting tab delimited files and the user wants to plot
these files with a favorite plotting or analysis application that runs
on their local host. In this case it is common to mount the remote
file system on the local host via NSF or SMB.

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

All of these should be done from the `rnet` queue to make use of our high speed ScienceDMZ network connection.

## Links
- [Setting up MobaXterm for File Transfers with the JHPCE Cluster](https://jhpce.jhu.edu/knowledge-base/setting-up-mobaxterm-for-file-transfers-with-the-jhpce-cluster/)
- [Using rclone to Access OneDrive](https://jhpce.jhu.edu/knowledge-base/using-rclone-to-access-onedrive/)
