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

In future lessons, we will use an Owens' lab server allowing you to access large computing resources. Today, we are going to use a small virtual machine hosted by UVic. Open a web browser and go to [UVic Syzygy Jupyter](https://uvic.syzygy.ca/jupyter/). Login using your UVic credentials. 

When you login, select "Terminal". You should see a blinking cursor and a screen similar to this:

![](../figs/terminal_1.png)



>### Tip
>
>Hot-key combinations are shortcuts for performing common commands.
>The hot-key combination for clearing the console is `Ctrl+L`. Feel free to try it and see for yourself.

***

## Downloading files. 
We first have to download the data files used in this lab. We are using data files from the software carpentry tutorial on unix shell (also a good resource if you want more practice!). We will be using the command 'wget', which requires a url. 


```bash
jupyter@5ee02e7b87eb$ wget https://swcarpentry.github.io/shell-novice/data/shell-lesson-data.zip
```

```output
--2024-12-17 23:12:39--  https://swcarpentry.github.io/shell-novice/data/shell-lesson-data.zip
Resolving swcarpentry.github.io (swcarpentry.github.io)... 185.199.110.153, 185.199.108.153, 185.199.109.153, ...
Connecting to swcarpentry.github.io (swcarpentry.github.io)|185.199.110.153|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 460289 (450K) [application/zip]
Saving to: ‘shell-lesson-data.zip’

shell-lesson-data.z 100%[==================>] 449.50K  --.-KB/s    in 0.01s   

2024-12-17 23:12:39 (36.2 MB/s) - ‘shell-lesson-data.zip’ saved [460289/460289]
```

This text is showing us that the .zip file has downloaded successfully. For larger files you'll be able to track how the download is going based on the status bar. 


## Navigating your file system

The part of the operating system that manages files and directories
is called the **file system**.
It organizes our data into files,
which hold information,
and directories (also called "folders"),
which hold files or other directories.

Several commands are frequently used to create, inspect, rename, and delete files and directories.



```bash
jupyter@5ee02e7b87eb$
```

The dollar sign is a **prompt**, which shows us that the shell is waiting for input;
your shell may use a different character as a prompt and may add information before
the prompt. When typing commands, either from these lessons or from other sources,
do not type the prompt, only the commands that follow it. In this case, the prompt is
showing that my username is **jupyter**, and I'm logged into **5ee02e7b87eb**. 
This unintelligble string of characters is the ID for a virtual machine that the server spun up.

For the rest of the lab activities, I will be showing the prompt as only $. 
Your prompt will be different, depending on which direct you are in, as well 
as other factors.

It's important to remember that anytime you are using the terminal, you are in a directory,
which is in other directories, which are in even more directories until we get the one directory that has everything. We often think of this in terms of a tree structure. When you are entering into
a directory you are moving upward towards the tips and when you are exiting directories you are moving downward towards the root. 

To  explicitely find out where you are lets run a command called `pwd`
(which stands for "print working directory").
At any moment, our **current working directory**
is the directory where any tasks will be run,
i.e.,
the directory that the computer assumes we want to run commands in,
unless we explicitly specify something else. For example, I could ran a command to print all the files, it would print all of the files **in the directory I am in**. 

Lets try to run `pwd` to see where we are:

```bash
pwd
```

```output
/home/jupyter
```
We are in the directory jupyter, which is in another directory called home. 

Before we start movign between directories, another critically important command is `ls`.  
This command **lists** all the files and directories in your current directory. 
Many times in this course you will try to run a command on a file and if the file is not in your directory, it will not work. Your first reflex when moving between directories or when a command doesn't
work is to use `ls` to see what files are there.

```bash
ls
```

```output
R  shell-lesson-data.zip
```

`ls` prints the names of the files and directories in the current directory in
alphabetical order, arranged neatly into columns.

Lets take a step back and talk more about how commands work in unix.
A basic unix command looks like this:
```
command [flags] [arguments]
```

Flags change how a command runs. They are preceded by a `-` when a flag is a single character or `--` when a flag is multiple characters.

```bash
ls -l     # Long format, shows detailed file information
ls -a     # Show all files, including hidden ones
ls -h     # Human-readable file sizes
ls --all     # Same as -a
ls --human   # Same as -h
```

You can also combine multiple single character flags in once
```bash
ls -lah   # Combines long, all, and human-readable flags
```

The argument is used to pass arbitrary information to the command. For example, in ls, the argument tells it which directory to list the output for. 

```bash
ls /home       # List contents of /home directory
ls /home/jovyan   # List contents of /home/joyvan folder
```
We can also combine flags and arguments:
```bash
ls -l /home    # Long format of Downloads folder
```

We will be working on data that you downloaded, but currently it is zipped into a single file. We first have to unzip it so we can explore the files.

```bash
unzip shell-lesson-data.zip
```

We've created a new directory called shell-lesson-data. We can double check that it's there with `ls`.

```bash
ls -l
```
You should see that it is there. If not, then your unzip command didn't work.

The command to change locations in our file system is `cd`, followed by a
directory name to change our working directory.
`cd` stands for "change directory".

To navigate into the shell-lesson-data directory we use the following command:

```bash
cd shell-lesson-data
```

Let's look at what is in this directory:

```bash
ls -F
```

```output
exercise-data/  north-pacific-gyre/
```


With the -F flag and ls, anything with a "/" after it is a directory. Things with a "\*" after them are programs. If
there are no decorations, it's a file.


>## Challenge
>
>Using the `ls` command, find out when the directory "exercise-data", was created.
>Hint, effective internet searching is a critical skill and will help you throughout your career.


***

It's hard to remember all the commands, but they will start to stick in your memory as you practice. In the mean time, refer back to cheat sheets like this: [cheat sheet](https://www.alexji.com/UNIXCheatSheet.pdf).

Let's go into the `untrimmed_fastq` directory and see what is in there.

```bash
cd exercise-data
ls -F
```

```output
alkanes/  animal-counts/  creatures/  numbers.txt  writing/
```


### Shortcut: Tab Completion

Typing out file or directory names can waste a
lot of time and it's easy to make typing mistakes. Instead we can use tab complete
as a shortcut. When you start typing out the name of a directory or file, then
hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the
directory or file name.

Return to your home directory:

```bash
cd ~ #The ~ is a special character which represents your home directory.
```

then enter:

```bash
cd she<tab>
```

The shell will fill in the rest of the directory name for
`shell-lesson-data`.

Now change directories to `alkanes`, which is in the `exercise-data` directory.

```bash
cd exercise-data
cd alkanes
```

Using tab complete can be very helpful. However, it will only autocomplete
a file or directory name if you've typed enough characters to provide
a unique identifier for the file or directory you are trying to access.

For example, if we now try to list the files which names start with `P`
by using tab complete:

```bash
ls p<tab>
```

When you hit
<kbd>Tab</kbd>, the shell will list the possible choices.

```output
pentane.pdb  propane.pdb
```

It doesn't know which file we actually want, so it doesn't fill it in for you.  Hot tip! If tab completion doesn't work it often is because you've done something wrong. For example, if you are trying to open a file and you can't tab complete the name of the file, it could be because it doesn't exist or you've spelled it wrong.

Tab completion can also fill in the names of programs, which can be useful if you
remember the beginning of a program name.

```bash
pw<tab><tab>
```

```output
pwck      pwconv    pwd       pwdx      pwunconv
```

This is showing the name of every program that starts with `pw`.

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

Use the commands we've learned so far to navigate to the `~/shell-lesson-data/exercise-data` directory, if
you're not already there.

```bash
cd ~
cd shell-lesson-data
cd exercise-data
```

What if we want to move back down to the shell-lesson-data directory? Can we type `cd shell-lesson-data`? Try it and see what happens.

```bash
cd shell-lesson-data
```

```output
-bash: cd: shell-lesson-data: No such file or directory
```

Your computer looked for a directory or file called `shell-lesson-data` within the
directory you were already in. It didn't know you wanted to look at a directory level
below the one you were located in.

We have a special command to tell the computer to move us back or down one directory level.

```bash
cd ..
```

Now we can use `pwd` to make sure that we are in the directory we intended to navigate
to, and `ls` to check that the contents of the directory are correct.

```bash
pwd
```

```output
/home/jupyter/shell-lesson-data
```

```bash
ls
```

```output
exercise-data  north-pacific-gyre
```

From this output, we can see that `..` did indeed take us back one level in our file system.

You can chain these together to target directories back up two layers. Here we show that using the
`ls` command, which lists the contents of the directory (but doesn't move your current working directory):

```bash
ls ../../
```

This is printing the contents of `/home`.


## Examining the contents of other directories

By default, the `ls` commands lists the contents of the working
directory (i.e. the directory you are in). You can always find the
directory you are in using the `pwd` command. However, you can also
give `ls` the names of other directories to view. Navigate to your
home directory if you are not already there.

```bash
cd ~
```

Then enter the command:

```bash
ls shell-lesson-data
```

```output
exercise-data  north-pacific-gyre
```

This will list the contents of the `shell_data` directory without
you needing to navigate there.

The `cd` command works in a similar way.

Try entering:

```bash
cd ~
cd shell-lesson-data/exercise-data
```

This will take you to the `exercise-data` directory without having to go through
the intermediate directory.

You can combine `..` with directories to move down and then up other directory chains. For example:

```bash
cd ../north-pacific-gyre/
```
For this, I'm starting in exercise-data, the '..' tells it to go down a level to shell-lesson-data then 'north-pacific-gyre' tells it to go up a level into that directory. 

##  Challenge

>### Finding hidden directories
>
>1. Find the animal-counts directory and enter it using `cd`.
>2. Without leaving the animal-counts directory, list the files in the north-pacific-gyre directory.
>3. You can view the first 10 lines for a file using the `head` command. While still in the animal-counts directory, print the first ten lines for unicorn.dat.


## Full vs. Relative Paths

The `cd` command takes an argument which is a directory
name. Directories can be specified using either a *relative* path or a
full *absolute* path.  So far we have been using relative paths, which
tell you where to go, relative to your starting directory. 
The full path tells you where to go, starting from the root of a computer.
This means that an absolute path will always get you to the same place, regardless
of where you're starting from.

Navigate to the home directory, then enter the `pwd`
command.

```bash
cd ~
pwd  
```

You will see:

```output
/home/jupyter
```

This is the full name of your home directory. This tells you that you
are in a directory called `jupyter`, which sits inside a directory called
`home` which sits inside the very top directory in the hierarchy. The
very top of the hierarchy is a directory called `/` which is usually
referred to as the *root directory*. So, to summarize: `jupyter` is a
directory in `home` which is a directory in `/`. More on `root` and
`home` in the next section.

Now enter the following command:

```bash
cd /home/jupyter/shell-lesson-data/exercise-data/
```

This jumps up multiple levels to the `exercise-data` directory.
Now go back to the home directory.

```bash
cd ~
```

You can also navigate to the `exercise-data` directory using:

```bash
cd shell-lesson-data/exercise-data/
```

These two commands have the same effect, they both take us to the `exercise-data` directory.
The first uses the absolute path, giving the full address from the home directory. The
second uses a relative path, giving only the address from the working directory. An absolute or full
path always starts with a `/`. A relative path does not.

A relative path is like getting directions from someone on the street. They tell you to
"go right at the stop sign, and then turn left on Main Street". That works great if
you're standing there together, but not so well if you're starting from a different street. 
A full path is like GPS coordinates. It tells you exactly
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
`/home/jupyter` and `cd /` will take you to `/`. 

Lets take a look at whats in the root:

```bash
cd /
ls 
```

```output
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

These are all the core directories for running the server. In this case, this is a virtual machine just for you,
so if you mess up something here, it won't affect anyone else and we could just remake it. For your laptop, if you 
mess up these files you may cause yourself a huge problem. Best practise is to not touch anything outside of your /home 
directory unless you really know what you're doing.


## Keypoints

- The `/`, `~`, and `..` characters represent important navigational shortcuts.
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


Now that we know how to navigate around our directory structure, let's
start working with files. First go into the creatures directory.

```bash
cd ~/shell-lesson-data/exercise-data/creatures
ls
```

These files have the .dat suffix. Suffixes tell you something about the format
of the data stored in them. Files can be human readable, which is to say that
they are made up of characters, or they can be only machine readable. A large
proportion of data you'll work with are human readable and it's always good practice
to look at the data in a raw format. Sometimes you can catch problems 

### Wildcards

Often we want to refer to multiple different files, for example if you wanted to do a task on all of them. 
One way we can use that is the asterisk or `*` character.

```bash
ls *.dat
```

```output
basilisk.dat  minotaur.dat  unicorn.dat
```

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character, or no character.
Thus, `*.dat` matches every file that ends with `.dat`.


You can have multiple `*` to be more broad or more specific. Try this command:

```bash
ls *o*
```
Here we are asking for any file that has anything, then an o, than anything else. Basically, it finds anything with an o in the name. 

##  Challenge


Do each of the following tasks from your current directory using a single
`ls` command for each:

1. List all of the files in `/usr/sbin` that start with the letter 'c'.
2. List all of the files in `/usr/sbin` that contain the letter 'a'.
3. List all of the files in `/usr/sbin` that end with the letter 'o'.

Bonus: List all of the files in `/usr/sbin` that contain the letter 'a' or the
letter 'c'.

Hint: The bonus question requires a Unix wildcard that we haven't talked about
yet. Try searching the internet for information about Unix wildcards to find
what you need to solve the bonus problem.



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
history
```

This shows a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

```output
259  ls *
260  ls /usr/sbin/*.sh
261  ls *dat
```

then you could repeat command #260 by entering:

```bash
!260
```

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.
For more information on advanced usage of `history`, read section 9.3 of
[Bash manual](https://www.gnu.org/software/bash/manual/html_node/index.html).

## Challenge



Find the line number in your history for the command that listed all the .sh
files in `/usr/sbin` starting with c. Rerun that command.



## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all the text using the program `cat`.

Enter the following command from within the `creatures` directory:

```bash
cat unicorn.dat
```


## Challenge

1. Print out the contents of the `minotaur.dat` file. 
   What is the last line of the file? What command would only print the last lines of a file?
2. From your home directory, and without changing directories,
  use one short command to print the contents of all of the files in
  the `creatures` directory.


***

`cat` is a terrific program, but when the file is really big, it can
be annoying to use. The program, `less`, is useful for this
case. `less` opens the file as read only, and lets you navigate through it. 
Enter the following command:

```bash
less unicorn.dat
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


For instance, let's search forward for the sequence `TTTTT` in our file.
You can see that we go right to that sequence, what it looks like,
and where it is in the file. If you continue to type `/` and hit return, you will move
forward to the next instance of this sequence motif. If you instead type `?` and hit
return, you will search backwards and move up the file to previous examples of this motif.


## Challenge

Find every instance of 'ACT' in unicorn.dat using less. What is a problem with using less for this search?



## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move
them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all
of those files. In cases like this, it's much faster to do these operations at the command line.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted.
For example, when you do sequencing your raw data is represented by a .fastq file  We don't want to accidentally change the original files, so we'll make a copy of them
and change the file permissions so that we can read from, but not write to, the files.

First, let's make a copy of one of our .dat files using the `cp` command.

Navigate to the `creatures` directory and enter:

```bash
cp unicorn.dat unicorn_copy.dat
ls -o
```

```output
total 10
-rw-r--r--. 1 jupyter 1838 Mar 12  2023 basilisk.dat
-rw-r--r--. 1 jupyter 1830 Mar 12  2023 minotaur.dat
-rw-r--r--. 1 jupyter 1833 Mar 12  2023 unicorn.dat
-rw-r--r--. 1 jupyter 1833 Dec 19 22:17 unicorn_copy.dat
```

We now have two copies of the `unicorn.dat` file, one of them named `unicorn_copy.dat`. We'll move this file to a new directory
called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create:

```bash
mkdir backup
```

### Moving / Renaming

We can now move our backup file to this directory. We can
move files around using the command `mv`:

```bash
mv unicorn_copy.dat backup
ls backup
```

```output
unicorn_copy.dat
```

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup:

```bash
cd backup
mv unicorn_copy.dat unicorn_copy_newname.dat
ls
```

```output
unicorn_copy_newname.dat
```

**Important**: It's important to note that if you move a file into a name of a file that already exists, you will overwrite the old file. There is no warning and you cannot reverse this. Unix has no recycle bin that you can rescue things from. Be careful
when moving or deleting files. 


### Removing

We can delete files with the `rm` command:

```bash
rm unicorn_copy_newname.dat
```


**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm` will not delete directories. The command to remove directories is `rmdir`, and it will only
remove empty directories. 


Lets test that out:

```bash
cd ..
cp unicorn.dat backup/
rmdir backup
```

```output
rmdir: failed to remove 'backup': Directory not empty
```

To actually remove that directory, we need to get rid of the files in it:
```bash
rm backup/unicorn.dat
rmdir backup
```

Earlier in the lesson we learned about using wildcards to target multiple files at once.
 Be extra careful when using `rm` with wildcards,
or with variables (covered later). A mistaken `rm` command can delete things you don't want to be deleted.


## Challenge

Starting in the `creatures` directory, do the following:

1. Make sure that you have deleted your backup directory and all files it contains.
2. Create a backup of each of your .dat files using `cp`. 
3. Use a wildcard to move all of your backup files to a new backup directory.

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


