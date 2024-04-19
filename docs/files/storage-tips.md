# STORAGE TIPS

## How much disk space are my files using?

There are several ways to get information about the amount of disk space used
by files and directories in a Linux environment.

The most common way to see how much space a file is using is with the
``ls -l`` command.

```
[login31 /users/mmill116]$ ls -l .bash_history 
-rw------- 1 mmill116 mmi 586047 Apr 19 12:35 .bash_history
```
The middle column of numbers gives the size in bytes.  So, my .bash_history file
is 586,047 bytes in size.

You can also use the ``du`` (disk usage) command to find the total disk space
usage for files and directories.  By default, the ``du`` command will
report the size of the file files in KB.  For example:
```
[login31 /users/mmill116]$ du .bash_history
229 .bash_history
```
reports that my .bash_history is using 229KB of space. Now, you may have
noticed that the output of ``du`` doesn't match the value shown in ``ls -l``.
This is due to the fact that most of the storage systems on JHPCE use native
ZFS compression to minimize the amount of actual disk space a file is using.
So, if you want to see how much disk space a file is using without the
compression you would add the ``--apparent-size`` option to your 
``du`` command.
```
[login31 /users/mmill116]$ du --apparent-size .bash_history 
573 .bash_history
```
This shows us a value that more closely matches the value from the ``ls``
command (586047 bytes / 1024 bytes/KB = 573 KB).

You can also add the ``-h`` option to the ``du`` command to show
human-readable units. This is helpful when dealing with files that are many
GBs in size.
```
[login31 /users/mmill116]$ du --apparent-size -h .bash_history 
573K    .bash_history
```

Another commonly used option is the ``-s`` option.  This is typically used on
directories, and will sum up the total amount of disk space used by all of the
files within the directory, and all subdirectories therein.
```
[login31 /users/mmill116]$ du -sh --apparent-size R
^C
[login31 /users/mmill116]$ srun --pty bash
srun: job 4707899 queued and waiting for resources
srun: job 4707899 has been allocated resources
[compute-048 /users/mmill116]$ time du -sh --apparent-size R
4.9G    R

real    4m49.219s
user    0m0.406s
sys 0m7.840s

```
Note that this can take quite a long time to run on directories with
a lot of files or subdirectories in them. In the above example, I started
the ``du`` command on the login node, and after a minute I cancelled it,
logged into a compute node, and ran it from the compute node, so as not to put
a heavy IO load on the login node.

It took nearly 5 minutes to run, and found
that the files in my R directory are using about 4.9GB  of actual space.  I ran
this again without the ``--apparent-size`` option, and found that my R
directory is using about 4.2 GB of actual disk space, factoring in the
compression.
```
[compute-048 /users/mmill116]$ time du -sh R
4.2G    R

real    0m51.687s
user    0m0.486s
sys 0m11.553s
```
It also ran much quicker this time, due to caching done on the compute node.
When data is accessed from a storage array, it is cached in the node's RAM
for a while so that subsequent access to the same data will be done more
efficiently.

### Another example of the effect of compression

As mentioned above, we have compression enabled on the storage arrays on the
JHPCE cluster.  An extreme example of compression at work can be seen if we
create a file of all zeros, which is very easliy compressible, vs a file of 
random data, which is not easily compressible.
```
[compute-048 /users/mmill116]$ dd if=/dev/zero of=$MYSCRATCH/zero-file bs=1M count=10000
10000+0 records in
10000+0 records out
10485760000 bytes (10 GB, 9.8 GiB) copied, 60.8073 s, 172 MB/s
[compute-048 /users/mmill116]$ dd if=/dev/urandom of=$MYSCRATCH/zero-file-rand bs=1M count=10000
10000+0 records in
10000+0 records out
10485760000 bytes (10 GB, 9.8 GiB) copied, 181.059 s, 57.9 MB/s
```
If we now use the ```du``` command with and without the ```--apparent-size``` 
option, we can see how compression makes a difference.
```
[compute-048 /users/mmill116]$ du -sh $MYSCRATCH/zero-file
512 /fastscratch/myscratch/mmill116/zero-file
[compute-048 /users/mmill116]$ du -sh $MYSCRATCH/zero-file-rand
9.8G    /fastscratch/myscratch/mmill116/zero-file-rand
[compute-048 /users/mmill116]$ du -sh --apparent-size $MYSCRATCH/zero-file
9.8G    /fastscratch/myscratch/mmill116/zero-file
[compute-048 /users/mmill116]$ du -sh --apparent-size $MYSCRATCH/zero-file-rand
9.8G    /fastscratch/myscratch/mmill116/zero-file-rand
```

## How much space do I have available?

As you're working on the JHPCE cluster, you may come across situations where
your job reports that it is out of disk space.  There are 2 main limiting
factors to space usage on the JHPCE cluster.  One is the user quotas that are
in place on home directories and fastscratch. The other is the size of the
filesystem that you're working in.

### Home directory quota

If you are working out of your home directory, and receive a message that you 
are out of disk space, you can see how much if your 100GB quota you are using
by running the ``hquota`` command.
```
[compute-048 /users/mmill116]$ hquota
Username     Space Used         Quota     
mmill116     62G                100G      
```
If you have exhausted your quota you should remove some files in your home
directory to free up some space.  Note that the ``hquota`` information is updated
every 15 minutes, so if you delete files, it may take some time for the change
to be reflected in the ``hquota`` output.

### Fastscratch quota
All users have a 1TB quota on their fastscratch space.  To see how much space
you are using in your fastscratch space, you can use the ``du`` command.

```
[compute-048 /users/mmill116]$ du -sh $MYSCRATCH
9.9G    /fastscratch/myscratch/mmill116
```

### Filesystem Usage - project space

If you are working in a project storage space and you receive an error that
you are out of disk space, you can check the amount of available storage by
using the ``df`` command.
```
[compute-048 /users/mmill116]$ df -h /dcs04/proj1/data
Filesystem                       Size  Used Avail Use% Mounted on
192.168.11.209:/srv/dcs04/proj1   60T   60T   43G 100% /dcs04/proj1
```
In this example the entire 60TB of /dcs04/proj1/data is used, and 
people using that space will need to delete some files to free up space. You
may want to check with your PI to see if they have another space available.
If you need more space, please reach out to us at bitsupport@lists.jhu.edu.

### Filesystem Usage - /tmp space
Many programs by default will use /tmp for storing temporary files.  While
this is fine for a single-use system, in
a shared environment where multiple users are accessing and utilizing /tmp
simultaneously, there's a risk of resource contention and performance issues.
If one user's processes generate large temporary files in /tmp, it can consume
valuable disk space and potentially impact the performance of other users'
processes. This can lead to slowdowns, crashes, or even denial of service for
users relying on the shared resources. Overall, it's advisable to avoid using
/tmp for and instead use your fastscratch space for temporary files

If you must use /tmp you should first check to make sure that there is
sufficient space for your temporary files. You could, for example, use
the code below to make sure there is ate least 10GB (10000000 KB) in /tmp.
```
FREETMP=`df -k /tmp | grep tmp | awk '{print $4}'`
if [ $FREETMP -lt 10000000 ]
then
   echo "Not enough space in /tmp. Only $FREETMP KB available."
   exit 1
fi
```
You should also make sure that your job deletes the files it created in /tmp.

Different applications will have different options for specifying the location
to use for temptoray files.  While we can't provide an exhausive list, here 
is how some commonly used applicaiotn on JHPCE set their temporary location.
  + In R, the `tmpdir()` setting will dictate where temporary files are stored.
    If you are generating 10s of GB of temporary files, change `tmpdir()` to
`fastscratch`.
  + In SAS, the default `WORK` directory will be located under your
    `fastscratch` directory.
  + In Stata, the default `tempfile` location is under `/tmp`. This can be
    changed by setting the `STATATMP` environment variable.

# Backing up storage
Home directory spaces get backed up nightly, however other project spaces may
not.  You should check with your PI to see if your project space is getting
backed up.

If not, you should be sure to copy any unique or difficult-to-repoduce
results to your home directory, or transfer them off of the JHPCE cluster, so
that you have a backup of the files.  See [this document](../storage/backups-restores.md)
for more information.

