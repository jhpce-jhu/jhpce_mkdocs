# JHPCE User Account Tools

Our [web portal](https://jhpce-app02.jhsph.edu/) has several sections.

The JHPCE User Account Tools section provide the means to:

* Reset your password
* Request Authenticator Code ("One Time Password")
* Update Contact Information
---
You can visit our [web portal](https://jhpce-app02.jhsph.edu/) which is at [https://jhpce-app02.jhsph.edu](https://jhpce-app02.jhsph.edu/)to get a one-time use verification code and/or reset your password.  This web site is only available on the school's network, so if you are off campus, you will need to login to the JHU VPN first.

You will first log into that web site with your JHED ID and password (so you'll need a valid JHED). Once logged in you should then proceed to the JHPCE Tools section of the site.  In that section you will find options for requesting a verification code or resetting your password.
 
After you ssh into the cluster:
 
- If you need to change your password, use the `kpasswd` command. You will need to use a password with three of the following four sets of
characters: upper-case, lower-case, numerical digits, and special characters.
 
- If you need to set up your Google Authenticator, you can use the following steps:
  
    1. On your smartphone, bring up the "Google Authenticator" app
    2. On the JHPCE cluster, run `auth_util`
    3. In `auth_util`, use option "5" to display the QR code (you may need to resize your ssh window - ”view->terminal unzoom” in MobaXterm)
    4. Scan the QR code with the Google Authenticator app
    5. Next in `auth_util` use option 2 to display your scratch codes
    6. In `auth_util`, use option "6" to exit from `auth_util`
 
Going forward, you would then use the 6 digit code from Google Authenticator when prompted for “Verification Code” when logging into the JHPCE cluster.

---

!!! Warning
    As of 20240306, C-SUB users cannot use these services, as they are not yet included in an underlying database.

!!! Note
    Your JHPCE cluster username and password are NOT the same as your JHED ID and password. These are maintained by different groups and do not change in one place when changed in the other.
    
!!! Tip "Password complexity requirements"
    After you get logged in, use the "kpasswd" command to choose a new password. You will need to use a password with three of the following four sets of characters: upper-case, lower-case, numerical digits, and special characters.
    


