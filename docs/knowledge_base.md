# JHPCE Knowledge Base

## Authentication
+ Access to the JHPCE cluster is provided via SSH.  
+ SSH allows you to establish a secure, encrypted connection between your laptop and a remote computer.
+  How you initiate the ssh connection is going to depend on what type of local laptop or desktop you are using.  
   + If you are on a Mac computes, you can start a Terminal which already has ssh.  
   + On a Windows system you will need to install an ssh client.  We recommend MobaXterm (https://jhpce.jhu.edu/knowledge-base/mobaxterm-configuration/)
+ We require that all users use enable 2 Factor Authentication on their JHPCE account. 

#### Login howto
+ When ssh-ing into the cluster you will be prompted for 2 pieces of information. 
  + First you will be prompted for “Verification Code:”, and for this you will enter the 6 digit number from your authenticator app (we recommend Google authenticator).
  + Next you will be prompted for “Password:”; this you this will use a traditional password that only you know.  *Note that when entering your Verification Code and Password, your cursor will not move when you type, so it will appear like nothing is happening*.  + If you have never accessed the cluster before, and “Initial Verification Code” and “Initial Password” will be provided to you during your JHPCE Cluster Orientation session.
+ If you only get prompted for “Password” and not “Verification Code”, you are likely using the wrong login ID.  Your login ID is provided to you during the JHPCE orientation; it is not the same as your JHED ID.
+ If you have entered your credentials correctly, you will see a login banner for the JHPCE cluster, and you will be sitting at a shell prompt on the login node.  However, if you are prompted for “Verification Code:” again, you likely mistyped either your password or Verification Code, and you should wait until your Google Authenticator code changes (it changes every 30 seconds) and you will need to try logging in again.
+ If you have tried several times to login, and you try to ssh again and get a “Connection Refused” message, you will need to wait 30 minutes before trying to login again.
+ If you continue to have login issues, please contact bitsupport@lists.jhu.edu.

- [2 Factor Authentication](https://jhpce.jhu.edu/knowledge-base/2-factor-authentication/)

## SSH
We also support and recommend the use of SSH public/private key authentication to access your account. 

### Unix-based machines (linux and mac osx)
+ First, on your local laptop/desktop, open a terminal and `cd` into your home directory  and invoke the ssh key generator:

```
ssh-keygen -t rsa
```

+ You will be prompted for a passphrase.  For security reasons, we recommend that you use a passphrase to protect your key.  For the other questions, you can select the default values.
+ The key generator will put two files in the .ssh subdirectory of your home directory, typically  `~/.ssh/id_rsa` and  `~/.ssh/id_rsa.pub`. 
+ *You should only ever have to run the ssh key generator once on your local host*.  If you have already configured passwordless login and you run the key generator a second time, it will overwrite your previous public and private key files. This will break all password-less logins that you set up with your previous keys.
+ Next you want to copy your public key file to the remote host and append it to the authorized_keys file in the `.ssh` subdirectory of your home directory on the remote host. 
  + If there is no `~/.ssh` directory in the remote host, you will need to login to the remote host and create one. Note, attempting to connect to another host, like `ssh github.com`, will create one if it isn't there already. 
+ You can perform the copy and append operations in one line as follows;

```
cat ~/.ssh/id_rsa.pub | ssh <your_userid>@jhpce01.jhsph.edu 'cat >> ~/.ssh/authorized_keys'
```
+ Where you replace `<your_userid>` with your JHPCE userid and where you enter your JHPCE password when you are prompted for it by ssh.
+ To test that everything is working you should be able to log into the remote host from your local host with the following command`ssh <your_userid>@jhpce01.jhsph.edu`.
+ When you start ssh, you will be prompted for the passphrase that you used to protect your key.  To avoid having to enter your passphrase every time you use ssh, you can use the `ssh-agent` program.  To use `ssh-agent`, run

```
ssh-add
ssh-agent
```
The ssh-agent will remain active for as long as your desktop or laptop is up and running.  If you reboot your desktop/laptop, you will need to rerun the ssh-add and ssh-agent commands.

#### Loging into nodes
Logging into a cluster node from a login node requires keypairs. If
your private key file is in `.ssh/` then it should work, since the
public key file is in your `authorized_keys` file. If you do not want
the same private key file to be used to log into nodes as the one used
to log into the login node, then repeat create a new public private
key / public key pair on the cluster.  Append the public key to
your`authorized_key` file. Note, when appending, add the public key,
do not overwrite the existing file.
  
#### Troubleshooting

1. Many users set up an alias for the ssh command so they don’t have to type as much to log into the remote host.  You can do this by adding the following line to your `~/.bashrc`, `alias hpc='ssh -X <your_userid>@jhpce01.jhsph.edu'`.
2. If you followed the directions and ssh is still asking for a password, then it is likely that the permissions of the  `~/.ssh` directory  on the remote host, are not set correctly. To fix the permissions, execute the following command on the remote host `chmod -R o-rwx ~/.ssh`
3. Finally, if your changed the permissions of your `.ssh` directory
   and you are still being asked for a password, then perhaps the
   permissions on your home directory are such that others are allowed
   to write to it. If this is the case, then this needs to be changed
   so that only you can write to it. Contact bit support to have this
   changed.

### Windows machines 
If your desktop/laptop runs Microsoft Windows then you first need to
install MobaXterm on your windows machine. If you are
using MobaXterm, please use the steps at the bottom of the MobaXterm
Configuration Page.

## Disk Storage Space on the JHPCE Cluster
+ There are several types of storage on the JHPCE cluster. 
  + Some space is for permanent storage of files, and other spaces can be used for short term storage of data.
  + For long term storage of files, most users make use of their 100GB of space in their home directory. 
  + For those groups needing more, we have large storage arrays and sell allocations on these large arrays. [Storage docs](https://jhpce-jhu.github.io/jhpce_mkdocs/storage/).
  + We build a new storage array about every 18 months; so if you are interested in purchasing an allocation please email bitsupport@lists.jhu.edu. 
  + In addition to these long-term storage offerings, there are scratch space areas for short-term data storage. 
    + Scratch space tends to be faster. Using scratch space avoids taking up space in your home or project storage space.
  + The best area used for scratch is the `fastscratch` array on the cluster. The fastscratch array provides 22TB of space that is built on faster SSD drives, vs traditional hard drives used for project space and home directories. All users have a 1TB quota for scratch space, and data older than 30 days is purged.
  + Traditionally in Unix/Linux, `/tmp` or `/var/tmp` directories are used for storing temporary files. On the JHPCE cluster, this is strongly discouraged as these directories are small. 
  + In R, the `tmpdir()` setting will dictate where temporary files are stored. If you are generating 10s of GB of temporary files, change `tmpdir()` to `fastscratch`.
  + In SAS, the default `WORK` directory will be located under your `fastscratch` directory.
  + In Stata, the default `tempfile` location is under `/tmp`. This can be changed by setting the `STATATMP` environment variable.

### Fastscratch
+ You can access your Personal Scratch space by using the `$MYSCRATCH` environment variable from an interactive cluster node session, or within a submitted job.  
+ The actual absolute path to your Personal Scratch space is `/fastscratch/myscratch/$USER`.
+ There are some very important restrictions for using this personal scratch space.
+ There is currently a 1TB quota set on the personal scratch space.
+ All files older than 30 days will be removed without exception.  
+ Abuse of this space may result in files being deleted on an as needed basis.  
+ *This Personal Scratch space is meant to be a short-term storage location; it is not a long-term storage solution.*
+ If you `untar` or `unzip` a file, and the extracted files have a timestamp older than 30 days from the original bundle, they will be removed when the daily purge begins. To work around this, you can use the `touch` command to update the timestamp on the extracted files.
+ Even though there is a 30 day automatic deletion of data, we ask that you please remove data from your Personal Scratch space once you have finished using it.
+ The Fast Scratch space is not visible on the login nodes.  You will need to log into a compute node or a transfer node or submit a job in order to see this scratch space.
+ There is no cost for using your personal scratch space.
+ The personal scratch space has no redundancy, so scratch space drive failures result in data loss. Thus, move important files needed to be kept from scratch space ASAP.

## Permissions via ACL
Traditional Unix file and group permissions can be used to share
access to files and directories.  However, there can be times when
more fine-grained control of shared access is needed. To accomplish
this, Access Control Lists (ACLs) can be used.

[ACL Documentation](https://jhpce-jhu.github.io/jhpce_mkdocs/acl/)

## Encrypted filesystem
+ The JHPCE Cluster currently supports the following mechanisms for
using encrypted filesystems.
+ Encrypted filesystem are used to provide “Encryption At Rest”, meaning that the data on disk will be safely
stored in an encrypted format, and only available in an unencrypted
state when the data is accessed by an approved user.
+ Userspace encrypted filesystems using `encfs` `ZFS/Lustre` encrypted
filesystems.
+ The `DCL02` storage array is built on encrypted disk
devices.

### [File Transfer](https://jhpce.jhu.edu/knowledge-base/file-transfer/)
- [Setting up MobaXterm for File Transfers with the JHPCE Cluster](https://jhpce.jhu.edu/knowledge-base/setting-up-mobaxterm-for-file-transfers-with-the-jhpce-cluster/)
- [Using rclone to Access OneDrive](https://jhpce.jhu.edu/knowledge-base/using-rclone-to-access-onedrive/)

### [GPUs on the JHPCE Cluster under SLURM](https://jhpce.jhu.edu/knowledge-base/gpus-on-the-jhpce-cluster-under-slurm/)

### [JHPCE Web Portal](https://jhpce.jhu.edu/knowledge-base/jhpce-web-portal/)

### [Knowledge Base Articles from Aki Nishimura](https://jhpce.jhu.edu/knowledge-base/knowledge-base-articles-from-aki-nishimura/)

### [Knowledge Base Articles from Lieber Institute](https://jhpce.jhu.edu/knowledge-base/knowledge-base-articles-from-lieber-institute/)

### [MobaXterm Configuration](https://jhpce.jhu.edu/knowledge-base/mobaxterm-configuration/)

### [Numerical Linear Algebra Libraries](https://jhpce.jhu.edu/knowledge-base/numerical-linear-algebra-libraries/)

### [Running RStudio Server on the JHPCE Cluster](https://jhpce.jhu.edu/knowledge-base/running-rstudio-server-on-the-jhpce-cluster/)

### [SLURM](https://jhpce.jhu.edu/knowledge-base/slurm/)

### [Using Globus to Transfer Files](https://jhpce.jhu.edu/knowledge-base/using-globus-to-transfer-files/)

### [HOW-TO](https://jhpce.jhu.edu/knowledge-base/how-to/)

### [Community](https://jhpce.jhu.edu/knowledge-base/community/)
- [Perl](https://jhpce.jhu.edu/knowledge-base/perl/)
- [Python](https://jhpce.jhu.edu/knowledge-base/python/)
- [R](https://jhpce.jhu.edu/knowledge-base/r/)
- [Short Read Tools](https://jhpce.jhu.edu/knowledge-base/short-read-tools/)

