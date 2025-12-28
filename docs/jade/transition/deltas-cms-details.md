---
tags:
  - jade
  - transition
  - cms
title: Guide to differences between C-SUB and JADE for CMS users
---
# Guide to differences between C-SUB and JADE for CMS users
You will find working in JADE very similar to doing so in the C-SUB.

However, there are differences, and we attempt to identify them here.
Click on a topic to read more about it (and click again when finished to
close it). Three colors were used to indicate the likelihood of
that topic impacting the typical user.

{==Some items are marked red but only require a one word change in your command
or your changing directory to a different location to retrieve your incoming files.
You will very quickly master the changes.==}

## Resources for you

We are trying to create documentation to put in the JADE section of this web site
as quickly as we can. Please bear with us.  Because paths to files change in
JADE, there are multiple documents about that which attempt to present similar
information but in different layouts. Here are the key documents:

* This document contains vital information.
* Instructions on [changing existing text files](sed-tips-jade.md) to replace C-SUB-appropriate
paths with JADE-appropriate paths.
* [Summary of JADE paths & their purposes](../images/jade-path-summary-table.pdf) - one page PDF
chart. A reference document for all JADE users for the long run. Contains summary of what kind of data
is best for each location. Read this before the next document below.
* [Summary of path changes for CMS users](images/jade-path-summary-table-cms.pdf) - one page PDF chart. Side-by-side comparison of C-SUB & JADE. Mentions accommodations to hide most path changes.
* [Description of data locations](images/paths-csub-to-jade.pdf), what to use
them for, and specific changes users need to make. Only describes paths. Seven pages of text
(because lots of white space was inserted for readability). Was written first.

<!-- ------------------------------------------------------------------------->
!!! tip "Color codes used for each topic"
    * red, danger - You must deal with/know about
    * orange, warning - You probably should know about
    * light blue, info - Less impactful changes

<!-- ------------------------------------------------------------------------->
??? tip "Please take note of these path element symbols"

    In this document and others, we show the convention used for some aspect of
    the cluster, such as the path to expect for this or that purpose.

    ***&lt;dua&gt;*** = for example, 59614 or 55548<br>
    ***&lt;cdua&gt;*** = for example, c59614 or c55548<br>
    ***&lt;comm&gt;*** = for example, cms or dbgap<br>
    ***&lt;comm_letter&gt;*** = for example, c or d<br>
    ***&lt;username&gt;*** = for example, c-jli89-10401<br>
    <br>
    E.g. `/transfer/in/<comm>/<cdua>/<username>/`<br>
         `/transfer/in/cms/c55548/c-jxu123-55548

<!-- ------------------------------------------------------------------------->
??? info "User Communities on JADE"
    {==If you had been using the C-SUB, you are a member of the "CMS community" on JADE.==}

    There are three communities (so far) in JADE. Each one has a unique name
    and a single character equivalent. You will see both appear in cluster
    configuration, such as the first letter of a username (c-jhedid-dua), 
    in paths to files (/transfer/in/cms/) and in names of SLURM partitions
    (c-shared, c-sas).

    ```Shell
    Character       Name
    ----------      ---------- 
    c               cms
    d               dbgap
    s               sysadmin
    ```
<!-- ------------------------------------------------------------------------->
??? info "Unchanged: Usernames, verification codes, and passwords (except 10201)"

    {==Most C-SUB users will continue using their usernames, verification codes,
    and passwords with JADE.==}

    C-SUB users with 10201 in their usernames will switch to new 10401 accounts.
    For each such user, systems administrators will make a copy of the C-SUB
    10201 home directory and place it into the JADE 10401 home directory with
    the name `csub-homedir`.

    10401 users can then look through `csub-homedir`, move files out of it they want
    to keep, and then delete the rest if they want to save disk space. (Some
    directories like ~/.cache/ can contain large amounts of material.)
<!-- ------------------------------------------------------------------------->
??? danger "Login node"
    {==You must change your settings or commands to use the JADE login server.==}

    JADE:   **jade01.jhsph.edu**<br>
    
    === "Windows users"
        If you are on a Windows system, you will need to create new MobaXterm SSH and SFTP "Sessions" for the jade01.jhsph.edu login node. There are notes for doing this on page 18 of the [C-SUB orientation](../../orient/images/latest-csub-orient.pdf), (though you would substitute **jade01.jhpce.edu** for jhpcecms01.jhsph.edu).
        
    === "Mac & Linux users"
        You will need to adjust your ssh and sftp commands to use **jade01.jhsph.edu** instead of jhpcecms01.jhsph.edu.
<!-- ------------------------------------------------------------------------->
??? danger "Paths have changed"
    {==A variety of paths have changed. Please review the details.==}

    The need to more carefully control data access in JADE, as well as
    reflection upon the design lessons learned from the C-SUB implementation, has
    resulted in a variety of path changes.  We have created symbolic links in
    various locations to allow paths that were valid in the C-SUB to continue
    to work in JADE.

    [This
    document](images/jade-path-summary-table-cms.pdf) is a one page summary table.

    [This document](images/paths-csub-to-jade.pdf) is longer, and provides details about each kind of data
    locations available in JADE. It describes changes which impact users and data
    moderators.

    We have a document explaining how to replace strings in documents
    [here](sed-tips-jade.md).
<!-- ------------------------------------------------------------------------->
??? warning "Compute node names have changed"
    {==If your batch job scripts specify the name of one or more compute nodes 
    to use, you will need to modify them so they work on JADE.==}

    Compute nodes in C-SUB had the prefix "compute-" and a suffix in the range
    132 to 139.
    
    Compute nodes in JADE have the prefix **"jcompute-"** and a suffix in the range
    032 and higher.

    There is a direct mapping of C-SUB compute-132 to JADE jcompute-032, etc.
    Add a "j" and change "1" to "0".
<!-- ------------------------------------------------------------------------->
??? info "Unix group changes"
    {==This change should be invisible.==}

    Each C-SUB user has a 'primary' group and one or more 'secondary' groups.
    When users are migrated to JADE, we will change both primary & secondary
    group memberships. Your primary will become `j-c-users` and your secondary
    will match your account's DUA number, e.g. `j-c55548`.
<!-- ------------------------------------------------------------------------->
??? warning "SLURM partition name changes"
    {==If your batch job scripts specified the name of the partition to use, you
    will need to modify them so they work on JADE.==}

    In C-SUB, the default partition was named "cms" and the SAS one "sas".

    In JADE, each community now has its own default partition. The CMS default is
    named "c-shared". Also, "sas" was renamed to be "c-sas".

    If you do not specify a partition when submitting an interactive or batch
    job, then it is assigned to the cluster's default partition. Because the C-SUB
    contained very few SLURM partitions, we expect that very few batch jobs
    explicitly specified "cms".

    We have added a modification to your home directory to accomodate a change
    in the name of the CMS community's default partition. The file
    `~/.slurm/defaults` contains a line specifying "c-shared" as the default.

    The command `slurmpic -a` will show you all of the currently defined nodes
    and partition names. (`scontrol show node <node_name>` will show you all of
    the info about a particular node, and `scontrol show partition
    <partition_name>` will display the full partition definition.)

    We have a document explaining how to replace strings in documents
    [here](sed-tips-jade.md).
<!-- ------------------------------------------------------------------------->
??? warning "Contents of ~/proposed/ directory are not automatically propagated"
    {==If you have active files in ~/proposed/ you will need to copy them to a new location.==}

    (The tilde character '~' is a special character in UNIX shells. It represents
    your home directory.)

    * `~/proposed` -- In the C-SUB this was a directory. In JADE this is a symbolic link to the new location for this type of files. When your account is migrated to JADE, the original ~/proposed is renamed `~/proposed.csub` (you will not lose them!) and a symbolic link is made to point to `/proposed/cms/<cdua>/<username>`. You can continue to use `~/proposed` as if it were a directory, in terms of copying files into it or listing it. {==If you have files being reviewed by data moderators, then you should cp them from `~/proposed.csub/` to `~/proposed/`.==}
<!-- ------------------------------------------------------------------------->
??? danger "Incoming/Outgoing files will not be copied from C-SUB to JADE"
    {==Files found in /cms01/incoming/ and /cms01/outgoing/ are not going to be
    copied over to JADE.==}

    Please copy any of them that you want to keep into your
    home directory before your group is migrated from the C-SUB to JADE.
<!-- ------------------------------------------------------------------------->
??? danger "SFTP data transfers"
    {==JADE uses the same TCP/IP networking ports as was used on the C-SUB:==}<br><br>
    Data going into the cluster: 22011<br>
    Data coming out of the cluster: 22027

    JADE uses different paths to store this kind of data. 
    We cannot create symbolic links to hide this change.

    C-SUB

        Incoming   /cms01/incoming/<username>
        Outgoing   `/cms01/outgoing/<username>`

    JADE

        Incoming   /transfer/in/cms/<cdua>/<username>
        Outgoing   /transfer/out/cms/<cdua>/<username>

        Example:
        Incoming   /transfer/in/cms/c55548/c-jxu123-55548
        Outgoing   /transfer/out/cms/c55548/c-jxu123-55548

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
    {==Program names have changed slightly; much newer version is installed.==}

    The graphic user interface [LibreOffice](https://www.libreoffice.org) is a 
    free, open-source office suite.  JADE is using a much newer version than the C-SUB.  

    Many LibreOffice programs have a single letter prefix. In C-SUB that was 'o'
    while in JADE it has changed to be 's'.  "ooffice" is now "soffice", "ocalc"
    is now "scalc", etc.

    These LibreOffice programs are found in the /user/local/bin/ directory
    (which is in your default PATH). You can see a list of them with the
    command<br>
    `ls -l /usr/local/bin/ | grep -i libre`

