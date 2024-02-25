---
title: TMP
element: lab
layout: default
---

## Objectives

- To learn how to call variants using bcftools and freebayes.
- To read a VCF file.
- To filter a VCF file.
- To visualize a VCF file.

****

When we are resequencing a species with a reference genome, the ultimate goal is to 
detect where your new sample differs from the reference genome and how multiple
samples differ from each other. To do this, we are going to use the aligned sequence
data to call variants. 

First thing, log into the server and remember to source your bash.sh file so that you have access to the modules.

I've supplied you with aligned bam files and a reference genomes on the server at "/project/ctb-grego/sharing/lab_7". 

```bash
ls -R /project/ctb-grego/sharing/lab_7
```
```output
bam  reference

/project/ctb-grego/sharing/lab_7/bam:
sample_0.bam      sample_2.bam.bai  sample_5.bam      sample_7.bam.bai
sample_0.bam.bai  sample_3.bam      sample_5.bam.bai  sample_8.bam
sample_1.bam      sample_3.bam.bai  sample_6.bam      sample_8.bam.bai
sample_1.bam.bai  sample_4.bam      sample_6.bam.bai  sample_9.bam
sample_2.bam      sample_4.bam.bai  sample_7.bam      sample_9.bam.bai

/project/ctb-grego/sharing/lab_7/reference:
SalmonReference.fasta
```
We could copy those files into your home directory, but that would mean there will be a different
copy for every student. When files are very large or there are many users that can be a problem. 
Instead, we're going to create a symbolic link. This looks like a file on your system, and you can use it like a file,
but the orginal data stays in one spot and isn't copied. 

```bash
#Make a directory for the new bam file links to go
mkdir lab_7/bam
cd lab_7/bam

#Link all the bam files
ln -s /project/ctb-grego/sharing/lab_7/bam/* ./

#Take a look and see if it worked.
ls
```

```output
sample_0.bam      sample_2.bam.bai  sample_5.bam      sample_7.bam.bai
sample_0.bam.bai  sample_3.bam      sample_5.bam.bai  sample_8.bam
sample_1.bam      sample_3.bam.bai  sample_6.bam      sample_8.bam.bai
sample_1.bam.bai  sample_4.bam      sample_6.bam.bai  sample_9.bam
sample_2.bam      sample_4.bam.bai  sample_7.bam      sample_9.bam.bai
```

Now lets do the same for the reference genome
```bash
mkdir ../reference
cd ../reference
ln -s /project/ctb-grego/sharing/lab_7/reference/* ./
```





