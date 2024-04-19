# Backups and Restores

## Caveat
We try to protect your data, but ultimately you need to keep copies of your
most vital files elsewhere.

## Home directories
We do backup all users' home directories on a nightly basis in 2 manners. First
we use ZFS snapshots to create a point-in-time view of the home directory.  We
maintain 2 week's work of snapshots, so if you need to recover a file from
your home directory to a state that it was in within the last 2 weeks, you
can restore you files using the instructions in the
[Self Service Restores](#self-service-restores) section below.

We also do nightly disk-to-disk backups from the cluster located at Bayview to
a storage array in the BSPH building. We retain snapshots of these backups
using what is commonly called a Grandfather-Father-Son (GFS)
backups scheme (or Grandparent-Parent-Child using non-gendered terminology). 

So, we have daily backups that we keep for 1 week, weekly backups (taken on
Sunday) that we keep for 1 month, monthly backups (taken on the 1st of the
month) that we keep for 1 year, and yearly backups (taken on January 1st) that
we keep for 10 years.  So we wouldn't be able to recover a file as it looked
on a specific date, say October 18, 2019, but we would have the file as it was
on January 1st 2019 or 2020. 

File recovery from the backup server would be done via a request
to bitsupport@lists.jhu.edu.

## Backing up other filesystems
In addition to home directories, we also have a number of PIs that have
requested to have backups of their DCS and/or DCL spaces. We do charge for
backing up project space, and backup space is billed space is via the standard
JHPCE quarterly billing process. The cost has historically been about
$20/TB/yr.  If you are interested in backing up your project storage
space, please email us at bitsupport@lists.jhu.edu.

### Self Service Restores
We make snapshots of the /users file system for fourteen days. You can restore files you have deleted recently by changing directory to the appropriate location and then copying the file or files back to your home directory (or anywhere else you desire).

At any one time there are fourteen subdirectories in the path `/users/.zfs/snapshot`

Here is an example of looking through the collection of snapshots to find copies of a file you want to restore. Let's say that you deleted a file inside your home directory named susan that was stored in the absolute path `/users/your-userid/bob/frank/susan` You can see from the `ls -ld` output when the file existed and also the size of the possibly various versions of that file across the collection of snapshots (perhaps you changed it several times in the last two weeks).

```ShellSession
cd /users/.zfs/snapshot
ls -ld */your-userid/bob/frank/susan
cp -p 2024-02-16-23:00/your-userid/bob/frank/susan $HOME/restored-susan
```

If restoring substantial amounts of data, please do that work on a compute node instead of a login node. Thank you.
