---
tags:
  - jade
  - transition
---
# SED - Using sed to modify files

## Overview and Introduction
Sed is the **S**tream **ED**itor. It performs requested text *transformations* on a
stream of input, as part of a shell pipeline.  (It can also edit a file in-place.)

UNIX offers many text manipulation utilities, such as `sed`, `tr`, and `awk`. Sed can
be extremely useful if you can identify a string which you would like to transform.

{==If you have a dozen SLURM batch job scripts which contain a file path string,
sed can be used to easily modify that path string to conform to a new
reality.==}

**The purpose of this document is to teach you how to do that specific task.**

You can modify a whole mass of files in a directory tree by using other
UNIX utilities like `find` and `xargs` to create a list of files for sed to work
on.

Sed's primary limitation is that it works one line at a time, so you cannot
easily change a string which wraps across multiple lines (unless you know that the
string is split in a consistent fashion). It IS possible but requires a
significant amount of regular expression knowledge -- [see
here](https://www.gnu.org/software/sed/manual/html_node/Text-search-across-multiple-lines.html)

<!-- ------------------------------------------------------------------------->
!!! example "Change name of the day of the week"
    Here we see sed operating in a pipeline, reading material from its standard
    input (stdin), possibly modifying it if a match is found, and printing the
    result to its standard output (stdout). (If no match is found, sed simply
    prints what was passed to it.)

    ```bash
    % date
    Fri Dec 26 19:28:49 EST 2025
    
    % date | sed -e 's/Fri/Friday/'
    Friday Dec 26 19:29:32 EST 2025
    ```

<!-- ------------------------------------------------------------------------->
!!! info "Structure of a sed program string"
    `-e 's/find_pattern/replace_pattern/optional_chars'`
    <br><br>

    * "-e" means a program will follow
    * 'programs need to be surrounded by single quote marks'
    * "s" means substitution<br>
    * "/" is the "delimiter character" (this does not need to be a forward * slash)<br>
    * "find_pattern" is the regular expression (RE) to look for in each line<br>
    * "replace_pattern" is the replacement RE<br>
    * "optional_chars" - a variety of letters, which can be combined to indicate
    actions to take or modifiers for the transformation process. Examples:
        * 'i' means case-insensitive
        * 'g' means change all matches found within a line

    {==Sed can be given a *large* number of combinations of arguments and
    programs. The above program string is just one form, and we describe it
    because it is how you can use sed to modify file path strings.==}

    Sed works with regular expressions (RE). RE's are *formulas* which follow
    certain rules. Some characters represent themselves, while others, such as
    square brackets [ ], ^ and $ have special meanings.

    Sed uses basic regular expressions (BRE) unless you specify
    the `-E` flag. Then sed works with extended, modern regular expressions.
    (For details, see the [GNU sed manual's RE
    section](https://www.gnu.org/software/sed/manual/sed.html#sed-regular-expressions).)
    An excellent resource for Regular Expressions is
    [https://regex101.com](https://regex101.com), which includes a test
    interpreter. You can provide sample text and a regular expression, and it
    will show you whether it finds any matches.
    
<!-- ------------------------------------------------------------------------->
??? example "Change multiple things with one command  (click to expand sections like this)"
    In the first example, we see sed making two transformations.

    The programs are implemented in the order specified, so beware of stepping on
    your own toes, where the first program changes strings such that second or
    third programs then fail to do what you expect.

    ```bash
    % date
    Fri Dec 26 19:28:49 EST 2025

    % date | sed -e 's/2025/2032/' -e 's/Fri/Freakday/'
    Freakday Dec 26 19:37:23 EST 2032
    ```

    !!! example "Change multiple occurences per line, case-insensitive"
        Each program is executed once, so only the first occurence of a match in a
        line is replaced, unless you add a 'g' after the third delimiter character.
        ```bash
        % echo "Bob susan franky-frank bobby" | sed -e 's/bob/paper/gi'
        paper susan franky-frank paperby
        ```
<!-- ------------------------------------------------------------------------->
??? example "Chg a pattern inline in a file while making a backup with suffix '.bak'"
 
    ```bash
    % sed -i.bak 's/LogDenied/LogApproved/' filename
    ```
    The result will be two files: `filename.bak` which contains the original
    contents, and `filename`, which contains either the original contents or
    altered contents if any occurrences of "LogDenied" were found. (If they
    were found, only the first match on each line was replaced because there is
    no trailing 'g'.)

    {==Note: there are no spaces between the "-i" and the ".bak" suffix name.==}

<!-- ------------------------------------------------------------------------->
## Modifying SLURM batch job scripts with sed

In the following sed examples, the original file is named `sample.sh` and contains:
 
```bash
#!/bin/bash
# SBATCH --mem=40GB
# SBATCH --partition=sas
 
cd /cms01/data/55548/deeper/stilldeeper/further
 
for i in sample1 sample3 sample7
do
    /users/55548/c-jxu123-55548/bin/myscript ${i}.sas7bdat
done
```

This C-SUB script contains three things which need to be modified to work in
JADE:

* In the C-SUB, Controlled Unclassified Information (CUI) was stored in `/cms01/data/<dua>/`<br>
In JADE, CMS CUI is stored in `/data/cms/<cdua>/cui/`
* In the C-SUB, home directories had the form `/users/<dua>/<username>/`<br>
In JADE, they have the form `/users/<comm>/<cdua>/<username>/`
* In the C-SUB, the SAS program only ran if it were launched in a "sas" job * partition.<br>
In JADE, each community's partitions' names begin with their community letter and a dash, so the CMS SAS partition is named `c-sas`.

<!-- ------------------------------------------------------------------------->
??? example "Replace `/cms01/data/55548/` with `/data/cms/c55548/cui/`"

    {==Those strings contain forward slashes, so we change our sed program delimiter character to “(“.==}
    ```bash
    sed -i.bak 's(/cms01/data/55548/(/data/cms/c55548/cui/(g' sample.sh
    ``` 
    The original file is found as: `sample.sh.bak`
 
    The modified resulting file (named `sample.sh`) contains:

    ```bash
    #!/bin/bash
    # SBATCH --mem=40GB
    # SBATCH --partition=sas
 
    cd /data/cms/c55548/cui/deeper/stilldeeper/further
 
    for i in sample1 sample3 sample7
    do
         /users/55548/c-jxu123-55548/bin/myscript ${i}.sas7bdat
    done
    ``` 
<!-- ------------------------------------------------------------------------->
??? example "Correct the home directory path"

    We can correct the home directory path in two ways -- give its exact full
    path, or hide the leading path elements using the ++tilde++ character. 

    In UNIX, ++tilde++ is replaced by the shell with the path to someone's home
    directory. If used for yourself, you can say either `~` or include your
    username, e.g.  `~c-jxu123-55548`. If used for someone else's home directory,
    then you always need to include their username, e.g. `~that-bob-guy`

    !!! example "Use the full path"
	```bash
        sed -i.bak2 's(/users/55548/c-jxu123-55548(/users/cms/c55548/c-jxu123-55548(g' sample.sh
	```
	The resulting `sample.sh` would contain <br>
         `/users/cms/c55548/c-jxu123-55548/bin/myscript ${i}.sas7bdat`

    !!! example "Use a tilde"
	```bash
        sed -i.bak2 's(/users/55548/c-jxu123-55548(~(g' sample.sh
	```
	The resulting `sample.sh` would contain <br>
         `~/bin/myscript ${i}.sas7bdat`
<!-- ------------------------------------------------------------------------->
??? example "Correct the partition name to be `c-sas`"

    ```bash
    sed -i.bak3's(--partition=sas(--partition=c-sas(' sample.sh
    ``` 
    {==We always recommend your making backups of files you modify with sed, but
    you'll need to be conscious of what you're doing so you can go back to the
    desired point in time. In this series of sed commands, I changed the suffix
    each time so you would have copies of the file before each change. If I had
    always used the same suffix, you would wind up with only the file before
    the last run.

    If you know that you've already made a backup of the original file, you can
    drop the file suffix to `-i` from second and later sed commands.

    Note!! Using the `-i` argument will always result in updating files 
    modification times, whether the file contained a matching pattern or not.
    So even files which whose content was not actually modified wind up looking
    changed according to an `ls -l` command.==}


<!-- ------------------------------------------------------------------------->
If you applied the three sed commands mentioned above, you would wind up with:

The original, unmodified file is found at `sample.sh.bak` while modified intermediate
files are named `sample.sh.bak2` and `sample.sh.bak3`
    
The final `sample.sh` contains:

```bash
#!/bin/bash
# SBATCH --mem=40GB
# SBATCH --partition=c-sas

cd /data/cms/c55548/cui/deeper/stilldeeper/further

for i in sample1 sample3 sample7
do
    ~/bin/myscript ${i}.sas7bdat
done
``` 
## Modifying many files at once

### Using a list of files found in a text file

You can make a list of file pathnames and process them with `sed`
using a `for` loop. This allows you to be certain about the files
you might change.

??? example "How to do that"
    Create a plain text file with one file on each line. The path to that file
    needs to either be absolute or, if relative, be correct relative to the
    directory in which you execute your `for` loop command.

    ```
    ./myproj/sample15/dog.R
    /data/cms/c34345/joe/sample97/deer.R
    ./myproj/sample15/mouse.sh
    ```
    Send the file names to `sed`:
    ```bash
    % for i in `cat myinput.txt` ; do echo $i ; sed -i.orig 's/pat1/pat2/gi' ${i};done
    ```

You can also use other commands to create a list of files in a file,
then edit that list to include only the files you want to modify. Or simply
verify for yourself that you're listing only the right type of files.

??? example "How to do that"
    ```bash
    ls -1 */script* journey/*.scr > myinput.txt
    nano myinput.txt
    % for i in `cat myinput.txt` ; do echo $i ; sed -i.orig 's/pat1/pat2/gi' ${i};done
    ```

### Using the find command

The `find` command will print the names of files which match the criteria you
specify. Find's first argument is a location in the file system. By default find
will start a recursive search starting at that point and proceeding downwards. That 
location can be "." if you want to start in the current working directory.

The following command will change a string, if found, in as many files which
exist that match the search criteria.

`find ~/project1 -type f -name “*\.html” -print0 | \`<br>
`   xargs -0 sed -i.orig 's/badstring/mygoodstring/g'`

That long command is broken down into two lines to improve readability using
an escape character.

??? tip "Escaping special characters"
    The `bash` shell you use gives certain characters special meanings. If you want
    to prevent them from being interpreted in the normal fashion, usually you 
    preceed that character with a ++backslash++. This is called "escaping" a
    character.

    The ++return++ key is one of those special characters. If you escape that key,
    then bash will allow you to continue a command onto the next line of the
    terminal window, where you indicate to bash that it should execute the
    command now with a final ++return++.

    You also need to escape characters inside of regular expressions, which has
    it's ***own*** list of special characters, include ++period++  The string
    "*\.html" includes a backslash to escape the period, which in REs stands
    for "any single character", not a period!

Breaking it down, left to right:

* start looking at the directory named "project1" inside of your home directory
* only consider files (not directories)
* only consider files ending in ".html"
* print names of matching files, in a way that works better with names containing spaces
* pass those names to the `xargs` program
* with the "-0" flag, which works with the `find` program's "-print0" flag
* and execute the sed program.
* Because the sed flag "-i.orig" is given, changes will be made to the named files
where they are located. Backups will be made of each file processed.

!!! tip "Find all bash scripts matching certain criteria, and fix a path"

    `find ~/project1 -type f -iname "run_test*" -print0 | xargs -0 file | \`<br>
    `  grep -i Bourne-Again | sed -e 's(: .*((' | \`<br>
    `  xargs sed -i 's(/cms01/incoming(/transfer/in/cms(g'`
    
    This example finds some files using a case-insensitive name, runs the `file`
    command on them, searches for ones that `find` reports to be Bourne-Again,
    uses sed to trim off everything `file` printed after the file name, then 
    has `xargs` execute the final `sed`

### Find command examples

You might find combinations of these useful when trying to identify the files you want to modify
with sed, or control which files are considered.

Print out the names of bash scripts in your home directory<br>
Note that this command does NOT correctly deal with filenames containing spaces!
You should use instead the example above where "find -print0" feeds into "xargs
-0"
`find ~ -type f -exec file {} \; | grep -i Bourne-Again | sed -e 's(: .*(('`

Print out the sizes of SLURM job output files owned by user c-jxu123-55548:<br>
`find /users/cms/c55548/shared -type f -name "*.out" -exec du -sh {} \;`

Print the names of files modified after noon of 9/11/2025<br>
```bash
% touch -t 202509111200 911-noon
% find . -newer 911-noon -print
```

Print the names of files _modified_ in February of this year:<br>
`find . -newermt "2025-02-01" -a ! -newermt "2025-02-28" -print`

Print the names of files _created_ in February of this year:<br>
`find . -newerct "2025-02-01" -a \! -newerct "2025-02-28" -print`

Print the names of executable files _modified_ in the last 24 hours:<br>
`find . -mtime 0 -executable`

Print out the names of files containing either "School" or "semester":<br>
`find . -name "*School*" -o -name "*semester*" -print`

Print out the names of files containing both "School" and "semester":<br>
`find . -name "*School*" -a -name "*semester*" -print`

Print out the names of files containing either "School" or "semester" but NOT susyq:<br>
`find . \( -name "*School*" -o -name "*semester*" \) -a \! -name 'susyq' -print`

Print the names of files except those found in directories named "bak":<br>
`find . \( -type d -name bak -prune \) -o -type f -print`

Print out the names of every directory in your home directory down to the third level:<br>
`find ~ -type d -maxdepth 3`

Print out the names of every immediate subdirectory of your home directory:<br>
`find ~ -type d -maxdepth 1`

Print the names of files named doghouse using a case-insensitive form of -name:<br>
`find /some_dir/susy -iname doghouse -print`

## Useful sed commands

For grins, in case any of them strike your interest.

Delete completely blank lines<br>
sed '/^$/d' 

Delete commented-out lines (only where # is the first character of a line)<br>
sed '/^#/d' 

Delete commented-out lines (where # is the first non-whitespace character of a line)<br>
sed '/^[[:space:]]*#/d' 

Delete white space (spaces and tabs)<br>
sed '/^[[:space:]]*$/d' 

Convert tabs into 6 spaces<br>
sed '/\t/      /' 

Delete the first seven line<br>
sed '7d'

Delete specified lines (5 thru 10, and 12)<br>
sed -i -e '5,10d;12d' filename


