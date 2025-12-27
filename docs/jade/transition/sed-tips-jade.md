---
tags:
  - jade
  - transition
---
# 20251226 - PAGE BEING REVISED

# SED - Using sed to modify files

For example, if you want to change a path string in a bunch of batch job
scripts.

TODO: Include how to generate a list of filenames using find print0 and xargs -0 being
passed to a sed command.

Sed is the Stream EDitor. It performs requested text *transformations* on a
stream of input, as part of a shell pipeline. It works one line at a time, so
you cannot change a string which wraps across multiple lines.  It can also edit
a file in-place.  

The transformations are expressed as a pair of basic regular
expressions. (Use `-E` to get extended, modern regular expressions. See
re_format(7) manual page.)

<!-- ------------------------------------------------------------------------->
!!! example "Change name of the day of the week"
    Here we see sed operating on its standard input (stdin) to print something different on its standard output (stdout).
    ```bash
    % date
    Fri Dec 26 19:28:49 EST 2025
    
    % date | sed -e 's/Fri/Friday/'
    Friday Dec 26 19:29:32 EST 2025
    ```

<!-- ------------------------------------------------------------------------->
??? example "Change multiple things in one pass"
    Now we see sed making two transformations.  Each `-e` flag represents a
    separate transformation or program. (The programs are implemented in the
    order specified, so beware of stepping on your toes (creating a new string
    with the first program which then matches in the second.)
    ```bash
    % date
    Fri Dec 26 19:28:49 EST 2025

    % date | sed -e 's/2025/2032/' -e 's/Fri/Freakday/'
    Freakday Dec 26 19:37:23 EST 2032
    ```

<!-- ------------------------------------------------------------------------->
!!! info "Structure of the program string"
    `'s/find_pattern/replace_pattern/optional_chars'`<br><br>
    "s" means substitution<br>
    "/" is the "delimiter character" - this does not need to be forward slash
    "find_pattern" means the regular expression to look for in each line<br>
    "replace_pattern" means the regular expression replace with<br>
    "optional_chars" - an 'I' means case-insensitive, a 'g' means change all matches within a line.<br> 
    
<!-- ------------------------------------------------------------------------->
??? example "Change multiple occurences per line, case-insensitive"
    Each program is executed once, so only the first occurence of a match in a
    line is replaced, unless you add a 'g' after the third delimiter character.
    ```bash
    % echo "Bob susan franky-frank bobby" | sed -e 's/bob/paper/gi'
    paper susan franky-frank paperby
    ```
<!-- ------------------------------------------------------------------------->
??? example "Chg a pattern inline in an existing file while making a backup with suffix '.bak'"
    Only the first instance of the first string in each line is replaced by the second string
 
    ```bash
    % sed -i.bak 's/LogDenied/LogApproved/' filename

    The result will be two files, filename.bak which contains the original
    contents, and filename, which contains either the original contents or
    altered contents in case any occurrences of "LogDenied" were found. If they
    were, only the first match on each line was replaced.
    ```
 
<!-- ------------------------------------------------------------------------->
In the following sed examples, the original file contains:
 
```bash
#!/bin/bash
# SBATCH --mem=40GB
# SBATCH --partition=sas
 
cd /cms01/data/55548/deeper/stilldeeper/
 
for i in sample1 sample3 sample7
do
    /users/55548/c-jxu123-55548/bin/myscript ${i}.sas7bdat
done
```

<!-- ------------------------------------------------------------------------->

# LEFT OFF EDITING HERE

<!-- ------------------------------------------------------------------------->
??? example "Replace one path with another
```
# Chg pattern inline in an existing file while making a backup with suffix=bak
# All instances of the first string are replaced by the second string, b/c a “g” was added to the command
# Here the “(“ character is used as the divider b/c the pattern to be replaced contains forward slashes
 
sed -i.bak 's(/users/55548/c-jxu123-55548(~(g' filename
 
The original file is found as: filename.bak
 
The resulting file  (named filename) contains:
 
```bash
#!/bin/bash
# SBATCH --mem=40GB
# SBATCH --partition=sas
 
cd /cms01/data/55548/deeper/stilldeeper/
 
for i in sample1 sample3 sample7
do
    ~/bin/myscript ${i}.sas7bdat
done
``` 
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
