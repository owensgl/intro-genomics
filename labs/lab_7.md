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

We're first going to use bcftools mpileup to call genotypes. We need to create a file that tells bcftools
where all the bam files are and what their names are. We can do that using shell commands.
```bash
cd ~/lab_7
ls bam | grep -v ".bai" | sed 's/^/bam\//g' > bamlist.txt
```
### Question
1. Break down the command used to create bamlist.txt and explain what step is doing.


Now we're going to call the first step, bcftools mpileup.

```bash
module load StdEnv/2023  gcc/12.3 bcftools/1.18

#We're using the -q 20 option here to require that reads have mapq >= 20.
bcftools mpileup -q 20 -f reference/SalmonReference.fasta -b bamlist.txt  > lab_7.gvcf
```

The output of this is like a vcf file but it includes information on all sites in the genome,
but hasn't actually called genotypes yet. This is an intermediate file format before we create
the final vcf. Lets take a look.

```bash
less lab_7.gvcf
```
```output
##FILTER=<ID=PASS,Description="All filters passed">
##bcftoolsVersion=1.18+htslib-1.18
##bcftoolsCommand=mpileup -q 20 -f reference/SalmonReference.fasta -b bamlist.txt
##reference=file://reference/SalmonReference.fasta
##contig=<ID=chr_1,length=5000000>
##contig=<ID=chr_2,length=5000000>
##ALT=<ID=*,Description="Represents allele(s) other than observed.">
##INFO=<ID=INDEL,Number=0,Type=Flag,Description="Indicates that the variant is an INDEL.">
##INFO=<ID=IDV,Number=1,Type=Integer,Description="Maximum number of raw reads supporting an indel">
##INFO=<ID=IMF,Number=1,Type=Float,Description="Maximum fraction of raw reads supporting an indel">
##INFO=<ID=DP,Number=1,Type=Integer,Description="Raw read depth">
##INFO=<ID=VDB,Number=1,Type=Float,Description="Variant Distance Bias for filtering splice-site artefacts in RNA-seq data (bigger is better)",Version="3">
##INFO=<ID=RPBZ,Number=1,Type=Float,Description="Mann-Whitney U-z test of Read Position Bias (closer to 0 is better)">
##INFO=<ID=MQBZ,Number=1,Type=Float,Description="Mann-Whitney U-z test of Mapping Quality Bias (closer to 0 is better)">
##INFO=<ID=BQBZ,Number=1,Type=Float,Description="Mann-Whitney U-z test of Base Quality Bias (closer to 0 is better)">
##INFO=<ID=MQSBZ,Number=1,Type=Float,Description="Mann-Whitney U-z test of Mapping Quality vs Strand Bias (closer to 0 is better)">
##INFO=<ID=SCBZ,Number=1,Type=Float,Description="Mann-Whitney U-z test of Soft-Clip Length Bias (closer to 0 is better)">
##INFO=<ID=SGB,Number=1,Type=Float,Description="Segregation based metric, http://samtools.github.io/bcftools/rd-SegBias.pdf">
##INFO=<ID=MQ0F,Number=1,Type=Float,Description="Fraction of MQ0 reads (smaller is better)">
##INFO=<ID=I16,Number=16,Type=Float,Description="Auxiliary tag used for calling, see description of bcf_callret1_t in bam2bcf.h">
##INFO=<ID=QS,Number=R,Type=Float,Description="Auxiliary tag used for calling">
##FORMAT=<ID=PL,Number=G,Type=Integer,Description="List of Phred-scaled genotype likelihoods">
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  bam/sample_0.bam        bam/sample_1.bam        bam/sample_2.bam        bam/sample_3.bam        ba>
chr_1   1       .       T       <*>     0       .       DP=1;I16=1,0,0,0,33,1089,0,0,60,3600,0,0,0,0,0,0;QS=1,0;MQ0F=0  PL      0,0,0   0,0,0   0,0,0   0,3,33  0,0,0   0,>
chr_1   2       .       G       <*>     0       .       DP=1;I16=1,0,0,0,14,196,0,0,60,3600,0,0,1,1,0,0;QS=1,0;MQ0F=0   PL      0,0,0   0,0,0   0,0,0   0,3,14  0,0,0   0,>
chr_1   3       .       G       <*>     0       .       DP=1;I16=1,0,0,0,33,1089,0,0,60,3600,0,0,2,4,0,0;QS=1,0;MQ0F=0  PL      0,0,0   0,0,0   0,0,0   0,3,33  0,0,0   0,>
chr_1   4       .       G       <*>     0       .       DP=1;I16=1,0,0,0,33,1089,0,0,60,3600,0,0,3,9,0,0;QS=1,0;MQ0F=0  PL      0,0,0   0,0,0   0,0,0   0,3,33  0,0,0   0,>
chr_1   5       .       C       <*>     0       .       DP=1;I16=1,0,0,0,33,1089,0,0,60,3600,0,0,4,16,0,0;QS=1,0;MQ0F=0 PL      0,0,0   0,0,0   0,0,0   0,3,33  0,0,0   0,>
chr_1   6       .       A       <*>     0       .       DP=1;I16=1,0,0,0,37,1369,0,0,60,3600,0,0,5,25,0,0;QS=1,0;MQ0F=0 PL      0,0,0   0,0,0   0,0,0   0,3,37  0,0,0   0,>
chr_1   7       .       A       <*>     0       .       DP=1;I16=1,0,0,0,37,1369,0,0,60,3600,0,0,6,36,0,0;QS=1,0;MQ0F=0 PL      0,0,0   0,0,0   0,0,0   0,3,37  0,0,0   0,>
chr_1   8       .       G       <*>     0       .       DP=1;I16=1,0,0,0,37,1369,0,0,60,3600,0,0,7,49,0,0;QS=1,0;MQ0F=0 PL      0,0,0   0,0,0   0,0,0   0,3,37  0,0,0   0,>
chr_1   9       .       G       <*>     0       .       DP=1;I16=1,0,0,0,37,1369,0,0,60,3600,0,0,8,64,0,0;QS=1,0;MQ0F=0 PL      0,0,0   0,0,0   0,0,0   0,3,37  0,0,0   0,>
chr_1   10      .       C       <*>     0       .       DP=1;I16=1,0,0,0,37,1369,0,0,60,3600,0,0,9,81,0,0;QS=1,0;MQ0F=0 PL      0,0,0   0,0,0   0,0,0   0,3,37  0,0,0   0,>
```

The file has a header denotated by the ## at the start of each line. This tells you what the abbreviations
used in the file mean. Then there's the line that starts with #CHROM, which is the column names for the rest
of the file. We have 9 information columns and then 1 column per sample. Each row after that is for a single
position in the genome. The most important parts of this file are:
- The first column, which is the chromosome.
- The second column, which is the position on that chromosome.
- The fourth column, which is the reference allele (i.e. the allele in the reference genome).
- The fifth column, which is the alternate allele(s). If there is no alternate allele, it is
labeled as <*>. 
- The DP value in the info column. This tells you hany reads there is total for this site.
- The 10th column onward shows the genotype likelihoods for each sample. The three values, separated
by ",", are the phred scaled likelihoods for 0/0, 0/1 and 1/1 respectively. A 0 phred scaled likelihood,
is the most likely. Higher values are less likely. Ultimately, the program is going to pick a single
genotype value based on comparing these likelihoods.





bcftools mpileup -q 20 -d 200 -f Sockeye/ReferenceGenome/SalmonReference.fasta -b bamlist.txt | bcftools call -mv > Biol470.vcf



