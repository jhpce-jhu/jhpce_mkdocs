| Name|Explanation|Notes|
|-----|-----|----|
|BeginTime|The job's earliest start time has not yet been reached||
|Dependency|Job waiting on a user-defined dependency|See why with `showjob <jobid>|grep -i depend`|
|DependencyNeverSatisfied|Something went wrong|You should investigate why. Incorrect dependency specified?|
|JobArrayTaskLimit|Array job configured to run only so many tasks at a time|Good way to control resource use|
|JobHeldAdmin|The job is held by a system administrator||
|JobHeldUser|The job is held by the user||
|Priority|One or more higher priority jobs exist for this partition or advanced reservation||
|QOSJobLimit|The job's QOS has reached its maximum job count||
|QOSMaxCpuPerUserLimit|Your other running jobs have consumed your CPU quota|`slurmuser --me` will show your used/pending resources|
|QOSMaxMemoryPerUser|Your other running jobs have consumed your RAM quota|`slurmuser --me` will show your used/pending resources|
|Reservation|Job waiting for its advanced reservation to become available||
|Resources|Needed resources not currently available in partition|`slurmpic -p <partition>` can show you what's used & consumed in that partition|
| ReqNodeNotAvail| Some node specifically required by the job is not currently available|Usu seen when all nodes in a partition are unavailable|

