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

###

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

###


```bash
module load StdEnv/2023 gcc/12.3 sra-toolkit/3.0.9



