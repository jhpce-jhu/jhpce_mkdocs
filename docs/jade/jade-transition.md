---
tags:
  - jade
  - topic-overview
---

# JADE Overview

The JHPCE Advanced Data Enclave (JADE) is a secured
High Performance Compute (HPC) cluster managed by the Joint High Perfomance
Computing Exchange (JHPCE) organization. The JADE cluster is designed to adhere to
additional security protocols in order to meet the NIST 800-171 data handling
requirements for Controlled Unclassified Information (CUI) data such as dbGaP.
The JADE cluster also housed the JHU consolidated Centers for Medicare and Medicaid
(CMS) data managed by the Health Analytics Research Platform (HARP) service center,
which was formerly housed on the CSUB cluster.

## Information for users migrating to JADE from JHPCE or CSUB

If you are a dbGaP or CMS data user on either the JHPCE or CSUB clusters, the JADE
cluster will feel very familiar to you.  We use the same SLURM scheduler that is used
in JHPCE and CSUB, and have the same applications/modules (R/SAS/Python ...) that we
have on the other clusters.

### Changes in  common for both dbGaP and CMS users

- The login node for the JADE cluster is "jade01.jhsph.edu".  So:
    - If you are on a Windows system, you will need to create a new MobaXterm session for the jade01.jhsph.edu login node. There are notes for doing this on page 18 from the CSUB orientation https://jhpce.jhu.edu/orient/images/latest-csub-orient.pdf, though you would substitute jade01.jhpce.edu for jhpcecms01.jhsph.edu
    - If you are on a Mac, you will need to adjust your ssh command to use jade01.jhsph.edu instead of either jhpce01.jhsph.edu or jhpcecms01.jhsph.edu.
- The data structure for the various CUI will be centered around the DUA number.  The general directory layout will be /data/cms/c23456/cui, where the "23456" represents your dua or project number. This structure will facilitate compliance to data handling requirements contained in the various DUAs.
- Data transfers via sftp will be done on ports specific to your community (CMS or dbGap) - More info to follow.
- Your userid will be in the format $COMMUNITY-$JHEDID-$DUA, so d-bsmith34-12345 for dbGaP data users or c-bsmith1-23456 for CMS data users.

### Information specific to the dbGaP community

- Access to the jade01 login node will only be available via the SAFE desktop. This 
additional access requirement is in place to meet the requiremnts from NIH as well as the JHU Data Security group.
- Data will be structure by DUA number, so the path to your data on JADE will
be /data/dbgap/d$DUA
- TODO - sftp ports

### Information specific to the CMS user community
- The path to your data will change. For example, for DUA 23456 the path will change from /cms01/data/dua/23456 to /data/cms/c23456/cui (but we have made a shortcut to point /data/cms/c23456/cui to /cms01/data/dua/23456 so you can continue to use the old paths for a while
- TODO - sftp ports

### JADE features that are Coming Soon!

- Open On Demand (OOD) - We are working on standing up an Open On Demand system for the JADE cluster.  This will enable access to various web-based applications, including an interactive shell environemnt, on JADE through a web interface.  This should eliminate the need to login to SAFE desktop in order to meet the DUA and JHU security requirements.
- Globus for file transfers - We are working on standing up a separate Globus transfer
node to facilitate data transfes to and from the JADE cluster.

# Health Policy & Management (HPM) Component of JADE for CMS data users

The Health Analytics Research Platform (HARP) provides data services to HBHI- affiliated and HEADS Center-affiliated investigators to facilitate research collaborations advancing HBHI’s strategic pillars.

**HPM Personnel**

- HEADS/HARP Director: Dan Polsky 
- HEADS/HARP Deputy Director: Matt Eisenberg 
- CMS Data Expert: Frank Xu

**Contact**

Please send C-SUB data-specific requests such as

- joining the C-SUB
- exporting files out of the C-SUB
- data inventory – current and desired additions or updates

to **support@harp-csub.freshdesk.com**

**Further Information**

- [Health Analytics Research Platform (HARP)](https://hbhi.jhu.edu/affiliate-resource/health-analytics-research-platform-harp)

- [Hopkins Business of Health Initiative (HBHI)](https://hbhi.jhu.edu/about-us)

- [Hopkins Economics of Alzheimer Disease & Services (HEADS)](https://publichealth.jhu.edu/hopkins-economics-of-alzheimers-disease-and-services-center)

## Differences between JHPCE & C-SUB Clusters

The CMS subcluster (C-SUB) makes use of some of the resources of the original JHPCE cluster. For example, the scientific software such as STATA and SAS.

However, in many other ways it operates differently than the rest of the cluster in order to keep CMS data from escaping.

Please keep that in mind when reading JHPCE documentation or asking for help via the bitsupport & bithelp mailing lists. Mention that you are a C-SUB user.

### Commonalities
- SLURM job scheduler
- Almost all of the software
  - use of modules
  - except for programs which run out of containers like RStudio Server

### Differences
- Different user accounts (all C-SUB have the form of `c-<JHEDID>-<DUA-NUMBER>`)
- Different login node (C-SUB: jhpcecms01.jhsph.edu, only accessible from JH networks (including the VPN))
- Different SLURM server, partitions and nodes (cluster name is "cms" instead of "jhpce3"). So job acounting records differ. Default partition for jobs in "jhpce3" is "shared" while in "cms" it is "cms".
- File transfer in and out is VERY different (see C-SUB orientation pdf)
- Home directory locations (under `/users/<DUA-NUMBER>/` instead of just `/users/`)
- Common group-writable file sharing areas among DUA members (e.g. /users/55548/shared/)

## Getting Help
Many of the pages on this web site will be useful to C-SUB users. However they will need to interpret what they read and think about whether applying it in the C-SUB requires any modification or re-interpretation.

Sources of assistance:

- This web site - use the search field
- This web site - see the section titled "Getting Help"
- the [orientation slides](../orient/images/latest-csub-orient.pdf) are improved over time. They contain much information! Because of a lack of staff time, this document will be improved ahead of adding C-SUB-specific information to this web site. There is a version date on the first page. You can use that to compare to the one you used during your orientation.
