---
tags:
  - in-progress
  - jeffrey
---
# ACL - Access Control Lists

Traditional Unix file and group permissions can be used to share
access to files and directories.  However, there can be times when
more fine-grained control of shared access is needed. To accomplish
this, Access Control Lists (ACLs) can be used. They add to the normal permissions.

!!! Tip
    Before defining ACLs, you should first read [our document about sharing files](..//files/sharing-files.md) for necessary background concepts and skills.

### Caution
JHPCE uses two kinds of file systems on its large storage servers: ZFS and Lustre.

You need to use a different pair of ACL commands for each type:
1. ZFS: nfs4_getfacl and nfs4_setfacl
2. Lustre: getfacl and setfacl

Instructions for Lustre file systems is found [later in this document](../files/acl.md#directories-in-lustre-file-systems).

As of March 2024, the only Lustre file systems are those which begin with the path `/dcl02/`

As of March 2024, all of the files originally found on the Lustre file server named DCL01 have been copied off to live on other, ZFS-using file servers. But the name /dcl01 has been preserved, for convenience.

## Overview

Some common notes that are applicable to both types of ACL commands:

+ ACLs can be used on both files and directories.
+ User's *umask* settings impact the permissions assigned to files and directories being created.
+ Use normal UNIX permissions where possible. ACLs can be complex to manage. Use ACLs to *extend* normal permissions.
+ ACLs should use the security notion of “least privilege”, meaning that ACLs should give only the needed access and nothing more.
+  For users to be able to work with a file stored several layer deep in the directory structure, they must be able _to get to it_. {==That requires that they need sufficient permissions, via either normal UNIX permissions or ACLs (or both), to "read" and "execute" (aka "search") **all of the directories above the final file or directory**==}. For example, if you are wanting to enable access to the directory `/dcs07/bob/data/project1/shared`, you would need to provide `READ-EXECUTE` access on `/dcs07/bob/data/project1`, `/dcs07/bob/data`, and `/dcs07/bob`. Using UNIX permissions where appropriate, and ACLs where necessary.
+ "Default" ACLs can be set on a directory, and this ACL will be inherited into the directory structure as new files and directories are created.
+ However, existing files and directories that exist beneath a directory that an ACL is being set on will not be affected by a new ACL. If you need to propagate an ACL into an exiting directory tree, you will need to use the `recursive` option to the ACL command.
+ With default ACLs, the individual user’s `umask` setting is important in assuring that new files and directories that get created have the correct permissions set. The `umask` setting will take precedence over the ACL, so *it must be more permissive than the ACL*. For example, if you want to set a default group ACL where the group has write access, you need to make sure that your umask is set to `0002` rather than `0022`, as the “2” in the group umask bit will prevent the group write capability.
+ Like any operation, running ACL commands on large numbers of files should be run on a compute node and not a login node. Long-running recursive ACL commands on large directory trees may be done via interactive sessions or submitted batch job scripts.


Example useage:

```
[alice@compute-123 ~]$ mkdir test1
[alice@compute-123 ~]$ umask
0022
[alice@compute-123 ~]$ nfs4_setfacl -a A:gfdi:hpscc@cm.cluster:RWX test1
[alice@compute-123 ~]$ touch test1/f1
[alice@compute-123 ~]$ nfs4_getfacl test1/f1

# file: test1/f1
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A:g:hpscc@cm.cluster:rtcy
A::EVERYONE@:rtcy
[alice@compute-123 ~]$ umask 0002
[alice@compute-123 ~]$ touch test1/f2
[alice@compute-123 ~]$ nfs4_getfacl test1/f2

# file: test1/f2
A::OWNER@:rwatTcCy
A::GROUP@:rwatcy
A:g:hpscc@cm.cluster:rwatcy
A::EVERYONE@:rtcy
```

### Basic commands
+ There are 2 commands for dealing with ACLs on `/users`, `/dcs04`, `/dcs05`, `/dcs06`, and `/dcs07`. 
+ The `nfs4_getfacl` command will display current ACL setting for a file or directory
+ The `nfs4_setfacl` command is used to modify ACLs.  
+ With ACLs you can grant different kinds of access on directories or files to specific users and groups, in addition to the normal group associated with the object, and in addition to "other".
+ The simplest permissions to use in ACLs are R for read access, W for write access, and X for execute and directory access. For detailed description of fine-grained permissions that can be set, please see ([https://www.osc.edu/book/export/html/4523](https://www.osc.edu/book/export/html/4523)).
+ By default, one’s home directory is only accessible to the
owner, and the original ACL should reflect this. For example, for the user alice, the ACL on their home directory would look like:

```
[alice@compute-123 ~]$ pwd
/users/alice
[alice@compute-123 ~]$ nfs4_getfacl .
# file: .
A::OWNER@:rwaDxtTcCy
A::GROUP@:tcy
A::EVERYONE@:tcy
```

### Read permisions

+ Now, if alice wanted to grant read-only access to their home
  directory to the user bob, they would use the `nfs4_setfacl`
  command:

```
[alice@compute-123 ~]$ nfs4_setfacl -a A::bob@cm.cluster:RX .
[alice@compute-123 ~]$ getfacl .
# file: .
A::OWNER@:rwaDxtTcCy
A::bob@cm.cluster:xtcy
A::GROUP@:tcy
A::EVERYONE@:tcy
At this point, bob would be able to access alice’s home directory. Now suppose there is a file that alice wants to let bob update. Alice could use ACLs to grant write access to a particular file:

[alice@compute-123 ~]$ ls -l shared-data.txt
-rw-r--r-- 1 alice users 79691776 Feb  2 07:06 shared-data.txt
[alice@compute-123 ~]$ nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
[alice@compute-123 ~]$ nfs4_setfacl -a A::bob@cm.cluster:RWX shared-data.txt 
[alice@compute-123 ~]$ nfs4_getfacl shared-data.txt
nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::bob@cm.cluster:rwaxtcy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
```

### Write permissions
+ Now suppose alice wanted to create a directory that bob could write to. 
+ Alice could create a directory, and grant bob write access to it:

```
[alice@compute-123 ~]$ mkdir shared
[alice@compute-123 ~]$ nfs4_setfacl -a A::bob@cm.cluster:RWX shared
[alice@compute-123 ~]$ nfs4_getfacl shared
nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::bob@cm.cluster:rwaxtcy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
Now the user bob can copy or save files in the “shared” directory.
```

### Inherited Defaults 
To set an inherited “default” ACL that will allow bob access on all
new files and directories that get saved into `/users/alice/shared`, you
would need to

1. first give bob the normal ACL permissions, then
2. add a second set of ACLs that will be inherited using the `fdi` option to the `nfs4_setacl` command.

One
issue we’ve seen is that a `@USER` and `@GROUP` ACL need to be
explicitly set for permissions to be properly set:

```
[alice@compute-123 ~]$ nfs4_setfacl -a A:fdi:bob@cm.cluster:RWX shared
[alice@compute-123 ~]$ nfs4_setfacl -a A:fdi:OWNER@:RWX shared
[alice@compute-123 ~]$ nfs4_setfacl -a A:fdi:GROUP@:RWX shared
[alice@compute-123 ~]$ nfs4_getfacl shared
nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::bob@cm.cluster:rwaxtcy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
A:fdi:OWNER@:rwaDxtTcCy
A:fdi:bob@cm.cluster:rwaDxtcy
A:fdi:GROUP@:rwaDxtcy
A:fdi:EVERYONE@:tcy
```

To set a Group ACL, you need to add the `g` option to the `nfs4_getfacl` command.

```
[alice@compute-123 ~]$ mkdir shared
[alice@compute-123 ~]$ nfs4_setfacl -a A:g:hpscc@cm.cluster:RWX shared
[alice@compute-123 ~]$ nfs4_getfacl shared
nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A:g:hpscc@cm.cluster:rwaDxtcy
A::EVERYONE@:rtcy
To set a Group Inherited ACL, you need to add the “gfdi” option to the “nfs4_getfacl” command. With this inherited group ACL set, all new files and directories will inherit the group settings.

[alice@compute-123 ~]$ nfs4_setfacl -a A:gfdi:swdev@cm.cluster:RWX shared
[alice@compute-123 ~]$ nfs4_setfacl -a A:fdi:OWNER@:RWX shared
[alice@compute-123 ~]$ nfs4_setfacl -a A:fdi:GROUP@:RWX shared
[alice@compute-123 ~]$ nfs4_getfacl shared

# file: shared
A::OWNER@:rwaDxtTcCy
A::GROUP@:rwaDxtcy
A:g:hpscc@cm.cluster:rwaDxtcy
A::EVERYONE@:rxtcy
A:fdi:OWNER@:rwaDxtTcCy
A:fdi:GROUP@:rwaDxtcy
A:fdig:swdev@cm.cluster:rwaDxtcy
A:fdi:EVERYONE@:tcy
```

### Removing an ACL
If you want to remove and ACL, you can use the `-x` option to `nfs4_setfacl`. Please note that you need to use the full ACL, and not the `RWX` shortcuts.

```
[alice@compute-123 ~]$ nfs4_setfacl -x A::bob@cm.cluster:rwaxtcy shared
[alice@compute-123 ~]$ nfs4_getfacl shared
nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
```

## Directories in Lustre file systems 

There are two commands for dealing with ACLs on the JHPCE cluster for
directories in Lustre file systems, which start with `/dcl02`.  The `getfacl` command will display
current ACL setting for a file or directory, and the `setfacl` command
is used to modify ACLs.  With ACLs you can grant either read-only
access, or read-write access on a directory or file to specific users.

Let’s say there is a directory `/dcl02/project/data/alice` that alice
owns. The `getfacl` command could be used to see the current ACL set
on the directory:

```
[alice@compute-123 ]$ pwd
/dcl02/project/data/alice
[alice@compute-123 ]$ getfacl .
# file: .
# owner: alice
# group: users
user::rwx
group::---
other::---
```

#### Read
Now, if alice wanted to grant read-only access to `/dcl02/project/data/alice` to the user bob, they would use the `setfacl` command:

```
[alice@compute-123 ]$ setfacl -m user:bob:rx .
[alice@compute-123 ]$ getfacl .
# file: .
# owner: alice
# group: users
user::rwx
user:bob:r-x
group::---
mask::r-x
other::---
```
#### Write
Now suppose there is a file that alice wants to let bob update. Alice could use ACLs to grant write access to a particular file:

```
[alice@compute-123 ]$ ls -l shared-data.txt
-rw-r--r-- 1 alice users 79691776 Feb  2 07:06 shared-data.txt
[alice@compute-123 ]$ getfacl shared-data.txt
# file: shared-data.txt
# owner: alice
# group: users
user::rw-
group::r--
other::r--
[alice@compute-123 ]$ setfacl -m user:bob:rw shared-data.txt 
[alice@compute-123 ]$ getfacl shared-data.txt
# file: shared-data.txt
# owner: alice
# group: users
user::rw-
user:bob:rw-
group::r--
mask::rwx
other::r--
```
#### Creating a common dir

Now suppose alice wanted to create a directory that bob could write to. Alice could create a directory, and grant bob write access to it:

```
[alice@compute-123 ~]$ mkdir shared
[alice@compute-123 ]$ setfacl -m user:bob:rwx shared
[alice@compute-123 ]$ getfacl shared
# file: shared
# owner: alice
# group: users
user::rwx
user:bob:rwx
group::r-x
mask::rwx
other::r-x
Now the user bob can copy or save files in the “shared” directory.
```

#### Default ACLs
To set an inherited default ACL that will allow bob access on all new files and directories that get saved into `shared`, you would need to use the `-d` option to the `setacl` command:

```
[[alice@compute-123 ~]$ setfacl -d -m user:bob:rwx shared
[alice@compute-123 ]$ getfacl shared
# file: shared
# owner: alice
# group: users
user::rwx
user:bob:rwx
group::r-x
mask::rwx
other::r-x
default:user::rwx
default:user:bob:rwx
default:group::--x
default:mask::rwx
default:other::r-x
If you want to remove and ACL, you can use the “-x” option to setfacl.

[alice@compute-123 ]$ setfacl -x user:bob shared
[alice@compute-123 ]$ getfacl shared
# file: shared
# owner: alice
# group: users
user::rwx
group::r-x
mask::r-x
other::r-x
```
