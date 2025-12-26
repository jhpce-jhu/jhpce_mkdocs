---
tags:
  - in-progress
  - jade
  - transition
  - dbgap
title: Differences between JHPCE and JADE for dbGaP users
---
# 20251225 - This page is under heavy revision

# Guide to differences between JHPCE and JADE

NIH has mandated that dbGaP data be protected by computers configured to meet
the stringent NIST 800-171 standard. This means that JADE is configured
differently from the main JHPCE cluster.  We attempt to identify differences here.

Click on a topic to read more about it (and click again to when finished to
close it). Three colors were used to indicate the likelihood of
that topic impacting the typical user.

!!! tip 
    * red, danger - You must deal with/know about
    * orange, warning - You probably should know about
    * light blue, info - Less impactful changes

!!! tip "Please take note of these path element symbols"

    In this document and others, we show the convention used for some aspect of
    the cluster, such as the path to expect for this or that purpose.

    ***&lt;dua&gt;*** = for example, 17293 or 15209<br>
    ***&lt;cdua&gt;*** = for example, d17293 or d41021<br>
    ***&lt;comm&gt;*** = for example, dbgap or cms<br>
    ***&lt;comm_letter&gt;*** = for example, d or c<br>
    ***&lt;username&gt;*** = for example, d-jli89-10201<br>
    <br>
    E.g. `/users/<comm>/<cdua>/<username>/`<br>
         `/users/dbgap/d17293/wshi13-17293/`

??? info "User Communities on JADE"
    {==Researchers who use dbGaP data are members of the "DBGAP community" on JADE.==}

    There are three communities (so far) in JADE. Each one has a unique name
    and a single character equivalent. You will see both appear in cluster
    configuration, such as the first letter of a username (c-jhedid-dua), 
    in paths to files (/transfer/in/cms/) and in names of SLURM partitions
    (d-shared).

    ```Shell
    Character       Name
    ----------      ---------- 
    c               cms
    d               dbgap
    s               sysadmin
    ```

??? danger "Login is only allowed from SAFE Desktop"
    {==dbGaP users must log into the SAFE Desktop before they can log into JADE.==}

    One must be connected to a Hopkins network to log into SAFE.

??? danger "Login requires OTP; ssh keys are not allowed"
    {==dbGaP users must use One Time Passwords to log into JADE.==}

??? danger "Login node"

    {==You must change your settings or commands to use the JADE login server.==}

    JADE:	*jade01.jhsph.edu*
    
    === "Windows users"
        If you are on a Windows system, you will need to create new MobaXterm
	SSH and SFTP Sessions for the jade01.jhsph.edu login node.
        
    === "Mac & Linux users"
        You will need to adjust your ssh and sftp commands to use
	*jade01.jhsph.edu* instead of either jhpce01.jhsph.edu or
	jhpce03.jhsph.edu.
        
??? danger "New accounts are used to access JADE"
    {==New usernames, verification codes, and passwords are used.==}

    You will receive new login credentials when your account is configured.

<!-- ------------------------------------------------------------------------->
??? info "Unix group changes"
    {==JADE uses completely separate groups to control data access.==}

    Each JADE user has a 'primary' group based on their community (`j-d-users`)
    and one or more 'secondary' groups (e.g. `j-d17293`).

<!-- ------------------------------------------------------------------------->
??? warning "SLURM partition name changes"
    {==If your batch job scripts specified the name of the partition to use, you
    will need to modify them so they work on JADE.==}

    We have a document explaining how to replace strings in documents
    [here](sed-tips-jade.md).

    In JHPCE, the default partition was named "shared". There were PI specific
    partitions.

    In JADE, each community now has its own default partition. The DBGAP default is
    named "d-shared". There currently are no PI owned partitions.

    When creating dbgap home directories, we have added to your home directory
    the file `~/.slurm/defaults`. It contains a line specifying "d-shared" as
    the default partition.

<!-- ------------------------------------------------------------------------->
??? danger "SFTP is used data transfers"
    {== LEFT OFF EDITING HERE ==}
    {==JADE uses specific TCP/IP networking ports for data ingress & egress: ==}<br><br>
    Data going into the cluster: 22511<br>
    Data coming out of the cluster: 22527

    There are two ways you work with this new path - when inside an SFTP
    transaction and when you are accessing files in the operating system.

    === "During SFTP"
        When you connect with SFTP to JADE, the SFTP server program places you
        inside the directory appropriate for your community. You can only see
        files below that point.

        So you "start from" either `/transfer/in/cms/` or `/transfer/out/cms/`

        To reach your own files, you need to cd into your group's directory, then
        into your own personal directory. 

        For example: `cd c55548/c-jxu123-55548`

        If you use the MobaXterm SFTP program, there is a place in the session's
        settings where you can specify your directory. If you configure that
        once, you will automatically "land" in your final destination without
        having to issue any cd commands every time.

    === "While in the operating system"
        After you transfer files into JADE, you need to use a different path to
        access them, whether from your shell prompt or the graphic file manager `thunar`.
        
        On the C-SUB, you used `/cms01/incoming/<username>`
        On JADE, you will use `/transfer/in/cms/<cdua>/<username>`

<!-- ------------------------------------------------------------------------->
??? danger "Data moderation process - differs only for moderators"

    {==Users do not need to change their procedure==}<br>
    {==The data moderation (DM) team needs to use two new paths - where you find
    proposed files and where you put them after approving them.==}
    
    We cannot create symbolic links to hide this change.
    
    Data moderators: After you have reviewed the files users have proposed for
    egress, you copy those files into the user-specific directory where they can
    access them from outside.

    On the C-SUB, you {==found user files to review in==} `/users/<dua>/<username>/proposed/`<br>
    On JADE, you will find user files to review in `/proposed/cms/<cdua>/<username>/`<br>


    On the C-SUB, you {==placed approved files into==} `/cms01/outgoing/<username>/`<br>
    On JADE, you will use `/transfer/out/cms/<cdua>/<username>/`<br>

    {==The data moderation process:==}

    1. Users create their files
    1. Users copy the ones that they want to export out of JADE to a proposed
    directory in their home directory, as they did before. However, in JADE we
    have replaced that actual directory with a symbolic link pointing to the
    actual location: `/proposed/cms/<cdua>/<username>/` Users can just work within
    `~/proposed/` as long as they don’t delete the link.  (DM reviewers cannot use
    the “proposed” symbolic links, e.g. they cannot say “cd ~c-tbrow261-55548/proposed”.
    They cannot do that because, in JADE, home directories are unreadable by anyone
    except the user. Which is the way it should be.)
    1. Users contact DM reviewer team asking for review
    1. DM reviewer (who is a member of the `j-c10401` group) changes directory to the
    "/proposed" directory for that user, and does what they do. 
    1. DM reviewer copies the approved files to `/transfer/out/cms/<cdua>/<username>/`
    1. DM reviewer notifies user
    1. User, from outside of JADE, `sftp -P 22027 <username>@jade01.jhsph.edu`
    1. User issues command “cd <cdua>/<username>” and “get file1”
    1. User quits sftp session.
    1. Files older than 30 days are automatically deleted from `/transfer/out/cms/<cdua>/<username>/`
    1. It is up to users to clean up their `~/proposed/` files.

<!-- ------------------------------------------------------------------------->
??? info "Mailing lists"
    {==The mailing list to reach all CMS users in a cluster has changed.==}

    C-SUB: c-sub-users@lists.jh.edu<br>
    JADE: jade-cms-users@lists.jh.edu

<!-- ------------------------------------------------------------------------->
??? info "Operating system has been updated"
    {==This change should be invisible. But you may run into version dependencies.==}

    Both clusters use the Rocky 9.x operating system. The C-SUB uses 9.4 while
    JADE uses 9.6.  The Rocky OS is designed to be stable, so dot releases
    (e.g. .4 to .6) within major releases (e.g. 8.x vs 9.x) add mainly bug
    fixes. However, they also incorporate some upgrades to components, such as
    Python going from 3.9.18 to 3.9.21.

    Please notify the systems administration team at bitsupport@lists.jh.edu of
    issues you find of this type. We will seek workarounds and be better able to
    help others. Thanks.

??? info "LibreOffice program names have changed"
    {==Program names have changed slightly==}

    The graphic user interface [LibreOffice](https://www.libreoffice.org) is a 
    free, open-source office suite.  JADE is using a much newer version than the C-SUB.  

    Many LibreOffice programs have a single letter prefix. In C-SUB that was 'o'
    while in JADE it has changed to be 's'.  "ooffice" is now "soffice", "ocalc"
    is now "scalc"

    These LibreOffice programs are found in the /user/local/bin/ directory
    (which is in your default PATH). You can see a list of them with the
    command<br>
    `ls -l /usr/local/bin/ | grep -i libre`


# MATERIAL BELOW HAS NOT YET BEEN INCORPORATED OR DELETED YET

COMMUNITIES


Shorthand variables you will see used below:

`<dua#>` = alphanumeric string, typically containing only digits
  e.g. 55548
`<cdua> = <community-letter><dua#>`
  e.g. c55548
  NOTE the "c" in `<cdua>` does NOT mean "cms", it means "c"ommunity

`<username> = c-<jhedid>-<dua#> (for cms users)`
`<username> = d-<jhedid>-<dua#> (for dbgap users)`
`<username> = s-<jhedid>-<dua#> (for sysadmin users)`

LOGIN NODE
---------------------------------------------------------------------------
C-SUB:	jhpcecms01.jhsph.edu
JADE:	jade01.jhsph.edu

MAILING LISTS
---------------------------------------------------------------------------
mailing list is jade-cms-users rather than c-sub-users (@lists.jh.edu)

SLURM PARTITION
---------------------------------------------------------------------------
In the C-SUB, everyone shares a default partition named "cms"
In JADE, the default partition for the cms community is c-shared.

Brand new accounts created by my scripts will be given a ~/.slurm/defaults
file, the last line of which will be a community-specific default partition.
We'll need to Do The Right Thing as we copy over C-SUB home directories of
testers and then later migrated users. (First checking that no one has such
a file, then, if not, copying a default one into place.)

USERNAME
---------------------------------------------------------------------------
The userid remains the same for C-SUB users.

UNIX USER GROUPS
---------------------------------------------------------------------------
We need to add users to new groups.

Existing groups used in the C-SUB will still be defined, at least for a time.
I do not know whether having them around will introduce misleading testing
results or actual problems, during testing or after a DUA group has migrated
to JADE. This needs to be studied and made part of the recipe for copying
/cms01/data/dua/<dua#> over into JADE. 

Add users to:
j-c-users
This is the group which identifies members of the cms community within JADE
C-SUB: 	c-users	  meant basically "all normal human users in the cluster"
JADE:	j-c-users	means "cms people"

Add users to:
j-<cdua>
This is the group which identifies access to a DUA's files.
C-SUB: 	c-<dua#> e.g. c-55548
JADE:	j-<cdua> e.g. j-c55548

Add data moderators to
j-c-sftp-out
This group replaces "c-sftp-out" for data moderators
Data moderators have account names in the 10201 DUA, i.e. c-<jhedid>-10201


PATHS PATHS PATHS PATHS PATHS
PATHS PATHS PATHS PATHS PATHS
PATHS PATHS PATHS PATHS PATHS
---------------------------------------------------------------------------

HOME DIRECTORIES
------------------------------------------------------------
C-SUB:	/users/<dua#>/<username>
	/users/10101/c-jtuniso1-10101

JADE:	/users/<comm>/<cdua>/<username>
	/users/cms/c77777/c-test1-77777

I have created symbolic links for each of the DUAs, e.g.
	/users/10101 -> /users/cms/c10101/

That will accomodate references to home directories or per-dua shared
directories.

However, I think that any new CMS DUA's that come along should not have a
link created, because the other JADE communities will not have them at all.

If users want to refer to home directories, as a general practice it is best
to use $HOME or ~ 

If users want to refer to per-dua shared/ directories, they should use the new
correct path.

INCOMING SFTP DATA
------------------------------------------------------------
The port for incoming CMS data, TCP/IP port 22011, will remain unchanged.

Data will now land in a new path.
C-SUB: 	/cms01/incoming/<userid>/
JADE:	/transfer/in/<comm>/<cdua>/<userid>

OUTGOING SFTP DATA (AFTER BEING APPROVED FOR EGRESS)
------------------------------------------------------------
The port for outgoing CMS data, TCP/IP port 22027, will remain unchanged.

Data will now be available, after data review, in a new path.
C-SUB: 	/cms01/outgoing/<userid>/
JADE:	/transfer/out/<comm>/<cdua>/<userid>

PROPOSED OUTGOING SFTP DATA (BEFORE BEING APPROVED FOR EGRESS)
------------------------------------------------------------
Data needing data review, will go into a new path.
C-SUB: 	/users/<dua#>/<userid>/proposed
JADE:	/proposed/<comm>/<cdua>/<userid>

This is being done so user home directories can be simpler and also be
standard across all communities.

20251013: I have NOT finished tweaking everything for this to work.

DUA DATA
------------------------------------------------------------
DUA data has been relocated to /data
We are still working out the details of what part of paths under
/data/ represent mount points.

C-SUB:	/cms01/data/dua/<dua#>	
JADE:	/data/cms/<cdua>/cui/
	/data/cms/<cdua>/cui/intermediate/
	/data/cms/<cdua>/cui-intermediate/

Why cui/intermediate/ ? We want groups to be able to keep CUI out of
/users/ as much as possible. Creating a default location helps keep that
in mind. When DUAs expire, the CUI is supposed to be deleted. If people
have a place to put their "near-raw" data then it will make it easier
for the group later to review files before deletion. Some users will may
have left -- if they put material in the right locations, then it makes it
easier for the PI or power users appointed by the PI as data stewards.

	/data/cms/<cdua>/shared/

Why shared?
The c57285 group wanted a group-writable location adjacent to their CUI
(the "raw" data from CMS) where they could store large amounts of data

(/cms01/data/dua/57285/ is stored on one of our spinning-hard-drive
servers, which is cheaper, slower but more capacious. The /users/<dua#>/shared
directories I created come from our SSD-equipped server, which is fast but
does not have as much space.)

In JADE, /users/<comm>/<cdua>/shared is stored on our SSD-equipped server.

"Shared" provides a directory under which groups can choose what they want
place there. If subdirectories are well organized, we can create whole new
file systems if their current one fills up, or if PIs want to pay extra to
create a file system on our SSD-equipped server.

We could to create some standard items under shared/, such as
	/data/cms/<cdua>/shared/<userid>
	/data/cms/<cdua>/shared/tools/
	/data/cms/<cdua>/shared/refdata/
SUBDIRECTORIES ARE REQUIRED IF THE GROUP wants to have different permissions
on different types of material !!!!!!
