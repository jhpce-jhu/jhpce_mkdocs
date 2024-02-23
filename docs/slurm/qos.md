---
tags:
  - needs-major-revision
  - slurm
---
# Quality of Service -- NEEDS REVISION FOR PUBLIC CONSUMPTION

## HOW WE ARE USING QOS TO DATE...

Our users all have a QOS value of "normal", which is
inherited from their parent account, named "jhpce"

(The account jhpce, of the organization jhpce, in the
cluster jhpce3 is the parent of all of the users.)

The "normal" QOS  (currently) has no limits defined for it.
(It is the default QOS defined in SLURM. We didn't create it.)

A fundamental element of our current practice is that the
user’s __default QOS__ entries in the database are "normal".

We’ve defined additional ones
and, given some users ones they can optionally use __in addition__
to normal. 
 
Those additional ones are being applied

1. via per-partition QOS= definitions in /etc/slurm/partitions.conf
    (e.g. QOS "interactive-default", "shared-default"), and 
2. via users specifying additional ones via per-job
    directives (b/c we granted them access to them and
    told them that they needed to use those directives).
    (Users do not have to specify optional QOS on every
    job thenceforward. Users can pick and choose what QOS to use for each job.
    

We have a [document containing useful QOS-related commands](tips-sacctmgr.md).
