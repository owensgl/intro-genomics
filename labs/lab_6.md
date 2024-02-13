---
title: Unknown
element: lab
layout: default
---

## Objectives

- To learn how to align sequence data to a reference.
- To learn how to interpret sam and bam files.
- To learn how to manipulate a bam file.
- To learn how to filter a bam file.

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








#Look around bam file
#Extract single region of bam file
