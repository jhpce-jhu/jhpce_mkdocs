# Using Luster filesystems, such as DCL02 

The DCL02 storage array is built on the [Lustre](https://www.lustre.org/) filesystem.  The Lustre filesystem is a distributed, parallel object-store based filesystem which is used by a large number of HPC environments to provide high-performance storage access to large storage arrays.

The Lustre filesystem is optimized to perform well for batch access to large files. Unfortunately, Lustre has the drawback of performing less well when used in interactive sessions or when dealing with lots of small files.  So, if you are making use of space under a /dcl02 directory, here are some tips which can help improve performance when working on DCL02.

- In general, the /dcl02 space should be used for storing large files and not lots of small files.  If you can combine lots of files into a large file with, say, the tar command, and your program work within the tar file, your jobs will generally perform better than if you need to work with lots of small files.

- Luster may be less efficient in cases where there are lots (hundreds or thousands) of files in 1 directory.  You will see better performance is you can store these files in some sort of hierarchy. So instead of 1000 files in 1 directory, Lustre will work better if you split this into, say, 10 directories with 100 files in each directory.

- Related to this, when using the “ls” command, you should use the “ls --color=none” option, rather than just “ls”.  For “ls” to color code the output, it needs to query the DCL02 server for every file in the directory, and this can take a long time, especially when there are hundreds of files in 1 directory, and the DCL02 server is busy.

- Jobs should not be run  you run jobs out of your /dcl02 space. The scripts that you use to submit your job should be in another location, like your home directory, and run from there, with an appropriate “cd” in the script to go to the /dcl02 direcotry you are using.  Slurm will open a output file for your job in the directory from which the job was submitted, and will need to keep this file handle open for the duration of the job.

- The /dcl02 space should not be used for storing small temporary or intermediary files.  If your job is storing temporary files in your /dcl02 space, the performance of your job may suffer.  Rather, you should probably use your 1TB of fastscratch space for these temporary files.  We have instructions for using the fastscratch  space at [this link](fastscratch.md) .


