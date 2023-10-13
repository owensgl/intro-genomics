---
title: Unix commandline
element: lab
layout: default
---

>## Challenge
>
>Use the `-l` option for the `ls` command to display more information for each item
>in the directory. What is one piece of additional information this long format
>gives you that you don't see with the bare `ls` command?
    
```bash
[grego@indri ~]$ ls -l
```
    
```output
total 4
drwxr-xr-x 2 grego grego 4096 Nov 15  2017 untrimmed_fastq
```

The additional information given includes the name of the owner of the file,
    when the file was last modified, and whether the current user has permission
    to read and write to the file.

***

### Finding hidden directories

First navigate to the `shell_data` directory. There is a hidden directory within this directory. Explore the options for `ls` to
find out how to see hidden directories. List the contents of the directory and
identify the name of the text file in that directory.

Hint: hidden files and folders in Unix start with `.`, for example `.my_hidden_directory`


## Solution

First use the `man` command to look at the options for `ls`.

```bash
[grego@indri shell_data]$ man ls
```

The `-a` option is short for `all` and says that it causes `ls` to "not ignore
entries starting with ." This is the option we want.

```bash
[grego@indri shell_data]$ ls -a
```

```output
.  ..  .hidden  untrimmed_fastq
```

The name of the hidden directory is `.hidden`. We can navigate to that directory
using `cd`.

```bash
[grego@indri shell_data]$ cd .hidden
```

And then list the contents of the directory using `ls`.

```bash
[grego@indri shell_data]$ ls
```

```output
youfoundit.txt
```

The name of the text file is `youfoundit.txt`.


***

## Navigating practice

Navigate to your home directory. From there, list the contents of the `untrimmed_fastq`
directory.


## Solution

```bash
$ cd
$ ls shell_data/untrimmed_fastq/
```

```output
bullkelp_001_R1.fastq.gz  bullkelp_001_R2.fastq.gz 
```

***

## Relative path resolution

Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
what will `ls ../backup` display?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original pnas_final pnas_sub`

![](figs/filesystem-challenge.svg){alt='File System for Challenge Questions'}


## Solution

1. No: there *is* a directory `backup` in `/Users`.
2. No: this is the content of `Users/thing/backup`,
  but with `..` we asked for one level further up.
3. No: see previous explanation.
  Also, we did not specify `-F` to display `/` at the end of the directory names.
4. Yes: `../backup` refers to `/Users/backup`.

***


Do each of the following tasks from your current directory using a single
`ls` command for each:

1. List all of the files in `/usr/bin` that start with the letter 'c'.
2. List all of the files in `/usr/bin` that contain the letter 'a'.
3. List all of the files in `/usr/bin` that end with the letter 'o'.

Bonus: List all of the files in `/usr/bin` that contain the letter 'a' or the
letter 'c'.

Hint: The bonus question requires a Unix wildcard that we haven't talked about
yet. Try searching the internet for information about Unix wildcards to find
what you need to solve the bonus problem.


## Solution

1. `ls /usr/bin/c*`
2. `ls /usr/bin/*a*`
3. `ls /usr/bin/*o`  

***

## Exercise

1. Print out the contents of the `~/shell_data/untrimmed_fastq/bullkelp_001_R1.fastq` file. 
   What is the last line of the file? What command would only print the last lines of a file?
2. From your home directory, and without changing directories,
  use one short command to print the contents of all of the files in
  the `~/shell_data/untrimmed_fastq` directory.


## Solution

1. The last line of the file is `C:CCC::CCCCCCCC<8?6A:C28C<608'&&&,'$`.
  `tail` prints the last lines of a file. 
2. `cat ~/shell_data/untrimmed_fastq/*`


***

## Exercise

Starting in the `shell_data/untrimmed_fastq/` directory, do the following:

1. Make sure that you have deleted your backup directory and all files it contains.
2. Create a backup of each of your FASTQ files using `cp`. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't
  learned yet how to do this
  with a wildcard.)
3. Use a wildcard to move all of your backup files to a new backup directory.
4. Change the permissions on all of your backup files to be write-protected.

:::::::::::::::  solution

## Solution

1. `rm -r backup`
2. `cp bullkelp_001_R1.fastq bullkelp_001_R1-backup.fastq` and `cp bullkelp_001_R2.fastq bullkelp_001_R2-backup.fastq`
3. `mkdir backup` and `mv *-backup.fastq backup`
4. `chmod -w backup/*-backup.fastq`  
  It's always a good idea to check your work with `ls -l backup`. You should see something like:

```output
-r--r--r-- 1 grego grego 47552 Nov 15 23:06 bullkelp_001_R1-backup.fastq
-r--r--r-- 1 grego grego 43332 Nov 15 23:06 bullkelp_001_R2-backup.fastq
```
  Bonus: `ls /usr/bin/*[ac]*`

