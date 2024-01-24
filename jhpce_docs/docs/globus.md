



Using Globus to transfer files | Joint HPC Exchange




 jvcf7\_loading\_url= "https://jhpce.jhu.edu/wp-content/plugins/contact-form-7/images/ajax-loader.gif";
 jvcf7\_invalid\_field\_design = "theme\_1";
 jvcf7\_show\_label\_error = "errorMsgshow";
 


 window.\_wpemojiSettings = {"baseUrl":"https:\/\/s.w.org\/images\/core\/emoji\/11\/72x72\/","ext":".png","svgUrl":"https:\/\/s.w.org\/images\/core\/emoji\/11\/svg\/","svgExt":".svg","source":{"concatemoji":"https:\/\/jhpce.jhu.edu\/wp-includes\/js\/wp-emoji-release.min.js?ver=5.0.20"}};
 !function(e,a,t){var n,r,o,i=a.createElement("canvas"),p=i.getContext&&i.getContext("2d");function s(e,t){var a=String.fromCharCode;p.clearRect(0,0,i.width,i.height),p.fillText(a.apply(this,e),0,0);e=i.toDataURL();return p.clearRect(0,0,i.width,i.height),p.fillText(a.apply(this,t),0,0),e===i.toDataURL()}function c(e){var t=a.createElement("script");t.src=e,t.defer=t.type="text/javascript",a.getElementsByTagName("head")[0].appendChild(t)}for(o=Array("flag","emoji"),t.supports={everything:!0,everythingExceptFlag:!0},r=0;r<o.length;r++)t.supports[o[r]]=function(e){if(!p||!p.fillText)return!1;switch(p.textBaseline="top",p.font="600 32px Arial",e){case"flag":return s([55356,56826,55356,56819],[55356,56826,8203,55356,56819])?!1:!s([55356,57332,56128,56423,56128,56418,56128,56421,56128,56430,56128,56423,56128,56447],[55356,57332,8203,56128,56423,8203,56128,56418,8203,56128,56421,8203,56128,56430,8203,56128,56423,8203,56128,56447]);case"emoji":return!s([55358,56760,9792,65039],[55358,56760,8203,9792,65039])}return!1}(o[r]),t.supports.everything=t.supports.everything&&t.supports[o[r]],"flag"!==o[r]&&(t.supports.everythingExceptFlag=t.supports.everythingExceptFlag&&t.supports[o[r]]);t.supports.everythingExceptFlag=t.supports.everythingExceptFlag&&!t.supports.flag,t.DOMReady=!1,t.readyCallback=function(){t.DOMReady=!0},t.supports.everything||(n=function(){t.readyCallback()},a.addEventListener?(a.addEventListener("DOMContentLoaded",n,!1),e.addEventListener("load",n,!1)):(e.attachEvent("onload",n),a.attachEvent("onreadystatechange",function(){"complete"===a.readyState&&t.readyCallback()})),(n=t.source||{}).concatemoji?c(n.concatemoji):n.wpemoji&&n.twemoji&&(c(n.twemoji),c(n.wpemoji)))}(window,document,window.\_wpemojiSettings);
 

img.wp-smiley,
img.emoji {
 display: inline !important;
 border: none !important;
 box-shadow: none !important;
 height: 1em !important;
 width: 1em !important;
 margin: 0 .07em !important;
 vertical-align: -0.1em !important;
 background: none !important;
 padding: 0 !important;
}

























 div.faq\_answer {display: block!important;}
 p.faq\_nav {display: none;}
 








[Joint HPC Exchange](https://jhpce.jhu.edu/ "Joint HPC Exchange")


High Performance Computing for the Life Sciences


[Skip to content](#content "Skip to content")
* [Home](https://jhpce.jhu.edu/ "https://jhpce.jhu.edu/")
* [Blog](https://jhpce.jhu.edu/blog/ "https://jhpce.jhu.edu/blog/")
* [FAQs](https://jhpce.jhu.edu/faq/ "https://jhpce.jhu.edu/faq/")
* [Help](https://jhpce.jhu.edu/contact/ "https://jhpce.jhu.edu/contact/")
* [Knowledge Base](https://jhpce.jhu.edu/knowledge-base/ "https://jhpce.jhu.edu/knowledge-base/")
	+ [Authentication](https://jhpce.jhu.edu/knowledge-base/authentication/ "https://jhpce.jhu.edu/knowledge-base/authentication/")
		- [2 Factor Authentication](https://jhpce.jhu.edu/knowledge-base/authentication/2-factor-authentication/ "https://jhpce.jhu.edu/knowledge-base/authentication/2-factor-authentication/")
		- [SSH Keypair](https://jhpce.jhu.edu/knowledge-base/authentication/ssh-key-setup/ "https://jhpce.jhu.edu/knowledge-base/authentication/ssh-key-setup/")
	+ [Disk storage space on the JHPCE cluster](https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/ "https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/")
		- [Encrypted Filesystem](https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/encrypted-filesystem/ "https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/encrypted-filesystem/")
			* [Encrypted Filesystem with encfs](https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/encrypted-filesystem/encrypted-filesystem-support/ "https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/encrypted-filesystem/encrypted-filesystem-support/")
		- [Fastscratch space on JHPCE](https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/fastscratch-space-on-jhpce/ "https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/fastscratch-space-on-jhpce/")
		- [General Storage Knowledge Sharing](https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/general-storage-knowledge-sharing/ "https://jhpce.jhu.edu/knowledge-base/disk-storage-space-on-the-jhpce-cluster/general-storage-knowledge-sharing/")
	+ [Environment Modules](https://jhpce.jhu.edu/knowledge-base/environment-modules/ "https://jhpce.jhu.edu/knowledge-base/environment-modules/")
	+ [File Transfer](https://jhpce.jhu.edu/knowledge-base/file-transfer/ "https://jhpce.jhu.edu/knowledge-base/file-transfer/")
		- [Setting up MobaXterm for file transfers with the JHPCE cluster](https://jhpce.jhu.edu/knowledge-base/file-transfer/mobaxterm-file-transfers/ "https://jhpce.jhu.edu/knowledge-base/file-transfer/mobaxterm-file-transfers/")
		- [Using rclone to access OneDrive](https://jhpce.jhu.edu/knowledge-base/file-transfer/using-rclone-to-access-onedrive/ "https://jhpce.jhu.edu/knowledge-base/file-transfer/using-rclone-to-access-onedrive/")
	+ [GPUs on the JHPCE cluster under SLURM](https://jhpce.jhu.edu/knowledge-base/gpus-on-the-jhpce-cluster/ "https://jhpce.jhu.edu/knowledge-base/gpus-on-the-jhpce-cluster/")
	+ [Granting File Permissions using ACLs](https://jhpce.jhu.edu/knowledge-base/granting-permissions-using-acls/ "https://jhpce.jhu.edu/knowledge-base/granting-permissions-using-acls/")
	+ [JHPCE Web Portal](https://jhpce.jhu.edu/knowledge-base/jhpce-web-application-portal/ "https://jhpce.jhu.edu/knowledge-base/jhpce-web-application-portal/")
	+ [Knowledge Base Articles from Aki Nishimura](https://jhpce.jhu.edu/knowledge-base/knowledge-base-articles-from-aki-nishimura/ "https://jhpce.jhu.edu/knowledge-base/knowledge-base-articles-from-aki-nishimura/")
	+ [Knowledge Base Articles from Lieber Institute](https://jhpce.jhu.edu/knowledge-base/knowledge-base-articles-from-lieber-institute/ "https://jhpce.jhu.edu/knowledge-base/knowledge-base-articles-from-lieber-institute/")
	+ [Mobaxterm Configuration](https://jhpce.jhu.edu/knowledge-base/mobaxterm-configuration/ "https://jhpce.jhu.edu/knowledge-base/mobaxterm-configuration/")
	+ [Numerical Linear Algebra Libraries](https://jhpce.jhu.edu/knowledge-base/numerical-linear-algebra-libraries/ "https://jhpce.jhu.edu/knowledge-base/numerical-linear-algebra-libraries/")
	+ [Running Rstudio Server on the JHPCE cluster](https://jhpce.jhu.edu/knowledge-base/rstudio-server/ "https://jhpce.jhu.edu/knowledge-base/rstudio-server/")
	+ [SLURM](https://jhpce.jhu.edu/knowledge-base/slurm/ "https://jhpce.jhu.edu/knowledge-base/slurm/")
	+ [Using Globus to transfer files](https://jhpce.jhu.edu/knowledge-base/using-globus-to-transfer-files/ "https://jhpce.jhu.edu/knowledge-base/using-globus-to-transfer-files/")
	+ [HOW-TO](https://jhpce.jhu.edu/knowledge-base/how-to/ "https://jhpce.jhu.edu/knowledge-base/how-to/")
	+ [Community](https://jhpce.jhu.edu/knowledge-base/community/ "https://jhpce.jhu.edu/knowledge-base/community/")
		- [Perl](https://jhpce.jhu.edu/knowledge-base/community/perl/ "https://jhpce.jhu.edu/knowledge-base/community/perl/")
		- [Python](https://jhpce.jhu.edu/knowledge-base/community/python/ "https://jhpce.jhu.edu/knowledge-base/community/python/")
		- [R](https://jhpce.jhu.edu/knowledge-base/community/r/ "https://jhpce.jhu.edu/knowledge-base/community/r/")
		- [Short Read Tools](https://jhpce.jhu.edu/knowledge-base/community/short-read-tools/ "https://jhpce.jhu.edu/knowledge-base/community/short-read-tools/")
* [Policies](https://jhpce.jhu.edu/policies/ "https://jhpce.jhu.edu/policies/")
	+ [Current Storage Offerings](https://jhpce.jhu.edu/policies/current-storage-offerings/ "https://jhpce.jhu.edu/policies/current-storage-offerings/")
* [Register](https://jhpce.jhu.edu/register/ "https://jhpce.jhu.edu/register/")
	+ [New project request](https://jhpce.jhu.edu/register/project/ "https://jhpce.jhu.edu/register/project/")
	+ [New user request](https://jhpce.jhu.edu/register/user/ "https://jhpce.jhu.edu/register/user/")
	+ [New C-SUB user orientation](https://jhpce.jhu.edu/register/3591-2/ "https://jhpce.jhu.edu/register/3591-2/")
	+ [New user orientation](https://jhpce.jhu.edu/register/orientation/ "https://jhpce.jhu.edu/register/orientation/")
* [Docs](https://jhpce.jhu.edu/documents/ "https://jhpce.jhu.edu/documents/")
* [HIPAA](https://jhpce.jhu.edu/hipaa/ "https://jhpce.jhu.edu/hipaa/")








Using Globus to transfer files
==============================



Globus is a “dropbox-like” service to enable data sharing between academic and research communities. By using Globus, you can  easily transfer files between Globus endpoints, share data with collaborators, and easily transfer data between the JHPCE cluster and your local desktop or laptop.


**Setting up an account:**


The first step in using Globus is setting up and account on the Globus site.  To start, go to [https://www.globus.org/](https://www.globus.org/ "https://www.globus.org/").  and click on the “Log in” button in the upper right. You should now see the screen below.  Select “Johns Hopkins” from the list of Organizations.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.40.45-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.40.45-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.40.45-PM.png")


You will now be directed to the JHU Login Screen.  Enter your JHED ID and Password:


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.41.02-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.41.02-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.41.02-PM.png")Once you enter your JHED information you will be sent to the main Globus window:


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.41.18-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.41.18-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.41.18-PM.png")


**Transferring Files**


With Globus, you can transfer data between nodes (known as “Endpoints”) that are part of the Globus network.  The endpoint for the JHPCE cluster is called “jhpce#globus01”.  To connect to the JHPCE endpoints, enter  “jhpce#globus01” in the “Collections field, and select it from the list of results displayed.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.10-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.10-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.10-PM.png")


Once you select the “jhpce#globus01” endpoint, you will be prompted to enter your JHPCE Login and Password:


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.23-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.23-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.23-PM.png")


And once you login you will be shown a flie list of your home directory on the JHCPE cluster.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.51-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.51-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.42.51-PM.png")


In order to transfer data to and from your desktop, you will need to install the Globus Connect Personal package ([https://www.globus.org/globus-connect-personal](https://www.globus.org/globus-connect-personal "https://www.globus.org/globus-connect-personal"))  on your desktop.  To do this, click on the “Two-Panel” icon at the top of your globus session, and then click on Install Globus Connect Personal.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-1.29.00-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-1.29.00-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-1.29.00-PM.png")


On the next screen, follow the steps on the next page to download the appropriate software package for your system, generate a Globus Key, and create a Globu name for your desktop/laptop. 


Once you install and start Globus Connect Personal, you will be able to easily transfer files between your desktop/laptop and the JHPCE cluster via the Globus web interface.


**Sharing data with others**


One of the benefits fo using Globus is that you can share data from the JHPCE cluster with outside collaborators.  To do this navigate to the directory you wish to share, select it, and either Right-Click on it, or select  “Share” from the menu on the right hand side of the screen.  In this example, I’m sharing the “$HOME/class-scripts/R-demo” directory from within my home directory.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.43.53-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.43.53-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.43.53-PM.png")Next, you will be prompted to provide a name for your share.  In this example, I’m calling it “MyRDemo”.  Once you enter the name and Description, click “Create Share”:


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.44.29-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.44.29-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.44.29-PM.png")


Next you will be shown the current access permissions, which should be just for your account to start with.  To grant others permission to access your share, click on “Add Permissions – Share With”


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.46.07-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.46.07-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.46.07-PM.png")You will now be able to select which users you wish to share your directory with.  The person you are sharing with must have a Globus account, and will need to provide their Globus ID or email with you.  Enter their Username or Email, click “Add”, and then click “Add Permission”.  We strongly recommend that you only grant “read” permission. for your share.[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.47.08-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.47.08-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.47.08-PM.png")


Alternatively. you can create an “anonymous share” to share a directory with anyone on Globus.  To do this select “all users”.  Again, We strongly recommend that you only grant “read” permission for your share.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.47.31-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.47.31-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.47.31-PM.png")


Once your share is created, you can notify your collaborators that they can access your data by using the Share name you created (in this example it’s “MyRDemo”), and can search for your share name.


[![](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.50.41-PM.png)](https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.50.41-PM.png "https://jhpce.jhu.edu/wp-content/uploads/2019/04/Screen-Shot-2019-04-26-at-12.50.41-PM.png")









* Search for:
* ### New Announcements


	+ [CLUSTER UPGRADE to SLURM and Rocky 9.2](https://jhpce.jhu.edu/2023/06/27/cluster-upgrade-to-slurm-and-rocky-9-2/ "https://jhpce.jhu.edu/2023/06/27/cluster-upgrade-to-slurm-and-rocky-9-2/")
	+ [JHPCE unavailable April 21st - May 1st for ARCH Cooling maintenance](https://jhpce.jhu.edu/2023/03/10/jhpce-unavailable-april-21st-may-1st-for-arch-cooling-maintenance/ "https://jhpce.jhu.edu/2023/03/10/jhpce-unavailable-april-21st-may-1st-for-arch-cooling-maintenance/")
	+ [Globus Server update on JHPCE, Saturday Dec. 10th at 9:00 PM](https://jhpce.jhu.edu/2022/12/08/globus-server-update-on-jhpce-saturday-dec-10th-at-900-pm/ "https://jhpce.jhu.edu/2022/12/08/globus-server-update-on-jhpce-saturday-dec-10th-at-900-pm/")
	+ [Please be judicious in your use of the email option in qsub](https://jhpce.jhu.edu/2022/09/27/please-be-judicious-in-your-use-of-the-email-option-in-qsub/ "https://jhpce.jhu.edu/2022/09/27/please-be-judicious-in-your-use-of-the-email-option-in-qsub/")
	+ [JHPCE Cluster to be unavailable from April 11th - April 15th for scheduled preventative maintenance on the HVAC equipment](https://jhpce.jhu.edu/2022/03/22/apr2022hvac/ "https://jhpce.jhu.edu/2022/03/22/apr2022hvac/")
* ### JHPCE Cluster Status Updates


	+ [Heat issue at MARCC colocation - JHPCE cluster unavailable.](https://jhpce.jhu.edu/2022/06/22/heat-issue-at-marcc-colocation-jhpce-cluster-unavailable/ "https://jhpce.jhu.edu/2022/06/22/heat-issue-at-marcc-colocation-jhpce-cluster-unavailable/")
	+ [2021-08-14 JHPCE cluster unavailable due to cooling issues at datacenter](https://jhpce.jhu.edu/2021/08/16/2021-08-14-jhpce-cluster-unavailable-due-to-cooling-issues-at-datacenter/ "https://jhpce.jhu.edu/2021/08/16/2021-08-14-jhpce-cluster-unavailable-due-to-cooling-issues-at-datacenter/")
	+ [2021-05-24 - JHPCE cluster unavailable due to cooling issue](https://jhpce.jhu.edu/2021/05/24/2021-05-24-jhpce-cluster-unavailable-due-to-cooling-issue/ "https://jhpce.jhu.edu/2021/05/24/2021-05-24-jhpce-cluster-unavailable-due-to-cooling-issue/")
	+ [Rebooting one DCL storage system Friday, April 2, 8:00AM - 9:00AM](https://jhpce.jhu.edu/2021/03/31/dcl01-oss04-reboot/ "https://jhpce.jhu.edu/2021/03/31/dcl01-oss04-reboot/")
	+ [2020-07-05 - 7:00 PM - JHPCE Cluster currently down due to cooling/power problems at datacenter](https://jhpce.jhu.edu/2020/07/05/2020-07-05-700-pm-jhpce-cluster-currently-down-to-to-cooling-power-problems-at-datacenter/ "https://jhpce.jhu.edu/2020/07/05/2020-07-05-700-pm-jhpce-cluster-currently-down-to-to-cooling-power-problems-at-datacenter/")
* ### Archives


	+ [June 2023](https://jhpce.jhu.edu/2023/06/ "https://jhpce.jhu.edu/2023/06/")
	+ [March 2023](https://jhpce.jhu.edu/2023/03/ "https://jhpce.jhu.edu/2023/03/")
	+ [December 2022](https://jhpce.jhu.edu/2022/12/ "https://jhpce.jhu.edu/2022/12/")
	+ [September 2022](https://jhpce.jhu.edu/2022/09/ "https://jhpce.jhu.edu/2022/09/")
	+ [June 2022](https://jhpce.jhu.edu/2022/06/ "https://jhpce.jhu.edu/2022/06/")
	+ [March 2022](https://jhpce.jhu.edu/2022/03/ "https://jhpce.jhu.edu/2022/03/")
	+ [August 2021](https://jhpce.jhu.edu/2021/08/ "https://jhpce.jhu.edu/2021/08/")
	+ [June 2021](https://jhpce.jhu.edu/2021/06/ "https://jhpce.jhu.edu/2021/06/")
	+ [May 2021](https://jhpce.jhu.edu/2021/05/ "https://jhpce.jhu.edu/2021/05/")
	+ [March 2021](https://jhpce.jhu.edu/2021/03/ "https://jhpce.jhu.edu/2021/03/")
	+ [July 2020](https://jhpce.jhu.edu/2020/07/ "https://jhpce.jhu.edu/2020/07/")
	+ [March 2020](https://jhpce.jhu.edu/2020/03/ "https://jhpce.jhu.edu/2020/03/")
	+ [December 2019](https://jhpce.jhu.edu/2019/12/ "https://jhpce.jhu.edu/2019/12/")
	+ [July 2019](https://jhpce.jhu.edu/2019/07/ "https://jhpce.jhu.edu/2019/07/")
	+ [January 2019](https://jhpce.jhu.edu/2019/01/ "https://jhpce.jhu.edu/2019/01/")
	+ [August 2018](https://jhpce.jhu.edu/2018/08/ "https://jhpce.jhu.edu/2018/08/")
	+ [January 2018](https://jhpce.jhu.edu/2018/01/ "https://jhpce.jhu.edu/2018/01/")
	+ [July 2017](https://jhpce.jhu.edu/2017/07/ "https://jhpce.jhu.edu/2017/07/")
	+ [June 2017](https://jhpce.jhu.edu/2017/06/ "https://jhpce.jhu.edu/2017/06/")
	+ [May 2017](https://jhpce.jhu.edu/2017/05/ "https://jhpce.jhu.edu/2017/05/")
	+ [March 2017](https://jhpce.jhu.edu/2017/03/ "https://jhpce.jhu.edu/2017/03/")
	+ [December 2016](https://jhpce.jhu.edu/2016/12/ "https://jhpce.jhu.edu/2016/12/")
	+ [September 2016](https://jhpce.jhu.edu/2016/09/ "https://jhpce.jhu.edu/2016/09/")
	+ [August 2016](https://jhpce.jhu.edu/2016/08/ "https://jhpce.jhu.edu/2016/08/")
	+ [June 2016](https://jhpce.jhu.edu/2016/06/ "https://jhpce.jhu.edu/2016/06/")
	+ [May 2016](https://jhpce.jhu.edu/2016/05/ "https://jhpce.jhu.edu/2016/05/")
	+ [August 2015](https://jhpce.jhu.edu/2015/08/ "https://jhpce.jhu.edu/2015/08/")
	+ [July 2015](https://jhpce.jhu.edu/2015/07/ "https://jhpce.jhu.edu/2015/07/")
	+ [March 2015](https://jhpce.jhu.edu/2015/03/ "https://jhpce.jhu.edu/2015/03/")
	+ [January 2015](https://jhpce.jhu.edu/2015/01/ "https://jhpce.jhu.edu/2015/01/")
	+ [November 2014](https://jhpce.jhu.edu/2014/11/ "https://jhpce.jhu.edu/2014/11/")
	+ [October 2014](https://jhpce.jhu.edu/2014/10/ "https://jhpce.jhu.edu/2014/10/")
	+ [September 2014](https://jhpce.jhu.edu/2014/09/ "https://jhpce.jhu.edu/2014/09/")
	+ [July 2014](https://jhpce.jhu.edu/2014/07/ "https://jhpce.jhu.edu/2014/07/")
	+ [June 2014](https://jhpce.jhu.edu/2014/06/ "https://jhpce.jhu.edu/2014/06/")
	+ [March 2014](https://jhpce.jhu.edu/2014/03/ "https://jhpce.jhu.edu/2014/03/")
* ### JHU Login



 function submitSamlForm(){ document.getElementById("miniorange-saml-sp-sso-login-form").submit(); }
 



  [Login with IDPJHEDU](# "#")


 





[Joint HPC Exchange](https://jhpce.jhu.edu/ "Joint HPC Exchange") 


[Proudly powered by WordPress.](https://wordpress.org/ "Semantic Personal Publishing Platform") 






/\* <![CDATA[ \*/
var wpcf7 = {"apiSettings":{"root":"https:\/\/jhpce.jhu.edu\/wp-json\/","namespace":"contact-form-7\/v1"},"recaptcha":{"messages":{"empty":"Please verify that you are not a robot."}}};
/\* ]]> \*/






