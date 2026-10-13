---
tags:
  - ood
  - jasper
  - in-progress
  - topic-overview
---

# Open OnDemand (OOD) Overview - JHPCE/JASPER Cluster

!!! Danger "The JHPCE OOD server is not yet available"
    We hope to bring one into service during the beginning of 2027. 

This document will serve as a launch point for instructions on how to use [OOD](https://www.openondemand.org/) on the JHPCE (aka JASPER) cluster.

This document will not contain all of the information about OOD, only an overview.

## About OOD

Open OnDemand provides remote access to a cluster via web browsers rather than SSH applications. No client software needs to be installed on users' computers. Work can be done using a Graphic User Interface (GUI) as well as a terminal for Command Line Interface (CLI) work.

## JHPCE-specific details

### An additional MFA code just for JHPCE OOD

!!! Danger "How OOD will work for JHPCE is not yet known"
    The JHPCE/JASPER cluster requires one of two forms of MFA: a One Time Password or SSH public keys. As of 10/2026 we do not yet know if SSH public keys will be supported during OOD logins. They probably will not be used.

!!! Warning "Two different OTP for JHPCE"
    To use JHPCE's OOD server, you need to use a dedicated One Time Password (OTP). This is different from, and _in addition_ to the OTP that you use when logging into the `jhpce01.jhsph.edu` or `jhpce03.jhsph.edu` servers using SSH. In other words, to be able to log in via both SSH and the Open OnDemand web interface, you will need to maintain access to two "OTP accounts".  ==You use the same username & password no matter which interface you use, but the OTP's are different.==

The JHPCE cluster's security model requires the use of a multi-factor credential during login. When you use SSH, you need to provide a username, password and a `One Time Password (OTP)` (technically it is a _Time-based_ One Time Password (TOTP)). The OTP is also called a `verification code`.   Users configure applications like Google or Microsoft Authenticator using a user-specific "secret" which is either a QR code image or a "secret" string of text characters.

On JHPCE, we use a software package called Keycloak to handle the MFA credential component. 

Instructions will follow on the web site about working with Keycloak to do things like generating a new OTP.
