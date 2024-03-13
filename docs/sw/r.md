## **Running R Jobs over SLURM**


### - Using R Interactively from command line

```
[test@login31 ~]$ srun --pty bash
srun: job 3176550 queued and waiting for resources
srun: job 3176550 has been allocated resources
[test@compute-152 ~]$ module load conda_R
Loading conda_R/4.3.x
(4.3.x)[test@compute-152 ~]$ module list

Currently Loaded Modules:
  1) JHPCE_ROCKY9_DEFAULT_ENV   2) JHPCE_tools/3.0   3) conda/3-23.3.1   4) conda_R/4.3.x

(4.3.x)[test@compute-152 ~]$ R

R version 4.3.2 Patched (2024-02-08 r85876) -- "Eye Holes"
Copyright (C) 2024 The R Foundation for Statistical Computing
Platform: x86_64-conda-linux-gnu (64-bit)
...
Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.
...
> 
```

### - Using R in batch mode

1. Write your R source in a file with .r extension (e.g. plot1.r)
```
# Set output file name
pdf("plot1-R-results.pdf")

# Define 2 vectors
cars <- c(1, 3, 6, 4, 9)
trucks <- c(2, 5, 4, 5, 12)

# Graph cars using a y axis that ranges from 0 to 12
plot(cars, type="o", col="blue", ylim=c(0,12))

# Graph trucks with red dashed line and square points
lines(trucks, type="o", pch=22, lty=2, col="red")

# Create a title with a red, bold/italic font
title(main="Autos", col.main="red", font.main=4)
```

2. Write a submit job script (e.g. plot1.sh)
```
#!/bin/bash

#SBATCH --mem=2G
#SBATCH --time=2:00

module load conda_R
R CMD BATCH plot1.r
```

3. Submit your R job
```
[test@login31 ~]$ sbatch plot1.sh 
Submitted batch job 3177833
```

4. Monitor the job status
```
[test@login31 ~]$ squeue --me
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           3177833    shared plot1.sh    test  R       0:03      1 compute-095
```

5. When the job is finished, the output files are created in your Current Working Directory
```
-rw-r--r--    1 test test        22 Mar 13 16:29  slurm-3177833.out
-rw-r--r--    1 test test      4988 Mar 13 16:29  plot1-R-results.pdf
```

6. Look at the result file, plot1-R-results.pdf; you can use xpdf to view it if you have set X11 forward when login.
   Otherwise, you need download the file to local machine to view it.
```
[test@login31 ~]$ xpdf plot1-R-results.pdf
```
