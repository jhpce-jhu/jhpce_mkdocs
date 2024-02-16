---
tags:
  - in-progress
---

# Monitoring SLURM Jobs

Making sure your jobs request the right amount of RAM and the right number of CPUs helps you and others using the clusters use these resources more effeciently, and in turn get work done more quickly. Below are some examples of how to measure your CPU and RAM (aka memory) usage so you can make this happen.

## sstat 
display process statistics of a running job/step
https://slurm.schedmd.com/archive/slurm-22.05.9/sstat.html

## sacct
display accounting data for jobs in the Slurm database
https://slurm.schedmd.com/archive/slurm-22.05.9/sacct.html

## Suggestions

The following was sent to a user who was having SAS jobs fail with errors indicating a problem with space or quota.

Instrumenting your job means adding commands to it to gather information. Running df commands to check the size and capacity of all or specific file systems. (df -h -x tmpfs might be a helpful variant). As mentioned in the orientation material, the sstat command can gather information for running jobs (as opposed to sacct which is oriented towards completed jobs). Some of the output fields available via sstat and sacct relate to memory usage while others can reveal disk input/output volumes.
 
Because your batch job script might not be able to run sstat’s during SAS’s execution, you may need to run the information-gathering commands interactively or via a second batch job. (I mean your batch job could run commands before or after invoking SAS, unless they put the SAS program into the background they won’t be able to run the commands while SAS is running. I don’t know what happens if one tries to run a program in the background (by putting an & after it, like one can run thunar for example) inside of a SLURM batch job.
 
Your gathering might involve the sleep command in between invocations. I would suggest also running the date command so you have timestamps.
 
You could put the sleep, date, sstat commands inside of a while (true) loop. Then cancel the loop with a control c or scancel depending on whether they are running interactive or batch. Redirecting the output to text files would be useful.
 
So your information-gathering efforts should probably start by running some sacct commands to inspect the parameters of previous failed jobs, and perhaps even the ones that succeeded to see if any differences appear.
 
The -e flag is available to both sstat and sacct to show the fields available. The sets overlap but also differ.
 
My text notes accumulated with sacct hints can give you ideas for ways to get at the info you want from sstat and sacct.
The arguments to the SACCT_FORMAT environment variable commands can be given on the command line with the -o or maybe the -O flag. But setting it in the environment variable means your commands can be shorter and easier to issue.
 
slurm:: sacct -S or -E now[{+|-}count[seconds(default)|minutes|hours|days|weeks]]
slurm:: sacct --helpformat # shows available fields # so does -e
slurm:: sacct: export SACCT_FORMAT="user,jobid,partition,nodelist,timelimit,submit,start,exitcode,state"
slurm:: sacct: export SLURM_TIME_FORMAT="%a %T"   #  https://strftime.org
slurm:: sacct: export SLURM_TIME_FORMAT="%a %m-%d %T"   #  https://strftime.org
slurm:: sacct: export SLURM_TIME_FORMAT="%a %m-%d %H:%M"   #  https://strftime.org
slurm:: sacct: export SACCT_FORMAT="user,jobid,jobname,nodelist%12,start%-15,end%-15,state%15,reqtres%40,maxrss,maxvmsize"
slurm:: sacct: export SACCT_FORMAT="user,jobid,jobname,nodelist%12,start%-15,end%-15,state%15,reqtres%40,TRESUsageInTot%60"
slurm:: sacct: export SACCT_FORMAT="user,jobid,jobname,nodelist%12,start%-20,end%-20,state%20,reqtres%40,TRESUsageInTot%200"
slurm:: sacct --allusers -o "user,jobid,state,nodelist,submit,start,end,exitcode,state"
slurm:: sacct -o SubmitLine%100 -j jobid # See what command user submitted
slurm:: sacct -B -j jobid # See what batch file user submitted
slurm:: sacct -o ALL -j jobid # See all fields for job
