---
tags:
  - in-progress
---

# Sharing Files

How do you safely collaborate with others in a UNIX environment in an on-going basis?

There are different locations where you can store files for the long term. The correct location to store files depends on several factors, including their size, whether other users should be able to read or write them, and whether they should remain for your group after you have gone to another organization.

Everyone has a home directory. By default this is a private space.

You don't want to open up your home directory to everyone in the cluster by making your home directory group or world writable.

## UNIX Permissions

You need to understand the basics of file ownership and permissions. Here are some good tutorials to read:

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
