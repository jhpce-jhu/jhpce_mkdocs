---
icon: material/eye
tags:
  - in-progress
  - jade
  - transition
  - dbgap
title: Differences between JHPCE and JADE for dbGaP users
---
# Guide to differences between JHPCE and JADE for dbGaP users

NIH has mandated that dbGaP data be protected by computers configured to meet
the stringent NIST 800-171 standard. This means that JADE is configured
differently from the main JHPCE cluster.  We attempt to identify differences here.

Click on a topic to read more about it (and click again to when finished to
close it). Three colors were used to indicate the likelihood of
that topic impacting the typical user.

<!-- ------------------------------------------------------------------------->
!!! tip 
    * purple, important - You must deal with/know about
    * orange, warning - You probably should know about
    * light blue, info - Less impactful changes

<!-- ------------------------------------------------------------------------->
??? tip "Please take note of these path element symbols"

    In this document and others, we use the terms shown below when describing
    conventions about aspects of the cluster, such as the path to expect for this
    or that purpose.

    ***&lt;dua&gt;*** = for example, 17293 or 15209<br>
    ***&lt;cdua&gt;*** = for example, d17293 or d41021<br>
    ***&lt;comm&gt;*** = for example, dbgap or cms<br>
    ***&lt;comm_letter&gt;*** = for example, d or c<br>
    ***&lt;username&gt;*** = for example, d-jli89-10201<br>
    <br>
    E.g. `/users/<comm>/<cdua>/<username>/`<br>
         `/users/dbgap/d17293/d-wshi13-17293/`

<!-- ------------------------------------------------------------------------->
??? tip "User Communities on JADE"
    {==Researchers who use dbGaP data are members of the "DBGAP community" on JADE.==}

    There are three communities (so far) in JADE. Each one has a unique name
    and a single character equivalent. You will see both appear in cluster
    configuration, such as the first letter of a username (`c-jhedid-dua`),
    in paths to files (`/data/dbgap/`) and in names of SLURM partitions
    (`d-shared`).

    ```Shell
    Character       Name
    ----------      ---------- 
    c               cms
    d               dbgap
    s               sysadmin
    ```

<!-- ------------------------------------------------------------------------->
??? example "Login is only allowed from SAFE or SAFER Desktop"
    {==dbGaP users must log into the SAFE Desktop before they can log into JADE.==}

    SAFE and SAFER Desktop are Windows virtual machines which are described
    [here](https://researchit.jhu.edu/safer/).  NIST 800-171 data security
    controls require a variety of services provided by these environments.

    One must be connected to a Hopkins network to log into SAFE.
    (You can also log into JADE using the newer SAFER Desktop service.)

<!-- ------------------------------------------------------------------------->
??? example "Login requires MFA via OTP; ssh keys are not allowed"
    {==dbGaP users must use One Time Passwords to log into JADE.==}

    The local program `auth_util` allows you to work with your Google
    Authenticator One Time Password configuration.  You will use it to configure
    your smartphone or other OTP apps to generate a stream of valid verification
    codes to use during login.

<!-- ------------------------------------------------------------------------->
??? example "Login node"

    {==You must change your settings or commands to use the JADE login server.==}

    JADE: **jade01.jhsph.edu**
    
    === "Windows users"
        If you are on a Windows system, you will need to create new MobaXterm SSH and SFTP Sessions for the jade01.jhsph.edu login node.
        
    === "Mac & Linux users"
        You will need to adjust your ssh and sftp commands to use **jade01.jhsph.edu** instead of either jhpce01.jhsph.edu or jhpce03.jhsph.edu.
        
<!-- ------------------------------------------------------------------------->
??? example "New accounts are used to access JADE"
    {==New usernames, verification codes, and passwords are used.==}

    You will receive new login credentials when your account is configured.

    JADE uses completely different file systems than JHPCE, including `/users/`.

    JHPCE: /users/<username><br>
           E.g. /users/tunison

    JADE:  /users/<comm>/<cdua>/<username><br>
           E.g. /users/dbgap/d17293/d-steward1-17293

    Consider replacing any explicit paths to your home directory in files such
    as SLURM batch job scripts with the tilde (++tilde++) symbol.

<!-- ------------------------------------------------------------------------->
??? example "Paths - Data is stored in specific locations in JADE"
    {==JADE is designed to monitor and control access to sensitive data. This
    impacts data storage. JADE is not as open as JHPCE. That is due to JADE
    having to meet the NIST standard 800-171.==}
    
    **The TL;DR:** 

    Files downloaded from dbGaP are considered Controlled
    Unclassified Information (CUI). In JADE those files will live in 
    consistently-named places which follow the convention of:<br>
    `/data/dbgap/d<dua>/cui/`
    The file system is meant to be mostly static, with files users generate
    being kept elsewhere. Files and directories in that location will be 
    writable by a "data steward" account, used by one or two group members
    designated by the PI.

    **Motivation:**

    In JHPCE, data has been stored in very unstructured locations which have
    accreted over time. For JADE, we have characterized a number of data types used
    by research groups and created conventions for where each type should be stored.
    {==This one page PDF is a [summary of JADE paths & their
    purposes](../images/jade-path-summary-table.pdf).==} The right-hand column
    describes what kind of data should be considered for that location, and
    mentions aspects of the storage that provides that location - fast vs slow,
    capacious vs constrained, cheaper vs more expensive.

    This design attempts to provide improvements over previous practices, such
    as enhancing our ability to add new file systems in consistent fashions as
    data grows and new DUAs are added to the cluster, place those file systems
    on future file servers without requiring users to embed file path strings in
    their scripts which include file server names (e.g. `/dcs07`). (We regularly
    retire file servers after several years. Sometimes we can retain the path
    name, sometimes we cannot.)

    The design aids researchers by helping them consider where to store files
    generated over the life cycle of their projects.  Users come and go, and
    files they share with others often need to remain accessible to the group.
    When DUA agreements expire, the CUI data and files containing "too much"
    of that CUI have to be deleted. Our design provides locations so research
    result files can be stored away from the CUI files, easing the task of
    groups having to examine and relocate files before some are deleted.

    {==A [longer paths document](images/paths-csub-to-jade.pdf) is available
    which discusses the design in more detail.  It was written for the CMS
    community, but the the descriptions of the standard data locations are more
    fully fleshed out.==}

    **Resource**
    {==We have a document explaining how to replace strings in files 
    programmatically, on the command line, using [the sed utility](sed-tips-jade.md).==}

    When you need to modify existing files, such as SLURM batch job scripts, you
    can do it one at a time in a text editor. The value of learning some sed
    commands is that you can implement changes across many files at a time
    so they will work in JADE.

<!-- ------------------------------------------------------------------------->
??? example "Transferring data into & out of JADE"
    {== THIS SECTION & RELATED WEB PAGES ARE UNDER HEAVY DEVELOPMENT 202601210 ==}<br>
    {==JADE is designed to monitor and control access to sensitive data. This
    impacts data movement. JADE is not as open as JHPCE. That is due to JADE
    having to meet the NIST standard 800-171.==}

    In general, it is hard to "push" data *into* JADE. It is much easier to "pull"
    data *from inside* JADE. 

    JADE is located on a secure network segment maintained by IT@JH. Their
    firewall is configured to deny everything by default.  JHPCE will work
    with IT@JH to get the firewall rules "in front of" JADE configured to
    meet your needs, where possible.

    ***(We are working on transfer-related web documentation.)***

    If you run into issues, please reach out to bitsupport@lists.jh.edu. To help
    us help you more effectively, please provide relevant details -- where in
    the Internet did you try to initiate a connection, using what software
    commands, to what other point (computer)? [This
    page](../../help/good-query.md) has some prompts and suggestions for you.

    From the inside of JADE, you can initiate connections to the internet using
    a variety of tools and protocols.

    From outside of JADE, on a SAFE or SAFER Desktop or from the DISCOVERY
    cluster, you can initiate SFTP connections to jade01.jhsph.edu,
    using these non-standard ports:

    Data going into the cluster (pushed): 22511<br>
    Data coming out of the cluster (pulled): 22527

<!-- ------------------------------------------------------------------------->
??? warning "SLURM partition name changes"
    {==If your batch job scripts specify the name of the partition to use, you
    will need to modify them so they work on JADE.==}

    In JHPCE, the default partition was named "shared". There were PI specific
    partitions.

    In JADE, each community now has its own default partition. The DBGAP default is
    named "**d-shared**". (There currently are no PI-owned dbGaP partitions.)

    {==When creating dbgap home directories, we have added to your home directory
    the file `~/.slurm/defaults`. It contains a line specifying "d-shared" as
    the default partition. That allows you to continue not having to specify a
    specific partition.==}

    The command `slurmpic -a` will show you all of the currently defined nodes
    and partition names. (`scontrol show node <node_name>` will show you all of
    the info about a particular node, and `scontrol show partition
    <partition_name>` will display the full partition definition.)

    {==We have a document explaining how to replace strings in files 
    programmatically, on the command line, using [the sed utility](sed-tips-jade.md).==}

    When you need to modify existing files, such as SLURM batch job scripts, you
    can do it one at a time in a text editor. The value of learning some sed
    commands is that you can implement changes across many files at a time
    so they will work in JADE.

	
<!-- ------------------------------------------------------------------------->
??? info "Unix group changes"
    {==JADE uses completely separate groups to control data access.==}

    Each JADE user has a 'primary' group based on their community (`j-d-users`)
    and one or more 'secondary' groups (e.g. `j-d17293`).

<!-- ------------------------------------------------------------------------->
??? info "Mailing lists"
    {==We have defined mailing list to reach all DBGAP users in the cluster.==}

    JADE: jade-dbgap-users@lists.jh.edu

    This is a moderated list. DBGAP users are also members of the
    jhpceuser@lists.jh.edu mailing list.

<!-- ------------------------------------------------------------------------->
??? info "Operating system has been updated"
    {==This change should be invisible. But you may run into version dependencies.==}

    Both clusters use the Rocky 9.x operating system. The JHPCE uses 9.4 while
    JADE uses 9.6.  The Rocky OS is designed to be stable, so dot releases
    (e.g. .4 to .6) within major releases (e.g. 8.x vs 9.x) add mainly bug
    fixes. However, they also incorporate some upgrades to components, such as
    Python going from 3.9.18 to 3.9.21.

    JHPCE has a small "rocky96" partition available for testing.

    Please notify the systems administration team at bitsupport@lists.jh.edu of
    issues you find of this type. We will seek workarounds and be better able to
    help others. Thanks.

<!-- ------------------------------------------------------------------------->
