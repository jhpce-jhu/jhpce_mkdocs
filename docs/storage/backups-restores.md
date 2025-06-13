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
We make snapshots of the /users file system for fourteen days (currently at 11:30pm). You can restore files you have deleted recently by changing directory to the appropriate location and then copying the file or files back to your home directory (or anywhere else you desire).

Files created and deleted in-between snapshots are not recoverable.

At any one time there are fourteen subdirectories in the path `/users/.zfs/snapshot`

Here is an example of looking through the collection of snapshots to find copies of a file you want to restore. Let's say that you deleted a file inside your home directory named susan that was stored in the absolute path `/users/your-userid/bob/frank/susan` You can see from the `ls -ld` output when the file existed and also the size of the possibly various versions of that file across the collection of snapshots (perhaps you changed it several times in the last two weeks).

```ShellSession
cd /users/.zfs/snapshot
ls
2025-05-29-23:30/  2025-06-01-23:30/  2025-06-04-23:30/  2025-06-07-23:30/  2025-06-10-23:30/
2025-05-30-23:30/  2025-06-02-23:30/  2025-06-05-23:30/  2025-06-08-23:30/  2025-06-11-23:30/
2025-05-31-23:30/  2025-06-03-23:30/  2025-06-06-23:30/  2025-06-09-23:30/  2025-06-12-23:30/

     HERE YOU RUN YOUR COMMANDS TO POKE AROUND AND IDENTIFY WHAT IS PRESENT
     IN ANY PARTICULAR SNAPSHOT
     SUCH AS LOOKING FOR A FILE NAMED "susan" AND CHECKING ON ITS DATE OR SIZE IN THE
     VARIOUS SNAPSHOTS. (REPLACE "your-userid" WITH YOUR CLUSTER USERNAME.)

ls -ld */your-userid/project1/sample5/susan
cp -p 2024-02-16-23:00/your-userid/project1/sample5/susan $HOME/restored-susan
```

Tips:

- If restoring substantial amounts of data, please do that work on a compute node instead of a login node.
- Take care not to overwrite existing files in your home directory -- create a restore directory and put stuff in there, for example. Since snapshots are only made once a day, if you nuke files in-between snapshots you cannot get them back.
- Consider using command options which retain original timestamps or other attributes of a file. For `cp` that is `-p`. For rsync there are a variety of options which impact the copying. The typically-used `-a` ("archive") flag is shorthand for a list of options, including `-t` for timestamps.
- If your files of restore interest include ones with ACL rules or symbolic links etc, you may need to use additional `rsync` arguments to keep those parameters.

