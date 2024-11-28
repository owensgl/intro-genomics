---
title: Unix commandline
element: lab
layout: default
---

# Section 1 
## Objectives

- Describe key reasons for learning shell.
- Navigate your file system using the command line.
- Access and read help files for `bash` programs and use help files to identify useful command options.
- Demonstrate the use of tab completion, and explain its advantages.

***

## Questions

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
(base) p165-072:~ gregoryowens$ ssh grego@indri.rcs.uvic.ca
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

## Downloading files. 
We first have to download the data files used in this lab. I have put them on a git repository 
(which you will learn about next lecture) and you download it using this code.

```bash
[grego@indri ~]$ git clone https://github.com/owensgl/shell_data.git
```

```output
Cloning into 'shell_data'...
remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 8 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (8/8), 3.71 MiB | 4.38 MiB/s, done.
```



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

For the rest of the lab activities, I will be showing the prompt as only $. 
Your prompt will be different, depending on which direct you are in, as well 
as other factors.

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
$ pwd
```

```output
/home/grego
```

Let's look at how our file system is organized. We can see what files and subdirectories are in this directory by running `ls`,
which stands for "listing":

```bash
$ ls
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
$ cd shell_data
```

Let's look at what is in this directory:

```bash
$ ls
```

```output
untrimmed_fastq
```

We can make the `ls` output more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories:

```bash
$ ls -F
```

```output
untrimmed_fastq/
```

Anything with a "/" after it is a directory. Things with a "\*" after them are programs. If
there are no decorations, it's a file.

`ls` has lots of other options. To find out what they are, we can type:

```bash
$ man ls
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


***

No one can possibly learn all of these arguments, that's what the manual page
is for. You can (and should) refer to the manual page or other help files
as needed.

Let's go into the `untrimmed_fastq` directory and see what is in there.

```bash
$ cd untrimmed_fastq
$ ls -F
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
$ cd
```

then enter:

```bash
$ cd she<tab>
```

The shell will fill in the rest of the directory name for
`shell_data`.

Now change directories to `untrimmed_fastq` in `shell_data`

```bash
$ cd shell_data
$ cd untrimmed_fastq
```

Using tab complete can be very helpful. However, it will only autocomplete
a file or directory name if you've typed enough characters to provide
a unique identifier for the file or directory you are trying to access.

For example, if we now try to list the files which names start with `SR`
by using tab complete:

```bash
$ ls bul<tab>
```

The shell auto-completes your command to `bullkelp_001_R`, because all file names in
the directory begin with this prefix. When you hit
<kbd>Tab</kbd> again, the shell will list the possible choices.

```bash
$ ls bullkelp_001_R<tab><tab>
```

```output
bullkelp_001_R1.fastq.gz  bullkelp_001_R2.fastq.gz
```

Tab completion can also fill in the names of programs, which can be useful if you
remember the beginning of a program name.

```bash
$ pw<tab><tab>
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
# Section 2

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
$ cd
$ cd shell_data
$ cd untrimmed_fastq
```

What if we want to move back up and out of this directory and to our top level
directory? Can we type `cd shell_data`? Try it and see what happens.

```bash
$ cd shell_data
```

```output
-bash: cd: shell_data: No such file or directory
```

Your computer looked for a directory or file called `shell_data` within the
directory you were already in. It didn't know you wanted to look at a directory level
above the one you were located in.

We have a special command to tell the computer to move us back or up one directory level.

```bash
$ cd ..
```

Now we can use `pwd` to make sure that we are in the directory we intended to navigate
to, and `ls` to check that the contents of the directory are correct.

```bash
$ pwd
```

```output
/home/grego/shell_data
```

```bash
$ ls
```

```output
untrimmed_fastq
```

From this output, we can see that `..` did indeed take us back one level in our file system.

You can chain these together like so:

```bash
$ ls ../../
```

prints the contents of `/home`.

##  Challenge

>### Finding hidden directories
>
>First navigate to the `shell_data` directory. There is a hidden directory within this directory. Explore the options for `ls` to
>find out how to see hidden directories. List the contents of the directory and
>identify the name of the text file in that directory.
>
>Hint: hidden files and folders in Unix start with `.`, for example `.my_hidden_directory`

***

In most commands the flags can be combined together in no particular order to obtain the desired results/output.

```
$ ls -Fa
$ ls -laF
```

## Examining the contents of other directories

By default, the `ls` commands lists the contents of the working
directory (i.e. the directory you are in). You can always find the
directory you are in using the `pwd` command. However, you can also
give `ls` the names of other directories to view. Navigate to your
home directory if you are not already there.

```bash
$ cd
```

Then enter the command:

```bash
$ ls shell_data
```

```output
untrimmed_fastq
```

This will list the contents of the `shell_data` directory without
you needing to navigate there.

The `cd` command works in a similar way.

Try entering:

```bash
$ cd
$ cd shell_data/untrimmed_fastq
```

This will take you to the `untrimmed_fastq` directory without having to go through
the intermediate directory.

##  Challenge

>## Navigating practice
>
>Navigate to your home directory. From there, list the contents of the `untrimmed_fastq`
>directory.



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
/home/grego
```

This is the full name of your home directory. This tells you that you
are in a directory called `grego`, which sits inside a directory called
`home` which sits inside the very top directory in the hierarchy. The
very top of the hierarchy is a directory called `/` which is usually
referred to as the *root directory*. So, to summarize: `grego` is a
directory in `home` which is a directory in `/`. More on `root` and
`home` in the next section.

Now enter the following command:

```bash
$ cd /home/grego/shell_data/.hidden
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

##  Challenge

## Relative path resolution

Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
what will `ls ../backup` display?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original pnas_final pnas_sub`

![](../figs/filesystem-challenge.svg)


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
shell_data
```

This prints the contents of your home directory, without you needing to
type the full path.

The commands `cd`, and `cd ~` are very useful for quickly navigating back to your home directory. We will be using the `~` character in later lessons to specify our home directory.

## Keypoints

- The `/`, `~`, and `..` characters represent important navigational shortcuts.
- Hidden files and directories start with `.` and can be viewed using `ls -a`.
- Relative paths specify a location starting from the current location, while absolute paths specify a location from the root of the file system.

***

# Section 3
## Objectives

- View, search within, copy, move, and rename files. Create new directories.
- Use wildcards (`*`) to perform operations on multiple files.
- Make a file read only.
- Use the `history` command to view and repeat recently used commands.

***

## Questions

- How can I view and search file contents?
- How can I create, copy and delete files and directories?
- How can I control who has permission to modify a file?
- How can I repeat recently used commands?

***

## Working with Files

### Our data set: FASTQ files

Now that we know how to navigate around our directory structure, let's
start working with our sequencing files. We did a sequencing experiment and
have two results files, which are stored in our `untrimmed_fastq` directory.

### Wildcards

Navigate to your `untrimmed_fastq` directory:

```bash
$ cd ~/shell_data/untrimmed_fastq
```

We are interested in looking at the FASTQ files in this directory. We can list
all files with the .fastq extension using the command:

```bash
$ ls *.fastq.gz
```

```output
bullkelp_001_R1.fastq.gz  bullkelp_001_R2.fastq.gz
```

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character.
Thus, `*.fastq.gz` matches every file that ends with `.fastq.gz`.

This command:

```bash
$ ls *R2.fastq.gz
```

```output
bullkelp_001_R2.fastq.gz
```

lists only the file that ends with `R2.fastq.gz`.

This command:

```bash
$ ls /usr/bin/*.sh
```

```output
/usr/bin/gettext.sh  /usr/bin/lesspipe.sh  /usr/bin/rescan-scsi-bus.sh  /usr/bin/setup-nsssysinit.sh
```

Lists every file in `/usr/bin` that ends in the characters `.sh`.
Note that the output displays **full** paths to files, since
each result starts with `/`.

##  Challenge


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


##  Challenge



`echo` is a built-in shell command that writes its arguments, like a line of text to standard output.
The `echo` command can also be used with pattern matching characters, such as wildcard characters.
Here we will use the `echo` command to see how the wildcard character is interpreted by the shell.

```bash
$ echo *.fastq.gz
```

```output
bullkelp_001_R1.fastq.gz  bullkelp_001_R2.fastq.gz
```

The `*` is expanded to include any file that ends with `.fastq.gz`. We can see that the output of
`echo *.fastq.gz` is the same as that of `ls *.fastq.gz`.

What would the output look like if the wildcard could *not* be matched? Compare the outputs of
`echo *.missing` and `ls *.missing`.


## Command History

If you want to repeat a command that you've run recently, you can access previous
commands using the up arrow on your keyboard to go back to the most recent
command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts:

- <kbd>Ctrl</kbd>\+<kbd>C</kbd> will cancel the command you are writing, and give you a
  fresh prompt.
- <kbd>Ctrl</kbd>\+<kbd>R</kbd> will do a reverse-search through your command history.  This
  is very useful.
- <kbd>Ctrl</kbd>\+<kbd>L</kbd> or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

```bash
$ history
```

to see a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

```output
259  ls *
260  ls /usr/bin/*.sh
261  ls *R1*fastq
```

then you could repeat command #260 by entering:

```bash
$ !260
```

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.
For more information on advanced usage of `history`, read section 9.3 of
[Bash manual](https://www.gnu.org/software/bash/manual/html_node/index.html).

## Challenge



Find the line number in your history for the command that listed all the .sh
files in `/usr/bin`. Rerun that command.



## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all the text using the program `cat`.

Enter the following command from within the `untrimmed_fastq` directory:

```bash
$ cat bullkelp_001_R1.fastq.gz
```

Notice anything weird? It's all unintelligeble characters. It may take a while, 
to print all the weird characters so cancel the `cat` command using <kbd>Ctrl</kbd>\+<kbd>C</kbd>. 
We can't read it because this file is gzipped, which means it is compressed and 
no longer a normal text file. To convert it into a format you can actually 
read, first we need to unzip it using `gunzip`.

```bash
$ gunzip *.fastq.gz
```

Try again to look at the file using `cat`, now that it is unzipped.

```bash
$ cat bullkelp_001_R1.fastq
```



## Challenge

1. Print out the contents of the `~/shell_data/untrimmed_fastq/bullkelp_001_R1.fastq` file. 
   What is the last line of the file? What command would only print the last lines of a file?
2. From your home directory, and without changing directories,
  use one short command to print the contents of all of the files in
  the `~/shell_data/untrimmed_fastq` directory.


***

`cat` is a terrific program, but when the file is really big, it can
be annoying to use. The program, `less`, is useful for this
case. `less` opens the file as read only, and lets you navigate through it. The navigation commands
are identical to the `man` program.

Enter the following command:

```bash
$ less bullkelp_001_R1.fastq
```

Some navigation commands in `less`:

| key   | action                                                                                                       | 
| ----- | ------------------------------------------------------------------------------------------------------------ |
| <kbd>Space</kbd> | to go forward                                                                                                | 
| <kbd>b</kbd>     | to go backward                                                                                               | 
| <kbd>g</kbd>     | to go to the beginning                                                                                       | 
| <kbd>G</kbd>     | to go to the end                                                                                             | 
| <kbd>q</kbd>     | to quit                                                                                                      | 

`less` also gives you a way of searching through files. Use the
"/" key to begin a search. Enter the word you would like
to search for and press `enter`. The screen will jump to the next location where
that word is found.

**Shortcut:** If you hit "/" then "enter", `less` will  repeat
the previous search. `less` searches from the current location and
works its way forward. Scroll up a couple lines on your terminal to verify
you are at the beginning of the file. Note, if you are at the end of the file and search
for the sequence "CAA", `less` will not find it. You either need to go to the
beginning of the file (by typing `g`) and search again using `/` or you
can use `?` to search backwards in the same way you used `/` previously.

For instance, let's search forward for the sequence `TTTTT` in our file.
You can see that we go right to that sequence, what it looks like,
and where it is in the file. If you continue to type `/` and hit return, you will move
forward to the next instance of this sequence motif. If you instead type `?` and hit
return, you will search backwards and move up the file to previous examples of this motif.


## Challenge

What are the next three nucleotides (characters) after the first instance of the sequence quoted above?




Remember, the `man` program actually uses `less` internally and
therefore uses the same commands, so you can search documentation
using "/" as well!

If you're using a small terminal window, you may notice that the lines are 
wrapping. This is fine now, but can be annoying when you're trying to view a
text file with many columns. You can remove text wrapping in less using the `-S` option.

```bash
$ less -S bullkelp_001_R1.fastq
```
In this, you can now use the left and right arrows to move left and write in the text file.

There's another way that we can look at files, and in this case, just
look at part of them. This can be particularly useful if we just want
to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at
the beginning and end of a file, respectively.

```bash
$ head bullkelp_001_R1.fastq
```

```output
@A01754:154:HWCGVDSX5:2:1101:4399:1094 1:N:0:ACTCCGAAGC+NCCGTGTTGC
CNCAGCCGAAACGGGCCAAAACTCGCCATAAACGAGGCTACGCCGAGTTTGCATCAAACCAGGAACTTTCTCAGATAGATATAGATGCGGACGACGATTCCGAAGTGCAGACCACGCTACACATTTTTCAAGTGAAAAACACCCCTAAAGA
+
F#FFFFFFF:FFFFFFFF:FFFFF:FFFFFFFFFFFFFFFFFF::FFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFF:FFFFFFFFFFFFFFF,FFFFFFF,FFF,
@A01754:154:HWCGVDSX5:2:1101:5412:1094 1:N:0:ACTCCGAAGC+NCCGTGTTGC
GNTGGAAACTGCTGTTGGCAGTACTAAAATTGTTGTCAAATAGTTCGAGCCGAACGCTGGACGGACGAATCACACCGACGGGCGAAGGAACGAACGAAGAGCGTTACCGACGGACGAATGGCTTAACGAAGGACGGTGTGCGAGACAAACC
+
F#FFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF,FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF,FFFFFFFFFFFFFFFFFFFFF
@A01754:154:HWCGVDSX5:2:1101:9191:1094 1:N:0:ACTCCGAAGC+NCCGTGTTGC
GNGTAGTGTCCTGCAATGAGCCCCACGACGTCAACGCACGTCAAACACTCTCATCTCGTTTGTATTCCACGATAAATGCGTCCAGGTAGGGAGGGACCTAGCCTTCAAGGCATTCCTGAAAAAATGCCTGAAGTACTTTTTCTGAAACATG
```

```bash
$ tail bullkelp_001_R1.fastq
```

```output
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFF
@A01754:154:HWCGVDSX5:2:1101:26368:33066 1:N:0:ACTCCGAAGC+TCCGTGTTGC
TGAATGGGATGAGCAAAAAAAAAAAGTGTGGTAATGACGAAAGTGATAGATCTACTGTGGCACACAGCAAGCAGTGGTCTGCCGCGGGGTTTCTTGACATGTACCAGGTATTGCGCTTGTACGCACGACGGTTACAAAGTAATCAACACTA
+
FFFFFFFFFFFFFFFFFFFFFFF,FFF,FF:FFFFFFFFFFF:FFFFF:FF,FFFFFF:F:FFFFFFFFFFFFFFFF,F:FFFFFFFFFFFFF::FFFF,FFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFF
@A01754:154:HWCGVDSX5:2:1101:27019:33066 1:N:0:ACTCCGAAGC+TCCGTGTTGC
CTTGAATTGTGTTTGCCTTGCCTTGTCGTCTCTTGTCTTGAATATTGTGTGTTGAATTATCAGAAACGTCATCTCGTTTGTTTTAGTGATTGCTGTAATGTGAGTTTGTATATGGAATATCTAATCAAGTTTTGGAACTTCATGTCATCGT
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFF
```

The `-n` option to either of these commands can be used to print the
first or last `n` lines of a file.

```bash
$ head -n 1 bullkelp_001_R1.fastq
```

```output
@A01754:154:HWCGVDSX5:2:1101:4399:1094 1:N:0:ACTCCGAAGC+NCCGTGTTGC
```

```bash
$ tail -n 1 bullkelp_001_R1.fastq
```

```output
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFF
```

## Details on the FASTQ format

Although it looks complicated (and it is), it's easy to understand the
[fastq](https://en.wikipedia.org/wiki/FASTQ_format) format with a little decoding. Some rules about the format
include...

| Line  | Description                                                                                                  | 
| ----- | ------------------------------------------------------------------------------------------------------------ |
| 1     | Always begins with '@' and then information about the read                                                   | 
| 2     | The actual DNA sequence                                                                                      | 
| 3     | Always begins with a '+' and sometimes the same info in line 1                                               | 
| 4     | Has a string of characters which represent the quality scores; must have same number of characters as line 2 | 

We can view the first complete read in one of the files in our dataset by using `head` to look at
the first four lines.

```bash
$ head -n 4 bullkelp_001_R1.fastq
```

```output
@A01754:154:HWCGVDSX5:2:1101:4399:1094 1:N:0:ACTCCGAAGC+NCCGTGTTGC
CNCAGCCGAAACGGGCCAAAACTCGCCATAAACGAGGCTACGCCGAGTTTGCATCAAACCAGGAACTTTCTCAGATAGATATAGATGCGGACGACGATTCCGAAGTGCAGACCACGCTACACATTTTTCAAGTGAAAAACACCCCTAAAGA
+
F#FFFFFFF:FFFFFFFF:FFFFF:FFFFFFFFFFFFFFFFFF::FFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFF:FFFFFFFFFFFFFFF,FFFFFFF,FFF,
```

We can see that the start of the read has an N, which means it's an unknown base, but most of
it looks reasonable. 
Line 4 shows the quality for each nucleotide in the read. Quality is interpreted as the
probability of an incorrect base call (e.g. 1 in 10) or, equivalently, the base call
accuracy (e.g. 90%). To make it possible to line up each individual nucleotide with its quality
score, the numerical score is converted into a code where each individual character
represents the numerical quality score for an individual nucleotide. For example, in the line
above, the quality score line is:

```output
F#FFFFFFF:FFFFFFFF:FFFFF:FFFFFFFFFFFFFFFFFF::FFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFF:FFFFFFFFFFFFFFF,FFFFFFF,FFF,
```

The `#` character and each of the `F` and `;` characters represent the encoded quality for an
individual nucleotide. The numerical value assigned to each of these characters depends on the
sequencing platform that generated the reads. The sequencing machine used to generate our data
uses the standard Sanger quality PHRED score encoding, Illumina version 1.8 onwards.
Each character is assigned a quality score between 0 and 42 as shown in the chart below.

```output
Quality encoding: !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJK
                  |         |         |         |         |
Quality score:    0........10........20........30........40..                          
```

Each quality score represents the probability that the corresponding nucleotide call is
incorrect. This quality score is logarithmically based, so a quality score of 10 reflects a
base call accuracy of 90%, but a quality score of 20 reflects a base call accuracy of 99%.
These probability values are the results from the base calling algorithm and dependent on how
much signal was captured for the base incorporation.

Looking back at our read:

```output
@A01754:154:HWCGVDSX5:2:1101:4399:1094 1:N:0:ACTCCGAAGC+NCCGTGTTGC
CNCAGCCGAAACGGGCCAAAACTCGCCATAAACGAGGCTACGCCGAGTTTGCATCAAACCAGGAACTTTCTCAGATAGATATAGATGCGGACGACGATTCCGAAGTGCAGACCACGCTACACATTTTTCAAGTGAAAAACACCCCTAAAGA
+
F#FFFFFFF:FFFFFFFF:FFFFF:FFFFFFFFFFFFFFFFFF::FFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFF:FFFFFFFFFFFFFFF,FFFFFFF,FFF,
```

We can see that the N base has a quality score of 2, which means a 63% chance of error.
The program that decoded the signal decided that 63% chance of error was too high, 
so it replaced the base with N. For the rest of the read, the quality is `F`, which 
corresponds to a Quality score of 37 or a 0.02% chance of error. 



## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move
them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all
of those files. In cases like this, it's much faster to do these operations at the command line.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted.
For this lesson, our raw data is our FASTQ files.  We don't want to accidentally change the original files, so we'll make a copy of them
and change the file permissions so that we can read from, but not write to, the files.

First, let's make a copy of one of our FASTQ files using the `cp` command.

Navigate to the `shell_data/untrimmed_fastq` directory and enter:

```bash
$ cp bullkelp_001_R1.fastq bullkelp_001_R1-copy.fastq
$ ls -F
```

```output
bullkelp_001_R1.fastq  bullkelp_001_R1-copy.fastq  bullkelp_001_R2.fastq
```

We now have two copies of the `bullkelp_001_R1.fastq` file, one of them named `bullkelp_001_R1-copy.fastq`. We'll move this file to a new directory
called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create:

```bash
$ mkdir backup
```

### Moving / Renaming

We can now move our backup file to this directory. We can
move files around using the command `mv`:

```bash
$ mv bullkelp_001_R1-copy.fastq backup
$ ls backup
```

```output
bullkelp_001_R1-copy.fastq
```

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup:

```bash
$ cd backup
$ mv bullkelp_001_R1-copy.fastq bullkelp_001_R1-backup.fastq
$ ls
```

```output
bullkelp_001_R1-backup.fastq
```

### File Permissions

We've now made a backup copy of our file, but just because we have two copies, it doesn't make us safe. We can still accidentally delete or
overwrite both copies. To make sure we can't accidentally mess up this backup file, we're going to change the permissions on the file so
that we're only allowed to read (i.e. view) the file, not write to it (i.e. make new changes).

View the current permissions on a file using the `-l` (long) flag for the `ls` command:

```bash
$ ls -l
```

```output
-rwxr-x--- 1 grego grego 43332 Nov 15 23:02 bullkelp_001_R1-backup.fastq
```

The first part of the output for the `-l` flag gives you information about the file's current permissions. There are ten slots in the
permissions list. The first character in this list is related to file type, not permissions, so we'll ignore it for now. The next three
characters relate to the permissions that the file owner has, the next three relate to the permissions for group members, and the final
three characters specify what other users outside of your group can do with the file. We're going to concentrate on the three positions
that deal with your permissions (as the file owner).

![](../figs/rwx_figure.svg)

Here the three positions that relate to the file owner are `rwx`. The `r` means that you have permission to read the file, the `w`
indicates that you have permission to write to (i.e. make changes to) the file, and the third position is a `x`, indicating that you
have permission to carry out the ability encoded by that space.

Our goal for now is to change permissions on this file so that you no longer have `w` or write permissions. We can do this using the `chmod` (change mode) command and subtracting (`-`) the write permission `-w`.

```bash
$ chmod -w bullkelp_001_R1-backup.fastq
$ ls -l 
```

```output
-r-xr-x---. 1 grego grego 43332 Nov 15 23:02 bullkelp_001_R1-backup.fastq
```

### Removing

To prove to ourselves that you no longer have the ability to modify this file, try deleting it with the `rm` command:

```bash
$ rm bullkelp_001_R1-backup.fastq
```

You'll be asked if you want to override your file permissions:

```output
rm: remove write-protected regular file ‘bullkelp_001_R1-backup.fastq'? 
```

You should enter `n` for no. If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra
measure of security, as there is one more step between us and deleting our data files.

**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm` will not delete directories. You can tell `rm` to
delete a directory using the `-r` (recursive) option. Let's delete the backup directory
we just made.

Enter the following command:

```bash
$ cd ..
$ rm -r backup
```

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory,
you will be asked whether you want to override your permission settings. Be extra careful when using `rm` with wildcards,
or with variables (covered later). A mistaken `rm` command can delete things you don't want to be deleted.


## Challenge

Starting in the `shell_data/untrimmed_fastq/` directory, do the following:

1. Make sure that you have deleted your backup directory and all files it contains.
2. Create a backup of each of your FASTQ files using `cp`. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't
  learned yet how to do this
  with a wildcard.)
3. Use a wildcard to move all of your backup files to a new backup directory.
4. Change the permissions on all of your backup files to be write-protected.

***

## Keypoints

- You can view file contents using `less`, `cat`, `head` or `tail`.
- The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories.
- You can view file permissions using `ls -l` and change permissions using `chmod`.
- The `history` command and the up arrow on your keyboard can be used to repeat recently used commands.

***

## Credit
This material is adapted from Becker et al. 2019, under CC-BY 4.0 licence. 

Erin Alison Becker, Anita Schürch, Tracy Teal, Sheldon John McKay, Jessica Elizabeth Mizzi, François Michonneau, et al. (2019, June). datacarpentry/shell-genomics: Data Carpentry: Introduction to the shell for genomics data, June 2019 (Version v2019.06.1). Zenodo. http://doi.org/10.5281/zenodo.3260560


