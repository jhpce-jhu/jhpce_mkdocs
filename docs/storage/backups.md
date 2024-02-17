---
tags:
  - in-progress
---

# Backups and Restores

## Caveat
We try to protect your data, but ultimately you need to keep copies of your most vital files elsewhere.

## Home directories
Frequency, restore window. Form to request restores. Or just a description of what is needed (what is missing? when did you last see it? where do you want it restored to?)

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

## Project space
We offer **optional** backup service.

What's included. Frequency, restore window, cost.

Limit on what we'll consider (ONLY whole file systems?)

Form to request restores. Or just a description of what is needed.
