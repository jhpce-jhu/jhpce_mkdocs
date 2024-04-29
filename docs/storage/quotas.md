# Disk Quotas

Disk quotas are used to control disk space for certain file systems. We use "hard" quotas. You are not allowed to use more than your quota.  

!!! Danger
    Reaching your disk quota can become an obstacle of simply logging in, as even a small file needed to record some detail about your login session, such as $HOME/.Xauthority, cannot be created. Keep your usage below your quota cap.

We use ZFS file systems for large volumes. Unfortunately, ZFS does not provide an end-user quota command with which to inspect your usage and remaining space.

Therefore we have configured our login nodes to display your home directory disk consumption and quota during the login process.

The figure shown during login are updated periodically. Every 30 minutes to an hour.

!!! tip
    We have written a `getquota` command, which will look up your or someone else's quota.

## Home Directory

In the JHPCE cluster, this quota is set to 100GB.

In the C-SUB cluster, this quota is set to 500GB.


### File Deletion and Delayed Change in Quota

When you delete files you may not see an immediate change in your disk consumption as far as the disk quota system is concerned.

You can see how much space you are using in your home directory with the commands

```Shell linenums="0"
cd
du -sh .
```

We use ZFS snapshots for home directories to make automated backups once an day[^1]. These are kept for a period of time[^2] so users and systems administrators can perform restores. See [this document](backups-restores.md) for instructions on performing your own restores!!!

[^1]: At eleven pm (as of 20240215).
[^2]: Fourteen days (as of 20240215).

Snapshots work by making a record of your files at an instant in time.  They take zero space at first. As your files change, disk space is consumed to hold the changed material. Snapshot consumption is counted as part of your disk quota.

Therefore it can take a number of days[^2] for files you have changed in the past but now deleted to stop being counted against your quota.

As you can imagine, the rate of change and size of files involved determines the amount of space held in snapshots.

If you find yourself in the position where you have run into your disk quota limit, have deleted significant amounts of files but are still impacted by your snapshot'ed files, please email us at bitsupport with the details. We will give you a temporary increase in disk quota to accomodate the snapshot "overhang"


## Fastscratch

The /fastscratch file system has a 1TB quota per user.

We have defined in the standard environment a variable `$MYSCRATCH` for users to use to access their space. (The actual absolute path to your personal scratch space is `/fastscratch/myscratch/$USER`)

There is no reporting system currently available to display fastscratch disk usage and your quota. You can use these commands to view your current usage:

```Shell linenums="0"
cd $MYSCRATCH
du -sh .
```
For more information on using fastscratch, please see [This page](fastscratch.md)
