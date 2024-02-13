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
The data files we need 


#Download some data
#Download a reference genome
#Index your reference genome
#Align data
#Look around bam file
#Sort bam file.
#Extract single region of bam file
#Remove PCR duplicates. 
