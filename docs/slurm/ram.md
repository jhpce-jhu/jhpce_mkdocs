## Requesting Additional Memory
All jobs need to use a certain amount of Memory or RAM (We use the terms Memory and RAM interchangeably) in order
to run.
By default on the JHPCE cluster when you submit a job with sbatch, or run srun, you are allotted 5GB of RAM   
for your job. 

If you are running a larger job and need more than the 5GB of RAM, you can request more (or less) RAM 
via 2 options:
```console
    --mem : memory per node (for all cores used)
    --mem-per-cpu : memory per core (harder to accurately estimate)
```
Some examples of using these options:
```console
sbatch --mem=10G job1.sh
```
- This would give your batch job a total of 10GB of RAM
```console
srun --mem-per-cpu=5G --cpus-per-task=4 --pty --x11 bash
```
- This would give your interactive session a total of 20GB of RAM and 4 cores

## Tools to use to get RAM usage information:

### sacct & sstat
The `sacct` command is used to query the SLURM job accounting database, usually
for jobs which have ended (one way or the other). Of course, if a job has not
started or finished, `sacct` cannot show you things it does not yet know. The
`sstat` program can look at stats about running jobs.

This page has instructions & examples:<br>
[https://jhpce.jhu.edu/slurm/tips-sacct](https://jhpce.jhu.edu/slurm/tips-sacct)

### seff & reportseff
seff is a program which looks up accounting data using the `sacct` command to
show some stats for a single completed job. `reportseff` allows you to look
over the information for a number of jobs.

This page has instructions & examples:<br>
[https://jhpce.jhu.edu/slurm/tips-reportseff](https://jhpce.jhu.edu/slurm/tips-reportseff)

## Estimating RAM usage
There is, sadly, no easy formula to know ahead of time how much RAM a job will need when working with large data. Every program has its own way of consuming RAM, and the size or complexity of a dataset also matters. Usually, an iterative process is required to determine the best value to use for your jobs.

Here are some tips to help find good values for your job:

- You can run a test job on a small subset of data, then a larger one.  From this kind of data you should be able to extrapolate the amount of RAM your full job will need. The `seff` and `reportseff` commands are useful here.
- One good place to start is to look at the size of the files you will be reading in. Add a bit extra, as a starting point.  If your job is reading in a 20GB image file, you may want to ask for 25GB or RAM. (But your code may wind up reading in segments of the file and not grow as much. Or it might copy the original data into a second array, thereby doubling the amount of space needed.)
- You can run `sacct` to gather info on a completed job, where JOBID is its number.</br>
> sacct -o JobID,JobName,ReqTRES%40,MaxVMSize,MAXRSS,State%20 -j JOBID
- You can run `sstat` to gather info on a running job.</br>
> sstat -a -o JobID,MaxVMSizeNode,MaxVMSize,AveVMSize,MaxRSS,AveRSS,MaxDiskRead,MaxDiskWrite,AveCPUFreq,TRESUsageInMax -j JOBID
- If the `STATE` from sacct command is `OUT_OF_MEMORY` it means that you job has run out of RAM, and you will need to resubmit your job with a larger RAM request.

## Impacts of RAM requests

Try to make your RAM request slightly higher than your expected usage.

   - Too low and your job will get killed for exceeding your request
   - Too high and
     - your job may take longer to get scheduled,
     - you’ll be squatting on RAM that others can use,
     - you'll be able to run fewer jobs if your partition has a quota, and
     - the job will cost more.

To get a feeling for what amounts of RAM are available on compute nodes, run the ```slurmpic``` command. You will see the total core and RAM on each node, as well as current core and RAM usage/availability.  If you want to request 750GB, you will see how many/few nodes could be considered candidates for your job.  About the three RAM-related columns:

- TOT_MEM - Total amount SLURM can use
- FREEMEM - Amount remaining that SLURM jobs can use. (When you request RAM, it is reserved for you. SLURM takes TOT_MEM and adds or subtracts the per-job requested RAM values to arrive at FREEMEM.)
- SYSFREEMEM - You can ignore this. It is the value that the node says is unused by any process. UNIX is aggressive about using spare memory to cache and buffer data. Such use is only cleared when the memory is actually needed by a different program. (It can reveal, though, if users requested lots of RAM but then didn't use it (yet).)
