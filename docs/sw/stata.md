## **Running basic Stata jobs over SLURM**

### - Using Stata interactively

```
[test@login31 ~]$ srun --mem 10G --x11 --pty bash
srun: job 3179341 queued and waiting for resources
srun: job 3179341 has been allocated resources
[test@compute-092 ~]$ module load stata
[test@compute-092 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) stata/17

[test@compute-092 ~]$ xstata
```

### - Using Stata in batch mode

1. Write your Stata source in a file with .do extension (e.g. stata-demo1.do)
```
program define hello
di "Hello There World 123"
end
hello
```

2. Write a submit job script (e.g. stata-demo1.sh)
```
#!/bin/bash

#SBATCH --mem=2G
#SBATCH --time=2:00

module load stata
stata -b stata-demo1.do
```

3. Submit your matlab job
```
[test@login31 ~]$ sbatch stata-demo1.sh
Submitted batch job 3179530
```

4. Monitor the job status
```
[test@login31 ~]$ squeue --me
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            3179530    shared stata-de    test  R       0:01      1 compute-092
```

5. When the job is finished, the output files are created in your Current Working Directory
```
-rw-r--r-- 1 test test   0 Mar 13 17:29 slurm-3179530.out
-rw-r--r-- 1 test test 890 Mar 13 17:29 stata-demo1.log
```

6. Look at the result
```
[test@login31 ~]$ cat stata-demo1.log
...
. do "stata-demo1.do" 

. program define hello
  1. di "Hello There World 123"
  2. end

. hello
Hello There World 123

. 
end of do-file
```
