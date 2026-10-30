# SAFE and SAFER Desktops

Users of JHPCE clusters may wish (or need) to consider applying for SAFE or SAFER Desktop accounts. JHPCE users can access the clusters via SSH from them (but not file access). {==Certain JADE cluster user communities e.g. dbGaP *MUST* use them to access JADE.==}

This document briefly describes them and their relationship with JHPCE user communities.

## Overview

[Johns Hopkins Information Technology](https://it.johnshopkins.edu/) provides investigators with secure Windows virtual workspaces integrated with common scientific tools and enhance productivity both within and beyond Johns Hopkins University.

The home page for these services is [https://researchit.jhu.edu/virtual-desktops/](https://researchit.jhu.edu/virtual-desktops/). It contains account request forms, descriptions of the two services and how they differ, the security standards they meet, specifications for the virtual machines (VMs) you can launch, and cost information.

The Secure Analytic Framework Environment (SAFE) and the Secure Analytic Framework Environment for Research (SAFER) Desktops primarily differ in these ways:

- SAFER meets more stringent security standards.
- SAFE is run on Hopkins computers while SAFER uses external cloud providers.
- SAFE is free while using SAFER VMs machines above the base level model __might__ cost money ("subsidized" rates have been "To Be Determined" for over a year as of late 2026).

## Reasons to use SAFE or SAFER include:

- It is free, while using JHPCE is not.
- Your sessions there continue even if you are accidentally disconnected (or choose to disconnect). (It is unknown how long such sessions might last -- you should always save your work.)
- The SAFE Desktop uses Citrix technology, which is more responsive than the "X11 Window System" used by most GUI applications in JHPCE.
- A variety of applications are installed, which might be useful to you for either learning or doing research. For example, you can learn how to use current versions of Windows itself, or one of the applications like SAS, StataSE, GenomeStudio, Matlab or Microsoft Office 365.
- The descriptions of VM resources for both SAFE and SAFER mention access to a GPU.
- They provide a place to back up critical files to.  Your most critical documents are probably small ones.

## Reasons to NOT use SAFE include:

- {==Your SAFE environment does not have access to the files stored in JHPCE clusters!!!==} Before you transfer any files there, you must consider whether licensing or other agreements/policies forbid that.
- Storage is limited. Your home directory is limited to 100GB. Up to 100TB are supposedly available but only for three year periods. We don't know what the transfer rates are for moving data into or out of these accounts.
- The descriptions mention users outside of Hopkins but it seems likely that a JHED account is required. (You have to have one to view a list of software apps installed.)


