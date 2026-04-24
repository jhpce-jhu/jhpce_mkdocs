# Using the FreeIPA Identity Management Software

JHPCE uses the open source [FreeIPA](https://www.freeipa.org) Identity Management suite to manage UNIX user accounts. (FreeIPA is often shortened "ipa", as that is the command you typically use.) Applications on our computers work with the operating system to consult our FreeIPA master server to get user names, passwords, the groups a user is in and similar facts.

You are using FreeIPA every time you login, and when you try to access a file. That is all invisible to you, which is the way it should be. However, there are a small number of situations where users might want to use ipa manually.

!!! Note
    **The primary `ipa` use case is where primary investigators ask for and are granted the ability to manage one of the UNIX user groups the systems administrators have created.**
    
    Those groups are the primary method for controlling access to project storage allocations such as `/dcs07/<primary-investigator>/data/`.
    
    Without the delegation, PIs or their authorized representatives must send change requests to `bitsupport@lists.jh.edu`
    
    This topic is covered in our document [Managing Allocation Access](../storage/allocation-access.md).

Instructions for managing group membership follows the IPA usage sections at [TL;DR](#tldr).

## Becoming authorized to use ipa

A service called `Kerberos` is used by FreeIPA to control access to information. When you want to work with ipa you need to prove to Kerberos that you are an authorized user, which involves providing your username and password. If successful, Kerberos issues you a security token called a "ticket" This is good for 24 hours.

Kerberos commands usually begin with the letter "k".  Users change their cluster password with `kpasswd`

Tickets are associated with user and computer "principals". Your "principal" or the user name you are known by when logging in or using the ipa command is: `<your cluster user name>@CM.CLUSTER`, e.g. bobmiller@CM.CLUSTER. Note the capitalization of CM.CLUSTER.

### Seeing your ticket(s)
Issue the command `klist` 

??? Note "Example klist output (click to open)""
    This example was generated on 04/18/2026 at 16:20, after all of the tickets listed expired. So the principal `tunison@CM.CLUSTER` has no valid current tickets.
    
    ```shellsession
    jhpce03:~% klist
    Ticket cache: KCM:42629:7180
    Default principal: tunison@CM.CLUSTER

    Valid starting       Expires              Service principal
    04/16/2026 16:14:12  04/17/2026 15:42:35  krbtgt/CM.CLUSTER@CM.CLUSTER
    04/16/2026 16:14:53  04/17/2026 15:42:35  HTTP/ns01.cm.cluster@CM.CLUSTER
    04/16/2026 19:57:00  04/17/2026 15:42:35  host/compute-140.cm.cluster@CM.CLUSTER
    ```
### Acquiring a ticket
Use the "kinit" command: `kinit <your principal name>` where that is as described earlier. 

## Getting ipa command syntax

The commands you will need are found in the [TL;DR](#tldr) section below.

If you need or want to learn more about FreeIPA:

The RHEL documentation for using FreeIPA to work with groups is found [here](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_idm_users_groups_hosts_and_access_control_rules/index). (JHPCE uses an operating system (Rocky Linux) which is a binary compatible version of the Red Hat Enterprise Linux (RHEL) operating system.)

The ipa manual page (seen with `man ipa`) has an ***EXAMPLES*** section but they are of course not a complete reference.  The main help tool to use is `ipa` itself.

Most usage of `ipa` involves providing a subcommand and arguments specific to that subcommand, e.g. `ipa user-find <string>`

The self-contained documentation uses "topics" to group subcommands together into families.

* To learn about "user" subcommands and how to use them, you run `ipa help user`
* To see a list of the user subcommands, run `ipa help commands | grep '^user-'` 

In this sample, note the format needed to get to the actual syntax of a specific command:

```shellsession
jhpce03:~% ipa help topics
# here I list only three results
group              Groups of users
host               Hosts/Machines
user               Users
jhpce03:~% ipa help group
[USEFUL INFO not shown here]
jhpce03:~% ipa group-add-member --help
Usage: ipa [global-options] group-add-member GROUP-NAME [options]

Add members to a group.
Options:
  -h, --help            show this help message and exit
  --external=STR        Members of a trusted domain in DOM\name or name@domain
                        form
  --all                 Retrieve and print all attributes from the server.
                        Affects command output.
  --raw                 Print entries as stored on the server. Only affects
                        output format.
  --no-members          Suppress processing of membership attributes.
  --users=STR           users to add
  --groups=STR          groups to add
  --services=STR        services to add
  --idoverrideusers=STR
                        User ID overrides to add
```

## Delegated ability to manage group membership

We have several related documents:

- [Sharing files](../files/sharing-files.md)
- [Managing Allocation Access](../storage/allocation-access.md)

<A ID="tldr"></A>
### TL;DR

In the following example, a path `/dcs10/trichard/data/` is accessible by members of the gateway group "cphit". A secondary group, `cphit_acgclustering` will be used to own the files and directories inside the top level of the allocation. The result will be able to accomodate future `cphit_*` groups, whose members may or may not also be members of `cphit_acgclustering`

Users must be added to to two groups `cphit` and `cphit_acgclustering`

1. Identify the group controlling the directory access with `ls -ld /dcs10/trichard/data/` Note that users have to be able to access _every single level above that point_ before your work will have the desired impact. (In this case that's a given but you need to check this before working with files or directories further into the directory tree.)

2. Verify membership of the groups before making changes. The first method does not use an ipa command. 
   3. `getent group cphit`
   4. `ipa group-show cphit --all`

5. Add users to the two groups with 
   6. `ipa group-add-member cphit --users={user1,user2}`
   7. `ipa group-add-member cphit_acgclustering --users={user1,user2}`

!!! Warning "Specifying one or several users"
    Note that there is some pickiness when specifying users to commands like `group-add-member`.
    
    - For multiple users, you must provide them in a comma-separated list (with no spaces) inside of curly braces { }, e.g. `--users={user1,user2,user3}`
    
    - If you only have one username to add, do not use the curly braces, e.g. `--users=user1`

Users can be deleted with the `group-remove-member` subcommand.

!!! Warning "Why are you deleting a group member?"
    If it is because you know that someone is leaving, please consider telling us at bitsupport@lists.jh.edu about the situation. Do we need to disable their ability to log in because they are done using the cluster? Delete their files so the PI doesn't need to pay for the storage?

??? Tip "Finding all group-management delegations (click to open)"
    We have provided a script named `find-group-managers`
    Here is a raw command example. Shown is one stanza, in which the managed group is "cphit" and the single group manager is "trichard":
    ```shellsession
    jhpce03 ~% ldapsearch -LLL -Y GSSAPI -b cn=users,cn=accounts,dc=cm,dc=cluster "(&(objectClass=person)(nsAccountLock=TRUE)(uid=c-*-*))" uid cn email|less
    dn: cn=cphit,cn=groups,cn=accounts,dc=cm,dc=cluster
    cn: cphit
    memberManager: uid=trichard,cn=users,cn=accounts,dc=cm,dc=cluster
    ```


### An example of how delegation was created

In the following example, a path `/dcs10/trichard/data/` is accessible by members of the gateway group "cphit". A secondary group, `cphit_acgclustering` will be used to own the files and directories inside the top level of the allocation.

Users must be added to to two groups `cphit` and `cphit_acgclustering`

```shellsession
jhpce03:prolog% ipa group-find cphit --all
---------------
1 group matched
---------------
  dn: cn=cphit,cn=groups,cn=accounts,dc=cm,dc=cluster
  Group name: cphit
  GID: 4281
  Member users: trichard, anishimu, xding
  ipauniqueid: 059f11f0-39c8-11f1-a6fc-3e9957d14bbb
  objectclass: top, groupofnames, nestedgroup, ipausergroup, ipaobject, posixgroup
----------------------------
Number of entries returned 1
----------------------------

jhpce03:~% ipa group-mod cphit --desc="Tom Richard alloc gateway group"
----------------------
Modified group "cphit"
----------------------
  Group name: cphit
  Description: Tom Richard alloc gateway group
  GID: 4281
  Member users: trichard, anishimu, xding
  
jhpce03:~% ipa group-add cphit_acgclustering --desc="Tom Richard alloc secondary group"
---------------------------------
Added group "cphit_acgclustering"
---------------------------------
  Group name: cphit_acgclustering
  Description: Tom Richard secondary group
  GID: 1295400066
  
jhpce03:~% ipa group-add-member cphit_acgclustering --users={trichard, anishimu, xding}
ipa: ERROR: command 'group_add_member' takes at most 1 argument

jhpce03:~% ipa group-add-member cphit_acgclustering --users={trichard,anishimu,xding}
  Group name: cphit_acgclustering
  Description: Tom Richard secondary group
  GID: 1295400066
  Member users: trichard, anishimu, xding
-------------------------
Number of members added 3
-------------------------

jhpce03:ipa-help% ipa group-add-member-manager cphit --users=trichard
  Group name: cphit
  Description: Tom Richard alloc gateway group
  GID: 4281
  Member users: trichard, anishimu, xding
  Membership managed by users: trichard
-------------------------
Number of members added 1
-------------------------

jhpce03:ipa-help% ipa group-add-member-manager cphit_acgclustering --users=trichard
  Group name: cphit_acgclustering
  Description: Tom Richard secondary group
  GID: 1295400066
  Member users: trichard, anishimu, xding
  Membership managed by users: trichard
-------------------------
Number of members added 1
-------------------------
```
