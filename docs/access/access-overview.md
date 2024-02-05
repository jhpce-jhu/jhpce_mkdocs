# Accessing the Cluster: Overview

## Cluster structure overview
Basically the cluster consists of some public-facing hosts and then resources isolated from other networks. Insert a simplistic diagram or an updated one of the one in the Orientation. Help people visualize.

## Public-facing: Login and Transfer 
What is visible to the Internet?
Mention that some interfaces require being on Hopkins networks (is this only the C-SUB's login node?)

(Whether here or in some other more-specific document: Do we want to say that we recommend being on a Hopkins network because of any factors? Better speed if you're actually on a Hopkins network? Lower speed if you use VPN? Anything?)

## SSH Is The Primary Method
Overview
Point to [this document](ssh.md)

## Web Portal
Describe capabilities of web portal.
Links to the apps page(s) about it.

## SAFE Desktop
Do we mention this as an access channel?

## File Transfers
Brief description of our transfer node and its capabilities. Admonition to not use login node for transfers (either in or out or within the cluster)

Point to some overview doc about this topic, which is going to have several documents in total, not one giant one, i.e.

* overview doc
* globus
* windows-specific? recommend a tool
* Internal file transfers - this deserves a document. Perhaps under the Managing Files header. Do's, don'ts, sample batch job, good rsync args to use, etc.
