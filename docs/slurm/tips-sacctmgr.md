---
tags:
  - slurm
---

# sacctmgr useful command examples
    
Sacctmgr is mostly used by systems administrators. Only they are allowed to make changes.

Note that the output field names displayed are not the same as the keywords used when modifying, e.g. MaxTRESPerUser is the keyword but MaxTRESPU is displayed.

We currently have two clusters, named "jhpce3" and "cms"

## Sacctmgr for Users

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

## Sacctmgr for Systems Administrators
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

# Define a QOS
sacctmgr add qos job-25run50sub
# You MUST define these flags for the QOS to work as expected
sacctmgr modify qos job-25run50sub set flags=DenyOnLimit,OverPartQOS
sacctmgr modify qos job-25run50sub set MaxJobsPerUser=25 MaxSubmitJobsPerUser=50

sacctmgr modify qos shared-default set MaxTRESPerUser=mem=524288 MaxTRESPerUser=cpu=100

# Dump the database
sacctmgr dump <cluster-name> file=<filename>
```
