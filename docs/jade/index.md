---
tags:
  - jade
  - topic-overview
---

##JADE Overview

The JHPCE Advanced Data Enclave (JADE) is a secured, NIST 800-171 compliant
High Performance Compute (HPC) cluster managed by the Joint High Perfomance
Computing Exchange (JHPCE) organization. The JADE cluster is designed to adhere to
additional security protocols in order to meet the NIST 800-171 data handling
requiremtns for Controlled Unclassified Information (CUI) data such as dbGaP.
The JADE cluster also housed the JHU consolidated Centers for Medicare and Medicaid
(CMS) data managed by the Health Analytics Research Platform (HARP) service center,
which was formerly housed on the CSUB cluster.

For users migrating from JHPCE or CSUB to JADE, please see [this transition document](jade-transition.md)
for information about changes in JADE.


## Health Policy & Management (HPM) Component of JADE

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
