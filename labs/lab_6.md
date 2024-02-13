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









#Look around bam file
#Sort bam file.
#Extract single region of bam file
#Remove PCR duplicates. 
