##  challenge

## Creating Files a Different Way

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


## Solution

1. The `touch` command generates a new file called `my_file.txt` in
  your current directory.  You
  can observe this newly generated file by typing `ls` at the
  command line prompt.  `my_file.txt` can also be viewed in your
  GUI file explorer.

2. When you inspect the file with `ls -l`, note that the size of
  `my_file.txt` is 0 bytes.  In other words, it contains no data.
  If you open `my_file.txt` using your text editor it is blank.

3. Some programs do not generate output files themselves, but
  instead require that empty files have already been generated.
  When the program is run, it searches for an existing file to
  populate with its output.  The touch command allows you to
  efficiently generate a blank text file to be used by such
  programs.

***

## Moving Files to a new folder

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


## Solution

```bash
$ mv sucrose.dat maltose.dat ../raw
```

Recall that `..` refers to the parent directory (i.e. one above the current directory)
and that `.` refers to the current directory.

***
##  challenge

## Renaming Files

Suppose that you created a plain-text file in your current directory to contain a list of the
statistical tests you will need to do to analyze your data, and named it `statstics.txt`

After creating and saving this file you realize you misspelled the filename! You want to
correct the mistake, which of the following commands could you use to do so?

1. `cp statstics.txt statistics.txt`
2. `mv statstics.txt statistics.txt`
3. `mv statstics.txt .`
4. `cp statstics.txt .`


## Solution

1. No.  While this would create a file with the correct name,
  the incorrectly named file still exists in the directory
  and would need to be deleted.
2. Yes, this would work to rename the file.
3. No, the period(.) indicates where to move the file, but does not provide a new file name;
  identical file names
  cannot be created.
4. No, the period(.) indicates where to copy the file, but does not provide a new file name;
  identical file names cannot be created.

***

##  challenge

## Moving and Copying

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


## Solution

We start in the `/Users/jamie/data` directory, and create a new folder called `recombined`.
The second line moves (`mv`) the file `proteins.dat` to the new folder (`recombined`).
The third line makes a copy of the file we just moved.
The tricky part here is where the file was copied to.
Recall that `..` means 'go up a level', so the copied file is now in `/Users/jamie`.
Notice that `..` is interpreted with respect to the current working
directory, **not** with respect to the location of the file being copied.
So, the only thing that will show using ls (in `/Users/jamie/data`) is the recombined folder.

1. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
2. Yes
3. No, see explanation above.  `proteins.dat` is located at `/Users/jamie/data/recombined`
4. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`

***

##  challenge

## Using `rm` Safely

What happens when we execute `rm -i thesis_backup/quotations.txt`?
Why would we want this protection when using `rm`?


## Solution

```output
rm: remove regular file 'thesis_backup/quotations.txt'? y
```

The `-i` option will prompt before (every) removal (use <kbd>Y</kbd> to confirm deletion
or <kbd>N</kbd> to keep the file).
The Unix shell doesn't have a trash bin, so all the files removed will disappear forever.
By using the `-i` option, we have the chance to check that we are deleting only the files
that we want to remove.

***

##  challenge

## Copy with Multiple Filenames

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


## Solution

If given more than one file name followed by a directory name
(i.e. the destination directory must be the last argument),
`cp` copies the files to the named directory.

If given three file names, `cp` throws an error such as the one below,
because it is expecting a directory name as the last argument.

```error
cp: target 'basilisk.dat' is not a directory
```

***

##  challenge

## List filenames matching a pattern

When run in the `alkanes` directory, which `ls` command(s) will
produce this output?

`ethane.pdb   methane.pdb`

1. `ls *t*ane.pdb`
2. `ls *t?ne.*`
3. `ls *t??ne.pdb`
4. `ls ethane.*`


## Solution

The solution is `3.`

`1.` shows all files whose names contain zero or more characters (`*`)
followed by the letter `t`,
then zero or more characters (`*`) followed by `ane.pdb`.
This gives `ethane.pdb  methane.pdb  octane.pdb  pentane.pdb`.

`2.` shows all files whose names start with zero or more characters (`*`) followed by
the letter `t`,
then a single character (`?`), then `ne.` followed by zero or more characters (`*`).
This will give us `octane.pdb` and `pentane.pdb` but doesn't match anything
which ends in `thane.pdb`.

`3.` fixes the problems of option 2 by matching two characters (`??`) between `t` and `ne`.
This is the solution.

`4.` only shows files starting with `ethane.`.

***
##  challenge

## More on Wildcards

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


## Solution

```bash
$ cp *calibration.txt backup/calibration
$ cp 2015-11-* send_to_bob/all_november_files/
$ cp *-23-dataset* send_to_bob/all_datasets_created_on_a_23rd/
```


***
##  challenge

## Organizing Directories and Files

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


## Solution

```bash
mv *.dat analyzed
```

Jamie needs to move her files `fructose.dat` and `sucrose.dat` to the `analyzed` directory.
The shell will expand \*.dat to match all .dat files in the current directory.
The `mv` command then moves the list of .dat files to the 'analyzed' directory.

***

##  challenge

## Reproduce a folder structure

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

##  challenge

## What Does `sort -n` Do?

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

:::::::::::::::  solution

## Solution

The `-n` option specifies a numerical rather than an alphanumerical sort.

***

##  challenge

## What Does `>>` Mean?

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


## Solution

In the first example with `>`, the string 'hello' is written to `testfile01.txt`,
but the file gets overwritten each time we run the command.

We see from the second example that the `>>` operator also writes 'hello' to a file
(in this case `testfile02.txt`),
but appends the string to the file if it already exists
(i.e. when we run it for the second time).

***

## challenge

## Appending Data


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


## Solution

Option 3 is correct.
For option 1 to be correct we would only run the `head` command.
For option 2 to be correct we would only run the `tail` command.
For option 4 to be correct we would have to pipe the output of `head` into `tail -n 2`
by doing `head -n 3 animals.csv | tail -n 2 > animals-subset.csv`

***

##  challenge

## Piping Commands Together

In our current directory, we want to find the 3 files which have the least number of
lines. Which command listed below would work?

1. `wc -l * > sort -n > head -n 3`
2. `wc -l * | sort -n | head -n 1-3`
3. `wc -l * | head -n 3 | sort -n`
4. `wc -l * | sort -n | head -n 3`


## Solution

Option 4 is the solution.
The pipe character `|` is used to connect the output from one command to
the input of another.
`>` is used to redirect standard output to a file.
Try it in the `shell-lesson-data/exercise-data/alkanes` directory!

***

##  challenge

## Pipe Reading Comprehension

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


## Solution

The `head` command extracts the first 5 lines from `animals.csv`.
Then, the last 3 lines are extracted from the previous 5 by using the `tail` command.
With the `sort -r` command those 3 lines are sorted in reverse order.
Finally, the output is redirected to a file: `final.txt`.
The content of this file can be checked by executing `cat final.txt`.
The file should contain the following lines:

```source
2012-11-06,rabbit,19
2012-11-06,deer,2
2012-11-05,raccoon,7
```

***

##  challenge

## Pipe Construction

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


## Solution

```bash
$ cut -d , -f 2 animals.csv | sort | uniq
```

***

##  challenge

## Which Pipe?

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


## Solution

Option 4. is the correct answer.
If you have difficulty understanding why, try running the commands, or sub-sections of
the pipelines (make sure you are in the `shell-lesson-data/exercise-data/animal-counts`
directory).

***

##  challenge

## Removing Unneeded Files

Suppose you want to delete your processed data files, and only keep
your raw files and processing script to save storage.
The raw files end in `.dat` and the processed files end in `.txt`.
Which of the following would remove all the processed data files,
and *only* the processed data files?

1. `rm ?.txt`
2. `rm *.txt`
3. `rm * .txt`
4. `rm *.*`


## Solution

1. This would remove `.txt` files with one-character names
2. This is the correct answer
3. The shell would expand `*` to match everything in the current directory,
  so the command would try to remove all matched files and an additional
  file called `.txt`
4. The shell expands `*.*` to match all filenames containing at least one
  `.`, including the processed files (`.txt`) *and* raw files (`.dat`)
  
***

##  challenge

## Write your own loop

How would you write a loop that echoes all 10 numbers from 0 to 9?


## Solution

```bash
$ for loop_variable in 0 1 2 3 4 5 6 7 8 9
> do
>     echo $loop_variable
> done
```

```output
0
1
2
3
4
5
6
7
8
9
```

***

##  challenge

## Variables in Loops

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


## Solution

The first code block gives the same output on each iteration through
the loop.
Bash expands the wildcard `*.pdb` within the loop body (as well as
before the loop starts) to match all files ending in `.pdb`
and then lists them using `ls`.
The expanded loop would look like this:

```bash
$ for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> do
>     ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> done
```

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

The second code block lists a different file on each loop iteration.
The value of the `datafile` variable is evaluated using `$datafile`,
and then listed using `ls`.

```output
cubane.pdb
ethane.pdb
methane.pdb
octane.pdb
pentane.pdb
propane.pdb
```

***

##  challenge

## Limiting Sets of Files

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

## Solution

4 is the correct answer. `*` matches zero or more characters, so any file name starting with
the letter c, followed by zero or more other characters will be matched.

***
##  challenge

## Limiting Sets of Files

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


## Solution

4 is the correct answer. `*` matches zero or more characters, so a file name with zero or more
characters before a letter c and zero or more characters after the letter c will be matched.

***

##  challenge

## Saving to a File in a Loop - Part One

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


## Solution

1. The text from each file in turn gets written to the `alkanes.pdb` file.
  However, the file gets overwritten on each loop iteration, so the final content of
  `alkanes.pdb`
  is the text from the `propane.pdb` file.

***

##  challenge

## Saving to a File in a Loop - Part Two

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

:::::::::::::::  solution

## Solution

3 is the correct answer. `>>` appends to a file, rather than overwriting it with the redirected
output from a command.
Given the output from the `cat` command has been redirected, nothing is printed to the screen.


  
