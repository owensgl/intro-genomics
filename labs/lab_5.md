---
title: TEST
element: lab
layout: default
---

## Objectives

- To learn how to download data from the SRA
- To learn how to filter a fastq file
- To understand the structure and format of genome files
- To learn tricks for manipulating a fastq file

****

Today we're going to learn how to access sequence data and prepare it for analysis. Please login to 
the "indri" server and go to your home directory to start. 

As a first step, we need to download some sequence data. The North American repository of sequence data
is the Sequence Read Archive (SRA). Whenever a researcher wants to publish a genomic
analysis, they need to upload their raw data to the SRA so that other
researchers can use replicate their results or use it for other purposes. 
The sample we're going to work with is identified as DRR053219. 

### Activity

- What species is this sample from?
- What project was this sample sequenced for?
- Each data file in the SRA has multiple layers of metadata. Specifically Bioproject, Biosample, Experiment and Run. 
Explain the difference between each of these layers.

****

To download data we are going to use fasterq-dump, a program that can pull reads from the SRA 
based on their ID number. 

```bash
#First we need to load the module system on the server, if you didn't modify your .bashrc in previous lessons.
source /cvmfs/soft.computecanada.ca/config/profile/bash.sh

#Then we load the sra-toolkit, which has fasterq-dump
module load StdEnv/2023 gcc/12.3 sra-toolkit/3.0.9

#Then we download the sample, splitting up the data into two files for the forward and reverse reads
fasterq-dump --skip-technical -S DRR053219

```

We now have two read files for this sample. It's good pratice to exam the quality of the data
using a program like FastQC. This program will process your samples and output a variety of stats 
about the data.


```bash
#First load the program
module load  fastqc/0.12.1
#Then run fastqc on our samples.
fastqc DRR053219_1.fastq DRR053219_2.fastq
```
It will produce an html report for each sample titled DRR053219_1_fastqc.html and DRR053219_2_fastqc.html.
To view those, you'll need to download them to your desktop. Open a new terminal and run scp.
```bash
scp grego@indri.rcs.uvic.ca:/home/grego/DRR*html Desktop/
```

Take a look at the output and answer the following questions
### Activity
- Which read has better base quality?
- What is an overrepresented sequence and what is the likely source? Think about how this sequence got there.
- Look at the "Per base sequence content" and compare it to an example in the FastQC manual (https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf). 
What is this result telling us about the sequencing library? If this were a normal whole genome sequencing library, would this be a 
good or bad sign?

****

If you don't see any red flags in fastqc, the next step is to trim the reads. This will do three different things:
1) Remove adapter sequences, which were added to your DNA but aren't from your target sample's genome.
2) Remove low quality reads.
3) Trim poor quality bases off the ends of reads. Base quality tends to decline so the ends of reads can be very low quality.

```bash
#First load the program
module load trimmomatic
#Then run trimmomatic on our samples.
java -jar $EBROOTTRIMMOMATIC/trimmomatic-0.39.jar PE DRR053219_1.fastq DRR053219_2.fastq DRR053219_1.trim.fastq DRR053219_1.unpaired.fastq DRR053219_2.trim.fastq DRR053219_2.unpaired.fastq ILLUMINACLIP:$EBROOTTRIMMOMATIC/adapters/TruSeq3-PE.fa:2:30:10:2:True LEADING:3 TRAILING:3 MINLEN:36
```
### Activity
- Look at the manual for trimmomatic and figure out what the arguments in the trimmomatic call mean
- If you wanted to make your filtering more stringent, what arguments would you change?
- What is TruSeq3-PE? Would you use this for every library?


  ***

  We can look at the 




