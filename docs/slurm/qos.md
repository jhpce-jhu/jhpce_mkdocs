# Quality of Service

20240207

HOW WE ARE USING QOS TO DATE

Our users all have a QOS value of "normal", which is
inherited from their parent account, named "jhpce"

The account jhpce, of the organization jhpce, in the
cluster jhpce3 is the parent of all of the users.

The "normal" QOS  (currently) has no limits defined for it.
(It is the default QOS defined in SLURM. We didn't create it.)

A fundamental element of our current practice is that the
user’s __default QOS__ entries in the database are "normal".

We’ve defined additional ones
and, given some users ones they can optionally use __in addition__
to normal. 
 
Those additional ones are being applied
(1) via per-partition QOS= definitions in partitions.conf
    (e.g. QOS "interactive-default", "shared-default"), and
(2) via users specifying additional ones via per-job
    directives (b/c we granted them access to them and
    told them that they needed to use those directives).
    (Users do not have to specify optional QOS on every
    job thenceforward. Users can pick and choose what QOS
    to use for each job.
    

## USEFUL COMMANDS
```
# See our currently defined QOS in a readable format
sacctmgr show qos format=Name%20,Priority,Flags%30,MaxWall,MaxTRESPU%20,MaxJobsPU,MaxSubmitPU,MaxTRESPA%25

# See all users database values
sacctmgr show user withassoc  | less

# See a particular user's database values
sacctmgr show user withassoc where name=smburke

# WHO HAS EXTRA QOS shortens the output width
#  (but needs improvement to align column entries)
sacctmgr show user withassoc|grep -v "normal "|awk '{printf "%s\t\t%s\t%s\t\t%s%\n", $1,$3,$4,$7}'

# Add a QOS to a user's existing allowed QOS:
sacctmgr mod user mmill116 set qos+=high-priority
# or, you can redefine their whole list
sacctmgr mod user where name=tunison set qos=normal,shared-200-2

# Remove a QOS from a user's existing allowed QOS:
sacctmgr mod user where name=tunison set qos-=shared-200-2 # to remove

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
```
