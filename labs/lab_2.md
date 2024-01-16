---
title: Shell scripting
element: lab
layout: default
---

# Section 1

## Objectives

- Create a directory hierarchy that matches a given diagram.
- Create files in that hierarchy using an editor or by copying and renaming existing files.
- Delete, copy and move specified files and/or directories.


## Questions

- How can I create, copy, and delete files and directories?
- How can I edit files?

## Logging into the remote server

## How to access the remote server

All students will be accessing the server using their UVic Netlink ID. You will use the `ssh` or `Secure Shell`
command to login to the remote server, while logged into the UVic VPN. The VPN is called 
"Cisco AnyConnect Secure Mobile Client". Open it and fill in "vpn.uvic.ca" into the connection box. 
It will then tell you to select a group, please select "3 - Student". This will then prompt you
to fill in your netlink ID and then request a Duo Push notification. 

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



## Creating directories

We now know how to explore files and directories,
but how do we create them in the first place?

In this lab we will learn about creating and moving files and directories,
using the `exercise-data/writing` directory as an example. 

We can get this data by downloading it from a git repository:

```bash
[grego@indri ~]$ git clone https://github.com/owensgl/shell_data.git
```

### Step one: see where we are and what we already have

First we should return to the home directory:

```bash
$ cd ~
```

Then enter the `lab_2_data` directory:

```bash
$ cd lab_2_data
```

Next we'll move to the `exercise-data/writing` directory and see what it contains:

```bash
$ cd exercise-data/writing/
$ ls -F
```

```output
haiku.txt  LittleWomen.txt
```

### Create a directory

Let's create a new directory called `thesis` using the command `mkdir thesis`
(which has no output):

```bash
$ mkdir thesis
```

Since `thesis` is a relative path
(i.e., does not have a leading slash, like `/what/ever/thesis`),
the new directory is created in the current working directory:

```bash
$ ls -F
```

```output
haiku.txt  LittleWomen.txt  thesis/
```

Since we've just created the `thesis` directory, there's nothing in it yet:

```bash
$ ls -F thesis
```

Note that `mkdir` is not limited to creating single directories one at a time.
The `-p` option allows `mkdir` to create a directory with nested subdirectories
in a single operation:

```bash
$ mkdir -p ../project/data ../project/results
```

The `-R` option to the `ls` command will list all nested subdirectories within a directory.
Let's use `ls -FR` to recursively list the new directory hierarchy we just created in the
`project` directory:

```bash
$ ls -FR ../project
```

```output
../project/:
data/  results/

../project/data:

../project/results:
```


## Good names for files and directories

Complicated names of files and directories can make your life painful
when working on the command line. Here we provide a few useful
tips for the names of your files and directories.

1. Don't use spaces.

Spaces can make a name more meaningful,
but since spaces are used to separate arguments on the command line
it is better to avoid them in names of files and directories.
You can use `-` or `_` instead (e.g. `north-pacific-gyre/` rather than `north pacific gyre/`).
To test this out, try typing `mkdir north pacific gyre` and see what directory (or directories!)
are made when you check with `ls -F`.

2. Don't begin the name with `-` (dash).

Commands treat names starting with `-` as options.

3. Stick with letters, numbers, `.` (period or 'full stop'), `-` (dash) and `_` (underscore).

Many other characters have special meanings on the command line.
We will learn about some of these during this lesson.
There are special characters that can cause your command to not work as
expected and can even result in data loss.

If you need to refer to names of files or directories that have spaces
or other special characters, you should surround the name in quotes (`""`).


### Create a text file

Let's change our working directory to `thesis` using `cd`,
then run a text editor called Nano to create a file called `draft.txt`:

```bash
$ cd thesis
$ nano draft.txt
```

## Which Editor?

When we say, '`nano` is a text editor' we really do mean 'text'. It can
only work with plain character data, not tables, images, or any other
human-friendly media. We use it in examples because it is one of the
least complex text editors. However, because of this trait, it may
not be powerful enough or flexible enough for the work you need to do
after this workshop. On Unix systems (such as Linux and macOS),
many programmers use [Emacs](https://www.gnu.org/software/emacs/) or
[Vim](https://www.vim.org/) (both of which require more time to learn),
or a graphical editor such as [Gedit](https://projects.gnome.org/gedit/)
or [VScode](https://code.visualstudio.com/). On Windows, you may wish to
use [Notepad++](https://notepad-plus-plus.org/).  Windows also has a built-in
editor called `notepad` that can be run from the command line in the same
way as `nano` for the purposes of this lesson.

No matter what editor you use, you will need to know where it searches
for and saves files. If you start it from the shell, it will (probably)
use your current working directory as its default location. If you use
your computer's start menu, it may want to save files in your Desktop or
Documents directory instead. You can change this by navigating to
another directory the first time you 'Save As...'

***

Let's type in a few lines of text.

![](../figs/nano-screenshot.png){alt="screenshot of nano text editor in action with the text It's not publish or perish any more, it's share and thrive"}

Once we're happy with our text, we can press <kbd>Ctrl</kbd>\+<kbd>O</kbd>
(press the <kbd>Ctrl</kbd> or <kbd>Control</kbd> key and, while
holding it down, press the <kbd>O</kbd> key) to write our data to disk. We will be asked
to provide a name for the file that will contain our text. Press <kbd>Return</kbd> to accept
the suggested default of `draft.txt`.

Once our file is saved, we can use <kbd>Ctrl</kbd>\+<kbd>X</kbd> to quit the editor and
return to the shell.


## Control, Ctrl, or ^ Key

The Control key is also called the 'Ctrl' key. There are various ways
in which using the Control key may be described. For example, you may
see an instruction to press the <kbd>Control</kbd> key and, while holding it down,
press the <kbd>X</kbd> key, described as any of:

- `Control-X`
- `Control+X`
- `Ctrl-X`
- `Ctrl+X`
- `^X`
- `C-x`

In nano, along the bottom of the screen you'll see `^G Get Help ^O WriteOut`.
This means that you can use `Control-G` to get help and `Control-O` to save your
file.

***

`nano` doesn't leave any output on the screen after it exits,
but `ls` now shows that we have created a file called `draft.txt`:

```bash
$ ls
```

```output
draft.txt
```

## Challenge: Creating Files a Different Way

We have seen how to create text files using the `nano` editor.
Now, try the following command:

```bash
$ touch my_file.txt
```

1. What did the `touch` command do?
  When you look at your current directory using the GUI file explorer,
  does the file show up?

2. Use `ls -l` to inspect the files.  How large is `my_file.txt`?

3. When might you want to create a file this way?

***

To avoid confusion later on,
we suggest removing the file you've just created before proceeding with the rest
of the episode, otherwise future outputs may vary from those given in the lesson.
To do this, use the following command:

```bash
$ rm my_file.txt
```

***

## What's In A Name?

You may have noticed that all of the files are named 'something dot
something', and in this part of the lesson, we always used the extension
`.txt`.  This is just a convention; we can call a file `mythesis` or
almost anything else we want. However, most people use two-part names
most of the time to help them (and their programs) tell different kinds
of files apart. The second part of such a name is called the
**filename extension** and indicates
what type of data the file holds: `.txt` signals a plain text file, `.pdf`
indicates a PDF document, `.cfg` is a configuration file full of parameters
for some program or other, `.png` is a PNG image, and so on.

This is just a convention, albeit an important one. Files merely contain
bytes; it's up to us and our programs to interpret those bytes
according to the rules for plain text files, PDF documents, configuration
files, images, and so on.

Naming a PNG image of a whale as `whale.mp3` doesn't somehow
magically turn it into a recording of whale song, though it *might*
cause the operating system to associate the file with a music player
program. In this case, if someone double-clicked `whale.mp3` in a file
explorer program,the music player will automatically (and erroneously)
attempt to open the `whale.mp3` file.

***

## Moving files and directories

Returning to the `shell-lesson-data/exercise-data/writing` directory,

```bash
$ cd ~/Desktop/shell-lesson-data/exercise-data/writing
```

In our `thesis` directory we have a file `draft.txt`
which isn't a particularly informative name,
so let's change the file's name using `mv`,
which is short for 'move':

```bash
$ mv thesis/draft.txt thesis/quotes.txt
```


One must be careful when specifying the target file name, since `mv` will
silently overwrite any existing file with the same name, which could
lead to data loss. By default, `mv` will not ask for confirmation before overwriting files.
However, an additional option, `mv -i` (or `mv --interactive`), will cause `mv` to request
such confirmation.

Note that `mv` also works on directories.

Let's move `quotes.txt` into the current working directory.
We use `mv` once again,
but this time we'll use just the name of a directory as the second argument
to tell `mv` that we want to keep the filename
but put the file somewhere new.
(This is why the command is called 'move'.)
In this case,
the directory name we use is the special directory name `.` that we mentioned earlier.

```bash
$ mv thesis/quotes.txt .
```

The effect is to move the file from the directory it was in to the current working directory.
`ls` now shows us that `thesis` is empty:

```bash
$ ls thesis
```

```output
$
```

Alternatively, we can confirm the file `quotes.txt` is no longer present in the `thesis` directory
by explicitly trying to list it:

```bash
$ ls thesis/quotes.txt
```

```error
ls: cannot access 'thesis/quotes.txt': No such file or directory
```

`ls` with a filename or directory as an argument only lists the requested file or directory.
If the file given as the argument doesn't exist, the shell returns an error as we saw above.
We can use this to see that `quotes.txt` is now present in our current directory:

```bash
$ ls quotes.txt
```

```output
quotes.txt
```


## Challenge: Moving Files to a new folder

After running the following commands,
Jamie realizes that she put the files `sucrose.dat` and `maltose.dat` into the wrong folder.
The files should have been placed in the `raw` folder.

```bash
$ ls -F
 analyzed/ raw/
$ ls -F analyzed
fructose.dat glucose.dat maltose.dat sucrose.dat
$ cd analyzed
```

Fill in the blanks to move these files to the `raw/` folder
(i.e. the one she forgot to put them in)

```bash
$ mv sucrose.dat maltose.dat ____/____
```

***

## Copying files and directories

The `cp` command works very much like `mv`,
except it copies a file instead of moving it.
We can check that it did the right thing using `ls`
with two paths as arguments --- like most Unix commands,
`ls` can be given multiple paths at once:

```bash
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
```

```output
quotes.txt   thesis/quotations.txt
```

We can also copy a directory and all its contents by using the
[recursive](https://en.wikipedia.org/wiki/Recursion) option `-r`,
e.g. to back up a directory:

```bash
$ cp -r thesis thesis_backup
```

We can check the result by listing the contents of both the `thesis` and `thesis_backup` directory:

```bash
$ ls thesis thesis_backup
```

```output
thesis:
quotations.txt

thesis_backup:
quotations.txt
```

It is important to include the `-r` flag. If you want to copy a directory and you omit this option
you will see a message that the directory has been omitted because `-r not specified`.

``` bash
$ cp thesis thesis_backup
cp: -r not specified; omitting directory 'thesis'
```


## Challenge: Renaming Files

Suppose that you created a plain-text file in your current directory to contain a list of the
statistical tests you will need to do to analyze your data, and named it `statstics.txt`

After creating and saving this file you realize you misspelled the filename! You want to
correct the mistake, which of the following commands could you use to do so?

1. `cp statstics.txt statistics.txt`
2. `mv statstics.txt statistics.txt`
3. `mv statstics.txt .`
4. `cp statstics.txt .`

***

## Challenge: Moving and Copying

What is the output of the closing `ls` command in the sequence shown below?

```bash
$ pwd
```

```output
/Users/jamie/data
```

```bash
$ ls
```

```output
proteins.dat
```

```bash
$ mkdir recombined
$ mv proteins.dat recombined/
$ cp recombined/proteins.dat ../proteins-saved.dat
$ ls
```

1. `proteins-saved.dat recombined`
2. `recombined`
3. `proteins.dat recombined`
4. `proteins-saved.dat`

***

## Removing files and directories

Returning to the `shell-lesson-data/exercise-data/writing` directory,
let's tidy up this directory by removing the `quotes.txt` file we created.
The Unix command we'll use for this is `rm` (short for 'remove'):

```bash
$ rm quotes.txt
```

We can confirm the file has gone using `ls`:

```bash
$ ls quotes.txt
```

```error
ls: cannot access 'quotes.txt': No such file or directory
```

## Deleting Is Forever

The Unix shell doesn't have a trash bin that we can recover deleted
files from (though most graphical interfaces to Unix do).  Instead,
when we delete files, they are unlinked from the file system so that
their storage space on disk can be recycled. Tools for finding and
recovering deleted files do exist, but there's no guarantee they'll
work in any particular situation, since the computer may recycle the
file's disk space right away.

***

## Using `rm` Safely

What happens when we execute `rm -i thesis_backup/quotations.txt`?
Why would we want this protection when using `rm`?

***

If we try to remove the `thesis` directory using `rm thesis`,
we get an error message:

```bash
$ rm thesis
```

```error
rm: cannot remove 'thesis': Is a directory
```

This happens because `rm` by default only works on files, not directories.

`rm` can remove a directory *and all its contents* if we use the
recursive option `-r`, and it will do so *without any confirmation prompts*:

```bash
$ rm -r thesis
```

Given that there is no way to retrieve files deleted using the shell,
`rm -r` *should be used with great caution*
(you might consider adding the interactive option `rm -r -i`).

## Operations with multiple files and directories

Oftentimes one needs to copy or move several files at once.
This can be done by providing a list of individual filenames,
or specifying a naming pattern using wildcards. Wildcards are
special characters that can be used to represent unknown characters
or sets of characters when navigating the Unix file system.


## Challenge: Copy with Multiple Filenames

For this exercise, you can test the commands in the `shell-lesson-data/exercise-data` directory.

In the example below, what does `cp` do when given several filenames and a directory name?

```bash
$ mkdir backup
$ cp creatures/minotaur.dat creatures/unicorn.dat backup/
```

In the example below, what does `cp` do when given three or more file names?

```bash
$ cd creatures
$ ls -F
```

```output
basilisk.dat  minotaur.dat  unicorn.dat
```

```bash
$ cp minotaur.dat unicorn.dat basilisk.dat
```



## Wildcards

`*` is a **wildcard**, which represents zero or more other characters.
Let's consider the `shell-lesson-data/exercise-data/alkanes` directory:
`*.pdb` represents `ethane.pdb`, `propane.pdb`, and every
file that ends with '.pdb'. On the other hand, `p*.pdb` only represents
`pentane.pdb` and `propane.pdb`, because the 'p' at the front can only
represent filenames that begin with the letter 'p'.

`?` is also a wildcard, but it represents exactly one character.
So `?ethane.pdb` could represent `methane.pdb` whereas
`*ethane.pdb` represents both `ethane.pdb` and `methane.pdb`.

Wildcards can be used in combination with each other. For example,
`???ane.pdb` indicates three characters followed by `ane.pdb`,
giving `cubane.pdb  ethane.pdb  octane.pdb`.

When the shell sees a wildcard, it expands the wildcard to create a
list of matching filenames *before* running the preceding command.
As an exception, if a wildcard expression does not match
any file, Bash will pass the expression as an argument to the command
as it is. For example, typing `ls *.pdf` in the `alkanes` directory
(which contains only files with names ending with `.pdb`) results in
an error message that there is no file called `*.pdf`.
However, generally commands like `wc` and `ls` see the lists of
file names matching these expressions, but not the wildcards
themselves. It is the shell, not the other programs, that expands
the wildcards.


## Challenge: List filenames matching a pattern

When run in the `alkanes` directory, which `ls` command(s) will
produce this output?

`ethane.pdb   methane.pdb`

1. `ls *t*ane.pdb`
2. `ls *t?ne.*`
3. `ls *t??ne.pdb`
4. `ls ethane.*`

***

## Challenge: More on Wildcards

Sam has a directory containing calibration data, datasets, and descriptions of
the datasets:

```bash
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   └── datasets
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    └── all_november_files
```

Before heading off to another field trip, she wants to back up her data and
send some datasets to her colleague Bob. Sam uses the following commands
to get the job done:

```bash
$ cp *dataset* backup/datasets
$ cp ____calibration____ backup/calibration
$ cp 2015-____-____ send_to_bob/all_november_files/
$ cp ____ send_to_bob/all_datasets_created_on_a_23rd/
```

Help Sam by filling in the blanks.

The resulting directory structure should look like this

```bash
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   │   ├── 2015-10-23-calibration.txt
│   │   ├── 2015-10-26-calibration.txt
│   │   └── 2015-11-23-calibration.txt
│   └── datasets
│       ├── 2015-10-23-dataset1.txt
│       ├── 2015-10-23-dataset2.txt
│       ├── 2015-10-23-dataset_overview.txt
│       ├── 2015-10-26-dataset1.txt
│       ├── 2015-10-26-dataset2.txt
│       ├── 2015-10-26-dataset_overview.txt
│       ├── 2015-11-23-dataset1.txt
│       ├── 2015-11-23-dataset2.txt
│       └── 2015-11-23-dataset_overview.txt
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    │   ├── 2015-10-23-dataset1.txt
    │   ├── 2015-10-23-dataset2.txt
    │   ├── 2015-10-23-dataset_overview.txt
    │   ├── 2015-11-23-dataset1.txt
    │   ├── 2015-11-23-dataset2.txt
    │   └── 2015-11-23-dataset_overview.txt
    └── all_november_files
        ├── 2015-11-23-calibration.txt
        ├── 2015-11-23-dataset1.txt
        ├── 2015-11-23-dataset2.txt
        └── 2015-11-23-dataset_overview.txt
```
***

## Challenge: Organizing Directories and Files

Jamie is working on a project, and she sees that her files aren't very well
organized:

```bash
$ ls -F
```

```output
analyzed/  fructose.dat    raw/   sucrose.dat
```

The `fructose.dat` and `sucrose.dat` files contain output from her data
analysis. What command(s) covered in this lesson does she need to run
so that the commands below will produce the output shown?

```bash
$ ls -F
```

```output
analyzed/   raw/
```

```bash
$ ls analyzed
```

```output
fructose.dat    sucrose.dat
```

***

## Challenge: Reproduce a folder structure

You're starting a new experiment and would like to duplicate the directory
structure from your previous experiment so you can add new data.

Assume that the previous experiment is in a folder called `2016-05-18`,
which contains a `data` folder that in turn contains folders named `raw` and
`processed` that contain data files.  The goal is to copy the folder structure
of the `2016-05-18` folder into a folder called `2016-05-20`
so that your final directory structure looks like this:

```output
2016-05-20/
└── data
   ├── processed
   └── raw
```

Which of the following set of commands would achieve this objective?
What would the other commands do?

```bash
$ mkdir 2016-05-20
$ mkdir 2016-05-20/data
$ mkdir 2016-05-20/data/processed
$ mkdir 2016-05-20/data/raw
```

```bash
$ mkdir 2016-05-20
$ cd 2016-05-20
$ mkdir data
$ cd data
$ mkdir raw processed
```

```bash
$ mkdir 2016-05-20/data/raw
$ mkdir 2016-05-20/data/processed
```

```bash
$ mkdir -p 2016-05-20/data/raw
$ mkdir -p 2016-05-20/data/processed
```

```bash
$ mkdir 2016-05-20
$ cd 2016-05-20
$ mkdir data
$ mkdir raw processed
```

## Solution

The first two sets of commands achieve this objective.
The first set uses relative paths to create the top-level directory before
the subdirectories.

The third set of commands will give an error because the default behavior of `mkdir`
won't create a subdirectory of a non-existent directory:
the intermediate level folders must be created first.

The fourth set of commands achieve this objective. Remember, the `-p` option,
followed by a path of one or more
directories, will cause `mkdir` to create any intermediate subdirectories as required.

The final set of commands generates the 'raw' and 'processed' directories at the same level
as the 'data' directory.

***

## keypoints

- `cp [old] [new]` copies a file.
- `mkdir [path]` creates a new directory.
- `mv [old] [new]` moves (renames) a file or directory.
- `rm [path]` removes (deletes) a file.
- `*` matches zero or more characters in a filename, so `*.txt` matches all files ending in `.txt`.
- `?` matches any single character in a filename, so `?.txt` matches `a.txt` but not `any.txt`.
- Use of the Control key may be described in many ways, including `Ctrl-X`, `Control-X`, and `^X`.
- The shell does not have a trash bin: once something is deleted, it's really gone.
- Most files' names are `something.extension`. The extension isn't required, and doesn't guarantee anything, but is normally used to indicate the type of data in the file.
- Depending on the type of work you do, you may need a more powerful text editor than Nano.

***
# Rest Break

***

# Section 2

## objectives

- Redirect a command's output to a file.
- Construct command pipelines with two or more stages.
- Explain what usually happens if a program or pipeline isn't given any input to process.
- Explain the advantage of linking commands with pipes and filters.

## questions

- How can I combine existing commands to do new things?

***

Now that we know a few basic commands,
we can finally look at the shell's most powerful feature:
the ease with which it lets us combine existing programs in new ways.
We'll start with the directory `shell-lesson-data/exercise-data/alkanes`
that contains six files describing some simple organic molecules.
The `.pdb` extension indicates that these files are in Protein Data Bank format,
a simple text format that specifies the type and position of each atom in the molecule.

```bash
$ ls
```

```output
cubane.pdb    methane.pdb    pentane.pdb
ethane.pdb    octane.pdb     propane.pdb
```

Let's run an example command:

```bash
$ wc cubane.pdb
```

```output
20  156 1158 cubane.pdb
```

`wc` is the 'word count' command:
it counts the number of lines, words, and characters in files (returning the values
in that order from left to right).

If we run the command `wc *.pdb`, the `*` in `*.pdb` matches zero or more characters,
so the shell turns `*.pdb` into a list of all `.pdb` files in the current directory:

```bash
$ wc *.pdb
```

```output
  20  156  1158  cubane.pdb
  12  84   622   ethane.pdb
   9  57   422   methane.pdb
  30  246  1828  octane.pdb
  21  165  1226  pentane.pdb
  15  111  825   propane.pdb
 107  819  6081  total
```

Note that `wc *.pdb` also shows the total number of all lines in the last line of the output.

If we run `wc -l` instead of just `wc`,
the output shows only the number of lines per file:

```bash
$ wc -l *.pdb
```

```output
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
```

The `-m` and `-w` options can also be used with the `wc` command to show
only the number of characters or the number of words, respectively.


## Why Isn't It Doing Anything?

What happens if a command is supposed to process a file, but we
don't give it a filename? For example, what if we type:

```bash
$ wc -l
```

but don't type `*.pdb` (or anything else) after the command?
Since it doesn't have any filenames, `wc` assumes it is supposed to
process input given at the command prompt, so it just sits there and waits
for us to give it some data interactively. From the outside, though, all we
see is it sitting there, and the command doesn't appear to do anything.

If you make this kind of mistake, you can escape out of this state by
holding down the control key (<kbd>Ctrl</kbd>) and pressing the letter
<kbd>C</kbd> once: <kbd>Ctrl</kbd>\+<kbd>C</kbd>. Then release both keys.


***

## Capturing output from commands

Which of these files contains the fewest lines?
It's an easy question to answer when there are only six files,
but what if there were 6000?
Our first step toward a solution is to run the command:

```bash
$ wc -l *.pdb > lengths.txt
```

The greater than symbol, `>`, tells the shell to **redirect** the command's output to a
file instead of printing it to the screen. This command prints no screen output, because
everything that `wc` would have printed has gone into the file `lengths.txt` instead.
If the file doesn't exist prior to issuing the command, the shell will create the file.
If the file exists already, it will be silently overwritten, which may lead to data loss.
Thus, **redirect** commands require caution.

`ls lengths.txt` confirms that the file exists:

```bash
$ ls lengths.txt
```

```output
lengths.txt
```

We can now send the content of `lengths.txt` to the screen using `cat lengths.txt`.
The `cat` command gets its name from 'concatenate' i.e. join together,
and it prints the contents of files one after another.
There's only one file in this case,
so `cat` just shows us what it contains:

```bash
$ cat lengths.txt
```

```output
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
```


## Output Page by Page

We'll continue to use `cat` in this lesson, for convenience and consistency,
but it has the disadvantage that it always dumps the whole file onto your screen.
More useful in practice is the command `less` (e.g. `less lengths.txt`).
This displays a screenful of the file, and then stops.
You can go forward one screenful by pressing the spacebar,
or back one by pressing `b`.  Press `q` to quit.


***

## Filtering output

Next we'll use the `sort` command to sort the contents of the `lengths.txt` file.
But first we'll do an exercise to learn a little about the sort command:


## Challenge: What Does `sort -n` Do?

The file `shell-lesson-data/exercise-data/numbers.txt` contains the following lines:

```source
10
2
19
22
6
```

If we run `sort` on this file, the output is:

```output
10
19
2
22
6
```

If we run `sort -n` on the same file, we get this instead:

```output
2
6
10
19
22
```

Explain why `-n` has this effect.

***

We will also use the `-n` option to specify that the sort is
numerical instead of alphanumerical.
This does *not* change the file;
instead, it sends the sorted result to the screen:

```bash
$ sort -n lengths.txt
```

```output
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
```

We can put the sorted list of lines in another temporary file called `sorted-lengths.txt`
by putting `> sorted-lengths.txt` after the command,
just as we used `> lengths.txt` to put the output of `wc` into `lengths.txt`.
Once we've done that,
we can run another command called `head` to get the first few lines in `sorted-lengths.txt`:

```bash
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
```

```output
  9  methane.pdb
```

Using `-n 1` with `head` tells it that
we only want the first line of the file;
`-n 20` would get the first 20,
and so on.
Since `sorted-lengths.txt` contains the lengths of our files ordered from least to greatest,
the output of `head` must be the file with the fewest lines.


## Redirecting to the same file

It's a very bad idea to try redirecting
the output of a command that operates on a file
to the same file. For example:

```bash
$ sort -n lengths.txt > lengths.txt
```

Doing something like this may give you
incorrect results and/or delete
the contents of `lengths.txt`.



## Challenge: What Does `>>` Mean?

We have seen the use of `>`, but there is a similar operator `>>`
which works slightly differently.
We'll learn about the differences between these two operators by printing some strings.
We can use the `echo` command to print strings e.g.

```bash
$ echo The echo command prints text
```

```output
The echo command prints text
```

Now test the commands below to reveal the difference between the two operators:

```bash
$ echo hello > testfile01.txt
```

and:

```bash
$ echo hello >> testfile02.txt
```

Hint: Try executing each command twice in a row and then examining the output files.



## Challenge: Appending Data


Consider the file `shell-lesson-data/exercise-data/animal-counts/animals.csv`.
After these commands, select the answer that
corresponds to the file `animals-subset.csv`:

```bash
$ head -n 3 animals.csv > animals-subset.csv
$ tail -n 2 animals.csv >> animals-subset.csv
```

1. The first three lines of `animals.csv`
2. The last two lines of `animals.csv`
3. The first three lines and the last two lines of `animals.csv`
4. The second and third lines of `animals.csv`


***

## Passing output to another command

In our example of finding the file with the fewest lines,
we are using two intermediate files `lengths.txt` and `sorted-lengths.txt` to store output.
This is a confusing way to work because
even once you understand what `wc`, `sort`, and `head` do,
those intermediate files make it hard to follow what's going on.
We can make it easier to understand by running `sort` and `head` together:

```bash
$ sort -n lengths.txt | head -n 1
```

```output
  9  methane.pdb
```

The vertical bar, `|`, between the two commands is called a **pipe**.
It tells the shell that we want to use
the output of the command on the left
as the input to the command on the right.

This has removed the need for the `sorted-lengths.txt` file.

## Combining multiple commands

Nothing prevents us from chaining pipes consecutively.
We can for example send the output of `wc` directly to `sort`,
and then send the resulting output to `head`.
This removes the need for any intermediate files.

We'll start by using a pipe to send the output of `wc` to `sort`:

```bash
$ wc -l *.pdb | sort -n
```

```output
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
```

We can then send that output through another pipe, to `head`, so that the full pipeline becomes:

```bash
$ wc -l *.pdb | sort -n | head -n 1
```

```output
   9  methane.pdb
```

This is exactly like a mathematician nesting functions like *log(3x)*
and saying 'the log of three times *x*'.
In our case,
the algorithm is 'head of sort of line count of `*.pdb`'.

The redirection and pipes used in the last few commands are illustrated below:

![](../figs/redirects-and-pipes.svg){alt='Redirects and Pipes of different commands: "wc -l \*.pdb" will direct theoutput to the shell. "wc -l \*.pdb > lengths" will direct output to the file"lengths". "wc -l \*.pdb | sort -n | head -n 1" will build a pipeline where theoutput of the "wc" command is the input to the "sort" command, the output ofthe "sort" command is the input to the "head" command and the output of the"head" command is directed to the shell'}


## Challenge: Piping Commands Together

In our current directory, we want to find the 3 files which have the least number of
lines. Which command listed below would work?

1. `wc -l * > sort -n > head -n 3`
2. `wc -l * | sort -n | head -n 1-3`
3. `wc -l * | head -n 3 | sort -n`
4. `wc -l * | sort -n | head -n 3`

***

## Tools designed to work together

This idea of linking programs together is why Unix has been so successful.
Instead of creating enormous programs that try to do many different things,
Unix programmers focus on creating lots of simple tools that each do one job well,
and that work well with each other.
This programming model is called 'pipes and filters'.
We've already seen pipes;
a **filter** is a program like `wc` or `sort`
that transforms a stream of input into a stream of output.
Almost all of the standard Unix tools can work this way.
Unless told to do otherwise,
they read from standard input,
do something with what they've read,
and write to standard output.

The key is that any program that reads lines of text from standard input
and writes lines of text to standard output
can be combined with every other program that behaves this way as well.
You can *and should* write your programs this way
so that you and other people can put those programs into pipes to multiply their power.


## Challenge: Pipe Reading Comprehension

A file called `animals.csv` (in the `shell-lesson-data/exercise-data/animal-counts` folder)
contains the following data:

```source
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
2012-11-06,deer,2
2012-11-06,fox,4
2012-11-07,rabbit,16
2012-11-07,bear,1
```

What text passes through each of the pipes and the final redirect in the pipeline below?
Note, the `sort -r` command sorts in reverse order.

```bash
$ cat animals.csv | head -n 5 | tail -n 3 | sort -r > final.txt
```

Hint: build the pipeline up one command at a time to test your understanding

***


## Challenge: Pipe Construction

For the file `animals.csv` from the previous exercise, consider the following command:

```bash
$ cut -d , -f 2 animals.csv
```

The `cut` command is used to remove or 'cut out' certain sections of each line in the file,
and `cut` expects the lines to be separated into columns by a <kbd>Tab</kbd> character.
A character used in this way is a called a **delimiter**.
In the example above we use the `-d` option to specify the comma as our delimiter character.
We have also used the `-f` option to specify that we want to extract the second field (column).
This gives the following output:

```output
deer
rabbit
raccoon
rabbit
deer
fox
rabbit
bear
```

The `uniq` command filters out adjacent matching lines in a file.
How could you extend this pipeline (using `uniq` and another command) to find
out what animals the file contains (without any duplicates in their
names)?


## Challenge: Which Pipe?

The file `animals.csv` contains 8 lines of data formatted as follows:

```output
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
...
```

The `uniq` command has a `-c` option which gives a count of the
number of times a line occurs in its input.  Assuming your current
directory is `shell-lesson-data/exercise-data/animal-counts`,
what command would you use to produce a table that shows
the total count of each type of animal in the file?

1. `sort animals.csv | uniq -c`
2. `sort -t, -k2,2 animals.csv | uniq -c`
3. `cut -d, -f 2 animals.csv | uniq -c`
4. `cut -d, -f 2 animals.csv | sort | uniq -c`
5. `cut -d, -f 2 animals.csv | sort | uniq -c | wc -l`

***

## Nelle's Pipeline: Checking Files

Nelle has run her samples through the assay machines
and created 17 files in the `north-pacific-gyre` directory described earlier.
As a quick check, starting from the `shell-lesson-data` directory, Nelle types:

```bash
$ cd north-pacific-gyre
$ wc -l *.txt
```

The output is 18 lines that look like this:

```output
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
```

Now she types this:

```bash
$ wc -l *.txt | sort -n | head -n 5
```

```output
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
```

Whoops: one of the files is 60 lines shorter than the others.
When she goes back and checks it,
she sees that she did that assay at 8:00 on a Monday morning --- someone
was probably in using the machine on the weekend,
and she forgot to reset it.
Before re-running that sample,
she checks to see if any files have too much data:

```bash
$ wc -l *.txt | sort -n | tail -n 5
```

```output
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
```

Those numbers look good --- but what's that 'Z' doing there in the third-to-last line?
All of her samples should be marked 'A' or 'B';
by convention,
her lab uses 'Z' to indicate samples with missing information.
To find others like it, she does this:

```bash
$ ls *Z.txt
```

```output
NENE01971Z.txt    NENE02040Z.txt
```

Sure enough,
when she checks the log on her laptop,
there's no depth recorded for either of those samples.
Since it's too late to get the information any other way,
she must exclude those two files from her analysis.
She could delete them using `rm`,
but there are actually some analyses she might do later where depth doesn't matter,
so instead, she'll have to be careful later on to select files using the wildcard expressions
`NENE*A.txt NENE*B.txt`.

## Challenge: Removing Unneeded Files

Suppose you want to delete your processed data files, and only keep
your raw files and processing script to save storage.
The raw files end in `.dat` and the processed files end in `.txt`.
Which of the following would remove all the processed data files,
and *only* the processed data files?

1. `rm ?.txt`
2. `rm *.txt`
3. `rm * .txt`
4. `rm *.*`


  

***



## keypoints

- `wc` counts lines, words, and characters in its inputs.
- `cat` displays the contents of its inputs.
- `sort` sorts its inputs.
- `head` displays the first 10 lines of its input.
- `tail` displays the last 10 lines of its input.
- `command > [file]` redirects a command's output to a file (overwriting any existing content).
- `command >> [file]` appends a command's output to a file.
- `[first] | [second]` is a pipeline: the output of the first command is used as the input to the second.
- The best way to use the shell is to use pipes to combine simple single-purpose programs (filters).

***
# Rest Break
***

# Section 3
## objectives

- Write a loop that applies one or more commands separately to each file in a set of files.
- Trace the values taken on by a loop variable during execution of the loop.
- Explain the difference between a variable's name and its value.
- Explain why spaces and some punctuation characters shouldn't be used in file names.
- Demonstrate how to see what commands have recently been executed.
- Re-run recently executed commands without retyping them.

## questions

- How can I perform the same actions on many different files?

***

**Loops** are a programming construct which allow us to repeat a command or set of commands
for each item in a list.
As such they are key to productivity improvements through automation.
Similar to wildcards and tab completion, using loops also reduces the
amount of typing required (and hence reduces the number of typing mistakes).

Suppose we have several hundred genome data files named `basilisk.dat`, `minotaur.dat`, and
`unicorn.dat`.
For this example, we'll use the `exercise-data/creatures` directory which only has three
example files,
but the principles can be applied to many many more files at once.

The structure of these files is the same: the common name, classification, and updated date are
presented on the first three lines, with DNA sequences on the following lines.
Let's look at the files:

```bash
$ head -n 5 basilisk.dat minotaur.dat unicorn.dat
```

We would like to print out the classification for each species, which is given on the second
line of each file.
For each file, we would need to execute the command `head -n 2` and pipe this to `tail -n 1`.
We'll use a loop to solve this problem, but first let's look at the general form of a loop,
using the pseudo-code below:

```bash
# The word "for" indicates the start of a "For-loop" command
for thing in list_of_things 
# The word "do" indicates the start of job execution list
do 
    # Indentation within the loop is not required, but aids legibility
    operation_using/command $thing 
# The word "done" indicates the end of a loop
done  
```

and we can apply this to our example like this:

```bash
$ for filename in basilisk.dat minotaur.dat unicorn.dat
> do
>     echo $filename
>     head -n 2 $filename | tail -n 1
> done
```

```output
basilisk.dat
CLASSIFICATION: basiliscus vulgaris
minotaur.dat
CLASSIFICATION: bos hominus
unicorn.dat
CLASSIFICATION: equus monoceros
```

## Follow the Prompt

The shell prompt changes from `$` to `>` and back again as we were
typing in our loop. The second prompt, `>`, is different to remind
us that we haven't finished typing a complete command yet. A semicolon, `;`,
can be used to separate two commands written on a single line.


***

When the shell sees the keyword `for`,
it knows to repeat a command (or group of commands) once for each item in a list.
Each time the loop runs (called an iteration), an item in the list is assigned in sequence to
the **variable**, and the commands inside the loop are executed, before moving on to
the next item in the list.
Inside the loop,
we call for the variable's value by putting `$` in front of it.
The `$` tells the shell interpreter to treat
the variable as a variable name and substitute its value in its place,
rather than treat it as text or an external command.

In this example, the list is three filenames: `basilisk.dat`, `minotaur.dat`, and `unicorn.dat`.
Each time the loop iterates, we first use `echo` to print the value that the variable
`$filename` currently holds. This is not necessary for the result, but beneficial for us here to
have an easier time to follow along.
Next, we will assign a file name to the variable `filename`
and run the `head` command.
The first time through the loop,
`$filename` is `basilisk.dat`.
The interpreter runs the command `head` on `basilisk.dat`
and pipes the first two lines to the `tail` command,
which then prints the second line of `basilisk.dat`.
For the second iteration, `$filename` becomes
`minotaur.dat`. This time, the shell runs `head` on `minotaur.dat`
and pipes the first two lines to the `tail` command,
which then prints the second line of `minotaur.dat`.
For the third iteration, `$filename` becomes
`unicorn.dat`, so the shell runs the `head` command on that file,
and `tail` on the output of that.
Since the list was only three items, the shell exits the `for` loop.

## Same Symbols, Different Meanings

Here we see `>` being used as a shell prompt, whereas `>` is also
used to redirect output.
Similarly, `$` is used as a shell prompt, but, as we saw earlier,
it is also used to ask the shell to get the value of a variable.

If the *shell* prints `>` or `$` then it expects you to type something,
and the symbol is a prompt.

If *you* type `>` or `$` yourself, it is an instruction from you that
the shell should redirect output or get the value of a variable.


***

When using variables it is also
possible to put the names into curly braces to clearly delimit the variable
name: `$filename` is equivalent to `${filename}`, but is different from
`${file}name`. You may find this notation in other people's programs.

We have called the variable in this loop `filename`
in order to make its purpose clearer to human readers.
The shell itself doesn't care what the variable is called;
if we wrote this loop as:

```bash
$ for x in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $x | tail -n 1
> done
```

or:

```bash
$ for temperature in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $temperature | tail -n 1
> done
```

it would work exactly the same way.
*Don't do this.*
Programs are only useful if people can understand them,
so meaningless names (like `x`) or misleading names (like `temperature`)
increase the odds that the program won't do what its readers think it does.

In the above examples, the variables (`thing`, `filename`, `x` and `temperature`)
could have been given any other name, as long as it is meaningful to both the person
writing the code and the person reading it.

Note also that loops can be used for other things than filenames, like a list of numbers
or a subset of data.


## Challenge: Write your own loop

How would you write a loop that echoes all 10 numbers from 0 to 9?



## Challenge: Variables in Loops

This exercise refers to the `shell-lesson-data/exercise-data/alkanes` directory.
`ls *.pdb` gives the following output:

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

What is the output of the following code?

```bash
$ for datafile in *.pdb
> do
>     ls *.pdb
> done
```

Now, what is the output of the following code?

```bash
$ for datafile in *.pdb
> do
>     ls $datafile
> done
```

Why do these two loops give different outputs?



## Challenge: Limiting Sets of Files

What would be the output of running the following loop in the
`shell-lesson-data/exercise-data/alkanes` directory?

```bash
$ for filename in c*
> do
>     ls $filename
> done
```

1. No files are listed.
2. All files are listed.
3. Only `cubane.pdb`, `octane.pdb` and `pentane.pdb` are listed.
4. Only `cubane.pdb` is listed.


How would the output differ from using this command instead?

```bash
$ for filename in *c*
> do
>     ls $filename
> done
```

1. The same files would be listed.
2. All the files are listed this time.
3. No files are listed this time.
4. The files `cubane.pdb` and `octane.pdb` will be listed.
5. Only the file `octane.pdb` will be listed.


## Challenge: Saving to a File in a Loop - Part One

In the `shell-lesson-data/exercise-data/alkanes` directory, what is the effect of this loop?

```bash
for alkanes in *.pdb
do
    echo $alkanes
    cat $alkanes > alkanes.pdb
done
```

1. Prints `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` and
  `propane.pdb`, and the text from `propane.pdb` will be saved to a file called `alkanes.pdb`.
2. Prints `cubane.pdb`, `ethane.pdb`, and `methane.pdb`, and the text from all three files
  would be concatenated and saved to a file called `alkanes.pdb`.
3. Prints `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, and `pentane.pdb`,
  and the text from `propane.pdb` will be saved to a file called `alkanes.pdb`.
4. None of the above.


  

## Challenge: Saving to a File in a Loop - Part Two

Also in the `shell-lesson-data/exercise-data/alkanes` directory,
what would be the output of the following loop?

```bash
for datafile in *.pdb
do
    cat $datafile >> all.pdb
done
```

1. All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, and
  `pentane.pdb` would be concatenated and saved to a file called `all.pdb`.
2. The text from `ethane.pdb` will be saved to a file called `all.pdb`.
3. All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb`
  and `propane.pdb` would be concatenated and saved to a file called `all.pdb`.
4. All of the text from `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb`
  and `propane.pdb` would be printed to the screen and saved to a file called `all.pdb`.

***

Let's continue with our example in the `shell-lesson-data/exercise-data/creatures` directory.
Here's a slightly more complicated loop:

```bash
$ for filename in *.dat
> do
>     echo $filename
>     head -n 100 $filename | tail -n 20
> done
```

The shell starts by expanding `*.dat` to create the list of files it will process.
The **loop body**
then executes two commands for each of those files.
The first command, `echo`, prints its command-line arguments to standard output.
For example:

```bash
$ echo hello there
```

prints:

```output
hello there
```

In this case,
since the shell expands `$filename` to be the name of a file,
`echo $filename` prints the name of the file.
Note that we can't write this as:

```bash
$ for filename in *.dat
> do
>     $filename
>     head -n 100 $filename | tail -n 20
> done
```

because then the first time through the loop,
when `$filename` expanded to `basilisk.dat`, the shell would try to run `basilisk.dat` as
a program.
Finally,
the `head` and `tail` combination selects lines 81-100
from whatever file is being processed
(assuming the file has at least 100 lines).


## Spaces in Names

Spaces are used to separate the elements of the list
that we are going to loop over. If one of those elements
contains a space character, we need to surround it with
quotes, and do the same thing to our loop variable.
Suppose our data files are named:

```source
red dragon.dat
purple unicorn.dat
```

To loop over these files, we would need to add double quotes like so:

```bash
$ for filename in "red dragon.dat" "purple unicorn.dat"
> do
>     head -n 100 "$filename" | tail -n 20
> done
```

It is simpler to avoid using spaces (or other special characters) in filenames.

The files above don't exist, so if we run the above code, the `head` command will be unable
to find them; however, the error message returned will show the name of the files it is
expecting:

```error
head: cannot open ‘red dragon.dat' for reading: No such file or directory
head: cannot open ‘purple unicorn.dat' for reading: No such file or directory
```

Try removing the quotes around `$filename` in the loop above to see the effect of the quote
marks on spaces. Note that we get a result from the loop command for unicorn.dat
when we run this code in the `creatures` directory:

```output
head: cannot open ‘red' for reading: No such file or directory
head: cannot open ‘dragon.dat' for reading: No such file or directory
head: cannot open ‘purple' for reading: No such file or directory
CGGTACCGAA
AAGGGTCGCG
CAAGTGTTCC
...
```

***

We would like to modify each of the files in `shell-lesson-data/exercise-data/creatures`,
but also save a version of the original files. We want to copy the original files to new
files named `original-basilisk.dat` and `original-unicorn.dat`, for example. We can't use:

```bash
$ cp *.dat original-*.dat
```

because that would expand to:

```bash
$ cp basilisk.dat minotaur.dat unicorn.dat original-*.dat
```

This wouldn't back up our files, instead we get an error:

```error
cp: target `original-*.dat' is not a directory
```

This problem arises when `cp` receives more than two inputs. When this happens, it expects the
last input to be a directory where it can copy all the files it was passed. Since there is
no directory named `original-*.dat` in the `creatures` directory, we get an error.

Instead, we can use a loop:

```bash
$ for filename in *.dat
> do
>     cp $filename original-$filename
> done
```

This loop runs the `cp` command once for each filename.
The first time,
when `$filename` expands to `basilisk.dat`,
the shell executes:

```bash
cp basilisk.dat original-basilisk.dat
```

The second time, the command is:

```bash
cp minotaur.dat original-minotaur.dat
```

The third and last time, the command is:

```bash
cp unicorn.dat original-unicorn.dat
```

Since the `cp` command does not normally produce any output, it's hard to check
that the loop is working correctly. However, we learned earlier how to print strings
using `echo`, and we can modify the loop to use `echo` to print our commands without
actually executing them. As such we can check what commands *would be* run in the
unmodified loop.

The following diagram
shows what happens when the modified loop is executed and demonstrates how the
judicious use of `echo` is a good debugging technique.

![](../fig/shell_script_for_loop_flow_chart.svg){alt='The for loop "for filename in .dat; do echo cp $filename original-$filename;done" will successively assign the names of all ".dat" files in your currentdirectory to the variable "$filename" and then execute the command. With thefiles "basilisk.dat", "minotaur.dat" and "unicorn.dat" in the current directorythe loop will successively call the echo command three times and print threelines: "cp basislisk.dat original-basilisk.dat", then "cp minotaur.datoriginal-minotaur.dat" and finally "cp unicorn.datoriginal-unicorn.dat"'}

## Nelle's Pipeline: Processing Files

Nelle is now ready to process her data files using `goostats.sh` ---
a shell script written by her supervisor. This calculates some statistics from a
protein sample file and takes two arguments:

1. an input file (containing the raw data)
2. an output file (to store the calculated statistics)

Since she's still learning how to use the shell,
she decides to build up the required commands in stages.
Her first step is to make sure that she can select the right input files --- remember,
these are ones whose names end in 'A' or 'B', rather than 'Z'.
Moving to the `north-pacific-gyre` directory, Nelle types:

```bash
$ cd
$ cd Desktop/shell-lesson-data/north-pacific-gyre
$ for datafile in NENE*A.txt NENE*B.txt
> do
>     echo $datafile
> done
```

```output
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
```

Her next step is to decide
what to call the files that the `goostats.sh` analysis program will create.
Prefixing each input file's name with 'stats' seems simple,
so she modifies her loop to do that:

```bash
$ for datafile in NENE*A.txt NENE*B.txt
> do
>     echo $datafile stats-$datafile
> done
```

```output
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
```

She hasn't actually run `goostats.sh` yet,
but now she's sure she can select the right files and generate the right output filenames.

Typing in commands over and over again is becoming tedious,
though,
and Nelle is worried about making mistakes,
so instead of re-entering her loop,
she presses <kbd>↑</kbd>.
In response,
the shell redisplays the whole loop on one line
(using semi-colons to separate the pieces):

```bash
$ for datafile in NENE*A.txt NENE*B.txt; do echo $datafile stats-$datafile; done
```

Using the <kbd>←</kbd>,
Nelle navigates to the `echo` command and changes it to `bash goostats.sh`:

```bash
$ for datafile in NENE*A.txt NENE*B.txt; do bash goostats.sh $datafile stats-$datafile; done
```

When she presses <kbd>Enter</kbd>,
the shell runs the modified command.
However, nothing appears to happen --- there is no output.
After a moment, Nelle realizes that since her script doesn't print anything to the screen
any longer, she has no idea whether it is running, much less how quickly.
She kills the running command by typing <kbd>Ctrl</kbd>\+<kbd>C</kbd>,
uses <kbd>↑</kbd> to repeat the command,
and edits it to read:

```bash
$ for datafile in NENE*A.txt NENE*B.txt; do echo $datafile;
bash goostats.sh $datafile stats-$datafile; done
```


## Beginning and End

We can move to the beginning of a line in the shell by typing <kbd>Ctrl</kbd>\+<kbd>A</kbd>
and to the end using <kbd>Ctrl</kbd>\+<kbd>E</kbd>.


***

When she runs her program now,
it produces one line of output every five seconds or so:

```output
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
```

1518 times 5 seconds,
divided by 60,
tells her that her script will take about two hours to run.
As a final check,
she opens another terminal window,
goes into `north-pacific-gyre`,
and uses `cat stats-NENE01729B.txt`
to examine one of the output files.
It looks good,
so she decides to get some coffee and catch up on her reading.


## Those Who Know History Can Choose to Repeat It

Another way to repeat previous work is to use the `history` command to
get a list of the last few hundred commands that have been executed, and
then to use `!123` (where '123' is replaced by the command number) to
repeat one of those commands. For example, if Nelle types this:

```bash
$ history | tail -n 5
```

```output
456  for datafile in NENE*A.txt NENE*B.txt; do   echo $datafile stats-$datafile; done
457  for datafile in NENE*A.txt NENE*B.txt; do echo $datafile stats-$datafile; done
458  for datafile in NENE*A.txt NENE*B.txt; do bash goostats.sh $datafile stats-$datafile; done
459  for datafile in NENE*A.txt NENE*B.txt; do echo $datafile; bash goostats.sh $datafile
stats-$datafile; done
460  history | tail -n 5
```

then she can re-run `goostats.sh` on the files simply by typing
`!459`.



## Other History Commands

There are a number of other shortcut commands for getting at the history.

- <kbd>Ctrl</kbd>\+<kbd>R</kbd> enters a history search mode 'reverse-i-search' and finds the
  most recent command in your history that matches the text you enter next.
  Press <kbd>Ctrl</kbd>\+<kbd>R</kbd> one or more additional times to search for earlier matches.
  You can then use the left and right arrow keys to choose that line and edit
  it then hit <kbd>Return</kbd> to run the command.
- `!!` retrieves the immediately preceding command
  (you may or may not find this more convenient than using <kbd>↑</kbd>)
- `!$` retrieves the last word of the last command.
  That's useful more often than you might expect: after
  `bash goostats.sh NENE01729B.txt stats-NENE01729B.txt`, you can type
  `less !$` to look at the file `stats-NENE01729B.txt`, which is
  quicker than doing <kbd>↑</kbd> and editing the command-line.
  


## Challenge: Doing a Dry Run

A loop is a way to do many things at once --- or to make many mistakes at
once if it does the wrong thing. One way to check what a loop *would* do
is to `echo` the commands it would run instead of actually running them.

Suppose we want to preview the commands the following loop will execute
without actually running those commands:

```bash
$ for datafile in *.pdb
> do
>     cat $datafile >> all.pdb
> done
```

What is the difference between the two loops below, and which one would we
want to run?

```bash
# Version 1
$ for datafile in *.pdb
> do
>     echo cat $datafile >> all.pdb
> done
```

```bash
# Version 2
$ for datafile in *.pdb
> do
>     echo "cat $datafile >> all.pdb"
> done
```



## Challenge: Nested Loops

Suppose we want to set up a directory structure to organize
some experiments measuring reaction rate constants with different compounds
*and* different temperatures.  What would be the
result of the following code:

```bash
$ for species in cubane ethane methane
> do
>     for temperature in 25 30 37 40
>     do
>         mkdir $species-$temperature
>     done
> done
```



## Keypoints

- A `for` loop repeats commands once for every thing in a list.
- Every `for` loop needs a variable to refer to the thing it is currently operating on.
- Use `$name` to expand a variable (i.e., get its value). `${name}` can also be used.
- Do not use spaces, quotes, or wildcard characters such as '\*' or '?' in filenames, as it complicates variable expansion.
- Give files consistent names that are easy to match with wildcard patterns to make it easy to select them for looping.
- Use the up-arrow key to scroll up through previous commands to edit and repeat them.
- Use <kbd>Ctrl</kbd>\+<kbd>R</kbd> to search through the previously entered commands.
- Use `history` to display recent commands, and `![number]` to repeat a command by number.





