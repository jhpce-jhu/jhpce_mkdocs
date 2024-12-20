---
tags:
  - csub
  - topic-overview
---

#C-SUB Overview

The CMS SUBcluster (C-SUB) makes use of some of the resources of an existing High Performance Computing cluster called JHPCE.

## Motivation to create the facility

The Centers for Medicare and Medicaid Services (CMS) is part of the U.S. Department of Health and Human Services. It provides important data for researchers of patients, their conditions, and the American health care system.

Acquiring and managing sensitive information from the Federal government is time-consuming, and requires on-going administrative and information technology support by groups with specific expertise.

To facilitate research, an effort has been made to create an infrastructure that can provide those resources, as well as a computational facility to store and analyze the data. The C-SUB is built such that it complies with  NIST SP 800-53 protocols, to ensure the protection of this sensitive data. The C-SUB's compliance with NIST SP 800-53 has been reviewed and approved by an external entitiy.

Researchers can leverage this existing infrastructure to more quickly and efficiently start and conduct their work.

The Health Analytics Research Platform (HARP) was created to implement this vision. It is a collaboration of existing personnel across multiple organizations. Its leaders provide funding and guidance to an IT group which created a computing facility named the C-SUB.

## Health Policy & Management (HPM) Component of C-SUB

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
