## Using Python Interactively on command line

```
[test@login31 ~]$ srun --pty bash
srun: job 3168742 queued and waiting for resources
srun: job 3168742 has been allocated resources
[test@compute-100 ~]$ module load python
[test@compute-100 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) python/3.9.14

[test@compute-100 ~]$ python3
Python 3.9.14 (main, May 16 2023, 14:32:18) 
[GCC 11.3.1 20220421 (Red Hat 11.3.1-2)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

## Running Python program in batch mode

1. Write your python source in a file with .py extension (e.g. python_example.py)
```
values = [88, 47, 96, 85, 72]
for i in values:
    print('Example number = ', i)
```

2. Write a submit job script (e.g. python_example.sh)
```
#!/bin/bash

#SBATCH --mem=2G
#SBATCH --time=2:00

#SBATCH -o slurm.%N.%J.%u.out   # STDOUT
#SBATCH -e slurm.%N.%J.%u.err   # STDERR

module load python
python3 python_example.py
```

3. Submit your python job
```
[test@login31 ~]$ sbatch python_example.sh 
Submitted batch job 3169570
```

4. Monitor the job status
```
[test@login31 ~]$ squeue --me
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           3169570    shared python_e    test R       0:01      1 compute-153
```

5. When the job is finished, the output files are created in your Current Working Directory
```
-rw-r--r-- 1 test test   0 Mar 13 12:45 slurm.compute-153.3169570.test.err
-rw-r--r-- 1 test test 105 Mar 13 12:45 slurm.compute-153.3169570.test.out
```

6. Look at the results from output file, file with stderr is empty(job without errors):
```
[test@login31 ~]$ cat slurm.compute-153.3169570.test.out
Example number =  88
Example number =  47
Example number =  96
Example number =  85
Example number =  72
```

## Installing Your Own Packages

Please see [this document](../sw/python-pkg.md)
