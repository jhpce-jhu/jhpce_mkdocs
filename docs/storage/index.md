---
tags:
   - topic-overview
---

# STORAGE OVERVIEW

## Types of storage
There are three types of data storage spaces available on the JHPCE cluster.

<style>
    table, th, td {
      border: 1px solid black;
      border-collapse: collapse;
    }

    .heatMap {
        width: 100%;
        text-align: center;
        align: left;
    }
    .heatMap th {
        background: lightgrey;
        word-wrap: break-word;
        text-align: center;
    }
    .heatMap tr:nth-child(1) { background: PowderBlue   ; }
    .heatMap tr:nth-child(2) { background: CornflowerBlue }
    .heatMap tr:nth-child(3) { background: PaleTurquoise; }
    .heatMap tr:nth-child(4) { background: LightSteelBlue   ; }
    .heatMap tr:nth-child(5) { background: CornflowerBlue ; }
    .heatMap tr:nth-child(6) { background: SkyBlue; }
    .heatMap tr:nth-child(7) { background: PaleTurquoise    ; }
</style>

<div class="heatMap">
<TABLE align="left">
<TR><TH>Type</TH><TH>Example Path</TH><TH>Quota?</TH><TH>Use</TH><TH>Cost</TH></TR>
<TR><TD>Home directory</TD><TD>/users/USERID</TD><TD>100GB</TD><TD>Small datasets, Programs, Applications</TD><TD>$350/TB/yr - max $35/yr if 100GB used</TD></TR>
<TR><TD>Project space</TD><TD>/dcs0?/*grpname*/data</TD><TD>Size of the purchased allocation</TD><TD>Research data</TD><TD>Between $25/TB/yr and $40/TB/yr</TD></TR>
<TR><TD>Scratch space</TD><TD>$MYSCRATCH</TD><TD>1TB</TD><TD>Temporary files</TD><TD>Free</TD></TR>

</TABLE>
</div>

## Home directories
All users have access to their own personal home directory space.  The path
to your home directory space is /users/USERID.  By default, only you have
access to you home directory.

Home directories can be used for storing programs you write, data that
you will be working with, or applications that you need to run.  In a Linux
shell, your home directory can be referred to by ~ or $HOME

All home directories have a 100 GB quota.  You can see how much space you
are using by running the "hquota" command.  This informaiton is also shown
to you each time you login to the cluster.
If you are finding that you need more space than the 100GB provided, please
email us at bitsupport@lists.jhu.edu. We will work with you to find additional
available space to use. This may include utilizing your 1TB of temporary
scratch space 

Home directories are backed up, but other storage areas are probably not. So
if you are working in another directory, and you are generating unique
data, you should copy it to your home directory, or copy it off of the JHPCE
cluster.

For home directory space you are only charged for the actual storage you are
using at a rate of $0.35/GB/yr.  As you add or remove files, your charge will
increase or decrease based on your usage. Home directory space is already compressed on the storage server, so you won't save any money by compressing your files with `tar -czf` etc.

## Project Spaces

Every 12 - 18 months, a new large storage array is purchased for
the JHPCE cluster, and allocations for these storage spaces are sold to 
the various PIs or groups that need storage space.  As part of the planning
process for bringing a new storage array online, we will reach out to all
active PIs on the cluster, and survey them for their expected storage needs.
We will then size the new storage array based on those needs.

Our last large storage build was in 2023, for the DCS07 storage array.  We
do still have some unsold capacity on this array, so please reach out to
us at bitsupport@lists.jhu.edu if you have a need for additional storage.

As of 2024-05-01, the currect project storage arrays in places are:
<div class="heatMap">
<TABLE align="left">
<TR><TH>Storage Name</TH><TH>Year Built</TH><TH># of Disks</TH><TH>Disk Size</TH><TH># of JBODs</TH><TH>Useable Space</TH><TH>Cost</TH><TH>Cost per TB</TH></TR>
<TR><TD>DCL02</TD><TD>2018</TD><TD>440</TD><TD>8TB</TD><TD>10</TD><TD>2.4PB</TD><TD>$164,870.14</TD><TD>$66.57</TD></TR>
<TR><TD>DCS04</TD><TD>2020</TD><TD>720</TD><TD>12TB</TD><TD>10</TD><TD>5.0PB</TD><TD>$202,451.29</TD><TD>$40.45</TD></TR>
<TR><TD>DCS05</TD><TD>2021</TD><TD>480</TD><TD>20TB</TD><TD>8</TD><TD>6.2PB</TD><TD>$240,881.36</TD><TD>$38.83</TD></TR>
<TR><TD>DCS06</TD><TD>2021</TD><TD>16</TD><TD>7.68</TD><TD>1</TD><TD>88TB</TD><TD>$34,207.00</TD><TD>$305.17</TD></TR>
<TR><TD>DCS07</TD><TD>2023</TD><TD>300</TD><TD>22TB</TD><TD>5</TD><TD>4.8PB</TD><TD>$145,453.99</TD><TD>$30.61</TD></TR>
</TABLE>
</div>
###### Notes:
    - Part of DCS04 was used for legacy-dcs01 space
    - Part of DCS05 was used for legacy-dcl01 space
    - SSD-based array used by CSUB cluster

Other Storage Arrays currently in use on the JHPCE cluster:

<div class="heatMap">
<TABLE align="left">
<TR><TH>Storage Name</TH><TH>Use</TH><TH>Year Built</TH><TH># of Disks</TH><TH>Disk Size</TH><TH># of JBODs</TH><TH>Useable Space</TH><TH>Cost</TH><TH>Cost per TB</TH></TR>
<TR><TD>DCS02</TD><TD>/home,/jhpce,/legacy</TD><TD>2016</TD><TD>40</TD><TD>6TB</TD><TD>1</TD><TD>172TB</TD><TD>$21,168.50</TD><TD>$122.50</TD></TR>
<TR><TD>DCS03</TD><TD>Backups</TD><TD>2017</TD><TD>450</TD><TD>4TB,6TB</TD><TD>10</TD><TD>2.1PB</TD><TD>$136,919.94</TD><TD>$62.55</TD></TR>
<TR><TD>Fastscratch</TD><TD>Scratch</TD><TD>2018</TD><TD>24</TD><TD>1TB</TD><TD>1</TD><TD>24TB</TD><TD>$17,983.45</TD><TD>$749.29</TD></TR>
</TABLE>
</div>
These storage arrays are built on the Dirt Cheap Storage and Dirt Cheap Lustre
architecture, hence the "DCS" and "DCL" names, as described in
[this paper](docs/greenDirtCheapStorage.pdf) from 2013. The storage arrays have
been historically built on commodity servers and large JBODS
(Just a Bunch Of Disks) from Supermicro. We use [ZFS](https://zfsonlinux.org/)
(with [Lustre](https://www.lustre.org/) on top of it in the case of the DCL
arrays).

## Scratch Space

We have a "Fastscratch" storage array that can be used by all users for
storing files for less than 30 days. This storage array is in place to provide
an additional 1TB of temporary storage space beyond the 100GB of home directory
space. It is not for long-term storage of data, and all files older than 30
days are purged from this "Fastscratch" space.  More information about
using Fastscratch, including important restrictions, can be
found [here](fastscratch.md).

## Backing up storage
You need to ensure that you have copies of your most vital files located somewhere else.
See [this document](backups-restores.md) for more information.

## Encrypted filesystem
 
Encrypted filesystem are used to
provide “Encryption At Rest”, meaning that the data on disk will be safely
stored in an encrypted format, and only available in an unencrypted
state when the data is accessed by an approved user. This may be desireable
when working with more sensitive data sources, or where Data Use Agreements
require “Encryption At Rest”.

The JHPCE Cluster currently supports the following mechanisms for
providing encrypted filesystems:

  + Userspace encrypted filesystems using encfs. See [ENCFS on JHPCE](../files/encfs.md).
  + If need be, a Project Storage space can be encrypted with ZFS encryption.
  + The `DCL02` storage array is built on encrypted disk devices.

## Deprecated Storage Arrays:
For historicla purposes, these are the storage arrays that have been used over 
time on the JHPCE cluster:
<br><sup>information deemed reliable but not guaranteed</sup>

|Storage Name|Year Built|Year Decom.|# of Disks|Disk Size|# of JBODs|Total Useable Space|Cost|Cost per TB|
|---|---|---|---|---|---|---|---|---|
|DCL01+exp|2015|2023|440|8TB|20|3.4PB|$164,870.14|$66.57|
|DCS01|2013|2021|360|3TB|8|688TB|$109,961.00|$159.82|
|amber03|2012|2020|72|2TB|2|100TB|$64,861.00|$648.61|
|amber02|2011|2016|24|1TB|1|16TB|$14,730.00|$920.62|
|dexter|2011|2016|12|1TB|1|30TB|$13,690.00|$456.33|
|thumper02|2010|2016|24|500G|1|16TB|$21,025.00|$1314.06|
|amber01 (/home)|2009|2016|96|500GB+1TB|2|72TB|$92,984.00|$1291.44|
|nexsan2|2009|2016|12|2TB|1|12TB|$14,436.00|$1203.00|
|thumper01|2008|2016|24|500G|1|16TB|$17,079.00|$1067.00|
|nexsan1|2006|2016|12|1TB|1|6TB|$19,060.00|$3176.66|
