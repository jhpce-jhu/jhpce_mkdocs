---
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
    * red, danger - You must deal with/know about
    * orange, warning - You probably should know about
    * light blue, info - Less impactful changes

<!-- ------------------------------------------------------------------------->
??? tip "Please take note of these path element symbols"

    In this document and others, we show the convention used for some aspect of
    the cluster, such as the path to expect for this or that purpose.

    ***&lt;dua&gt;*** = for example, 17293 or 15209<br>
    ***&lt;cdua&gt;*** = for example, d17293 or d41021<br>
    ***&lt;comm&gt;*** = for example, dbgap or cms<br>
    ***&lt;comm_letter&gt;*** = for example, d or c<br>
    ***&lt;username&gt;*** = for example, d-jli89-10201<br>
    <br>
    E.g. `/users/<comm>/<cdua>/<username>/`<br>
         `/users/dbgap/d17293/d-wshi13-17293/`

<!-- ------------------------------------------------------------------------->
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

<!-- ------------------------------------------------------------------------->
??? danger "Login is only allowed from SAFE or SAFER Desktop"
    {==dbGaP users must log into the SAFE Desktop before they can log into JADE.==}

    SAFE and SAFER Desktop are Windows virtual machines which are described
    [here](https://researchit.jhu.edu/safer/).  NIST 800-171 data security
    controls require a variety of services provided by these environments.

    One must be connected to a Hopkins network to log into SAFE.
    (You can also log into JADE using the newer SAFER Desktop service.)

<!-- ------------------------------------------------------------------------->
??? danger "Login requires MFA via OTP; ssh keys are not allowed"
    {==dbGaP users must use One Time Passwords to log into JADE.==}

    The local program `auth_util` allows you to work with your Google
    Authenticator One Time Password configuration.  You will use it to configure
    your smartphone or other OTP apps to generate a stream of valid verification
    codes to use during login.

<!-- ------------------------------------------------------------------------->
??? danger "Login node"

    {==You must change your settings or commands to use the JADE login server.==}

    JADE: **jade01.jhsph.edu**
    
    === "Windows users"
        If you are on a Windows system, you will need to create new MobaXterm
	SSH and SFTP Sessions for the jade01.jhsph.edu login node.
        
    === "Mac & Linux users"
        You will need to adjust your ssh and sftp commands to use
	**jade01.jhsph.edu** instead of either jhpce01.jhsph.edu or
	jhpce03.jhsph.edu.
        
<!-- ------------------------------------------------------------------------->
??? danger "New accounts are used to access JADE"
    {==New usernames, verification codes, and passwords are used.==}

    You will receive new login credentials when your account is configured.

    JADE uses completely different file systems than JHPCE, including `/users/`.

    JHPCE: `/users/<username><br>`
           `/users/tunison`

    JADE:  `/users/<comm>/<cdua>/<username><br>`
           `/users/dbgap/d17293/d-steward1-17293`

<!-- ------------------------------------------------------------------------->
??? info "Unix group changes"
    {==JADE uses completely separate groups to control data access.==}

    Each JADE user has a 'primary' group based on their community (`j-d-users`)
    and one or more 'secondary' groups (e.g. `j-d17293`).

<!-- ------------------------------------------------------------------------->
??? warning "SLURM partition name changes"
    {==If your batch job scripts specify the name of the partition to use, you
    will need to modify them so they work on JADE.==}

    In JHPCE, the default partition was named "shared". There were PI specific
    partitions.

    In JADE, each community now has its own default partition. The DBGAP default is
    named "**d-shared**". There currently are no PI owned partitions.

    When creating dbgap home directories, we have added to your home directory
    the file `~/.slurm/defaults`. It contains a line specifying "d-shared" as
    the default partition. That allows you to continue not having to specify a
    specific partition.

    The command `slurmpic -a` will show you all of the currently defined nodes
    and partition names. (`scontrol show node <node_name>` will show you all of
    the info about a particular node, and `scontrol show partition
    <partition_name>` will display the full partition definition.)

    We have a document explaining how to replace strings in documents
    [here](sed-tips-jade.md).

<!-- ------------------------------------------------------------------------->
??? danger "Transferring data into & out of JADE"
    {== THIS SECTION IS UNDER DEVELOPMENT 20251226 ==}
    {==JADE uses specific TCP/IP networking ports for data ingress & egress: ==}<br><br>

    JADE is located on a secure network segment maintained by IT@JH. Their
    firewall is configured to deny everything by default.

    From the inside of JADE, you can initiate connections to the internet using
    a variety of tools and protocols.

    From outside of JADE, you can initiate SFTP connections to jade01.jhsph.edu,
    using these non-standard ports:

    Data going into the cluster: 22511<br>
    Data coming out of the cluster: 22527

    (?More description in an jade/access/ page?)

<!-- ------------------------------------------------------------------------->
??? info "Mailing lists"
    {==We have defined mailing list to reach all DBGAP users in a cluster.==}

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
