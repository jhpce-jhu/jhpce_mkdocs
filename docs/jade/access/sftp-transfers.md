---
tags:
  - in-progress
  - jade
---

# SFTP Transfers Into and Out of JADE

## Overview

SFTP transfers are only allowed to be initiated from outside of JADE (so the connections are controlled by the SSH configuration files on JADE servers). Therefore the `scp` and `sftp` programs are not executable by users. This does not prevent users from installing their own copies. Any user who does so then bears the legal responsibility of the consequences if CUI files are found out on the Internet which came from JADE.

## Ports
JADE supports multiple communities of users. To date we have been assigning two specific TCP/IP ports to each community, one for inbound and one for outbound transfers.

* Port number - Community name, Direction of data movement
* 22011 - cms, inbound
* 22027 - cms, outbound

* 22511 - dbgap, inbound
* 22527 - dbgap, outbound

* 22111 - misc, inbound
* 22127 - misc, outbound

* 22211 - nist800171, inbound
* 22227 - nist800171, outbound

* 22311 - sysadmin, inbound
* 22327 - sysadmin, outbound

## SFTP session restrictions
To comply with the NIST 800-171 data security standards, the JADE SFTP server daemons handling each port are configured to allow the use of only a subset of SFTP commands. For example, when connected to an outbound port, you are not allowed to "put" files (meaning upload them into the cluster). When connected to an inbound port, you are not allowed to "get" files (meaning download them from the cluster).

## Your working directory after connecting
Inbound connections: The SFTP server configured to handle incoming will "place you" in your home directory. You can then navigate (change directory) to other locations before uploading or downloading files.
    
The same is true for outbound connections for everyone except CMS users.

This group of users, when connecting to their outbound port 22027, are placed by the SFTP server in the specific directory `/transfer/out/cms/`.  Your SFTP software is unable to see any other paths on the computer -- you are locked at `/transfer/out/cms` and below. In other words, CMS users cannot change directory to `/users/<blah>` or `/data/<blah>`.  Below that starting point are directories for each "cdua", e.g. "c55548" Below those are CMS user per-person directories, in the generic path `<cdua>/<username>`.

## MobaXterm Users
MobaXterm allows you to use many different kinds of connections. SFTP Sessions are different than SSH Sessions. You should configure two "SFTP Session" so you are prepared to move data in both directions. You will wind up with three JADE-related Sessions saved in your Favorites (aka Bookmarks)(the gold star icon in the left-side toolbar). Once those are configured you can re-use them. It can be a good idea to, when defining an SFTP Session, to go into the "Bookmarks settings" tab and change the name of the session to reflect its use, say "JADE inbound SFTP".

!!! warning
    ==You need to change a special setting when defining SFTP sessions!!!== There is a check box in the “Advanced Sftp settings” tab, titled “2-steps authentication”. Enabling that tells MobaXterm to handle the prompt for a verification code.

## /transfer file system
Members of the CMS community must place files they wish to transfer out of JADE into `/proposed/cms/<cdua>/<username>` directories. They then ask a team of HARP "data moderators" to review the files. Those moderators move approved files from `/proposed/<blah>` to the appropriate `/transfer/out/cms/<cdua>/<username>/` so CMS users can then run SFTP from outside JADE to "pick them up."

When JADE was designed and built, it was felt that it would be helpful to the logging and auditing efforts to have files being uploaded into the cluster (on inbound ports 2*11) land in the `/transfer/in/<comm>/` directory trees, and outbound files (on ports 2*27) be retrieved only from the `/transfer/out/<comm>/` directory trees.

As of March 2026, the only directory used inside `/transfer/` is `/transfer/out/cms/`  

Currently no communities moderate incoming files, so `/transfer/in/<comm>/` directories are not needed. We may keep them around in case we have to change course.

