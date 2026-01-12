---
icon: material/eye
---

# **Navigating Deep Directory Trees**

Everyone on a computer use directory and file names to create logical structures
to hold their files. Researchers can create complex and/or numerous directories
to organize their files as new projects and data sets come along. Long file or
directory names are used to make things as self-obvious as possible.

Over time one winds up needing to work with files in widely scattered directories.

It can be a hassle to move around quickly and efficiently between
frequently-used directories with long names. Typing everything out every time is
tiresome and error-prone.

These two techniques can make life on the command line better:

1. A "redirect" directory of symbolic links
2. The commands `pushd`, `popd` and `dirs`

## **(A Nest of) Redirecting Symbolic Links**

### **TL;DR**

Create a directory of aliases pointing to far-flung files or directories.

1. Create a directory with a short name in your home directory.
2. Place symbolic links in that directory to files or directories. The target objects can be located all around the file system.
3. Use the tilde symbol (++tilde++), a forward slash, and your short redirect
directory name to quickly refer to this directory (no matter where you are
currently located in the file system), e.g. `~/redir/`
4. Now you can edit a far-away file or cd to a far-away directory using a _very short path_.
5. You can keep the name you use to refer to the object unchanged even if you
change the symbolic link to point to a different object, e.g. `~/redir/my-current-project`

### **Explanation and examples**
A *symbolic link* is a special kind of file which points at another file or a
directory. Also known as symlinks. The symbolic link takes up almost no space.
It is not a copy of the original file. Deleting the symlink does not delete the
file being pointed to. This alias does not have to have the same name as the
target object.

You can refer to the symbolic link and in most cases the results will be the
same as if you specified the original file. The most frequent change you will
need to make in your normal habits is, if you want to list the contents of
a symbolic link which points to a directory, you will need to add a trailing
forward slash. (Warning: some commands treat symbolic links in ways you might
not expect: cp, rm, rsync, tar. They may or may not by default operate on the
symbolic link itself, or on the target object. Each of their
man pages will discuss how they treat symlinks.)

You create a symlink like this:

`ln –s realfile newname`

The resulting files when listed with `ls –l`:

```bash
lrwxr-xr-x  1 tunison  wheel  8 Jan 11 10:12 newname@ -> realfile
-rw-r--r--  1 tunison  wheel  1386 Jan 11 10:11 realfile
```

So a way to make use of symlinks is to create a directory of them, and refer to
those when doing things like changing directories. I use the following scheme
myself. You can name your directory whatever you want, such as "redir" or “redirect”
instead of the shorter ”r” shown here.

```bash
mkdir ~/r
cd ~/r
ln –s /cms01/data/puf-free/VERICRED/2019 v2019
ln –s /transfer/in/c-myjhed-98765 incoming
ln –s ~/mycode/thatlanguage/src/analyze-cancer mynobelprize
ln –s ~/notes/2026/fall-semester/september-worklog.txt worklog.txt
```

Now you can use these names in your commands instead of the longer real directory names:
```
cd ~/r/v2019
vi ~/r/worklog.txt
```
And, when October 2026 rolls around, you can delete the symlink
`~/r/worklog.txt` and point it at the new file (because in this example you're trying to keep
your lab notes in single month increments).

## **The Pushd, Popd and Dirs Commands**

If you’re repeatedly working in several directories and don’t need to open multiple windows to look at them simultaneously, these commands allow you to switch rapidly between them. They use a “stack” data structure. Think of a stack as a pile of plates in a cafeteria. When you want one, you usually take the one from the top. Then a fresh one is exposed. You’ve “popped” a plate from the stack. If you “push” three plates onto the stack, then the stack is deeper. You can access the top plate easily, but in this case you can also get at the third plate down.

Here is a representation of pushing elements onto a stack, and then popping them off again:

![Image title](../files/images/dir-stack-push-pop.png)

`pushd directoryname`

changes your directory from the current one to directoryname, and creates a stack of two directories.
 
`dirs -v`

will list those directories in the stack.
 
`pushd`

by itself will switch you between the current directory and the top one in the stack. This is often the main way I use it.

`pushd +2`

will switch you between the current directory and the third down into the stack (third because the index into the stack starts with zero not one).

`popd`

will cd back to the top directory in the stack while removing your current directory from the stack.

??? Tip "Create aliases for an even sweeter experience"
    The `-v` option to dirs is so handy for figuring out what number to use with `pushd` that you may want to make that the default by adding an alias to your `$HOME/.bashrc file`. The author uses these aliases to make using these commands even better:

    ```
    alias dirs='dirs -v'
    alias pu='pushd'
    alias po='popd'
    ```

### **Create a default directory stack**

You can create a stack ahead of time using the –n option to pushd. That option adds the directory to the stack but does not change to it.
 
So if you used the nano text editor to add this to your .bashrc file, it would create a set of directories you can pushd between every time you log in!!!
 
Set up directory stack. **Note that they appear in stack in reverse order than listed here!!!**

`dirs -v` will show their order (and index number)

```
for i in /cms01/data /cms01/outgoing /users/55548 /cms01/incoming; do pushd -n $i 1>/dev/null;done
```
 
Here we use that technique after adding it to our .bashrc file:
 
```shell
[~]$ source .bashrc

[~]$ dirs
~ /cms01/incoming /users/55548 /cms01/outgoing /cms01/data

[~]$ pushd +2
/users/55548 /cms01/outgoing /cms01/data ~ /cms01/incoming

[55548]$ pwd
/users/55548

[55548]$ dirs -v
 0  /users/55548
 1  /cms01/outgoing
 2  /cms01/data
 3  ~
 4  /cms01/incoming
```
