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

* port number - community, direction
* 22011 - cms, inbound
* 22027 - cms, outbound
* 22111 - misc, inbound
* 22127 - misc, outbound
* 22211 - nist800171, inbound
* 22227 - nist800171, outbound
* 22311 - sysadmin, inbound
* 22327 - sysadmin, outbound
* 22511 - dbgap, inbound
* 22527 - dbgap, outbound

!!! note "Your working directory after connecting"
    In general: The SFTP server configured to handle each port will "place you" in your home directory. You can then navigate (change directory) to other locations before uploading or downloading files.
    
    CMS users connecting to their outbound port 22027, are placed by the SFTP server configuration in the specific directory `/transfer/out/cms/`.  Their directories are found below that starting point, in the generic path `<cdua>/<username>`.

## /transfer file system
When JADE was designed and built, it was felt that it would be helpful to the logging and auditing efforts to have files being uploaded into the cluster (on inbound ports 2*11) pass through the `/transfer/in/<comm>/` directory trees, and outbound files (on ports 2*27) be first placed into the `/transfer/out/<comm>/` directory trees.

As of March 2026, the only directory used inside `/transfer/` is `/transfer/out/cms/`  Members of the CMS community must place files they wish to transfer out of JADE into `/proposed/cms/<cdua>/<username>` directories. They then a team of HARP "data moderators" to review them. Those moderators move approved files from `/proposed/` to the `/transfer/out/cms/<cdua>/<username>/` so CMS users can then run SFTP from outside JADE to "pick them up."

Currently no communities moderate incoming files, so `/transfer/in/<comm>/` directories are not needed. We will keep them around in case we have to change course.

