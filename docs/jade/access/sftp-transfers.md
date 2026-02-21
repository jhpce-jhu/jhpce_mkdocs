---
tags:
  - in-progress
  - jade
---

# SFTP Transfers Into and Out of JADE

## Overview
A primary directive for JADE is that Controlled Unclassified Information (CUI) is not allowed to leave the cluster unless we can log the essential details, which are: which user transferred which file at what time.

SFTP was chosen as the primary command to allow because we can configure allowed data locations, control which commands are executed, and log transfers at the appropriate level. JADE serves multiple communities, each of which can have unique data movement requirements. (In particular, the "cms" community must have specific users approve each outbound file on behalf of the file's owner.)

SFTP transfers are only allowed to be initiated from outside of JADE (so the connections are controlled by the SSH configuration files on the JADE server). Therefore the `scp` and `sftp` programs are not executable by users. This does not prevent users from installing their own copies. Any user who does so then bears the legal responsibility of the consequences if CUI files are found out on the Internet which came from JADE.

## Ports
JADE supports multiple communities of users. To date we have been assigning two specific TCP/IP ports to each community, one for inbound and one for outbound transfers. (This is required to implement the aforementioned SSH configuration options.)

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

## /transfer file system
CMS users connecting to their ports, are placed by the SFTP server configuration, in specific directories: `/transfer/in/cms/` and `/transfer/out/cms/`.  Their directories are found below that starting point.
