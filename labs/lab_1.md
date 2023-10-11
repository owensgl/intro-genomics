---
title: Unix commandline
element: lab
layout: default
---

# Introduction 
## objectives

- Describe key reasons for learning shell.
- Navigate your file system using the command line.
- Access and read help files for `bash` programs and use help files to identify useful command options.
- Demonstrate the use of tab completion, and explain its advantages.

***

## questions

- What is a command shell and why would I use one?
- How can I move around on my computer?
- How can I see what files and directories I have?
- How can I specify the location of a file or directory on my computer?

***

## What is a shell and why should I care?

A *shell* is a computer program that presents a command line interface
which allows you to control your computer using commands entered
with a keyboard instead of controlling graphical user interfaces
(GUIs) with a mouse/keyboard/touchscreen combination.

There are many reasons to learn about the shell:

- Many bioinformatics tools can only be used through a command line interface. Many more
  have features and parameter options which are not available in the GUI.
  BLAST is an example. Many of the advanced functions are only accessible
  to users who know how to use a shell.
- The shell makes your work less boring. In bioinformatics you often need to repeat tasks with a large number of files. 
  With the shell, you can automate those repetitive tasks and leave you free to do more exciting things.
- The shell makes your work less error-prone. When humans do the same thing a hundred different times
  (or even ten times), they're likely to make a mistake. Your computer can do the same thing a thousand times
  with no mistakes.
- The shell makes your work more reproducible. When you carry out your work in the command-line
  (rather than a GUI), your computer keeps a record of every step that you've carried out, which you can use
  to re-do your work when you need to. It also gives you a way to communicate unambiguously what you've done,
  so that others can inspect or apply your process to new data.
- Many bioinformatic tasks require large amounts of computing power and can't realistically be run on your
  own machine. These tasks are best performed using remote computers or cloud computing, which can only be accessed
  through a shell.

In this lesson you will learn how to use the command line interface to move around in your file system.

## How to access the shell

On a Mac or Linux machine, you can access a shell through a program called "Terminal", which is already available
on your computer. The Terminal is a window into which we will type commands. If you're using Windows,
you'll need to download a separate program to access the shell.

To save time, we are going to be working on a remote server where all the necessary data and software available.
When we say a 'remote server', we are talking about a computer that is not the one you are working on right now.
You will access the remote server where everything is prepared for the lesson.
We will learn the basics of the shell by manipulating some data files. Some of these files are very large
, and would take time to download to your computer.
We will also be using several bioinformatic packages in later lessons and installing all of the software
would take up time even more time. A 'ready-to-go' server lets us focus on learning.

## How to access the remote server

All students will be accessing the server using their UVic Netlink ID. You will use the `ssh` or `Secure Shell`
command to login to the remote server, while logged into the [UVic VPN](https://www.uvic.ca/systems/services/internettelephone/remoteaccess/).

```bash
ssh your_id@indri.rcs.uvic.ca
```
You will be prompted to use Duo, two-factor authentication before you enter your password. After approving the authentication on your phone,
you will use your netlink ID to login.

```output
(base) p165-072:~ gregoryowens$ ssh grego@fossa.rcs.uvic.ca
*
* Acceptable Use of Electronic Information Resources
*
* Access to this system is prohibited except for authorized University of
* Victoria students, faculty, and staff.
*
* All activity may be logged/monitored and unauthorized access will be handled
* per the provisions of UVic's Policy IM7200: Acceptable Use of Electronic
* Information Resources.
*
* https://www.uvic.ca/universitysecretary/assets/docs/policies/IM7200_6030_.pdf
*
(grego@fossa.rcs.uvic.ca) Duo two-factor login for grego

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-XXXX

Passcode or option (1-1):
```


After logging in, you will see a screen showing something like this:

```output
To access CVMFS modules please source the appropriate profile.
For example: 'source /cvmfs/soft.computecanada.ca/config/profile/bash.sh'

Last failed login: Thu Oct  5 10:44:07 PDT 2023 from 142.104.165.72 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Thu Oct  5 09:48:45 2023 from 142.104.165.72
```

This provides information about the remote server that you're logging into. We're not going to use most of this information for
our workshop, so you can clear your screen using the `clear` command.

Type the word `clear` into the terminal and press the `Enter` key.

```bash
[grego@indri ~]$ clear
```

This will scroll your screen down to give you a fresh screen and will make it easier to read.
You haven't lost any of the information on your screen. If you scroll up, you can see everything that has been output to your screen
up until this point.


>### Tip
>
>Hot-key combinations are shortcuts for performing common commands.
>The hot-key combination for clearing the console is `Ctrl+L`. Feel free to try it and see for yourself.

***

## Navigating your file system

The part of the operating system that manages files and directories
is called the **file system**.
It organizes our data into files,
which hold information,
and directories (also called "folders"),
which hold files or other directories.

Several commands are frequently used to create, inspect, rename, and delete files and directories.



```bash
[grego@indri ~]$
```

The dollar sign is a **prompt**, which shows us that the shell is waiting for input;
your shell may use a different character as a prompt and may add information before
the prompt. When typing commands, either from these lessons or from other sources,
do not type the prompt, only the commands that follow it. In this case, the prompt is
showing that my username is **grego**, and I'm logged into **indri**. The **~** is a special
represents character that represents the **home** directory. You will notice that as you move directories
this changes to show you the current directory you are in. 

To more explicitely find out where you are lets run a command called `pwd`
(which stands for "print working directory").
At any moment, our **current working directory**
is our current default directory,
i.e.,
the directory that the computer assumes we want to run commands in,
unless we explicitly specify something else.
Here,
the computer's response is `/home/grego`,
which is the top level directory within our cloud system:

```bash
[grego@indri ~]$ pwd
```

```output
/home/grego
```

Let's look at how our file system is organized. We can see what files and subdirectories are in this directory by running `ls`,
which stands for "listing":

```bash
[grego@indri ~]$ ls
```

```output
shell_data
```

`ls` prints the names of the files and directories in the current directory in
alphabetical order,
arranged neatly into columns.
We'll be working within the `shell_data` subdirectory, and creating new subdirectories, throughout this workshop.

The command to change locations in our file system is `cd`, followed by a
directory name to change our working directory.
`cd` stands for "change directory".

Let's say we want to navigate to the `shell_data` directory we saw above.  We can
use the following command to get there:

```bash
[grego@indri ~]$ cd shell_data
```

Let's look at what is in this directory:

```bash
[grego@indri ~]$ ls
```

```output
untrimmed_fastq
```

We can make the `ls` output more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories:

```bash
[grego@indri ~]$ ls -F
```

```output
untrimmed_fastq/
```

Anything with a "/" after it is a directory. Things with a "\*" after them are programs. If
there are no decorations, it's a file.

`ls` has lots of other options. To find out what they are, we can type:

```bash
[grego@indri ~]$ man ls
```

`man` (short for manual) displays detailed documentation (also referred as man page or man file)
for `bash` commands. It is a powerful resource to explore `bash` commands, understand
their usage and flags. Some manual files are very long. You can scroll through the
file using your keyboard's down arrow or use the <kbd>Space</kbd> key to go forward one page
and the <kbd>b</kbd> key to go backwards one page. When you are done reading, hit <kbd>q</kbd>
to quit.


>## Challenge
>
>Use the `-l` option for the `ls` command to display more information for each item
>in the directory. What is one piece of additional information this long format
>gives you that you don't see with the bare `ls` command?


### Solution
<details>
<summary> <b>Show Solution</b> </summary>

    ```bash
    [grego@indri ~]$ ls -l
    ```
    
    ```output
    total 8
    drwxr-xr-x 2 grego grego 4096 Nov 15  2017 untrimmed_fastq
    ```

    The additional information given includes the name of the owner of the file,
    when the file was last modified, and whether the current user has permission
    to read and write to the file.

</details>

# How to add a collapsible section in markdown
## 1. Example
<details>
  <summary>Click me</summary>

  ### Heading
  1. Foo
  2. Bar
     * Baz
     * Qux

  ### Some Javascript
  ```js
  function logSomething(something) {
    console.log('Something', something);
  }
  ```
</details>


***

No one can possibly learn all of these arguments, that's what the manual page
is for. You can (and should) refer to the manual page or other help files
as needed.

Let's go into the `untrimmed_fastq` directory and see what is in there.

```bash
[grego@indri ~]$ cd untrimmed_fastq
[grego@indri ~]$ ls -F
```

```output
bullkelp_001_R1.fastq.gz  bullkelp_001_R2.fastq.gz
```

This directory contains two files with `.fastq.gz` extensions. FASTQ is a format
for storing information about sequencing reads and their quality. GZ is an extension,
which means that it is compressed using gzip to reduce the amount of storage it takes up.
We will be learning more about FASTQ files in a later lesson.

### Shortcut: Tab Completion

Typing out file or directory names can waste a
lot of time and it's easy to make typing mistakes. Instead we can use tab complete
as a shortcut. When you start typing out the name of a directory or file, then
hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the
directory or file name.

Return to your home directory:

```bash
[grego@indri ~]$ cd
```

then enter:

```bash
[grego@indri ~]$ cd she<tab>
```

The shell will fill in the rest of the directory name for
`shell_data`.

Now change directories to `untrimmed_fastq` in `shell_data`

```bash
[grego@indri ~]$ cd shell_data
[grego@indri shell_data]$ cd untrimmed_fastq
```

Using tab complete can be very helpful. However, it will only autocomplete
a file or directory name if you've typed enough characters to provide
a unique identifier for the file or directory you are trying to access.

For example, if we now try to list the files which names start with `SR`
by using tab complete:

```bash
[grego@indri untrimmed_fastq]$ ls bul<tab>
```

The shell auto-completes your command to `bullkelp_001_R`, because all file names in
the directory begin with this prefix. When you hit
<kbd>Tab</kbd> again, the shell will list the possible choices.

```bash
[grego@indri untrimmed_fastq]$ ls bullkelp_001_R<tab><tab>
```

```output
bullkelp_001_R1.fastq.gz  bullkelp_001_R2.fastq.gz
```

Tab completion can also fill in the names of programs, which can be useful if you
remember the beginning of a program name.

```bash
[grego@indri untrimmed_fastq]$ pw<tab><tab>
```

```output
pwck      pwconv    pwd       pwdx      pwunconv
```

Displays the name of every program that starts with `pw`.

## Summary

We now know how to move around our file system using the command line.
This gives us an advantage over interacting with the file system through
a GUI as it allows us to work on a remote server, carry out the same set of operations
on a large number of files quickly, and opens up many opportunities for using
bioinformatic software that is only available in command line versions.

In the next few labs, we'll be expanding on these skills and seeing how
using the command line shell enables us to make our workflow more efficient and reproducible.

## keypoints

- The shell gives you the ability to work more efficiently by using keyboard commands rather than a GUI.
- Useful commands for navigating your file system include: `ls`, `pwd`, and `cd`.
- Most commands take options (flags) which begin with a `-`.
- Tab completion can reduce errors from mistyping and make work more efficient in the shell.

***

# Navigating files and directories


## Objectives

- Use a single command to navigate multiple steps in your directory structure, including moving backwards (one level up).
- Perform operations on files in directories outside your working directory.
- Work with hidden directories and hidden files.
- Interconvert between absolute and relative paths.
- Employ navigational shortcuts to move around your file system.

***

## Questions

- How can I perform operations on files outside of my working directory?
- What are some navigational shortcuts I can use to make my work more efficient?

***

## Moving around the file system

We've learned how to use `pwd` to find our current location within our file system.
We've also learned how to use `cd` to change locations and `ls` to list the contents
of a directory. Now we're going to learn some additional commands for moving around
within our file system.

Use the commands we've learned so far to navigate to the `shell_data/untrimmed_fastq` directory, if
you're not already there.

```bash
[grego@indri untrimmed_fastq]$ cd
[grego@indri ~]$ cd shell_data
[grego@indri shell_data]$ cd untrimmed_fastq
```

What if we want to move back up and out of this directory and to our top level
directory? Can we type `cd shell_data`? Try it and see what happens.

```bash
[grego@indri untrimmed_data]$ cd shell_data
```

```output
-bash: cd: shell_data: No such file or directory
```

Your computer looked for a directory or file called `shell_data` within the
directory you were already in. It didn't know you wanted to look at a directory level
above the one you were located in.

We have a special command to tell the computer to move us back or up one directory level.

```bash
[grego@indri untrimmed_data]$ cd ..
```

Now we can use `pwd` to make sure that we are in the directory we intended to navigate
to, and `ls` to check that the contents of the directory are correct.

```bash
[grego@indri shell_data]$ pwd
```

```output
/home/grego/shell_data
```

```bash
[grego@indri shell_data]$ ls
```

```output
untrimmed_fastq
```

From this output, we can see that `..` did indeed take us back one level in our file system.

You can chain these together like so:

```bash
[grego@indri shell_data]$ ls ../../
```

prints the contents of `/home`.

##  Challenge

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



In most commands the flags can be combined together in no particular order to obtain the desired results/output.

```
[grego@indri shell_data]$ ls -Fa
[grego@indri shell_data]$ ls -laF
```

## Examining the contents of other directories

By default, the `ls` commands lists the contents of the working
directory (i.e. the directory you are in). You can always find the
directory you are in using the `pwd` command. However, you can also
give `ls` the names of other directories to view. Navigate to your
home directory if you are not already there.

```bash
[grego@indri shell_data]$ cd
```

Then enter the command:

```bash
[grego@indri ~]$ ls shell_data
```

```output
untrimmed_fastq
```

This will list the contents of the `shell_data` directory without
you needing to navigate there.

The `cd` command works in a similar way.

Try entering:

```bash
[grego@indri ~]$ cd
[grego@indri ~]$ cd shell_data/untrimmed_fastq
```

This will take you to the `untrimmed_fastq` directory without having to go through
the intermediate directory.

##  Challenge

## Navigating practice

Navigate to your home directory. From there, list the contents of the `untrimmed_fastq`
directory.

:::::::::::::::  solution

## Solution

```bash
$ cd
$ ls shell_data/untrimmed_fastq/
```

```output
SRR097977.fastq  SRR098026.fastq 
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Full vs. Relative Paths

The `cd` command takes an argument which is a directory
name. Directories can be specified using either a *relative* path or a
full *absolute* path. The directories on the computer are arranged into a
hierarchy. The full path tells you where a directory is in that
hierarchy. Navigate to the home directory, then enter the `pwd`
command.

```bash
$ cd  
$ pwd  
```

You will see:

```output
/home/dcuser
```

This is the full name of your home directory. This tells you that you
are in a directory called `dcuser`, which sits inside a directory called
`home` which sits inside the very top directory in the hierarchy. The
very top of the hierarchy is a directory called `/` which is usually
referred to as the *root directory*. So, to summarize: `dcuser` is a
directory in `home` which is a directory in `/`. More on `root` and
`home` in the next section.

Now enter the following command:

```bash
$ cd /home/dcuser/shell_data/.hidden
```

This jumps forward multiple levels to the `.hidden` directory.
Now go back to the home directory.

```bash
$ cd
```

You can also navigate to the `.hidden` directory using:

```bash
$ cd shell_data/.hidden
```

These two commands have the same effect, they both take us to the `.hidden` directory.
The first uses the absolute path, giving the full address from the home directory. The
second uses a relative path, giving only the address from the working directory. A full
path always starts with a `/`. A relative path does not.

A relative path is like getting directions from someone on the street. They tell you to
"go right at the stop sign, and then turn left on Main Street". That works great if
you're standing there together, but not so well if you're trying to tell someone how to
get there from another country. A full path is like GPS coordinates. It tells you exactly
where something is no matter where you are right now.

You can usually use either a full path or a relative path depending on what is most convenient.
If we are in the home directory, it is more convenient to enter the full path.
If we are in the working directory, it is more convenient to enter the relative path
since it involves less typing.

Over time, it will become easier for you to keep a mental note of the
structure of the directories that you are using and how to quickly
navigate amongst them.

:::::::::::::::::::::::::::::::::::::::  challenge

## Relative path resolution

Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
what will `ls ../backup` display?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original pnas_final pnas_sub`

![](fig/filesystem-challenge.svg){alt='File System for Challenge Questions'}

:::::::::::::::  solution

## Solution

1. No: there *is* a directory `backup` in `/Users`.
2. No: this is the content of `Users/thing/backup`,
  but with `..` we asked for one level further up.
3. No: see previous explanation.
  Also, we did not specify `-F` to display `/` at the end of the directory names.
4. Yes: `../backup` refers to `/Users/backup`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Navigational Shortcuts

The root directory is the highest level directory in your file
system and contains files that are important for your computer
to perform its daily work. While you will be using the root (`/`)
at the beginning of your absolute paths, it is important that you
avoid working with data in these higher-level directories, as
your commands can permanently alter files that the operating
system needs to function. In many cases, trying to run commands
in `root` directories will require special permissions which are
not discussed here, so it's best to avoid them and work within your
home directory. Dealing with the `home` directory is very common.
The tilde character, `~`, is a shortcut for your home directory.
In our case, the `root` directory is **two** levels above our
`home` directory, so `cd` or `cd ~` will take you to
`/home/dcuser` and `cd /` will take you to `/`. Navigate to the
`shell_data` directory:

```bash
$ cd
$ cd shell_data
```

Then enter the command:

```bash
$ ls ~
```

```output
R  r_data  shell_data
```

This prints the contents of your home directory, without you needing to
type the full path.

The commands `cd`, and `cd ~` are very useful for quickly navigating back to your home directory. We will be using the `~` character in later lessons to specify our home directory.

:::::::::::::::::::::::::::::::::::::::: keypoints

- The `/`, `~`, and `..` characters represent important navigational shortcuts.
- Hidden files and directories start with `.` and can be viewed using `ls -a`.
- Relative paths specify a location starting from the current location, while absolute paths specify a location from the root of the file system.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Credit
This material is adapted from Becker et al. 2019, under CC-BY 4.0 licence. 

Erin Alison Becker, Anita Schürch, Tracy Teal, Sheldon John McKay, Jessica Elizabeth Mizzi, François Michonneau, et al. (2019, June). datacarpentry/shell-genomics: Data Carpentry: Introduction to the shell for genomics data, June 2019 (Version v2019.06.1). Zenodo. http://doi.org/10.5281/zenodo.3260560


