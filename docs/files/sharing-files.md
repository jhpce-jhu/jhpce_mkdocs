---
tags:
  - in-progress
  - jeffrey
---

# Sharing Files

How do you safely collaborate with others in a UNIX environment in an on-going basis?

There are different locations where you can store files for the long term. The correct location to store files depends on several factors, including their size, how private they are, whether other users should be able to read or write them, and whether they should remain for your group after you have gone to another organization.

## **Where to share?**

### Project Space
The best place for long-term sharing of files, and the only place to share any significant volume of files, is in file systems created for research groups who purchase allocations of one of our storage servers.

If your need is temporary or you cannot purchase your own space, you might be able to secure permission from an existing owner to use their space. They will need to create a subdirectory within their allocation for you and change the permissions of the directories above that to allow you and your group to see into those directories. We are happy to create a new UNIX group so you can use it in your permissions scheme. Send the request with the desired group name and a list of members to bitsupport.

### Home Directory

Everyone has a home directory. By default this is a private space. Things like SSH can break if the permissions on the home directory are set incorrectly.

You don't want to open up your home directory to everyone in the cluster by making your home directory group or world readable or writable. By default all users belong to the same group. It's not just that people you like and trust can see your files -- so can hackers if they get access to anyone's account. Your configuration files reveal details about your account, your accounts elsewhere and, if writable, allow hackers to set traps so they can become you. Then possibly access your home or other computers.

If you must use your home directory, please use ACLs to give specific access to specific people. We have written [an ACL document](../files/acl.md) to guide you. We are happy to create a new UNIX group so you can use it in your permissions scheme. Send the request with the desired group name and a list of members to bitsupport.

### Scratch Space

If you need to _briefly_ share some files with someone and they are too large to send via email (say 5MB), you might consider placing them in shared scratch space. This is not recommended, but we want you to be informed about this choice before you make it.

If you and your recipient are members of some common group other than the default, it is best to make your temporary directory group readable and searchable but remove access for "other".

**/tmp**
: An acceptable place for a quick one-time _exchange_ of small files which don't contain protected info _IF YOU ARE CAREFUL_. This directory is world-readable and writable. A prime consideration is whether there is enough room in /tmp for your purpose. The operating system and other users depend on there ALWAYS being enough free space in this file system. Delete your files ASAP after the exchange.

**/fastscratch**
: An acceptable place for _exchanging_ larger files, but not for sharing over times longer than, say, a week. Everyone has a 1TB quota on this fast file system, but there is only 22TB of space. This space is meant to enable fast access to data read or written by jobs on the compute nodes. Files older than 30 days are deleted automatically. This document describes this file system in more detail.

## UNIX Permissions

You {==must==} understand the basics of file ownership and permissions to share files. Here are some good tutorials to read:

- tutorial1
- tutorial2

!!! Note
    Users have to have appropriate permissions on all of the parts of a path needed to reach a final file or directory.

### Group Writable Directories

#### Group sticky bit

When you want to make a directory tree group writable but also configure the SGID bit on directories, you cannot use a simple recursive chmod command. 

These commands are examples of how you can recursively set correct permissions. They use the `find` command to print path names to directories or files, then pass those to xargs to execute the given commands upon.  

```Shell
cd into-the-top-of-the-directory-tree
find . -type d | xargs chmod u+rwx,g+rwx,g+s,o-rwx
find . -type f | xargs chmod u+rwx,g+rw,o-rwx
```

## Access Control Lists - ACLs

Sometimes you want to give access to an individual or a group in ways that the traditional UNIX permissions and ownership model do not allow. You can use an ACL to grant those permissions, including 

See [this document](acl.md) for details on how to do this.

## Creating A Group Writable Directory

!!! Warning "Raw Material Here"
    The following needs to be rewritten.

The list command, ls, is crucial to figuring out what is possible, where.
The long argument, -l, will show you ownership and permission information, as seen in my previous email.
Sometimes you also want to use the -d argument to tell ls that you want to see the information about a directory and not what is inside it.
 
So using ls -l  on /users/shared/55548/ will show you that many subdirectories are not group writable. Your failed efforts involve trying to create files or directories inside of directories that are group readable and searchable and sticky but not writable.
 
Every directory in a path to the destination needs to have suitable permissions.
Readability: every directory in the path /users/55548/shared/MA_switching needs to be readable by for you to be able to list its contents.
Writability: only the last directory in the path needs to be writable for you to be able to modify its contents.
 
jhpcecms01:/users/55548/shared# ls -l
total 300
drwxrwsrwx  7 c-jlevy33-55548  c-55548  7 Dec  5 11:10 biosim_leverage/
drwxr-s---  3 c-eblasco1-55548 c-55548  3 Feb  1 14:00 c-eblasco1-55548/
drwxr-s---  7 c-tbrow261-10201 c-55548  9 Dec 20 12:17 Elixhauser/
drwxr-s---  3 c-ttotoja1-55548 c-55548  3 Feb 16 16:09 forOwen/
drwxrwxr-x 16 c-jxu123-10201   c-55548 22 Jul 15  2023 from-yoda/
drwxrwsrwx  7 c-jlevy33-55548  c-55548  7 Feb  7 10:54 hipfracture_adrd/
drwxr-s---  7 c-jlevy33-55548  c-55548  7 Dec 20 14:03 levy_partd/
drwxr-s---  2 c-aliu63-55548   c-55548  6 Feb 19 16:39 MA_partB/
drwxr-s---  2 c-aliu63-55548   c-55548  5 Dec 15 14:03 MA_switching/
drwxr-s--- 10 c-rwu32-55548    c-55548 13 Nov  8 10:31 mltss_duals/
drwxrws---  4 c-yyang279-55548 c-55548  4 Nov 30 10:13 shen_yang/
drwxr-s---  2 c-ykuang6-55548  c-55548  4 Jan 30 12:13 test/
drwxr-s---  2 c-jlevy33-55548  c-55548  3 Aug 29 15:19 toofull/
 
Only the owner of a directory can change its permissions. So in this case c-aliu63-55548 needs to change the permissions on MA_switching to make it group writable.
This command run by them would change the directory:
chmod g+w /users/55548/shared/MA_Switching
This command run by them would change the directory and everything within it using the recursive flag:
chmod -R g+w /users/55548/shared/MA_Switching
 
Why are files and directories created with the permissions that they get?
A key factor here is the “umask” setting in the user’s environment when files and directories are being created.
The command “umask -S” will show you the permissions that will be used for new files and directories.
Here you see an example of my inspecting the contents of my umask setting and then changing it to make it such that when I create a file it will be group-writable.
If you want such a setting to be used in the future for all of your logins, you would add a line to the bottom of your .bashrc file in your home directory.
 
jhpcecms01:/users/55548/shared# umask -S
u=rwx,g=rx,o=rx
jhpcecms01:/users/55548/shared# umask u=rwx,g=rwx,o=rx
jhpcecms01:/users/55548/shared# umask -S
u=rwx,g=rwx,o=rx
 
Many of the people modifying files in the shared directory may want to adjust the permissions of existing files and directories, and change their umask values. If they want directory trees to be group writable and new files or directories to be the same. Or some other set of permissions. Not everything within the shared directory needs to be shared with every other user of the DUA.
