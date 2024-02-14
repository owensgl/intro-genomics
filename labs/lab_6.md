---
title: Alignment
element: lab
layout: default
---

## Objectives

- To learn how to align sequence data to a reference.
- To learn how to interpret sam and bam files.
- To learn how to manipulate a bam file.

****

In this lab we're going to learn how to align illumina data to a reference genome. 
This is the first step in many genomic analyses. We start off we a large number
of anonymous reads from somewhere in the genome (or perhaps a different genome all together)
and we need to know where they come from before we can say anything about genetic variation
or gene expression. Log into the server to get started, and source 

After logging in, lets make a new directory for this lab.

```bash
#First we source our bash profile to make sure we have access to the modules.
source /cvmfs/soft.computecanada.ca/config/profile/bash.sh
#Next make your new directory
mkdir lab_6
#And enter it
cd lab_6
```

I've put the required files in a shared directory on the system. We'll
copy it over to our current directory.
```bash
cp /project/ctb-grego/sharing/lab6_data.tar.gz ./
```

This is a tar.gz file. A tar file (originally named for tape archive) is 
a way of putting multiple files into a single file. In our case, we are also
gzipping it, which compresses the size. Most programs you download will be
stored in something like a tar.gz file. Before we can access the files in the 
tar.gz file, we have to open and unzip it. 
```bash
tar -xzf lab6_data.tar.gz
```

It's hard to remember the specific letter flags to use when unzipping a tar.gz file, 
so my trick is to picture someone with a thick german accent saying "Extract Zee Files!", 
for X, Z, F

```bash
ls
```
```output
lab6_data.tar.gz  data
```
The files we need are in the data directory, lets take a look.

```bash
cd data
ls
```

```output
SalmonReference.fasta                           SalmonSim.Stabilising.p10.i2.80000_R1.fastq.gz
SalmonSim.Stabilising.p10.i1.80000_R1.fastq.gz  SalmonSim.Stabilising.p10.i2.80000_R2.fastq.gz
SalmonSim.Stabilising.p10.i1.80000_R2.fastq.gz
```

Here we have a miniature version of the chinook salmon reference genome, named "SalmonReference.fasta". 
We also have sequencing data for two samples, SalmonSim.Stabilising.p10.i1.80000 and SalmonSim.Stabilising.p10.i2.80000.
Each of them have both forward and reverse reads. This name is fairly long and complicated because of 
how I generated this sequence data, but this sort of complicated naming is often what you'll see. It's
better to have a long and descriptive name (e.g. "SpeciesX.Pop102.Sample53.Tissue39"), than a short non-descriptive
one ( e.g. "Sample_A"). When plotting the data, you can switch to easier to read sample names if necessary.

Before we can start aligning, we need to index our reference genome. Last week we indexed our reference
genomes using samtools. The alignment program we're using also needs to index the reference genome. 

```bash
module load StdEnv/2023
module load samtools/1.18
module load bwa-mem2/2.2.1

samtools faidx SalmonReference.fasta
bwa-mem2 index SalmonReference.fasta
```

We now have index files for our reference genome. Take a look at them. In contrast to the .fai file we worked with 
last lab, these files are not actually useful to us on their own. They're necessary for the alignment 
program, but they're only machine readable. Regardless, lets align our first sample!

```bash
bwa-mem2 mem -t 2 SalmonReference.fasta SalmonSim.Stabilising.p10.i1.80000_R1.fastq.gz SalmonSim.Stabilising.p10.i1.80000_R2.fastq.gz > SalmonSim.Stabilising.p10.i1.80000.sam
```

This will output a lot of text as it progresses. For most projects, this step will take minutes or hours,
but here we're working with a small dataset of simulated reads from a small genome, so it finishes
immediately. 

We now have our data in "sam" format. This stands for Sequence Alignment/Map format. In the sam file,
we have our base calls for each read, along with the quality score. This means a sam file has all the information
in our original fastq format, plus additional info! Specifically, the sam file contains information about
whether and where each read has mapped in the genome. 

```bash
head SalmonSim.Stabilising.p10.i1.80000.sam | less -S
```

```output
@SQ     SN:chr_1        LN:5000000
@SQ     SN:chr_2        LN:5000000
@PG     ID:bwa-mem2     PN:bwa-mem2     VN:2.2.1        CL:bwa-mem2 mem -t 2 SalmonReference.fasta SalmonSim.Stabi>
chr_1_1_0_0     99      chr_1   4629813 60      126M    =       4630544 857     CTCTGCCATGCTGTGTTGTCATGTGTTACTGTCA>
chr_1_1_0_0     147     chr_1   4630544 60      126M    =       4629813 -857    TGGGGTCCAATAAAACCTAGTAGTATCTTATATT>
chr_1_1_1_0     99      chr_1   3307901 60      126M    =       3308085 310     TTCACTATTTTGTGGTAGCTCTGAACTTGCTCAC>
chr_1_1_1_0     147     chr_1   3308085 60      126M    =       3307901 -310    TGCTTGACGGATTACTTGGGGGGGGGGTTGACAC>
chr_1_1_2_0     99      chr_1   2893195 60      126M    =       2894284 1215    CACCTGCCTTGTATTTCTCAGATTTCTATCTGAG>
chr_1_1_2_0     147     chr_1   2894284 60      126M    =       2893195 -1215   TTGAGGCCTCATCTTCACTTTCACTTTCCAACCT>
chr_1_1_3_0     99      chr_1   4532319 60      126M    =       4532871 678     TCAGTGCCTCTTTACTTGACATCATGGGAAAATC>
```

The first two lines are headers, which list all the contigs in the reference genome, along with their size. After that,
we have more header lines that specify all the programs and commands used to create this file. In this case,
it has the bwa-mem2 call (and specific information about the flags used for that command call). As we 
modify the sam file, more lines will get added to the header recording the steps we did. This
is very helpful if you forget what you did exactly. 

Next, we have one line per read with a bunch of columns. 
1) The query template name. This is a unique name for each read or read pair. In this case, it has "chr_1"
because it was simulated using chr_1 as a reference, but for real data this will be a random string of characters.
2) The bitwise flag. This number tells us about the read and its mapping. There are multiple different possible 
things the read could have, and the total value of the number can the translated into that information. For example, 
if the read is paired, add + 1. If the read is unmapped, add + 8. Therefore, if the final number is 9, that means it
is paired and unmapped. The decoding of all possible numbers can be found (here)[https://broadinstitute.github.io/picard/explain-flags.html].
3) The contig that the read is mapped to
4) The starting location where the read maps to, on the contig.
5) The mapping quality. 60 is the highest (generally) and means high confidence in where the read is mapped.
The lowest value is 0, which means it is equally likely to map elsewhere.
6) The CIGAR string describing how the read is aligned.
7) The ID of the read pair mate (in this case, the same as the original)
8) The mapping location of the read pair mate.
9) The distance between read map pairs (sort of)
10) The sequence of the read
11) The base quality of the read
12) Any extra flags.

## Questions
1) For one read, you find a CIGAR string "12M1I113M". Refer to the (sam format manual)[https://samtools.github.io/hts-specs/SAMv1.pdf] and figure out
what this means for how the read is aligned.
2) Find the read with the lowest mapping quality in your dataset using command line programs.
3) How are the reads ordered in this sam file?
4) What are three possible reasons why a read could have very low mapping quality?

****

As you can tell, reads are not ordered based on where they are aligned in the genome. Most programs
require that reads be ordered by their position in the genome because it allows them to be
rapidly accessed. Imagine if the reads were randomly ordered but we wanted to get all the reads
from the start of chromosome 1. The program would need to search through the entire file to find
those reads before it could use them (which can take a while). When reads are ordered, 
programs can know exactly where to find all the reads for a particular region. 

Therefore, we are going to sort the sam file by read position. At the same time, we'll 
convert it to a "bam" file. This is a binary version of the sam file, so the file size 
is smaller, but we will need to use samtools to convert it to a human readable version if 
we later want to look at it. 

```bash
samtools sort SalmonSim.Stabilising.p10.i1.80000.sam > SalmonSim.Stabilising.p10.i1.80000.sort.bam
```
We've now changed the name to end with .sort.bam to remind us that this version of the file
is sorted and it is a bam file. To access the file now, using standard text viewing programs
won't work. We need to use samtools view. Lets try that.


```bash
#This will print weird characters, because its in binary
head SalmonSim.Stabilising.p10.i1.80000.sort.bam

#This will convert the file to standard text and then print it to screen
samtools view -h SalmonSim.Stabilising.p10.i1.80000.sort.bam | head
```

Another important step in processing alignment files is to mark duplicates. These
are cases where we have sequenced the same molecule more than once. Since they
don't represent independent observations of the genome, we need to mark them so
that we only use one copy in our calculations. We'll use picardtools for this.

```bash
module load picard/3.1.0
java -jar $EBROOTPICARD/picard.jar MarkDuplicates I=SalmonSim.Stabilising.p10.i1.80000.sort.bam O=SalmonSim.Stabilising.p10.i1.80000.sort.markdup.bam M=SalmonSim.Stabilising.p10.i1.80000.dupmetrics.txt
```
In cases where there are many options to a single command, it can be easier to put each option on its own line. 
As long as we end each line with \ the system will treat it as if it was a single line. Be careful
not to have a space or anything after the \, because then it will think that the line really has ended (and your command will mess up).

```bash
#Alternate formatting for command
java -jar $EBROOTPICARD/picard.jar MarkDuplicates \
  I=SalmonSim.Stabilising.p10.i1.80000.sort.bam \
  O=SalmonSim.Stabilising.p10.i1.80000.sort.markdup.bam \
  M=SalmonSim.Stabilising.p10.i1.80000.dupmetrics.txt
```

For this example data, there are no duplicates because this is simulated. In most cases,
you will have at least some duplicates. 

Lastly, we need to index the bam file. Many programs require the file to be indexed,
before it can be used. The index files are prefixed with .bai for bam index.

```bash
samtools index SalmonSim.Stabilising.p10.i1.80000.sort.markdup.bam
```

We now have a nicely sorted and labelled bam file. Lets get some stats on it, 
using samtools commands.

```bash
samtools coverage SalmonSim.Stabilising.p10.i1.80000.sort.markdup.bam
```
This shows you a summary of the depth and coverage for your sample. 
Depth indicates how many reads are on each base (on average) and 
coverage tells you how much of the reference sequence is covered by
at least one base. 

### Questions
1) Use samtools stats to find out how many reads had MAPQ less than 10 in sample "SalmonSim.Stabilising.p10.i1.80000".
2) Use samtools flagstats to find out what percent of reads are mapped to the genome.


We can also look at the alignment itself by using samtools tview.
```bash
samtools tview SalmonSim.Stabilising.p10.i1.80000.sort.markdup.bam SalmonReference.fasta
```
Once you're inside, use the "?" key to bring up the help menu. Then use keys to move
and look at your alignment. For this sample, we have very little differences between 
our sample and the reference, but when we start doing variant calling we can use
this to actually look at the reads and make sure that the variants are real. 

***

We often sequence many samples, and each of them needs to be processed individually. 
We could make one long script that does each step for each sample individually, 
but it's much more manageable to use loops to repeat the same steps for 
many different samples. Lets go through a toy example to learn some tricks. 

First lets start with a list of fastq files. You can make this file in nano by 
copying and pasting and name it "fastq_files.txt". You could imagine making
this file using the "ls" command with real data.
```bash
cat fastq_files.txt
```
```output
sample01_R1.fastq.gz
sample01_R2.fastq.gz
sample02_R1.fastq.gz
sample02_R2.fastq.gz
sample29_R1.fastq.gz
sample29_R2.fastq.gz
```

First thing is that we have both R1 and R2 files, but we're going to be treating
those together as a single sample. So lets filter out the R2 lines using "grep"

```bash
cat fastq_files.txt | grep -v R2.fastq.gz > fastq_files.R1.txt
```
Now we have a list of files that we want to work on. Here are two ways
we can loop through each line in that file.

Option 1:
```bash
for x in `cat fastq_files.R1.txt`
do
  echo $x
done
```
In this option, we're doing a creating our list of $x variables by using
a mini command call in the `` marks. We could modify it on the fly with additional filters. 
For example
Option 1a:
```bash
for x in `cat fastq_files.R1.txt | grep -v sample29`
do
  echo $x
done
```
Here we've excluded sample29. 

In the second option, we use a "while" loop and the "read" command.
The file that it is reading is specified at the end of the while loop. 
Option 2:
```bash
while read x; do
  echo $x
done < fastq_files.R1.txt
```

A second challenge when making shell scripts is that we often want to
make a series of files with similar names but different suffixes. In this case
we start off with "sample29_R1.fastq.gz". The bam file we create from this should
be sample29.bam, not sample29_R1.fastq.gz.bam. 

Here are two options for removing suffixes. 
Option 1:
```bash
x=sample01_R1.fastq.gz
y=$(basename $x _R1.fastq.gz)
echo $y
```
The "basename" removes a suffix that you specify for a string. It
can also function to remove directories before a filename. 
```bash
x=fastq/data/sample01_R1.fastq.gz
y=$(basename $x)
echo $y
```

A more general approach is to use sed (a.k.a. stream editor) 
to modify a variable. Sed can be used to find and substitute 
any character pattern in a string. It works like this:
s/character to find/character to replace it with/g
The "s" at the start is for subtitution.
The "g" at the end is for global (which means it finds all instances
of the replacement, not just the first). 

Take a look at these examples and see if you understand how they work:
```bash
echo "cats" | sed s/a/o/g

echo "cats" | sed s/a//g

echo "cats" | sed s/a/aaaaaa/g

echo "cats" | sed s/ats/ats_are_funky/g
```
We can use this to rename variables.
```bash
x=sample01_R1.fastq.gz
y=$(echo $x | sed s/_R1.fastq.gz//g)
echo $y
```

### Questions
1) Use multiple sed commands to change the first sentence into the second sentence. "My favorite city is Vancouver because Vancouver is much better than Victoria" into "My favorite city is Victoria because Victoria is much better than Vancouver".
2) Extract the sample name using a command from the file named "fastq/pool_1/Pool_1.sample902_R1.fastq.gz"

For the lab questions, you'll be working on a small set of real data. Lets copy that into your directory
```bash
cd ~
mkdir lab_6_questions
cd lab_6_questions
cp /project/ctb-grego/sharing/lab_6/* ./
```
