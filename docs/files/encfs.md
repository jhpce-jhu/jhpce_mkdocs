# Encrypted Filesystem with encfs | Joint HPC Exchange
One mechanism available on the JHPCE cluster for implementing an encrypted filesystem or directory is via the use of the FUSE based “encfs” application.  One key benefit of using “encfs” is that it is completely user based and thus can be set up by any user on the cluster at any time.

**Initial Setup**

Before using “encfs”, you will need to be added to a special group on the JHPCE cluster. Please email “bitsupport” and indicate that your would like to use encrypted filesystem. Once your account has been configured, you may proceed.

To setup your encrypted filespace, you will first need to login to a compute node, and create 2 directories, one that will contain your encrypted data and one directory that will be used as the point of access to your encrypted data.

```
[jhpce01 /users/bob]$ srun --pty --x11 bash
Last login: Thu Nov  3 14:33:05 2016 from jhpce01.cm.cluster
[compute-070 /users/bob]$ mkdir ENCRYPTED
[compute-070 /users/bob]$ mkdir UNENCRYPTED
```


Next, load the “encfs” module, and use the “encfs” program to initialize the encrypted directory, and enable access to the encrypted directory through the unencrypted directory. Before running the “encfs” program, you should have a password in mind that you will use to secure the directory. **Be sure that you use the full pathnames to the encrypted and unencrypted directories when using the “encfs” command.**

```
[compute-070 /users/bob/]$ module load encfs
[compute-070 /users/bob/]$ 
encfs /users/bob/ENCRYPTED /users/bob/UNENCRYPTED
Creating new encrypted volume.
Please choose from one of the following options:
 enter "x" for expert configuration mode,
 enter "p" for pre-configured paranoia mode,
 anything else, or an empty line will select standard mode.
?> (just hit enter here)

Standard configuration selected.

Configuration finished.  The filesystem to be created has
the following properties:
Filesystem cipher: "ssl/aes", version 3:0:2
Filename encoding: "nameio/block", version 3:0:1
Key Size: 192 bits
Block Size: 1024 bytes
Each file contains 8 byte header with unique IV data.
Filenames encoded using IV chaining mode.
File holes passed through to ciphertext.

Now you will need to enter a password for your filesystem.
You will need to remember this password, as there is absolutely
no recovery mechanism.  However, the password can be changed
later using encfsctl.

New Encfs Password: (enter password)
Verify Encfs Password: (re-enter password)
[compute-070 /users/bob]$ df -h
...
encfs                  50T   17T   33T  34% /users/bob/UNENCRYPTED
```


Please be sure that you remember the password used to set up your encrypted space. If the password is lost, there is no way to access the unencrypted data.

Now, you can copy data into your unencrypted directory, and it will become automatically encrypted in your encrypted directory.  All work should be done in the unencrypted directory.

When you have finished using your encrypted space, you can remove access to the unencrypted space by using the “fusermount” command:

```
[compute-070 /users/bob]$ ls ENCRYPTED
[compute-070 /users/bob]$ ls UNENCRYPTED
[compute-070 /users/bob]$ cd UNENCRYPTED
[compute-070 /users/bob/UNENCRYPTED]$ echo "This is a test" > file1
[compute-070 /users/bob/UNENCRYPTED]$ cd ..
[compute-070 /users/bob]$ ls -l UNENCRYPTED
-rw-r----- 1 bob mmi 15 Jan 27 16:34 file1
[compute-070 /users/bob]$ ls -l ENCRYPTED
-rw-r----- 1 bob mmi   23 Jan 27 16:34 Fng4AKmAzYP6OrpOrCPH3J,x
[compute-070 /users/bob]$ cat UNENCRYPTED/file1
This is a test
[compute-070 /users/bob]$ cat ENCRYPTED/Fng4AKmAzYP6OrpOrCPH3J,x 
?tS?$??#?q?%o띁?
[compute-070 /users/bob]$ fusermount -u /users/bob/UNENCRYPTED
[compute-070 /users/bob]$ ls -l UNENCRYPTED
[compute-070 /users/bob]$ ls -l ENCRYPTED
-rw-r----- 1 bob mmi   23 Jan 27 16:34 Fng4AKmAzYP6OrpOrCPH3J,x
[compute-070 /users/bob]$ exit
Connection to compute-070.cm.cluster closed.
```


Once you run the “fusermount” command, your unencrypted access point will be removed, and your data will remain safely encrypted on the system. Other users will not have access to the unencrypted data.

**Ongoing Usage**

The next time you need to access your data in an unencrypted state, login to a compute nodes, and rerun the “encfs” command.

```
[jhpce01 /users/bob]$ srun --pty --x11 bash
Last login: Fri Jan 27 16:10:27 2017 from jhpce01.cm.cluster
[compute-097 /users/bob]$ ls -l UNENCRYPTED
[compute-097 /users/bob]$ ls -l ENCRYPTED
-rw-r----- 1 bob mmi   23 Jan 27 16:34 Fng4AKmAzYP6OrpOrCPH3J,x
[compute-097 /users/bob]$ module load encfs
[compute-097 /users/bob]$ 
encfs /users/bob/ENCRYPTED /users/bob/UNENCRYPTED
EncFS Password: (reenter the password you used to encrypt the data)
[compute-097 /users/bob]$ ls -l UNENCRYPTED
-rw-r----- 1 bob mmi 15 Jan 27 16:34 file1
[compute-097 /users/bob]$ cd UNENCRYPTED
[compute-097 /users/bob/UNENCRYPTED]$ cat file1
This is a test
```


As before, when you are done accessing your data, use the “fusermount” command to remove access to the unencrypted space.

```
[compute-097 /users/bob/UNENCRYPTED]$ cd ..
[compute-097 /users/bob]$ fusermount -u /users/bob/UNENCRYPTED
[compute-097 /users/bob]$ ls -l /users/bob/UNENCRYPTED
[compute-097 /users/bob]$ ls -l ENCRYPTED
-rw-r----- 1 bob mmi   23 Jan 27 16:34 Fng4AKmAzYP6OrpOrCPH3J,x
[compute-097 /users/bob]$ exit
Connection to compute-097.cm.cluster closed.
```


One final important note. If you do not run the “fusermount -u” command before logging out, the unencrypted directory **will** remain available on the compute node. While access to it will not be available to other users, this is in all likelihood undesirable. Please be sure to run “fusermount -u” before logging out of the compute node.

**Sharing Data**

If you want to grant access to your unencrypted data to other users, it is possible, but you will need to perform a few additional steps. First you will need to set up permissions on the files and directories at the Unix level using either standard group permissions or with ACLs. While it is possible to do this in the encrypted space, it is highly recommended that this be done while you have the space unencrypted so that you can see the true file names that you are working with. Once you have granted access to the files at the Unix level, other users can use the “encfs” command to create their own unencrypted entry point to the encrypted data. They will need to use the same password that was used when the encrypted file space was set up.

So, the first step in sharing your data is to grant access to your directory (and directories above it) to another user.

```
[jhpce01 /users/bob]$ srun --pty --x11 bash
Last login: Fri Jan 27 17:12:11 2017 from jhpce01.cm.cluster
[compute-108 /users/bob]$ module load encfs
[compute-108 /users/bob]$ 
encfs /users/bob/ENCRYPTED /users/bob/UNENCRYPTED
EncFS Password: (reenter the password you used to encrypt the data)
[compute-108 /users/bob]$ setfacl -m user:chuck:rx .
[compute-108 /users/bob]$ setfacl -m user:chuck:rwx UNENCRYPTED
```


Next, the other user will need to login to a compute node, and run “encf”, however they will need to use a different directory for their unencrypted space.

```
[jhpce01 /users/chuck]$ srun --pty --x11 bash
Last login: Fri Jan 27 15:44:11 2017 from jhpce01.cm.cluster
[compute-049 /users/chuck]$ module load encfs
[compute-049 /users/chuck]$ mkdir UNENCRYPTED
[compute-049 /users/chuck]$ 
encfs /users/bob/ENCRYPTED /users/chuck/UNENCRYPTED
EncFS Password: (reenter the password you used to encrypt the data)
[compute-049 /users/chuck]$ cd UNENCRYPTED
[compute-049 /users/chuck/UNENCRYPTED]$ ls -l
-rw-r----- 1 bob mmi 15 Jan 27 16:34 file1
[compute-049 /users/chuck/UNENCRYPTED]$ echo "This is chuck" > file2
[compute-049 /users/chuck/UNENCRYPTED]$ ls -l
-rw-r----- 1 bob   mmi 15 Jan 27 16:34 file1
-rw-r----- 1 chuck mmi 14 Jan 27 17:24 file2
```


At this point the original user will also be able to see the changes that the new user made.

```
[compute-108 /users/bob]$ ls -l /users/bob/UNENCRYPTED
-rw-r----- 1 bob   mmi 15 Jan 27 16:34 file1
-rw-r----- 1 chuck mmi 15 Jan 27 17:24 file2
```


The original user can use remove their unencrypted space, and the second user can continue to use their space. There is no dependency on having the creator active.

```
[compute-108 /users/bob]$ fusermount -u /users/bob/UNENCRYPTED
[compute-108 /users/bob]$ exit
```


Of course, the second user should also use “fusermount -u” on their own directories before logging out.

```
[compute-049 /users/chuck/UNENCRYPTED]$ cd ..
[compute-049 /users/chuck] fusermount -u /users/chuck/UNENCRYPTED
[compute-049 /users/chuck] exit
```


This process can be extended to several users – it is not limited to just 2 users.
