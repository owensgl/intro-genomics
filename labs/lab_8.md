---
title: test_8
element: lab
layout: default
---

## Objectives

- To run a population structure analysis
- To run a PCA on a vcf file.
- To visualize the results of a PCA and population structure
- To calculate FST on a vcf file.
- To plot FST across a genome.


****

Today we're going to explore how to measure relatedness or genetic similarity between samples
using a vcf file. Log into the indri server and remember to source your bash.sh file 
so that you can access the module system. First we're going to make some directories and
copy in an example vcf file. 

```bash
cd ~
mkdir lab_8
cd lab_8
mkdir vcf
cp /project/ctb-grego/sharing/lab_8/chinook.g5.m2.c5.d5.20miss.subsample.vcf.gz vcf/
```

This contains a thinned set of SNPs for some Chinook salmon. 

## Question
- How many samples are in the vcf file? How many SNPs?
- Extract a list of the sample names in the file. 
HINT: Sample names are in the header line starting with #CHROM. 
HINT: You can convert tabs to newlines using the 'tr' command

We want to group these samples into populations, so we're going to use the program "admixture".
To use it, we need to have our data in "bed" format. Bed is the binary version of ped format,
which basically organizes the data into a table with genotype calls. Bed format also has
accessory files "fam" and "bim" which have information about family structure and SNP identity.
We're going to plink to convert the vcf to bed. Plink is a very large set of tools that do many
tasks in population genetics, although it's largely designed for humans. This means it expects
human chromosome labels, and you have to specify when its not. 

```bash
module load plink
plink --vcf chinook.g5.m2.c5.d5.20miss.subsample.vcf.gz \
  --out chinook.g5.m2.c5.d5.20miss.subsample \
  --make-bed \
  --allow-extra-chr \
  --double-id
ls
```
The options for this include:
"--out" which specifies the prefix of the outfile.
"--make-bed" which tells it to make a bed file output.
"--allow-extra-chr" which allows us to use non-human chromosome names.
"--double-id" which prevents it from interpreting the "_" in the file name as indicating familial relationships.

Take a look at the files its made, you should see a bed, fam, log, nosex and bim file.
The next step is to run admixture. We have to give it our bed file and also a k value.
The k value is the number of groups that admixture will try to place our individuals into. 
We can start with k=1, as a baseline.

```bash
module load admixture
admixture chinook.g5.m2.c5.d5.20miss.subsample.bed 1
```

```output
[grego@indri vcf]$ admixture chinook.g5.m2.c5.d5.20miss.subsample.bed 1
****                   ADMIXTURE Version 1.3.0                  ****
****                    Copyright 2008-2015                     ****
****           David Alexander, Suyash Shringarpure,            ****
****                John  Novembre, Ken Lange                   ****
****                                                            ****
****                 Please cite our paper!                     ****
****   Information at www.genetics.ucla.edu/software/admixture  ****

Random seed: 43
Point estimation method: Block relaxation algorithm
Convergence acceleration algorithm: QuasiNewton, 3 secant conditions
Point estimation will terminate when objective function delta < 0.0001
Estimation of standard errors disabled; will compute point estimates only.
Invalid chromosome code!  Use integers.
```
Uh oh, invalid chromosome code? This is another relic of programs being design for
human data. It expects that the chromosome names should be "1" instead of an arbitrary
series of letters, which is what ours are. The solution to this is to rename our chromosomes to 
numbers. I don't know of a tool to do that, so I wrote one for the class.

```bash
zcat chinook.g5.m2.c5.d5.20miss.subsample.vcf.gz | \
  perl /project/ctb-grego/sharing/vcf2numericchrs.pl \
  chinook.g5.m2.c5.d5.20miss.subsample.keyfile.txt > chinook.g5.m2.c5.d5.20miss.subsample.nchrs.vcf
```
Now we've created a new vcf, where each chromososome is a number, and 
a keyfile for later if we want to convert them back into the original names 
(chinook.g5.m2.c5.d5.20miss.subsample.keyfile.txt). Lets repeat the same plink steps and
try admixture again.

```bash
plink --vcf chinook.g5.m2.c5.d5.20miss.subsample.nchrs.vcf \
  --out chinook.g5.m2.c5.d5.20miss.subsample.nchrs \
  --make-bed \
  --allow-extra-chr \
  --double-id \
  --autosome-num 95
```
You can see, we've added an extra flag here "--autosome-num 95". Now that we have
converted the chromosomes to numbers, it expects that there should be a max of 26. 
We're putting in 95 here, not because salmon have 95 chromosomes, but because its the max value
and we just want it to process this without giving more complaints. Since we're just converting
the file, it doesn't really matter how many chromosomes there are.

```bash
admixture chinook.g5.m2.c5.d5.20miss.subsample.nchrs.bed 1
```
```output
****                   ADMIXTURE Version 1.3.0                  ****
****                    Copyright 2008-2015                     ****
****           David Alexander, Suyash Shringarpure,            ****
****                John  Novembre, Ken Lange                   ****
****                                                            ****
****                 Please cite our paper!                     ****
****   Information at www.genetics.ucla.edu/software/admixture  ****

Random seed: 43
Point estimation method: Block relaxation algorithm
Convergence acceleration algorithm: QuasiNewton, 3 secant conditions
Point estimation will terminate when objective function delta < 0.0001
Estimation of standard errors disabled; will compute point estimates only.
Size of G: 61x14346
Performing five EM steps to prime main algorithm
1 (EM)  Elapsed: 0.015  Loglikelihood: -712273  (delta): 930843
2 (EM)  Elapsed: 0.015  Loglikelihood: -712273  (delta): 0
3 (EM)  Elapsed: 0.015  Loglikelihood: -712273  (delta): 0
4 (EM)  Elapsed: 0.015  Loglikelihood: -712273  (delta): 0
5 (EM)  Elapsed: 0.015  Loglikelihood: -712273  (delta): 0
Initial loglikelihood: -712273
Starting main algorithm
1 (QN/Block)    Elapsed: 0.052  Loglikelihood: -712273  (delta): 0
Summary:
Converged in 1 iterations (0.184 sec)
Loglikelihood: -712272.711731
Writing output files.
````





