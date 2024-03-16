# Meaning of rsync itemized output

The `--itemize-changes` flag is especially helpful when trying to compare two directory trees. However, it produces a cryptic string 11 letters long for each file or directory. We have copied a useful chart from the Internet that you can consult.

There is also a copy of the section of the rsync manual page [here](../files/rsync-itemize-manpage.md) breaking this down further.

 The general format is like the string YXcstpoguax, where  Y  is replaced  by the type of update being done, X is replaced by the file-type, and the other letters represent attributes that may be output if they are being modified.

```
  YXcstpoguax  path/to/file
  |||||||||||
  `----------- the type of update being done::
   ||||||||||   <: file is being transferred to the remote host (sent).
   ||||||||||   >: file is being transferred to the local host (received).
   ||||||||||   c: local change/creation for the item, such as:
   ||||||||||      - the creation of a directory
   ||||||||||      - the changing of a symlink,
   ||||||||||      - etc.
   ||||||||||   h: the item is a hard link to another item (requires
  --hard-links).
   ||||||||||   .: the item is not being updated (though it might have attributes
  that are being modified).
   ||||||||||   *: means that the rest of the itemized-output area contains a
  message (e.g. "deleting").
   ||||||||||
   `---------- the file type:
    |||||||||   f for a file,
    |||||||||   d for a directory,
    |||||||||   L for a symlink,
    |||||||||   D for a device,
    |||||||||   S for a special file (e.g. named sockets and fifos).
    |||||||||
    `--------- c: different checksum (for regular files)
     ||||||||     changed value (for symlink, device, and special file)
     `-------- s: Size is different
      `------- t: Modification time is different
       `------ p: Permission are different
        `----- o: Owner is different
         `---- g: Group is different
          `--- u: The u slot is reserved for future use.
           `-- a: The ACL information changed

```
