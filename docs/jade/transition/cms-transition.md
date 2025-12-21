---
tags:
  - jade
  - cms
  - transition
---

# Transition of CMS Users of the C-SUB Cluster to the JADE Cluster

The JHPCE Advanced Data Enclave (JADE) is a secured
High Performance Compute (HPC) cluster managed by the Joint High Perfomance
Computing Exchange (JHPCE) organization. The JADE cluster is designed to adhere to
additional security protocols in order to meet the NIST 800-171 data handling
requirements for Controlled Unclassified Information (CUI) data. 800-171 is an
improvement over an older standard required by the Centers for Medicare and
Medicaid Services (CMS) that was implmented on the C-SUB.
CMS data used on the C-SUB is managed by the Health Analytics Research Platform (HARP) service center,

The JADE cluster also houses data from the Database of Genotypes and Phenotypes (dbGaP)
organization. So there are two sub-communities sharing JADE, "cms" and "dbgap".
The JADE design process involved ensuring that data for each community (and DUA
within each community) is visible only to the correct users.

## Information for users migrating to JADE CSUB

If you are a CMS data user on the C-SUB cluster, the JADE cluster will feel very familiar to you. Both clusters use the same technology -- the operating system, the job scheduler SLURM, and the applications/modules (R/SAS/Python ...). JADE uses newer versions of those components, of course, because the C-SUB is several years old. Your username will stay the same (except for the 10201 DUA, who will switch to 10401).

There are a number of changes which require your attention.

### New login node


The login node for the JADE cluster is "jade01.jhsph.edu".  So you'll need to change that component of accessing a cluster, whether by SSH or SFTP. (The port numbers used for SFTP (22011 and 22027) are the same.)

??? tip "Click to see details for each operating system"
    - If you are on a Windows system, you will need to create new MobaXterm SSH and SFTP sessions for the jade01.jhsph.edu login node. There are notes for doing this on page 18 from the C-SUB orientation https://jhpce.jhu.edu/orient/images/latest-csub-orient.pdf, though you will substitute jade01.jhpce.edu for jhpcecms01.jhsph.edu

    - If you are on a Mac or Linx computer, you will need to adjust your ssh or sftp commands to use jade01.jhsph.edu instead of jhpcecms01.jhsph.edu.

### Path changes

JADE uses different locations for a number of kinds of files. {==We tried to minimized the impact of those changes by creating symbolic links in the file system so that the most important C-SUB paths will point at the new JADE locations.==}

We have written a paths change document which details all of the JADE data locations and how they correspond to C-SUB ones. 

In both clusters the DUA number is a key component of paths.

- The data structure for the various CUI will be centered around the DUA number.  The general directory layout will be /data/cms/c23456/cui, where the "23456" represents your dua or project number. This structure will facilitate compliance to data handling requirements contained in the various DUAs.

- The path to your data will change. For example, for DUA 23456 the path will change from /cms01/data/dua/23456 to /data/cms/c23456/cui (but we have made a shortcut to point /data/cms/c23456/cui to /cms01/data/dua/23456 so you can continue to use the old paths for a while

### JADE features that are Coming Soon!

- Open On Demand (OOD) - We are working on standing up an Open On Demand system for the JADE cluster.  This will enable access to various web-based applications, including an interactive shell environemnt, on JADE through a web interface.  This should eliminate the need to login to SAFE desktop in order to meet the DUA and JHU security requirements.
- Globus for file transfers - We are working on standing up a separate Globus transfer
node to facilitate data transfes to and from the JADE cluster.

# MATERIAL BELOW NOT YET REWRITTEN
**Contact**

Please send CMS data-specific requests such as

- joining the C-SUB
- approval to export files out of the C-SUB
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
- the [orientation slides](https://jhpce.jhu.edu/orient/images/latest-csub-orient.pdf) are improved over time. They contain much information! Because of a lack of staff time, this document will be improved ahead of adding C-SUB-specific information to this web site. There is a version date on the first page. You can use that to compare to the one you used during your orientation.
