## Compiling and installing software with no root privilege

You are allowed to download and install small software packages in your own home directory. In this page, we go through an example of installing a piece of free software that converts between different units of measurements, to show you the general steps needed to install a software from source.

!!! Warning "Warning"
    **Please do the software installation from a compute node**

### Step 1: Download source code
```
[test@compute-110 ~]$ mkdir download
[test@compute-110 ~]$ cd download/
[test@compute-110 download]$ wget http://ftp.gnu.org/gnu/units/units-2.23.tar.gz
--2024-03-18 11:57:38--  http://ftp.gnu.org/gnu/units/units-2.23.tar.gz
Resolving ftp.gnu.org (ftp.gnu.org)... 209.51.188.20, 2001:470:142:3::b
Connecting to ftp.gnu.org (ftp.gnu.org)|209.51.188.20|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1423494 (1.4M) [application/x-gzip]
Saving to: ‘units-2.23.tar.gz’

units-2.23.tar.gz                                 100%[==========================================================================================================>]   1.36M  --.-KB/s    in 0.1s    

2024-03-18 11:57:39 (12.7 MB/s) - ‘units-2.23.tar.gz’ saved [1423494/1423494]
```

### Step 2: Extract the source code
```
[test@compute-110 download]$ ls -l
-rw-r--r-- 1 test test 1423494 Feb 18 22:45 units-2.23.tar.gz

[test@compute-110 download]$ tar -zxvf units-2.23.tar.gz 
units-2.23/
units-2.23/definitions.units
units-2.23/units.txt
...
```

### Step 3: Configure the software
```
[test@compute-110 download]$ cd units-2.23/
[test@compute-110 units-2.23]$ ./configure --prefix=$HOME/mysoftware/units-2.23
```
!!! Note "Note"  
    The first thing to do is carefully read the `README` and `INSTALL` text files (use the `less` command). These contain important information on how to compile and the run the software.  
    Since you do not have root privilege to install the software on system area, you will need to specify the installation directory using `--prefix=/path/to/your/softwre`.  
 
### Step 4: Build and install
```
[test@compute-110 units-2.23]$ make
[test@compute-110 units-2.23]$ make install
```

!!! Note "Note"
    This will install the files into the `~/mysoftware/units-2.23` directory that you specified with `./configure`.

### Step 5: Add the software to path
```
[test@compute-110 ~]$ export PATH=$PATH:$HOME/mysoftware/units-2.23/bin
```
!!! Note "Note"
    You can add the above line in your `.bashrc` file so the software would be available when you login

### Step 6: Run the software
```
[test@compute-110 ~]$ units
...
You have: tempF(75)
You want: tempC
	23.888889
You have: exit
[test@compute-110 ~]$
```
