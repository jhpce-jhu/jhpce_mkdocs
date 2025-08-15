---
tags:
  - slurm
---

# **sacctmgr -- useful command examples**
    
[Sacctmgr](https://slurm.schedmd.com/archive/slurm-22.05.9/sacctmgr.html) is mostly used by systems administrators. Only they are allowed to make changes. Users can use it to view settings.

`sacctmgr` is used by systems administrators to create and modify user accounts as well as QOS (qualities of service).

Also note that by "account" SLURM means the parent object of user accounts. We do not currently put our users into different accounts (many other clusters do).

## **Sacctmgr for Users**

```
# See your entry in the sacctmgr database
sacctmgr show user withassoc where name=$USER

# See our currently defined QOS in default format
sacctmgr show qos

# See our currently defined QOS in a readable format
# (*we put this in the script "showqos")
sacctmgr show qos format=Name%20,Priority,Flags%30,MaxWall,MaxTRESPU%20,MaxJobsPU,MaxSubmitPU,MaxTRESPA%25

# See all users database values
sacctmgr show user withassoc  | less

# See a particular user's database values
sacctmgr show user withassoc where name=smburke
```

## **Sacctmgr for Systems Administrators**

We currently have two clusters, named "jhpce3" and "cms" You don't have to specify which cluster you want to consult/change, as we have a SLURM server for each cluster. And because they aren't connected in any way, you cannot extract data from one when logged into a node in the other cluster.

Like many administrative SLURM commands, you can run the `sacctmgr` command to enter into its shell version. This is useful if you are exploring some situation, although you cannot paginate output. It supports up-arrow to get at previous commands. Issuing the command "verbose" when in the interactive shell may display interesting info.

You can add the CLI flag "-i" to avoid the "are-you-sure" 30 second prompt and delay. Useful when scripting.

### **Managing QOS for users**

See our [QOS page](../slurm/qos.md) for more information.

Our users have no limits on them, at the user account level, other than a 10,000 job limit. The typical account has a default QOS named "normal". If you run `showqos` you will see that "normal" doesn't have any restrictions other than the 10k job limit. Therefore users entitled to run jobs in their private per-research group partitions are not limited in how many of their nodes' resources they can consume. (Users found running jobs on partitions they are not entitled to use will have their jobs killed and have to acknowledge that they understand that they need to use public partitions.)

For public partitions like "shared" and "interactive", the slurm config file /etc/slurm/partitions.conf specifies a default partition QOS of, for example, "shared-default". We use that to control how many CPUs and how much RAM each user can use in public partitions. We can change that setting for everyone as the cluster's resources shrink or grow by changing the appropriate single QOS.

When changing qos for something only use the '=' operator when wanting to explicitly set the qos to something.  In most cases you will want to use the '+=' or '-=' operator to either add to or remove from the existing qos already in place.

```
# Add a QOS to a user's existing allowed QOS:
sacctmgr -i mod user mmill116 set qos+=high-priority
# or, you can redefine their whole list
sacctmgr mod user where name=tunison set qos=normal,shared-200-2

# Remove a QOS from a user's existing allowed QOS:
sacctmgr -i mod user where name=tunison set qos-=shared-200-2 # to remove

# Limit a user's ability to run and submit jobs
sacctmgr -i mod user where name=tunison set MaxJobs=100,MaxSubmit=200

# How JHPCE3 users accounts are created in the sacctmgr database
sacctmgr -i create user name=$userid cluster=jhpce3 account=jhpce 

# How C-SUB users accounts are created in the sacctmgr database on jhpcecms01
sacctmgr -i create user name=$userid account=generic cluster=cms
```

#### Associations

To see all of the information about a user, you need to add the argument "withassoc" to sacctmgr commands.

Our users normally have a single entry in the sacctmgr database. This is called an association. Associations are 4-tuples of {user, account, cluster, partition}. There are limits in each association, such as maxjobs, maxsubmit(jobs), ....  

If you add limits or grant access to additional QOS, the user now has more than one association -- the one called "normal" plus one or more others. When we grant a user the ability to request a QOS, that happens in a new association.  Their QOS field in that association contains a comma-separated list of QOS names. If you want to set a per-partition limit, that requires its own association.

To find "non-standard" users who have more than the "normal" association, you run "sacctmgr show user withassoc" and remove the output lines which contain "normal" in them. 

Here are tips about looking into the database for "non-standard" users.

```
# WHO HAS EXTRA ASSOCIATIONS

sacctmgr show user withassoc|grep -v "normal "

produces a sixteen column wide display. Here are the fields
and their awk field number, in case you want to craft an awk
command to print only a few columns.

$1 user
$2 defaccount
$3 adminpriv
$4 cluster
$5 account
$6 partition
$7 share
$8 priority
$9 maxjobs
$10 maxnodes
$11 maxcpus
$12 maxsubmit
$13 maxwall
$14 maxcpumins
$15 qos
$16 defaultqos

# This command was an attempt to print only some interesting columns,
# but it does not behave as expected -- even the column header lines printed
# do not make sense.
# The printf args need field width specifications to better align column entries
sacctmgr show user withassoc|grep -v "normal "|awk '{printf "%s\t\t%s\t%s\t\n", $1,$6,$15}'
```

### **Managing QOS**

{==To delete a parameter from a QOS, set it to the value -1==}

{==You MUST define flags=DenyOnLimit,OverPartQOS for a QOS to work as expected==}

{==We now limit all partitions to 10,000 jobs per user. 
Therefore every such QOS needs `MaxSubmitJobsPU=10000`==}

{==Note that the output field names displayed are not necessarily the same as the keywords used when modifying, e.g. MaxTRESPerUser is the keyword but the more concise MaxTRESPU is displayed.==}

If you provide the `-i` flag, the operation is immediately implemented. Without it, you have 30 seconds to respond (which doesn't work in scripts).

There are a number of variants of some categories of parameters -- by user, by group, by account, etc.

`MaxJobs*` means jobs that can be running at one time
`MaxJobsSubmit*` means jobs that can be both pending and running, altogether, in total, e.g. `MaxJobsSubmitPerUser`

!!! Note
    There are a number of parameters which allow you to place limits based on a user's Group or Account. We cannot use those sets of limits.  We do not place users in Accounts. (In many other organizations, Accounts are created for entities like departments and PIs, and can be given consumable resouce allocations, such as so many CPU hours, which then decline towards zero as they are used. Think of them like bank accounts.) Group is the user's primary UNIX group at the time they were added to the sacctmgr database. A users' SLURM group does not change when their UNIX primary group is changed by systems administrators. JRT believes that you cannot see or modify a sacctmgr user object's group -- you have to delete that user and recreate them.  A user's secondary groups are ignored by SLURM. By default our cluster users are all in the same group ("users" in jhpce3 and "c-users" in cms). SLURM doesn't look at secondary group membership, only the primary group. AND we don't maintain groups for all of our PI groups. In jhpce3, we maintain UNIX user groups primarily to control disk storage permissions. (In jhpce3 some users DO have primary groups different from "users".)
    
    Accounts are similar to groups in that our current model has all user accounts belonging to a single higher-level account object ("jhpce" in jhpce3 and "generic" in cms (for some reason -- there's also a csub account) (and the small number of c-*-10101 users happen to be in the "sysadmin" account)).
    
    Note that one _can_ use the fact that all users are in the same account to limit the total number of users in the cluster who can do some things, like submit jobs, or whatever the parameter category is. But take care to consider whether you are limiting what everyone can do, in aggregate, not individually.


```
# Define a QOS which will limit a user to 25 running jobs with a max of 25 more pending
# This QOS won't change any behavior until it is added to a partition or association etc
sacctmgr -i add qos job-25run50sub

# You MUST define these flags for the QOS to work as expected
# We limit all partitions to 10,000 jobs per user
sacctmgr -i modify qos job-25run50sub set flags=DenyOnLimit,OverPartQOS
sacctmgr -i modify qos job-25run50sub set MaxSubmitJobsPU=10000

# Add the job-limiting arguments 
sacctmgr -i modify qos job-25run50sub set MaxJobsPerUser=25 MaxSubmitJobsPerUser=50

# Remove one of those parameters
sacctmgr modify qos job-25run50sub set MaxJobsPerUser=-1

# Delete the QOS (they can't be renamed!!)
sacctmgr -i delete qos job-25run50sub

# Limit each user to 100 CPU and 512GB (you must specify RAM in megabytes):
sacctmgr modify qos shared-default set MaxTRESPerUser=mem=524288 MaxTRESPerUser=cpu=100

# Limit the amount of a Trackable RESource
sacctmgr modify qos public-gpu-limit set MaxTRESPerUser=gres/gpu=4

# Limit maximum job run time to 1 day
sacctmgr modify qos cms-larger set MaxWall=1-0

# Limit each user to one of: a pending or running job.
# Limit whole cluster to running 1 job with up to one more pending
# This means only one job using this QOS can run in the whole cluster. To put a lid on things. If you include MaxJobsPA=1 then no second user can submit
#
sacctmgr modify qos cms-larger set MaxJobsPerUser=1 MaxSubmitJobsPerUser=1  MaxSubmitJobsPA=2

# You cannot rename a QOS, so you have to delete the old name -- be aware that jobs might be using it!!!
sacctmgr delete qos qosname
```

### TRES

Display TRES (Trackable RESources) (will be different on each cluster).
We can add TRES items to the default list, and need to do so for, for example, each model of GPU card found in the cluster.

TRES can be specified in QOS (fewer in version 22.05 than in newer versions)

```
sacctmgr show tres
    Type            Name     ID 
-------- --------------- ------ 
     cpu                      1 
     mem                      2 
  energy                      3 
    node                      4 
 billing                      5 
      fs            disk      6 
    vmem                      7 
   pages                      8 
    gres             gpu   1001 
    gres     gpu:tesv100   1002 
    gres      gpu:titanv   1003 
    gres     gpu:tesa100   1004 
    gres    gpu:tesv100s   1005 
```

### Backing Up Users and QOS

Currently JRT has stored some in `/root/slurm/sacctmgr-stuff/sacctmgr-dumps/`

```
# Dump the accounting database
sacctmgr dump <cluster-name> file=<filename>
```
That backup only contains users!!!! Other things are stored in the sacctmgr database, such as QOS definitions.

```
# Dump the QOS definitions
sacctmgr show qos > qos_backup-date.txt
```

### Database Transaction History

You can use variants of `sacctmgr list transactions` to inspect the database going back to its founding. The kinds of things you can see:

* who ran which command that changed the database
* what commands involved a specific user
* what commands changed a QOS

#### Output formatting

Output columns are: Time, Action, Actor, Where, Info

"Where" contains things like the names of users and QOS

You can control field width by adding the ++"%"++ character then a field width number value.

You can specify what you want to see with e.g. "format=user,time%10"

#### Actions

Some of the actions you can search for are:

* Add Users
* Remove Users
* Add Associations
* Add QOS
* Modify QOS
* Modify Clusters - I think that this might be `slurmctld` restarts.

The right-most column in the default output is called "Info". It is truncated such that you can't see full QOS definition commands etc. You can see that info by adding format arguments or simply with the `-P` ("parsable 2") flag.


```
sacctmgr show transaction | less

sacctmgr -P show transaction > /tmp/lookatme

# transactions involving cluster user simmons
sacctmgr list transactions Users=simmons

# Which users were created in January 2025?
# Here is the version that will produce one user per line
sacctmgr list transactions Action="Add Users" Start=2025-01-01 End=2025-01-31 format=Where,Time%10
```

### Miscellaneous

```
# View the slurmdbd configuration in force
sacctmgr show Configuration | less

# Perform a self-check of database
sacctmgr show problems

# Check RPC statistics (watch out for users with extremely high numbers of RPC)
# (these stats might be since the last time the slurmctld was restarted??)
sacctmgr show stats | less

# You can clear the stats and then watch how quickly a suspicious user racks up RPC calls
sacctmgr clear stats
```

```

