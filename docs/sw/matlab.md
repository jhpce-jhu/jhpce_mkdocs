## **Running basic Matlab jobs over SLURM**

### - Using Matlab interactively

```
[test@login31 ~]$ srun --mem 10G --x11 --pty bash
srun: job 3109996 queued and waiting for resources
srun: job 3109996 has been allocated resources
[test@compute-152 ~]$ module load matlab
[test@compute-152 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) matlab/R2023a

[test@compute-152 ~]$ matlab
```

### - Using Matlab in batch mode

1. Write your matlab source in a file with .m extension (e.g. matlab_example.m)
```
x = [1 2 3 4];
fprintf('Example number = %i\n', x)
```

3. Write a submit job script (e.g. matlab_example.sh)
```
#!/bin/bash

#SBATCH --mem=2G
#SBATCH --time=5:00

#SBATCH -o slurm.%N.%J.%u.out   # STDOUT
#SBATCH -e slurm.%N.%J.%u.err   # STDERR

module load matlab
matlab -nojvm -nodisplay -r "matlab_example;quit;"
```

3. Submit your matlab job
```
[test@login31 ~]$ sbatch matlab_example.sh 
Submitted batch job 3110505
```

4. Monitor the job status
```
[test@login31 ~]$ squeue --me
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           3110505    shared matlab_e    test  R       0:03      1 compute-100
```

5. When the job is finished, the output files are created in your Current Working Directory
```
-rw-r--r-- 1 test test   0 Mar 12 17:09 slurm.compute-100.3110505.test.err
-rw-r--r-- 1 test test 410 Mar 12 17:10 slurm.compute-100.3110505.test.out
```

6. Look at the results from output file, file with stderr is empty(job without errors):
```
[test@login31 ~]$ cat slurm.compute-100.3110505.test.out
                                May 25, 2023

For online documentation, see https://www.mathworks.com/support
For product information, visit www.mathworks.com.
 
Example number = 1
Example number = 2
Example number = 3
Example number = 4
```
