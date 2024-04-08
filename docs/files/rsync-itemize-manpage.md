# Rsync itemized output manual page section

The `-i` or `--itemize-changes` flag is especially helpful when trying to compare two directory trees when used with the `-n` or `--dry-run` argument. Or to document what was changed during a copy (direct the output into a text file since there can be a lot of info flying past you). 

However, it produces a cryptic string 11 letters long for each file or directory. We have copied a useful chart from the Internet that you can consult.

The general format is like the string **YXcstpoguax**
 
See also:

1. [Main rsync page](../files/rsync.md)
2. A visual [key to itemized output](../files/rsync-itemize-table.md)


```
The  "%i"  escape  has a cryptic output that is 11 letters long.
The general format is like the string YXcstpoguax,  where  Y  is
replaced  by the type of update being done, X is replaced by the
file-type, and the other letters represent attributes  that  may
be output if they are being modified.
  
The update types that replace the Y are as follows:
  
o      A  < means that a file is being transferred to the remote
       host (sent).
  
o      A > means that a file is being transferred to  the  local
       host (received).
  
o      A  c  means that a local change/creation is occurring for
       the item (such as the creation  of  a  directory  or  the
       changing of a symlink, etc.).
  
o      A  h  means  that the item is a hard link to another item
       (requires --hard-links).
  
o      A . means that the item is not being updated  (though  it
       might have attributes that are being modified).
  
o      A  * means that the rest of the itemized-output area con‐
       tains a message (e.g. "deleting").
  
The file-types that replace the X are: f for a file, a d  for  a
directory,  an  L for a symlink, a D for a device, and a S for a
special file (e.g. named sockets and fifos).
  
The other letters in the string indicate if some  attributes  of
the file have changed, as follows:
  
o      "." - the attribute is unchanged.
  
o      "+" - the file is newly created.
  
o      " "  - all the attributes are unchanged (all dots turn to
       spaces).
  
o      "?" - the change is unknown (when  the  remote  rsync  is
       old).
  
o      A letter indicates an attribute is being updated.
  
The attribute that is associated with each letter is as follows:
  
o      A  c  means  either  that  a regular file has a different
       checksum (requires --checksum) or that a symlink, device,
       or  special  file  has a changed value.  Note that if you
       are sending files to an rsync prior to 3.0.1, this change
       flag  will be present only for checksum-differing regular
       files.
  
o      A s means the size of a regular  file  is  different  and
       will be updated by the file transfer.
  
o      A t means the modification time is different and is being
       updated to the sender's value (requires --times).  An al‐
       ternate  value of T means that the modification time will
       be set  to  the  transfer  time,  which  happens  when  a
       file/symlink/device is updated without --times and when a
       symlink is changed and the receiver can't set  its  time.
       (Note:  when  using  an rsync 3.0.0 client, you might see
       the s flag combined with t instead of the proper  T  flag
       for this time-setting failure.)
  
o      A p means the permissions are different and are being up‐
       dated to the sender's value (requires --perms).
  
o      An o means the owner is different and is being updated to
       the sender's value (requires --owner and super-user priv‐
       ileges).
  
o      A g means the group is different and is being updated  to
       the sender's value (requires --group and the authority to
       set the group).
  
o      A u|n|b indicates the following information: u  means the
       access  (use)  time  is different and is being updated to
       the sender's value (requires --atimes); n means the  cre‐
       ate  time  (newness) is different and is being updated to
       the sender's value (requires  --crtimes);  b  means  that
       both the access and create times are being updated.
  
o      The a means that the ACL information is being changed.
  
o      The  x  means  that the extended attribute information is
       being changed.
  
One other output is possible: when deleting files, the "%i" will output  the  string  "*deleting" for each item that is being removed (assuming that you are talking to a  recent  enough  rsync that  it  logs deletions instead of outputting them as a verbose message).
```
