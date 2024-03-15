---
tags:
  - needs-to-be-written
  - jeffrey
---

# Archive Files

JHPCE users store petabytes of data on our storage servers in millions of files. When 

## Archive commands

cpio
tar
zip
bzip2

## Compression
Most archive commands have optional arguments enabling the compression or decompression of files being added to or extracted from archives.

Some data compresses more than others. Plain text files compress very well. Binary files holding data read out of lab instruments or images compress less well or are already compressed as a result of the file format used.

JHPCE uses ZFS file systems with compression enabled for home directories and data stores like /dcs05.

{==Therefore you do not need to spend time or CPU cycles compressing files or archives already stored in the cluster.==}  That may be a worthwhile activity for reducing the size of files or archives before importing them into the cluster. Usually that is the case when the path from the data source to JHPCE contains links with limited bandwidth.

## Clean up before creating an archive.

Command to delete unwanted files matching patterns

touch -t 201909111200 911-noon; find . -newer 911-noon -print

find . \( \( -type d -name Library -prune \) -o \( -type d -name .dropbox -prune \) \) -o -newer 911 -depth 4 -print # ignore directories named Library and .dropbox, limit how deep to search

find ~/Library -name com.microsoft.rdc\* -print0 | xargs -0 ls -ltd # -print0 and -0 for xargs allows searches to deal with PATH NAMES CONTAINING SPACES

find /usr/bin/ -exec file {} \;|grep Perl # find Perl scripts

