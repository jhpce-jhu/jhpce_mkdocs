---
tags:
  - done
  - slurm
---
# Quality of Service

Systems administrators can define resource limits called QOS and assign them to a variety of objects, mainly users and job partitions. We use them to share resources equitably and to allow exceptions.

For example, we have a QOS named `shared-default` which is normally set to allow a user to use 100 CPU cores and 1TB of RAM at any one time in the `shared` partition.  These values were chose to represent roughly 20% of those available in that partition. When Primary Investigators who own nodes need to remove them from the shared partition for their own use, that QOS' definition can be changed to a lower value to maintain the 20% goal.

Vendor QOS documentation about them can be found [here](https://slurm.schedmd.com/qos.html). There are also entries in the manual pages for various SLURM commands about QOS[^1].

[^1]: Capitalization doesn't matter when specifying QOS in commands.

QOS definitions for users and partitions are stored in a database.

We have a [document containing useful QOS-related commands](tips-sacctmgr.md). Those commands include ones like this one which allow you to see the value of all defined QOS in a readable format:
: `sacctmgr show qos format=Name%20,Priority,Flags%30,MaxWall,MaxTRESPU%20,MaxJobsPU,MaxSubmitPU,MaxTRESPA%25`
For your convenience we've put that command into a shell script called `showqos`

## Using QOS
If you need to submit a job with a particular QOS, AND that QOS is in **_both_** your **and** the specified partition's AllowedQOS list (see below sections), then you need to add an extra SLURM directive in your batch job or on the command line.

`sbatch --qos=shared-200-2 myjobscript`

You can modify a pending job with jobid# with this command:

`scontrol update job jobid# qos=shared-200-2`

Specifying a QOS that you do not have access to or which your specified partition does not allow will cause the job to be rejected immediately. Pending jobs which specify a QOS that you lose access to will probably fail the next time the scheduler inspects them. If not then, then when they are otherwise ready to be started.

## Partition QOS
Job partitions like `shared` have two QOS-related attributes:

1. Qos - If present, this specifies the QOS which by default applies to all jobs submitted. The `interactive` partition has `QoS=interactive-default`. The `shared` partition has `QoS=shared-default`.

2. AllowQoS - By default, this value is set to ALL. If set to a comma-separated list, then only those can be used or requested by users. The `interactive` partition has `AllowQos=normal,interactive-default`

You can see the _configuration_ of a partition with the scontrol command. ([Vendor's scontrol manual page](https://slurm.schedmd.com/archive/slurm-22.05.9/scontrol.html). Our [local scontrol tips](../slurm/tips-scontrol.md) page.) Here is a command which will show you the QOS attributes on an exmple partition named *partitionname*

```bash linenums="0"
scontrol show partition partitionname | grep -i qos
```

## User QOS

User accounts have two QOS-related attributes:

1. Qos - Our users (currently) all have a QOS value of "normal", (which is inherited from their parent "bank" account, named "jhpce").  The "normal" QOS  (currently) has no limits defined for it. (Therefore any QOS that applies to a user will come from either the partition's default QOS or is specified by the user (because they have access to an enhanced QOS).)

2. Allowed Qos - By default, this value is empty. If set to a comma-separated list, then the user can _choose_ to submit jobs using one of them.  Jobs request a QOS using the "--qos=" option to the sbatch, salloc, and srun commands. (If the partition does not allow a QOS to be used, then your job will be rejected.)

See your own user database values:
`sacctmgr show user withassoc where name=your-cluster-username`

The SLURM FAQ includes [an entry](../slurm/slurm-faq.md/#job-violates-accountingqos-policy) about the error you receive if you ask for more RAM in a single job than is allowed. 

If you submit jobs which together ask for more memory than you are allowed to use at one time, then ones that "fit" inside the limit will run and the remaining jobs will be waiting in PENDING state. 

The "Reason" code shown in the output of `squeue --me -t pd` ("show me my pending jobs") will be `QOSMaxMemoryPerUser`for jobs waiting for your running jobs to be using less than the RAM limit defined in the QOS that is impacting you.  The Reason will be `QOSMaxCpuPerUserLimit` for jobs waiting for your core usage to drop below that which is allowed.

You can see jobs running with a specific user-provided QOS with the command `squeue --qos <qos_list>` where qos_list can be one or a comma-separated list.


## HOW WE ARE USING QOS TO DATE...

This is a summary of how QOS is configured at JHPCE.

Our users all have a QOS value of "normal", which is
inherited from their parent account, named "jhpce"

(The "bank" account jhpce, of the organization jhpce, in the
cluster jhpce3 is the parent of all of the users.)

The "normal" QOS  (currently) has no limits defined for it.
(It is the default QOS defined in SLURM. We didn't create it.)

A fundamental element of our current practice is that the
user’s __default QOS__ entries in the database are "normal".

We’ve defined additional ones and, for some users, changed the Allowed QoS field in their user entries to list ones that they can optionally use __in addition__ to "normal". 
 
Those additional ones are being applied

1. via per-partition QOS= definitions in /etc/slurm/partitions.conf
    (e.g. QOS "interactive-default", "shared-default"), and 
2. via users specifying additional ones via per-job
    directives (b/c we granted them access to them and
    told them that they needed to use those directives).
    (Users do not have to specify optional QOS on every
    job thenceforward. Users can pick and choose what QOS to use for each job.
    


