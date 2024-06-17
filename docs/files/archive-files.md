# **Archive Files**

It can be advantageous to use archive commands to gather up individual files into archive files.

JHPCE users store petabytes of data on our storage servers in billions of files. When data is not being actively used, it can be a good idea to store files in archives.

It is easier to deal with fewer files when transferring or storing for the long term.  There are fewer individual items to process, document, run checksums against, or ask programs to process. File systems also work better with fewer files per directory. Creating and altering entries for each file takes time. Network transfers proceed faster when data can be read and manipulated in larger blocks.


## **Archive commands**

A variety of programs exist which will add files into and extract files from archives of different formats. The features offered can include: checks for integrity, encryption, compression, Unicode filenames.

When choosing a format, consider its popularity. You do not want to learn years later that you can no longer access a program that can work with your old archive file.

Also consider which operating systems you may need to use when working with the archive file.

Also consider researching any known limitations of a format. Most formats have such limitations!!!  Some of these can bite you. For example, what is the maximum number of characters in a file name? Maximum number of characters in the complete path to the file?  Given how quickly data accumulates and the increase in file sizes over the years, other attributes can be an issue, such as: What is largest file size that can be contained within an archive? The size of the resulting archive? The number of files?  

A comparison of archive formats can be found [here](https://en.wikipedia.org/wiki/List_of_archive_formats#Comparison).

Popular programs:

- bzip2
- cpio
- gzip
- tar
- zip
- 7z

## **Compression**
Most archive commands have optional arguments enabling the compression or decompression of files being added to or extracted from archives. Usually those compression algorithms have a range of compression levels, where the CPU required to compress or decompress varies in relation to the amount of file compression achieved.

JHPCE uses ZFS file systems with compression enabled for home directories and data stores like /dcs05.

{==Therefore you do not need to spend time or CPU cycles compressing files or archives already stored in the cluster. They are uncompressed by the file server when you open them.==} 

Some data compresses more than others. Plain text files compress very well. Binary files holding data read out of lab instruments or images compress less well or are already compressed as a result of the file format used.

When is compressing worthwhile?

1. If you want to reduce files to absolute minimum size, you might enable maximum compression levels, e.g. `gzip -9`. Test your results -- sometimes compressing files can increase their size.
2.  Compressing files may be a worthwhile activity for reducing the size of files or archives *before* importing them into the cluster. 
3. Or if exporting cluster files to another location where space is limited or the network bandwidth is very limited. Remember that ssh programs (ssh, scp, sftp) do compression by default. 

## **Do your work on the right computer**

Login nodes are needed by *_everyone_*. They should not be bogged down by your work with files. Use a compute node for anything except the smallest file operations. If you aren't sure how much space is in a deep directory tree, don't start a check on a login node!

## **Preparing to create an archive**

### **Space**
Do you have enough space to create your archive? Do you need to create several smaller archives rather than an enormous single one? (That can have its own advantages.)

How much space is available in a file system?
: `df -h .` will show you info for your current working directory (CWD) including the hostname of the file server (if you're in an NFS file system)

How much space is consumed by a directory?
: `du -sh that-dir` will show you disk consumption of the named directory
: `du -sh that-dir/*` will show you disk consumption of files and directories inside the named directory

### **Data cleaning and preparation**

You may want to (carefully) prepare a set of files before archiving them. The amount of caution is of course related to the uniqueness and value of the data.

This data maintenance can include:

* renaming files or directories,
* deleting unwanted files,
* setting consistent file permissions,
* setting consistent user/group ownership,
* re-organizing files into directories,
* creating README files describing the contents of the archive, its creation method, and anything else you or someone else might want to know. (It is a great idea to keep a copy of such files both inside and outside of the archive!!)

Some commands like tar can accept arguments indicating that they should exclude certain file names or patterns. You can also create a list of files to archive out of a larger set of files and tell the archive program to only archive those files.

### **Precautions**
You can capture critical information before beginning (and while making changes) with simple commands like a recursive listing of a directory tree's contents such as `ls -lR mydir > mydir.contents`

If you are concerned that you might make mistakes during this process, consider:

* the backup process that might be protecting the storage area you are working within,
* if there is a backup done, its frequency,
* creating a scratch archive of the original material so you can fall back to it if needed,
* making one containing only the most essential files if you do not have the storage space for one containing everything 

While doing prepatory work, consider what commands or command options you can use to check what will happen *before* implementing changes such as file deletions. 

A helpful technique can be moving files aside into temporary trash directories before deleting them. If things work as desired, you can then delete the trash directories. You can also rename files instead of deleting them -- e.g. add a DEL suffix.

## **Before deleting the original files**

Test your work. Can you extract various sample files? Ones with the longest total path length in characters? Can you generate a list of the archive contents? Does creating a checksum with `md5sum archive-name.tar` succeed (it will cause every byte of the archive to be read)?

Have you created documentation about what the archive contains and the commands used to create it?

## **Example commands**

There are plenty of tutorials online for working with archives. Our goal with this document is to mention things to consider and some specific example commands.

### **Archiving commands**

Create archive from two directories and a file. The archive file is being created in a different file system, because you knew that the directories contain several terabytes of information (and want to store the archive there).  /dcs05 and /dcs07 come from two different file servers, so reading from one while writing to the other can be faster.

```
cd /dcs05/mygroup/data/trial5/experiment8/
tar -cf /dcs07/mygroup/my-tar-file.tar some-directory other-dir2 file9
```

Create a compressed archive of a directory tree using multiple CPU threads. (Four in this example.) (You need to request four CPU from SLURM to be able to have them to use on the compute node.) By default `pigz` deletes files after compressing them, so be sure to use the `-k` "keep" argument.

```
cd /dcs05/mygroup/data/trial5/experiment8/
tar -cf /dcs07/mygroup/my-tar-file.tar --use-compress-program="pigz -k -p4" some-large-directory
```

Extract everything from an archive. It is always a good idea to look at the contents of the archive to see what file structure will be created during extraction. You do not want to overwrite other files. There are options like `--keep-old-files` and `--keep-newer-files` (and others) which affect extraction behavior. Better safe than sorry!

```
tar -tzf compressed-archive.tgz | less
tar -xvzf compressed-archive.tgz
```

Extract multiple files from an archive. You need to first examine the contents of the archive and look for occurences of the names you want to extract -- there may be multiple items which match. You may need to provide partial paths to indicate which copy of the file you want.

```
tar -tf somearchive.tar > mylisting.txt
less mylisting.txt
tar -xvf somearchive.tar "file1" "file2"
```

Extract files matching a wildcard pattern
```
tar -xvf somearchive.tar --wildcards '*.php'
```


### **Find command**
The find command is a very useful tool. If you want to modify files or directories found with find, it is recommended to use xargs.

Note that file or directory names containing spaces or other such characters cause a world of problems in UNIX. It is extremely highly recommended to strictly avoid creating or retaining such names.

Here is an example that finds Perl scripts
```
find /usr/bin/ -exec file {} \; | grep Perl
```

Find the size of cache directories inside of "mydir" to help you decide whether to delete or exclude them. Store the results in a text file. Inspect it with the less command (press q to quit out of it). The find option -print0 and the xargs option -0 allow searches to deal with PATH NAMES CONTAINING SPACES.
```
find mydir -type d -name .cache -print0 | xargs -0 du -sh > /tmp/cache-list.txt
less /tmp/cache-list.txt
```

Find files ending in ".o" (but not directories) and rename them to ".o.DELETEME" The asterisk needs to be escaped by the backwards slash to prevent the shell from expanding the wildcard in the current directory before starting the find program.
```
find mydir -type f -name \*.o -print0 | xargs --null -I{} mv {} {}.DELETEME
```


Find files or directories modified after a certain date and time (09/11/2019 noon)

```
touch -t 201909111200 911-noon
find somedirofmine -newer 911-noon -print0
```

This more complex example ignores directories named Library and .dropbox, limits how deep to search
```
find . \( \( -type d -name Library -prune \) -o \( -type d -name .dropbox -prune \) \) -o -newer 911 -depth 4 -print0 
```


