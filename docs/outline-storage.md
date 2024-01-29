# STORAGE TOPIC - page to develop outline

Whether this is all on one page or broken up into multiple pages, what about storage do we need to include in our site? Some of the items below are not yet included in our existing SGE-mentioning page.

## Types of storage
Mention local versus network.
Speed differences between SSD (dcs06, fastscratch) and spinning.


## Specific File Systems You Need To Know About
In table, include the environment variable you can use to refer to it. $HOME $FASTSCRATCH ...
What about compute node's /tmp? Users need to know about it, because they may not know that software often writes there, and they need to understand that it is a shared limited resource. if they are explicitly writing to /tmp they need to know to limit their usage and clean up after themselves

## Home directories
How to learn how much you're using. How often updated?
Disk quotas - only hard, no soft (so no warning)
Issue with snapshots preventing you from being able to immediately reduce usage. Ask for increase in disk quota while overhang is being deleted? 

## Buying additional storage space
Periodic offerings.
How paid for (and therefore length of committment)
What about requesting increases in /users quota?

## Backing up storage
Home directories. Frequency, restore window, cost.

Project space:
We offer it.
What's included. Frequency, restore window.
Limit on what we'll consider (ONLY whole file systems?)
Cost.
Form to request restores.

## Getting information about storage
du -sh
df -h
df -h .

Consider potential impact of running certain commands that will cause lots of I/O.
