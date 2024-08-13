---
tags:
  - slurm
---

# **sacctmgr -- useful command examples**
    
[Sacctmgr](https://slurm.schedmd.com/archive/slurm-22.05.9/sacctmgr.html) is mostly used by systems administrators. Only they are allowed to make changes. Users can use it to view settings.  The basic command for that is `sacctmgr show qos`. This displays all possible QOS fields, and the output field widths are large. So we wrote a shell script called `showqos` that tries to print out only the QOS attributes that we are using and with more readable field widths. Be aware that that script may not show attributes we add in the future!!

Note that the output field names displayed are not necessarily the same as the keywords used when modifying, e.g. MaxTRESPerUser is the keyword but the more concise MaxTRESPU is displayed.

Also note that by "account" SLURM means the parent object of user accounts. We do not currently put our users into different accounts (many other clusters do).

We currently have two clusters, named "jhpce3" and "cms" You usually don't have to specify which cluster you want to consult/change, as we have a SLURM server for each cluster.

## **Sacctmgr for Users**

```
# See our currently defined QOS in a readable format
sacctmgr show qos format=Name%20,Priority,Flags%30,MaxWall,MaxTRESPU%20,MaxJobsPU,MaxSubmitPU,MaxTRESPA%25

# See all users database values
sacctmgr show user withassoc  | less

# See a particular user's database values
sacctmgr show user withassoc where name=smburke

# WHO HAS EXTRA QOS -- shortens the output width
#  (but needs improvement to align column entries)
sacctmgr show user withassoc|grep -v "normal "|awk '{printf "%s\t\t%s\t%s\t\t%s%\n", $1,$3,$4,$7}'
```

## **Sacctmgr for Systems Administrators**

Like many administrative commands, you can just run the command to enter into a shell. Useful if you are exploring some situation, although you cannot paginate output.

### Make A Backup Of Users and Their Settings
```
# Dump the accounting database
sacctmgr dump <cluster-name> file=<filename>
```

### **Managing QOS for users**

See our [QOS page](../slurm/qos.md) for more information.

Our users have no limits on them, at the user account level. The typical account has a default QOS named "normal". If you run `showqos` you will see that "normal" doesn't have any restrictions. Therefore users entitled to run jobs in their private per-research group partitions are not limited in how many of their nodes' resources they can consume. (Users found running jobs on partitions they are not entitled to use will have their jobs killed and have to acknowledge that they understand that they need to use public partitions.)

For public partitions like "shared" and "interactive", the slurm config file /etc/slurm/partitions.conf specifies a default partition QOS of, for example, "shared-default". So  we can control how many CPUs and how much RAM each user can use in public partitions by changing the appropriate single QOS. 

```
# Add a QOS to a user's existing allowed QOS:
sacctmgr mod user mmill116 set qos+=high-priority
# or, you can redefine their whole list
sacctmgr mod user where name=tunison set qos=normal,shared-200-2

# Remove a QOS from a user's existing allowed QOS:
sacctmgr mod user where name=tunison set qos-=shared-200-2 # to remove

# Limit a user's ability to run and submit jobs
sacctmgr mod user where name=tunison set MaxJobs=100,MaxSubmit=200

# How JHPCE3 users accounts are created in the sacctmgr database
sacctmgr -i create user name=$userid cluster=jhpce3 account=jhpce 

# How C-SUB users accounts are created in the sacctmgr database on jhpcecms01
sacctmgr -i create user name=$userid account=generic cluster=cms 
```

### **Managing QOS**

{== To delete a parameter from a QOS, set it to the value -1 ==}

{== You MUST define flags=DenyOnLimit,OverPartQOS for a QOS to work as expected ==}

There are a number of variants of some categories of parameters -- by user, by group, by account, etc.

`MaxJobs*` means jobs that can be running at one time
`MaxJobsSubmit*` means jobs that can be both pending and running, altogether, in total.

!!! Note
    There are a number of parameters which feature Group or Account. We cannot use these easily, because for example as far as "Groups" are concerned, by default our users are all in the same group ("users" in jhpce3 and "c-users" in cms). SLURM doesn't look at secondary group membership, only the primary group. AND we don't maintain groups for all of our PI groups. We maintain groups not to give access to computers but to control storage permissions. (In jhpce3 some users DO have primary groups different from "users".)
    
    Accounts are similar to groups in that our current model has all user accounts belonging to a single higher-level account object ("jhpce" in jhpce3 and "generic" in cms (for some reason -- there's also a csub account) (and the small number of c-*-10101 users happen to be in the "sysadmin" account)).
    
    Note that one _can_ use the fact that all users are in the same account to limit the total number of users in the cluster who can do some things, like submit jobs, or whatever the parameter category is. 


```
# Define a QOS limiting a user to 25 running jobs with a max of 50 pending or running
sacctmgr add qos job-25run50sub

# You MUST define these flags for the QOS to work as expected
sacctmgr modify qos job-25run50sub set flags=DenyOnLimit,OverPartQOS
sacctmgr modify qos job-25run50sub set MaxJobsPerUser=25 MaxSubmitJobsPerUser=50

# Remove one of those parameters
sacctmgr modify qos job-25run50sub set MaxJobsPerUser=-1

# Delete the QOS (they can't be renamed)
sacctmgr delete qos job-25run50sub

# Limit each user to 512GB and 100 CPU:
sacctmgr modify qos shared-default set MaxTRESPerUser=mem=524288 MaxTRESPerUser=cpu=100

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

### Other Tasks


```
# View the slurmdbd configuration in force
sacctmgr show Configuration | less

# Perform a self-check of database
sacctmgr show problems

# Check history of sacctmgr operations - useful to see users added, QOS changes
# "Modify Clusters" might be slurmctld restarts
# (Output width is inadequate but most/all cut-off material isn't that useful)
# (which you can see by running command with -p argument and redirect to a file)
sacctmgr show transaction | less
sacctmgr -p show transaction > /tmp/lookatme

# Check RPC statistics (watch out for users with extremely high numbers of RPC)
# (these stats might be since the last time the slurmctld was restarted??)
sacctmgr show stats | less

# You can clear the stats and then watch how quickly a suspicuous user racks up RPC calls
sacctmgr clear stats
```

```
# Display TRES (Trackable RESources) (our clusters may not be identical)
# TRES can be iunvolved in QOS (fewer in version 22.05 than in 24.05)
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

