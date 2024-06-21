---
tags:
  - slurm
---

# **sacctmgr -- useful command examples**
    
[Sacctmgr](https://slurm.schedmd.com/archive/slurm-22.05.9/sacctmgr.html) is mostly used by systems administrators. Only they are allowed to make changes. Users can use it to view settings.  The basic command for that is `sacctmgr show qos`. This displays all possible QOS fields, and the output field widths are large. So we wrote a shell script called `showqos` that tries to print out only the QOS attributes that we are using and with more readable field widths. Be aware that that script may not show attributes we add in the future!!

Note that the output field names displayed are not necessarily the same as the keywords used when modifying, e.g. MaxTRESPerUser is the keyword but the more concise MaxTRESPU is displayed.

Also note that by "account" SLURM means the parent object of user accounts. We do not currently put our users into different accounts (many other clusters do).

We currently have two clusters, named "jhpce3" and "cms" You usually don't have to specify which cluster you want to consult/change, as we have a SLURM server for each cluster.

{== To delete a parameter from a QOS, set it to the value -1 ==}

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

```
# Dump the accounting database
sacctmgr dump <cluster-name> file=<filename>
```

### **Managing QOS for users**

```
# Add a QOS to a user's existing allowed QOS:
sacctmgr mod user mmill116 set qos+=high-priority
# or, you can redefine their whole list
sacctmgr mod user where name=tunison set qos=normal,shared-200-2

# Remove a QOS from a user's existing allowed QOS:
sacctmgr mod user where name=tunison set qos-=shared-200-2 # to remove

# Limit a user's ability to run and submit jobs
sacctmgr mod user where name=tunison set MaxJobs=100,MaxSubmit=200

# How users accounts are created in the sacctmgr database
sacctmgr -i create user name=$userid cluster=jhpce3 account=jhpce 

# How C-SUB users accounts are created in the sacctmgr database on jhpcecms01
sacctmgr -i create user name=$userid account=generic cluster=cms 
```

### **Managing QOS**

{== You MUST define flags=DenyOnLimit,OverPartQOS for a QOS to work as expected ==}

There are a number of parameters which feature Group or Grp. We cannot use these, because by default our users are all in the same group ("users" in jhpce3 and "c-users" in cms).

There are a number of variants of some parameters -- by user, by account, etc.

`MaxJobs*` means jobs that can be running at one time
`MaxJobsSubmit*` means jobs that can be both pending and running, altogether, in total

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

# Limit each group to one running and one pending job, where a user in the group cannot have both a running and a pending job
sacctmgr modify qos cms-larger set MaxJobsPerUser=1 MaxSubmitJobsPerUser=1 GrpJobs=1 GrpSubmitJobs=2

# You cannot rename a QOS, so you have to delete the old name -- be aware that jobs might be using it!!!
sacctmgr delete qos qosname
```
