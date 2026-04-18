# Techniques for managing project allocation access

## Overview
Primary investigators pay for the creation of data-containing file systems (aka `FS`) called "allocations". These FS are stored on one of our DCS (Dirt Cheap Storage) servers, whose hostnames begin with 'dcs' and end in a number, such as 'dcs07' or 'dcs10' The file path to an allocation have the generic form 

`/<server_name>/<pi_or_project>/data/` e.g. `/dcs07/scharpf/data/`

We have a detailed document about [sharing files](../files/sharing-files.md) the files and directories. It is critical that you are familiar with that material. You can also look up other descriptions of the same concepts with an internet search.

## Consider gateway groups
A key objective of this document is to alert PIs creating or managing allocations to the potential downsides of using the default arrangement.  If they anticipate, or simply want to be prepared for, working with multiple groups or subgroups of users inside their allocation, it is best to request a "gateway group" be created.

## The default set up

Access to the allocation's files is controlled by a UNIX users group being able to read and optionally write them. Individual files or directories (aka "objects") can be made private to the owner, readable by the group, etc by changing the permissions and group ownership after object creation, and/or by setting the user's `umask` value beforehand.

When creating an allocation the PI will tell us what group name to use and which users should be members.

```shellsession
jhpce01% ls -ld /dcs10/bob/data`
drwxrws--- 3 bob bobgrp 3 Nov 10  2025 /dcs10/bob/data
```
!!! Note "What these settings portray"
    
    + A PI whose username is 'bob' is the owner of the top level
    + A group named 'bobgrp' is the group owner of the top level
    + Members of 'bobgrp' can read and write the directory, meaning they can create, modify and delete objects inside it.
    + If bob wants to control the next level of objects (those inside "data") then a more appropriate setting would be 'r-s'. This prevents users from modifying things at that level.
    + The group's permission bits (rws) have an 's' where normally you would see an 'x'. 'x' means that users can pass into the directory. An 's' implies 'x' but adds what is called "group sticky" functionality, which is that new file and directory objects created inside are given the same group ownership as is found on that data directory.
    + A directory can be made "sticky" by setting the group ownership to the correct value, then issuing `chmod g+s <dir>`
    + The "other" permission bits (---) means that people who are not bob or members of bobgrp cannot see or pass into the directory.

This might be the contents after some actions by users bob, jane and frank. Note the variety of owners and permissions and what that means for people sharing files.

```shellsession
jhpce01% ls -l /dcs10/bob/data`
drwxrws--- 3 bob bobgrp 3 Nov 10  2025 /dcs10/bob/data
drwxrws--- 3 bob bobgrp 3 Dec 15  2025 /dcs10/bob/data/dir1/
drwx------ 3 bob users  3 Dec 15  2025 /dcs10/bob/data/mydir/
drwxr-s--- 3 jane bobgrp 3 Nov 12  2025 /dcs10/bob/data/dir2/
-rwx--s--- 3 frank bobgrp 3 Dec 20  2025 /dcs10/bob/data/file10
```

## Multiple projects sharing an allocation

### The problem
In the default set up, there is a single group involved (bobgrp). Users must be bobgrp members to access **anything** in the allocation. If only a subset of bobgrp should be able to access an object the only option is to define Access Control Lists. We describe ACL's and their usage [here](../files/acl.md). ACL's can be tricky to manage.

What happens when the PI starts a new project "genes", wants to use their existing allocation, but "genes" researchers should not be able to access the material owned by "bobgrp"?

The only solution is to reorganize the allocation's lay out. Which, if paths to the objects have been embedded in, say, sbatch shell scripts, means those have to be modified. There may be years and years of references to consider accomodating.

In some cases symbolic links can be used to allow previous paths to continue functioning but with new directories with different permissions/ownsership.

### The solution - gateway groups

If such scenarios are anticipated, we recommend requesting that we create a **"gateway group"** which allows access **into** the allocation, and then one or more secondary groups which are used to control access to the objects at levels inside `/dcs10/bob/data/`

It does mean that people have to be managed in two groups. But it lays down the foundation for an extensible data structure.
 
We can make a PI or some list of people they designate to be “group administrators” who can manage the membership of any particular group. Consult our [instructions for using freeipa](../sw/freeipa.md) to add or remove group members.

In this example there is only one group of users but the PI is prepared to work with other groups as they come along.

The gateway group is "bobgrp" and at first there is only one subordinate group "bob_atoms". **The same users are members of both groups**, but the objects inside of `/dcs10/bob/data/` meant to be used by the team **are group-owned by "bob_atoms".**

```shellsession
jhpce01% ls -l /dcs10/bob/data`
drwxrwx--- 3 bob bobgrp 3 Nov 10  2025 /dcs10/bob/data
drwxr-s--- 3 bob bobgrp 4 Dec 15  2025 /dcs10/bob/data/tools/
drwxrws--- 3 bob bob_atoms 5 Dec 18  2025 /dcs10/bob/data/atoms/
drwx------ 3 bob users 2 Dec 15  2025 /dcs10/bob/data/mydir/
drwxr-s--- 3 jane bob_atoms 23 Nov 12  2025 /dcs10/bob/data/dir2/
-rwx------ 3 frank users 3 Dec 20  2025 /dcs10/bob/data/file10
```
!!! Note "What these settings portray"
    
    + "data" is no longer group sticky. Objects will be created using the user's primary group, which is here "users". Objects meant to be shared with a group must be manually changed from "users" to the new group with `chgrp [-R] bob_atoms` or `chgrp [-R] bobgrp` (only add the "-R" recursive flag when appropriate).
    + "data" could be left group sticky. Changes in group ownership will still be needed where appropriate.
    + "atoms" is perhaps the "parent" directory for the researchers work
    + "file10" is private to frank
    + "mydir/" is private to bob
    + "dir2" is accessible by bob_atoms but jane must create writable subdirectories if desired. Jane had to use chown to set the group ownership.
    + "tools" is perhaps a location for all of the future researcher groups' members to find a common set of code or reference data

Later on, a new "genes" project comes along, with some researchers being in "bob_atoms" and some not. In that case, the gateway group is "bobgrp" and there are two subordinate groups "bob_atoms" and "bob_genes"


