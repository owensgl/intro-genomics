Certainly! Here's the improved formatting of the markdown document:

```markdown
---
title: Shell Scripting Lab
element: lab
layout: default
---

# Objectives

- Create a directory hierarchy that matches a given diagram.
- Create files in that hierarchy using an editor or by copying and renaming existing files.
- Delete, copy and move specified files and/or directories.

# Questions

- How can I create, copy, and delete files and directories?
- How can I edit files?

# Creating Directories

We now know how to explore files and directories, but how do we create them in the first place? In this lab, we will learn about creating and moving files and directories, using the `exercise-data/writing` directory as an example.

## Step One: See Where We Are and What We Already Have

First, we should return to the home directory:

```bash
$ cd ~
```

Then enter the `lab_2_data` directory:

```bash
$ cd lab_2_data
```

Next, we'll move to the `exercise-data/writing` directory and see what it contains:

```bash
$ cd exercise-data/writing/
$ ls -F
```

Output:
```bash
haiku.txt  LittleWomen.txt
```

### Create a Directory

Let's create a new directory called `thesis` using the command `mkdir thesis` (which has no output):

```bash
$ mkdir thesis
```

Since `thesis` is a relative path (i.e., does not have a leading slash, like `/what/ever/thesis`), the new directory is created in the current working directory:

```bash
$ ls -F
```

Output:
```bash
haiku.txt  LittleWomen.txt  thesis/
```

Since we've just created the `thesis` directory, there's nothing in it yet:

```bash
$ ls -F thesis
```

Note that `mkdir` is not limited to creating single directories one at a time. The `-p` option allows `mkdir` to create a directory with nested subdirectories in a single operation:

```bash
$ mkdir -p ../project/data ../project/results
```

The `-R` option to the `ls` command will list all nested subdirectories within a directory. Let's use `ls -FR` to recursively list the new directory hierarchy we just created in the `project` directory:

```bash
$ ls -FR ../project
```

Output:
```bash
../project/:
data/  results/

../project/data:

../project/results:
```

## Callout: Good Names for Files and Directories

Complicated names of files and directories can make your life painful when working on the command line. Here we provide a few useful tips for the names of your files and directories.

1. Don't use spaces. Spaces can make a name more meaningful, but since spaces are used to separate arguments on the command line, it is better to avoid them in names of files and directories. You can use `-` or `_` instead (e.g., `north-pacific-gyre/` rather than `north pacific gyre/`). To test this out, try typing `mkdir north pacific gyre` and see what directory (or directories!) are made when you check with `ls -F`.

2. Don't begin the name with `-` (dash). Commands treat names starting with `-` as options.

3. Stick with letters, numbers, `.` (period or 'full stop'), `-` (dash) and `_` (underscore). Many other characters have special meanings on the command line. We will learn about some of these during this lesson. There are special characters that can cause your command to not work as expected and can even result in data loss. If you need to refer to names of files or directories that have spaces or other special characters, you should surround the name in quotes (`""`).

### Create a Text File

Let's change our working directory to `thesis` using `cd`, then run a text editor called Nano to create a file called `draft.txt`:

```bash
$ cd thesis
$ nano draft.txt
```

## Callout: Which Editor?

When we say, '`nano` is a text editor,' we really do mean 'text.' It can only work with plain character data, not tables, images, or any other human-friendly media. We use it in examples because it is one of the least complex text editors. However, because of this trait, it may not be powerful enough or flexible enough for the work you need to do after this workshop. On Unix systems (such as Linux and macOS), many programmers use [Emacs](https://www.gnu.org/software/emacs/) or [Vim](https://www.vim.org/) (both of which require more time to learn), or a graphical editor such as [Gedit](https://projects.gnome.org/gedit/) or [VScode](https://code.visualstudio.com/). On Windows, you may wish to use [Notepad++](https://notepad-plus-plus.org/). Windows also has a built-in editor called `notepad` that can be run from the command line in the same way as `nano` for the purposes of this lesson.

No matter what editor you use, you will need to know where it searches for and saves files. If you start it from the shell, it will (probably) use your current working directory as its default location. If you use your computer's start menu, it may want to save files in your Desktop or Documents directory instead. You can change this by navigating to another directory the first time you 'Save As...'

![Nano Screenshot](figs/nano-screenshot.png)

Let's type in a few lines of text.

Once we're happy with our text, we can press <kbd>Ctrl</kbd> + <kbd>O</kbd> (press the <kbd>Ctrl</kbd> or <kbd>Control</kbd> key and, while holding it down, press the <kbd>O</kbd> key) to write our data to disk. We will be asked to provide a name for the file that will contain our text. Press <kbd>Return</kbd> to accept the suggested default of `draft.txt`.

Once our file is saved, we can use <kbd>Ctrl</kbd> + <kbd>X</kbd> to quit the editor and return to the shell.

## Callout: Control, Ctrl, or ^ Key

The Control key is also called the 'Ctrl' key. There are various ways in which using the Control key may be described. For example, you may see an instruction to press the <kbd>Control</kbd> key and, while holding it down, press the <kbd>X</kbd> key, described as any of:

- `Control-X`
- `Control+X`
- `Ctrl-X`
- `Ctrl+X`
- `^X`
- `C-x`

In nano, along the bottom of the screen you'll see `^G Get Help ^O WriteOut`. This means that you can use `Control-G` to get help and `Control-O` to save your file.

`nano` doesn't leave any output on the screen after it exits, but `ls` now shows that we have created a file called `draft.txt`:

```bash
$ ls
```

Output:
```bash
draft.txt


```
```
