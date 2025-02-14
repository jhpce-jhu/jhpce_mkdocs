---
tags:
  - in-progress
  - jeffrey
---
# ACL - Access Control Lists

!!! Tip
    Before defining ACLs, you should first read [our document about sharing files](..//files/sharing-files.md) for necessary background concepts and skills.

Traditional Unix file and group permissions can be used to share
access to files and directories.  However, there can be times when
more fine-grained control of shared access is needed. To accomplish
this, Access Control Lists (ACLs) can be used. They *add* to the normal permissions, not replace them. ACLs and permissions can interact in unexpected ways.

??? "ZFS versus Lustre: If you are working in /dcl02/ click here..."
    JHPCE uses two kinds of file systems on its large storage servers: **ZFS** and **Lustre**. A different pair of ACL commands is used for each type.

    As of March 2024, the only Lustre file systems are those which begin with the path `/dcl02/` **For these file systems, use getfacl and setfacl.**
    
    For *everything else*, use **nfs4_getfacl** and **nfs4_setfacl**. (All of the files originally found on the Lustre file server named DCL01 have been copied off to live on other, ZFS-using file servers. But the name /dcl01 has been preserved, for convenience.)

    Instructions for Lustre file systems are found [later in this document](../files/acl.md#acls-in-lustre-file-systems).

## **ACL Commands**

An Access Control List is a series of ***Access Control Entry*** (ACE) rules associated with a file or directory.

These rules are displayed and manipulated by two commands:

**nfs4_getfacl**
: Display the ACL of a file or directory.

**nfs4_setfacl**
: Add, remove or modify ACE entries for an ACL on a file or directory. The main arguments you will use:

* **-a** - Add an ACE
* **-x** - Remove an ACE (must exactly match the ACE to work)
* **-e** - Enter an editor to edit all of the ACEs in the ACL (don't use unless you have defined the environment variable EDITOR[^1])
* **--test** - Display the results of applying, but do not actually change
* **-R** - Appy the command recursively[^2]

[^1]: The editor `vi` will be used unless you set a variable to specify a different one, e.g. `export EDITOR /usr/bin/nanon` (which you can add to your ~/.bashrc file for the future). Many people don't know how to use `vi` If you need to quit from vi, use these keystrokes in order: ++escape++ ++colon++ q ++enter++

[^2]: You may not want to apply some ACEs recursively, because files and directories can need different rules. See [this section](#applying-acls-recursively) for a technique to apply changes to only files or only directories.

## **Access Control Entry (ACE)**

An ***ACE***, or ***Access Control Entry***, is a single control statement, indicating the access of a specific entity (usually a user or group). Thus, an Access Control List (ACL) is a list of ACEs. We here discuss some simple and common options for an ACE, but for a full description see the nfs4_acl(5) man page. A [later section](#ace-flags-explained) of this page gives the meaning for each of the single characters that are combined in ACEs to create cryptic strings like "rwaDxtTcCy"

We will begin with the structure of an ACE:

```
access type:[flags]:principal:permissions
```

All parts are required for every operation, though the `[flags]` section may be empty. Therefore all of your ACEs will always contain three colons.

**access type**
: A (for Allow) (A D (for Deny) can be specified, but it is not needed & leads to trouble.)

**flags**
: **g** - If present, this means that this ACE applies to group principals, not user principals.
: **fdi** - If present, this set of flags mean that the ACE is to be inherited.

**principal**
: The principal is the *entity* to which the ACE applies.
: A key thing to know is that JHPCE uses a *Kerberos* database to store user and group information, and that our *NFSv4* (Network File System, version 4) file systems look to that Kerberos database for ownership and permission information. Kerberos databases can hold information for one or more *domains*. JHPCE only contains one domain and that is "cm.cluster"
: **entity@domain** – Therefore our user and group principals have the form of **name@cm.cluster** If you fail to provide the full name for a principal, your ACL commands will fail.
: **OWNER@** – This special principal refers to the owner of the file/directory. It must always be present. This means every file must have an @OWNER ACE in its ACL.
: **GROUP@** – This special principal refers to the default group of a file. It must always be present. This means every file must have an GROUP@ ACE in its ACL.
: **EVERYONE@** – This is a special catch-all principal which applies to any entity that is not matched by any of the above. Think of this as equivalent to “other” (aka “world”) in the traditional UNIX/Linux permissions model.

**permissions**
!!! Note
    Needs to be written

## **Important Big-Picture Notes and Suggestions**

### ACL Facts

+ ACLs can be used on both files and directories. As with normal permissions, settings on files can have different impacts than those on directories (e.g. the X execute bit or default inheritance settings).
+ You can tell that a file or directory has an ACL defined by the presence of a ++plus++ symbol in the output of `ls -l` or `ls -ld`
+ Every file or directory appears to have an ACL if inspected with `nfs4+getfacl`.  This is just a representation of the basic UNIX ownership and permissions. When we speak in this document about adding an ACL we're talking about adding an ACE to those default values.
+ Making changes with normal commands like `chmod` and `chown` will impact what you see if you inspect the ACL afterwards.
+ If you want to change something about the special principals `OWNER@`, `GROUP@`, and `EVERYONE@`you probably want to do that with normal commands, not `nfs4_setfacl`
+ {==User's *umask* settings impact the permissions assigned to files and directories being created==} whether you are using traditional UNIX permissions or ACLs.
+ Setting an ACL on a directory does not change existing files and directories inside it unless you use the `-R` recursive option to the ACL command.
+  For users to be able to work with a file stored several layer deep in the directory structure, they must be able _to get to it_. {==That requires that they need sufficient permissions, via either normal UNIX permissions or ACLs (or both), to "read" and "execute" (aka "search") **all of the directories above the final file or directory**==}. For example, if you are wanting to enable access to the directory `/dcs07/bob/data/project1/shared`, you would need to provide `READ-EXECUTE` access on `/dcs07/bob/data/project1`, `/dcs07/bob/data`, and `/dcs07/bob`. {==Use UNIX permissions where appropriate, and ACLs where necessary.==}
+ ACLs can be set on files and directories located in file systems of types other than [NFS](https://en.wikipedia.org/wiki/Network_File_System). That is not what JHPCE users do. Trying to use an nfs4_*acl command in somewhere like /tmp will throw off an error like this: `Operation to request attribute not supported: test1`


### Suggestions

+ **Use normal UNIX permissions where possible.** ACLs can be complex to manage. Use ACLs to *extend* normal permissions.
+ ACLs should use the security notion of “least privilege”, meaning that ACLs should give only the needed access and nothing more.
+ By default if an ACL does not allow something, that permission is denied. ACEs that start with an "A" are "Allow ACEs", those that start with a "D" are "Deny ACEs". Avoid using "Deny ACEs" -- they increase the chances of something not working as expected.
+ Like any operation, commands modifying ACL commands on large numbers of files should be run on a compute node and not a login node. Long-running recursive ACL commands on large directory trees may be done via interactive sessions or submitted batch job scripts.
+ Check the values of files and directories before, during, and after changing their permissions or defining ACLs. Use both `ls -l` and `nfs4_getfacl` to get the full picture. Test what happens afterwards. Don't change many files at once until you are confident that your commands are correct. You can capture the original configuration using commands like `ls -lR directoryname > saved-listing.txt` for normal UNIX permissions and `nfs4_getfacl --recursive directoryname > saved-acls.txt`

### Default or inherited ACLs on Directories
+ "Default" ACLs can be set on a directory, and this ACL will be _inherited_ into the directory structure as new files and subdirectories are created. You might need to create different "default" ACLs to control the creation of new files and others to control the creation of new subdirectories.
+ With "default" or "inherited" ACLs, the individual user’s `umask` setting is important in assuring that future new files and directories being created have the correct permissions set. The `umask` setting will take precedence over the ACL, so *it must be more permissive than the ACL*. For example, if you want to set a default group ACL where the group has write access, you need to make sure that your umask is set to `0002` rather than `0022`, as the “2” in the group umask bit will prevent the group write capability.

## **ACLs in ZFS File Systems**

### Basic commands
There are 2 commands for dealing with ACLs in file systems like `/users`, `/dcl01`, `/dcs04`, `/dcs05`, `/dcs06`, and `/dcs07`. 

+ `nfs4_getfacl` - display current ACL setting
+ `nfs4_setfacl` - modify ACLs.  

With ACLs you can grant different kinds of access on directories or files to specific users and groups, in addition to the normal group associated with the object, and in addition to "other".

The simplest permissions to use in ACLs are R for read access, W for write access, and X for execute and directory access. 

A [later section](#ace-flags-explained) of this page gives the meaning for each of the single characters that are combined in ACEs to create cryptic strings like "rwaDxtTcCy"

These strings contain so many elements because ACLs can be used to create fine-grained permissions.

By default, one’s home directory is only accessible to the owner. This is accomplished with basic UNIX permissions, not ACLs.[^1] The original ACL reflects this. For the user alice, the ACL on their home directory would look like:

[^1]: Not true in the C-SUB, where home directories are protected with ACLs.

```Shell
[alice@compute-123 ~]$ pwd
/users/alice
[alice@compute-123 ~]$ ls -ld .
drwx------ 63 alice users 134 Apr 12 14:36 ./
[alice@compute-123 ~]$ nfs4_getfacl .
# file: .
A::OWNER@:rwaDxtTcCy
A::GROUP@:tcy
A::EVERYONE@:tcy

```

If you see the string "tcy", that means "no access".

If you see the string "fdi", that indicates that the ACE is a "default" or "inherited" one.

### **Read permisions**

+ Now, if alice wanted to grant read-only access to their home
  directory to the user bob, they would use the `nfs4_setfacl`
  command:

```Shell
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

```Shell
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

{==One issue we’ve seen is that, in addition to the ACE you would expect was needed, you need to also add both a `@USER` and `@GROUP` ACE need to be explicitly set for permissions to be work as expected. ==}

```Shell
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

```Shell
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


Example showing the impact of adding a default (or inherited) ACE to a directory which provides read, write, and execute permissions for the UNIX group "hpscc"  Then, depending on the umask in force, files created inside this directory will be created with different permissions.

```Shell
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

### **Removing an ACL**
Use the `-x` option to `nfs4_setfacl` to remove an ACE from an ACL. Please note that you need to use the full (and exact) ACE, and not the `RWX` shortcuts.

```Shell
[alice@compute-123 ~]$ nfs4_setfacl -x A::bob@cm.cluster:rwaxtcy shared
[alice@compute-123 ~]$ nfs4_getfacl shared
nfs4_getfacl shared-data.txt
# file: shared-data.txt
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
```
### **Applying ACLs Recursively**

You can use the `-R` flag to `nfs4_setfacl` to apply your changes recursively.

Sometimes you want to set a rule only on files or on directories. You can use the `find` command to generate lists of each type of object which are then passed to the `xargs` command, which runs the command it is given on each object. By default `find` starts at the path you give it and, if it is a directory, descends into it recursively.  Note that this example adds arguements to `find` and `xargs` which handle the case where files or directory names contain evil, no-good, awful characters like white space, quote marks, or backslashes:

```Shell
cd into-the-top-of-the-directory-tree
find . -type d -print0 | xargs --null nfs4_setfacl -a A::bob@cm.cluster:RWX
find . -type f -print0 | xargs --null nfs4_setfacl -a A::bob@cm.cluster:R
```

### **Duplicating Existing ACLs**

!!! Caution "This section under development"
    The following information needs to be revised and reviewed. It is provided in case it is helpful until then.

You can copy working ACLs but you must be very careful and check the results. Did your umask contribute an unexpected change to the process?

One technique is to save and restore the settings. The second example uses input/output redirection to connect the two commands.

```Shell
nfs4_getfacl -R source_dir > tmpfile1
setfacl --restore tmpfile1 destination_dir

nfs4_getfacl -R source_dir |
setfacl --restore - destination_dir
```

You can copy the ACLs on a directory with rsync. You **have to** put trailing slashes on BOTH the source and destination.

`rsync -A public-exam1/ /dcs05/somegroup/data/otherdir/`

While -Ar preserves ACLs, it doesn't preserves ownerships and permissions. Use -Arogp (o=owner, g=group, p=permissions) or shorter and more common -Ara (a=archive) for a better copy of your directory.

This [page](https://portal.perforce.com/s/article/12405) has example scripts for saving and restoring ACLs on many files at once.

This [page](https://stackoverflow.com/questions/3450250/is-it-possible-to-create-a-script-to-save-and-restore-permissions) has different advice for the same goal.

## **ACE Flags Explained**

The ACL on a normal directory looks like this:

```
# file: .
A::OWNER@:rwaDxtTcCy
A::GROUP@:tcy
A::EVERYONE@:tcy
```

What does "tcy" mean? How about "rwaDxtTcCy"?  This section contains the answer, with flags grouped by common occurrence. Copied from [this page](https://www.osc.edu/book/export/html/4523).

### ALIAS FLAGS

Aliases such as 'R', 'W', and 'X' can be used as permissions. These work simlarly to POSIX Read/Write/Execute. 

|Flag|Name|Function|
|----|----|--------|
|R | Read | rntcy | 
|W | Write | watTNcCy (with D added to directory ACE's) |
|X | Execute | xtcy |



### INHERITENCE FLAGS

You will see "fdi" groups of flags in "default" ACLs

|Flag|Name|Function|
|----|----|--------|
f	| file-inherit	| New files will have the same ACE minus the inheritence flags 
d	| directory-inherit	| New subdirectories will have the same ACE
i	| inherit-only | New files and subdirectories will have this ACE but the ACE for the directory with the flag is null
n	| no-propogate inherit	| New subdirectories will inherit the ACE minus the inheritence flags

### NORMAL FLAGS

|Flag|Name|Function|
|----|----|--------|
r	| read-data (files) / list-directory (directories) | Can you read the object's contents?
w	| write-data (files) / create-file (directories) | 
a	| append-data (files) / create-subdirectory (directories)
x	| execute (files) / pass-through-directory (directories)| You don't have to read a directory to cd *through* it into a subdirectory |
d	| delete the file/directory ||
D	| delete-child : remove a file or subdirectory from the given directory (directories only)
t	| read the attributes of the file/directory ||
T	| write the attribute of the file/directory ||
n	| read the named attributes of the file/directory ||
N	| write the named attributes of the file/directory ||
c	| read the file/directory ACL |
C	| write the file/directory ACL | Danger, Will Robinson! |
o	| change ownership of the file/directory | Danger, Will Robinson! |

## ACLs in Lustre file systems 

Currently, the only Lustre file system we have is `/dcl02`. So if your files are located anywhere else, you can ignore this section.

There are two commands for dealing with ACLs on the JHPCE cluster for
directories in Lustre file systems, which start with `/dcl02`.

The `getfacl` command will display
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
```

Now the user bob can copy or save files in the “shared” directory.

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
```

If you want to remove and ACL, you can use the “-x” option to setfacl.

```
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
****
