# Joint HPC Exchange

## About Us
The Joint High Performance Computing Exchange (JHPCE) is a
High-Performance Computing (HPC) facility in the Department of
Biostatistics at the Johns Hopkins Bloomberg School of Public
Health. This fee-for-service core began in 2008 as a collaborative
effort between Biostatistics and the Computational Biology & Research
Computing group in the department of Molecular Microbiology and
Immunology. The facility has grown over the years and is open to
all Johns Hopkins affiliated researchers.

## The JHPCE Service Center manages 3 HPC computing environments.

### JHPCE
The larger HPC environment is for general HPC computing on non-HIPAA data.
We generally refer to this as the "J H P C E" cluster (each letter pronounced). 
Information on the JHPCE cluster can be found at the [Introduction](../joinus/index.md)
link.

### C-SUB
In 2022 JHPCE staff built a smaller sub-cluster for working on [Medicare and Medicaid
Services](https://www.cms.gov) (CMS) data. We refer to this cluster as the 
C-SUB (Cms SUB) cluster. The JHPCE service center manages this HPC
cluster on behalf of the
[Health Analytics Research Platform] (https://hbhi.jhu.edu/affiliate-resource/health-analytics-research-platform-harp) (HARP) organization in JHU. 

Technical infomation about the C-SUB can be found at the
[C-SUB Overview](../csub/csub-overview.md).

This cluster will be retired in 2026.

### JADE

In 2025 JHPCE staff built a new cluster, JHPCE Advanced Data Enclave (JADE).
This is a secured, NIST 800-171 compliant HPC facility, which in 2026 replaced
the C-SUB for the CMS community and began supporting researchers using 
[database of Genotypes and Phenotypes](https://dbgap.ncbi.nlm.nih.gov/home/) (dbGaP) datasets.

Technical infomation about JADE can be found at [JADE Overview](../jade/jade-overview.md).

## JHPCE Community
The facility is used primarily by labs and research groups in the
Johns Hopkins Bloomberg School of Public Health (SPH), the Johns
Hopkins School of Medicine (SOM) and the Lieber Institute for Brain
Development (LIBD). We support the HPC needs of over 2000 user accounts,
with 300 active users each quarter.

## JHPCE Cluster Details
As of November, 2024, the computing and storage systems are optimized for genomics and
biomedical research. The cluster has 84 compute nodes, providing about
3100 cores, 40TB of DRAM and over 20 PB of networked mass
storage. The JHPCE cluster also have 37 Nvidia GPUs in support of AI and ML research done
in our community.  The network fabric consists of 10/40 Gbps ethernet connections.
The facility is connected via a 40Gbps network to the University’s Science DMZ.

Networked mass storage uses open-source file systems (ZFS and
Lustre-over-ZFS) to provide low cost file systems. We also have a 2PB
disk-to-disk backup system off site for backing up more critical data.

The JHPCE cluster is optimized for the embarrassingly parallel
applications that are the bread-and-butter of our stakeholders, e.g.,
genomics and statistical applications, rather than the tightly-coupled
applications that are typical in traditional HPC fields, e.g.,
physics, fluid-dynamics, quantum simulation etc.  Job scheduling is
performed with the Simple Linux Utility for Resource Management
([SLURM](https://slurm.schedmd.com)).

## JHPCE Cost Recovery
The JHPCE operates as a formal Common Pool Resource (CPR) Hierarchy
with rights to specific resources based on stakeholder ownership of
resources. To benefit the entire research community, excess computing
capacity is made available to non-stakeholders on an as-available
basis, in exchange for fees that defray the operating costs of the
stakeholders.

If your lab is interested in joining the JHPCE community, either as a
stakeholder or as a non-stakeholder, please contact the directors
(jhpce@jhu.edu) to determine whether we can accommodate your needs.

If your lab is already a member, and you need to add new users, then
have the users fill out the [JHPCE new user request
form](../joinus/new-users-form.md).

Mark Miller and Brian Caffo  
Co-Directors, JHPCE

