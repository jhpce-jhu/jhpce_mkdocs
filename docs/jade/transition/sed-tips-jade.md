---
tags:
  - jade
  - transition
---
# THIS IS SIMPLY A PLACEHOLDER PAGE

# EXPLANATION OF SED AND USING IT TO MODIFY FILES IN PLACE

For example, if you want to change a path string in a bunch of batch job
scripts.

Include how to generate a list of filenames using find0 and xargs0 being
passed to a sed command.

This is a starting point:

```
Sed is a stream editor. It performs requested text transformations on a file or in the middle of a pipeline.   It works one line at a time. Here are some examples:
 
# Chg a pattern inline in an existing file while making a backup with suffix=.bak
# Only the first instance of the first string in each line is replaced by the second string
 
sed -i.bak 's/LogDenied/LogApproved/' filename
 
The command that sed will execute is provided within the single quotes. In this particular command, the “s” characters means “substitute”  The forward slashes are dividers between the original and the replacement strings.
 
In the following sed examples, the original file contains:
 
#!/bin/bash
# SBATCH --mem=40GB
# etcetera
 
cd /cms01/data/55548/deeper/stilldeeper/
 
for i in sample1 sample3 sample7
do
    /users/55548/c-jxu123-55548/bin/myscript ${i}.sas7bdat
done
 
# Chg pattern inline in an existing file while making a backup with suffix=bak
# All instances of the first string are replaced by the second string, b/c a “g” was added to the command
# Here the “(“ character is used as the divider b/c the pattern to be replaced contains forward slashes
 
sed -i.bak 's(/users/55548/c-jxu123-55548(~(g' filename
 
The original file is found as: filename.bak
 
The resulting file  (named filename) contains:
 
#!/bin/bash
# SBATCH --mem=40GB
# etcetera
 
cd /cms01/data/55548/deeper/stilldeeper/
 
for i in sample1 sample3 sample7
do
    ~/bin/myscript ${i}.sas7bdat
done
 
# This sed runs two separate commands in one pass. Each command is indicated by a “-e” flag
# All instances of the first string are replaced by the second string, b/c a “g” was added to the command
 
sed -i.bak -e 's(deeper/stilldeeper/(short/newplace/(' -e 's(/users/55548/c-jxu123-55548(~(g' filename
 
The original file is found as: filename.bak
 
The resulting file (named filename) contains:
 
#!/bin/bash
# SBATCH --mem=40GB
# etcetera
 
cd /cms01/data/55548/short/newplace/
 
for i in sample1 sample3 sample7
do
    ~/bin/myscript ${i}.sas7bdat
done
 
# Changing a bunch of files which have the same “badstring” in them
# This example runs sed on all of the files which have a certain suffix
# It does not make a backup copy of the files it modifies
# All instances of the first string are replaced by the second string, b/c a “g” was added to the command
 
find ~/project1 -name “*.html” -print0 | xargs -0 sed -i 's/badstring/mygoodstring/g'
```
