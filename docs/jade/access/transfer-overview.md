---
tags:
  - in-progress
  - jade
---

# Data Transfers Into, Out of & Within JADE

!!! note "SFTP Is The Supported Method"
    Currently (March 2026) the primary supported mechanism is SFTP transfers done through the login node, `jade01.jhsph.edu`. They must be initiated from outside the JADE cluster, using specific TCP/IP port numbers configured in your SFTP client software. See [details here](sftp-transfers.md).

## Overview
A primary directive for JADE is that Controlled Unclassified Information (CUI) is not allowed to leave the cluster unless we can log the essential details, which are: **which user** transferred **which file** at **what time** in which **direction**.

!!! warning "Ultimately Protecting CUI Is Your Responsibility"
    The system administrators have attempted to configure the cluster to do the things required by NIST 800-171. Users can bypass some of these measures if they wish to, by, for example, installing their own binaries or using R or Python routines/libraries. Our changes include some meant to make it harder for a user to use undesirable commands (e.g. `scp`) to copy data out, whether unintentionally by force of habit or by reading some Google suggestion.

    Any user who bypasses the supported SFTP method then bears the legal responsibility of the consequences if CUI files are found out on the Internet which came from JADE. We audit every access to CUI files so we will be able to provide Hopkins Legal with forensic data.

??? tip "Future Transfer Server"
    Later in 2026 we will create a server named `jade-transfer01` which will take over SFTP duties from `jade01`. It will also offer a `Globus Endpoint Server` equipped with `High Assurance` software. We will announce when the new server is available.

!!! warning "Some Programs Have Been Modified"
    We have modified programs which can be used to transfer data out of the cluster. See details below.

## Transfer tools
Here is a run down of common UNIX built-in commands used for transferring files. The primary tool to use is running SFTP on a computer outside of JADE.

### SFTP
Each community has been assigned a pair of specific TCP/IP port numbers, one for inbound data and one for outbound data. Users must configure their SFTP client software with the appropriate port numbers.

!!! warning "SFTP Should Be Initiated From Outside JADE"
    You cannot use sftp from inside the cluster. The binary is not executable by normal users.

SFTP was chosen as the primary command to allow because we can configure allowed data locations, control which commands are executed, and log transfers at the appropriate level. JADE serves multiple communities, each of which can have unique data movement requirements. (In particular, the "cms" community must have specific users approve each outbound file on behalf of the file's owner.)

For more SFTP details see [this document](sftp-transfers.md).

### Curl
[](){#jade-curl-details}
`Curl` has been disabled. Please use `wget` instead.

### Globus
[](){#jade-globus-details}
In 2026 we will create a server to be named `jade-transfer01` which will take over SFTP duties from `jade01`. It will also offer a Globus Endpoint Server equipped with `High Assurance` software.

### rsync
[](){#jade-rsync-details}
`rsync` has been disabled. 

### scp
[](){#jade-scp-details}
`scp` has been disabled. 

## The /transfer file system
[](){#the-/transfer-file-system}
When JADE was designed and built, it was felt that it would be helpful to the logging and auditing efforts to have files being uploaded into the cluster (on inbound ports 2*11) pass through the `/transfer/in/<comm>/` directory trees, and outbound files (on ports 2*27) be first placed into the `/transfer/out/<comm>/` directory trees.

As of March 2026, the only directory used inside `/transfer/` is `/transfer/out/cms/`  Members of the CMS community must have files they wish to transfer out of JADE first be reviewed by a team of HARP "data moderators." Those moderators move approved files from `/proposed/cms/` directory to the `/transfer/out/cms/` so CMS users can then run SFTP from outside JADE to "pick them up."
