# Backups and Restores

## Caveat
We try to protect your data, but ultimately you need to keep copies of your
**most vital files** elsewhere. That's the reality of any data protection scheme.

Our storage servers in the ARCH data center near Bayview Hospital use RAID across many hard drives to protect against data loss caused by failures of individual disks. We have a backup server in the Bloomberg School of Public Health building. That also uses RAID, but holds only a few project file systems.

## Back up of project data filesystems
We offer petabytes of data file systems from our DCS (Dirt Cheap Storage) servers such as dcs07. PIs can choose to
requested to have backups made of their DCS spaces.  Only a minority of these allocations are backed up. We charge for backing up project space via the standard JHPCE quarterly billing process. The cost has historically been about around $20/TB/yr.  If you are interested in backing up your project storage space, please email us at bitsupport@lists.jhu.edu.

## Home directories
We do backup all users' home directories on a nightly basis in 2 manners.

### ZFS snapshots

We use ZFS snapshots to create a point-in-time view of the home directory.  We
maintain 2 week's work of snapshots, so if you need to recover a file from
your home directory to a state that it was in within the last 2 weeks, you
can restore you files using the instructions in the
[Self Service Restores](#self-service-restores) section below.

### Backups to external server

We also do nightly disk-to-disk backups from the cluster located at Bayview to
a storage array in the BSPH building. We retain snapshots of these backups
using what is commonly called a Grandfather-Father-Son (GFS)
backups scheme (or Grandparent-Parent-Child using non-gendered terminology). 

Data retention:

We have daily backups that we keep for 1 week, weekly backups (taken on
Sunday) that we keep for 1 month, monthly backups (taken on the 1st of the
month) that we keep for 1 year, and yearly backups (taken on January 1st) that
we keep for 10 years.  So we wouldn't be able to recover a file as it looked
on a specific date, say October 18, 2019, but we would have the file as it was
on January 1st 2019 or 2020. 

File recovery from the backup server would be done via a request to bitsupport@lists.jhu.edu.

!!! Note "What we need to know to do restores"
    
    As many details as possible, especially if you are unsure of things such as exact file name patterns. Items to include:
    
    1. What is the full path to the files? If you don't know that, at what point in the path does your uncertainty begin?
    2. If you don't know the names of the files, do you have any description of them, such as file name substrings or suffixes? Rough file sizes?
    3. When did the files last exist?
    4. When do you think they were deleted or overwritten?
    5. Where do you want files restored to?
    6. If the files were owned by a different user, who should own the restored files? Please try to have the original owner or the PI contact us to give their permission for any change of owner or access.

### Self Service Restores
We make snapshots of the /users file system for fourteen days (currently at 11:30pm). You can restore files you have deleted recently from one or more of those directories. 

Files created and deleted in-between snapshots are not recoverable.

At any one time there are fourteen subdirectories in the path `/users/.zfs/snapshot/` They have the form of `YYYY-MM-DD-HH:MM` Immediately below them are the home directories for everyone, which are named `<your-userid>`.

```ShellSession
ls /users/.zfs/snapshot
2025-05-29-23:30/  2025-06-01-23:30/  2025-06-04-23:30/  2025-06-07-23:30/  2025-06-10-23:30/
2025-05-30-23:30/  2025-06-02-23:30/  2025-06-05-23:30/  2025-06-08-23:30/  2025-06-11-23:30/
2025-05-31-23:30/  2025-06-03-23:30/  2025-06-06-23:30/  2025-06-09-23:30/  2025-06-12-23:30/
```
WORKING WITH THESE DIRECTORIES

It is common to not know which files are available to restore, and which ones are the best candidates.

Do **not** try to drill down by simply changing directory with `cd` into a directory tree and then `ls` each level to see what is present!

It is important for you to know that these diretories are actually file systems which are *_created on the fly_* by the operating system kernel. It frequently happens very quickly in your working with these file systems that the operating system gets confused and begins preventing you from accessing some of them. That should not happen, but it does. The error you will see will look like this:

```Shellsession
cd /users/.zfs/snapshot/
cd 2026-04-09-23:30
bash: cd: 2026-04-09-23:30: Stale file handle
```

After some period of time where you don't try to work with that directory again, the files **will** become accessible again. This confusion is also confined to the particular computer you are logged into at the time. (So you can move to a different machine and try to use the knowledge you have gained from your previous search attempts to begin at a newer level.)

**What we have found to work best is to use your understanding of the paths to your home directory** 

`/users/.zfs/snapshot/<YYYY-MM-DD-HH:MM>/<your-username>`

**to perform tight, controlled file listing commands from outside of the `/users/.zfs/snapshot/` directory.**

Let's say that you deleted a file inside your home directory named `<something>susan.<some-suffix>` which was stored in the absolute path `/users/your-userid/bob/frank/`  What are the actual file names? When did they exist, and with what size (you might have changed it several times in the last two weeks)?

Use appropriate wildcards and extra arguments for `ls` to see as much relevant information as possible in the fewest commands. To try to avoid the dreaded "stale file handle" situation.

`ls -ld /users/.zfs/snapshot/*/<your-username>/bob/frank/*susan*`

??? Tip "Using wildcards in paths and dealing with dot files"
    By wildcards, I mean “*” which represents zero or more characters in any combinations, and “?” which represents any one single character.
    
    By extra flags for `ls`, I mean use `ls -ld` to get a long listing (the “l” argument) and not provide a listing of the contents of directories (unless you need to).
    
    You also need to understand that, by default, the `ls` command does not show “dot files” like `.bashrc`. And the asterisk wild card does not include dot files. To see a file containing the string `bashrc` which might be a dot file, you would use `ls -ld <path>/.*bashrc* <path>/*bashrc*`

The command

`ls -ld /users/.zfs/snapshot/2026-0?/<your-username>/.*rc`

will list all of your dot files ending in “rc” in all of the backup directories for the first nine months of 2026. (We only keep 14 directories at a time, but we're illustrating the use of wildcards.)

!!! Tip "Tips for restoring"
    - If restoring substantial amounts of data, please do that work on a compute node instead of a login node.
    - Take care not to overwrite existing files in your home directory -- create a scratch restore directory and put stuff in there
    - If needing to do a recursive file copy, instead of using `cp -rp`, consider using `rsync -av`
    - Consider using command options which retain original timestamps or other attributes of a file. For `cp` that is `-p`. For rsync there are a variety of options which impact the copying. The typically-used `-a` ("archive") rsync flag is shorthand for a list of options, including `-t` for timestamps and `-p` for permissions.
    - If your files of restore interest include ones with ACL rules or symbolic links etc, you may need to use additional `rsync` arguments to keep those parameters. `cp -rp` will **not** recreate symbolic links. See the manual pages for `cp` and `rsync` for details.

```ShellSession
mkdir ~/restored-susan

# If susan is a file
cp -p 2024-02-16-23:00/<your-userid>/project1/sample5/susan ~/restored-susan/

# If susan is a directory
rsync -av 2024-02-16-23:00/<your-userid>/project1/sample5/susan/ ~/restored-susan/
```






