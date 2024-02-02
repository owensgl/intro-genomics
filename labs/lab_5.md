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
- How many reads were removed by the filtering? Can you calculate that just by comparing the files?


***

We also need a reference genome to compare our sample with. Reference genomes are often available 
on the NCBI website. Here's a link to the yeast (S. cerevisiae) reference genome: https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_000146045.2/

### Activity
- Using the NCBI website, find the GPX1 gene in the yeast reference genome. Where is it? What is the nucleotide sequence of the gene? What is the function of the gene?
- What are the genes next to GPX1 in the genome?
- Using BLAST, find where this gene is in the genome:
```bash
>Unknown gene
ATGGTCAAATTAACTTCAATCGCCGCTGGTGTCGCTGCCATCGCTGCTACTGCTTCTGCAACCACCACTC
TAGCTCAATCTGACGAAAGAGTCAACTTGGTGGAATTGGGTGTCTACGTCTCTGATATCAGAGCTCACTT
AGCCCAATACTACATGTTCCAAGCCGCCCACCCAACTGAAACCTACCCAGTCGAAGTTGCTGAAGCCGTT
TTCAACTACGGTGACTTCACCACCATGTTGACCGGTATTGCTCCAGACCAAGTGACCAGAATGATCACCG
GTGTTCCATGGTACTCCAGCAGATTAAAGCCAGCCATCTCCAGTGCTCTATCCAAGGACGGTATCTACAC
TATCGCAAACTAG
```

***

To work with the yeast reference genome, we have to download it to the server. 
Go back to the first yeast genome page and click on the "Curl" button. It will show you a URL, which you should copy. 
We can use that URL and the "curl" command to download the reference genome files

```bash
curl -o yeast_genome.zip 'https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000146045.2/download?include_annotation_type=GENOME_FASTA,GENOME_GFF,RNA_FASTA,CDS_FASTA,PROT_FASTA,SEQUENCE_REPORT'

unzip yeast_genome.zip

```

Now lets take a look at what we got.

```bash
cd ncbi_dataset/data/GCF_000146045.2
ls
```

```output
cds_from_genomic.fna  GCF_000146045.2_R64_genomic.fna  genomic.gff  protein.faa  rna.fna  sequence_report.jsonl
```

First lets talk about "GCF_000146045.2_R64_genomic.fna". This is the genome sequence in fasta format. 
It uses a .fna file suffix because it's a fasta with only nucleotide information (and not amino acids, for example). 

```bash
head GCF_000146045.2_R64_genomic.fna
```
```output
>NC_001133.9 Saccharomyces cerevisiae S288C chromosome I, complete sequence
ccacaccacacccacacacccacacaccacaccacacaccacaccacacccacacacacacatCCTAACACTACCCTAAC
ACAGCCCTAATCTAACCCTGGCCAACCTGTCTCTCAACTTACCCTCCATTACCCTGCCTCCACTCGTTACCCTGTCCCAT
TCAACCATACCACTCCGAACCACCATCCATCCCTCTACTTACTACCACTCACCCACCGTTACCCTCCAATTACCCATATC
CAACCCACTGCCACTTACCCTACCATTACCCTACCATCCACCATGACCTACTCACCATACTGTTCTTCTACCCACCATAT
TGAAACGCTAACAAATGATCGTAAATAACACACACGTGCTTACCCTACCACTTTATACCACCACCACATGCCATACTCAC
CCTCACTTGTATACTGATTTTACGTACGCACACGGATGCTACAGTATATACCATCTCAAACTTACCCTACTCTCAGATTC
CACTTCACTCCATGGCCCATCTCTCACTGAATCAGTACCAAATGCACTCACATCATTATGCACGGCACTTGCCTCAGCGG
TCTATACCCTGTGCCATTTACCCATAACGCCCATCATTATCCACATTTTGATATCTATATCTCATTCGGCGGTcccaaat
attgtataaCTGCCCTTAATACATACGTTATACCACTTTTGCACCATATACTTACCACTCCATTTATATACACTTATGTC
```

Each chromosome (or contig) will be represented by a different fasta entry. 
You can see that this has an ID for the first chromosome "NC_001133.9" as well
as a description of that entry. Some of the bases are lowercase, while others are
uppercase. The lowercase bases have been identified as being repetitive and are
what we call "soft-masked". Some programs will ignore soft-masked regions of the genome.

We can manipulate the reference fasta file using samtools. 
```bash
#First we load the samtools module
module load samtools/1.18

#Then we need to index the fasta file.
samtools faidx GCF_000146045.2_R64_genomic.fna

#This creates the index file "GCF_000146045.2_R64_genomic.fna.fai". 
#Sometimes index files are only machine-readable, but in this case the index file is actually helpful
head GCF_000146045.2_R64_genomic.fna.fai
```
```output
NC_001133.9	230218	76	80	81
NC_001134.8	813184	233249	80	81
NC_001135.5	316620	1056676	80	81
NC_001136.10	1531933	1377332	80	81
NC_001137.3	576874	2928491	80	81
NC_001138.5	270161	3512653	80	81
NC_001139.9	1090940	3786270	80	81
NC_001140.6	562643	4890926	80	81
NC_001141.2	439888	5460680	80	81
NC_001142.9	745751	5906143	80	81
```
This shows all of the contigs in the fasta file (first column) and the second column shows the number
of basepairs in each contig. 

### Activity
- Sometimes you will want to extract the sequence from a specific region of the genome. 
You can do this using samtools faidx. Figure out how to output the region NC_001135.5:10030-10130.
- Other times, you'll want to work on only a subset of chromosomes. Find the two longest chromosomes 
and output only those two chromosomes to a separate fasta file.

***

The directory also contains information on the annotated genes in the genome. Specifically:
1) "cds_from_genomic.fna". This is the coding sequence for all genes
2) "rna.fna". This is the RNA sequence for all genes
3) "protein.faa" This is the translated amino acid sequence for all genes.
4) "genomic.gff" This includes the locations and descriptions of all features in the genome (not only genes)

The cds and rna are very similar, but the cds only includes bases that code for amino acids while
the rna includes transcribed bases upstream of the start codon and downstream of the stop codon. 

Some genome assemblies have additional information files and you can find their definitions here: https://www.ncbi.nlm.nih.gov/genome/doc/ftpfaq/

Lets take a look a the gff file.

```bash
#We're piping it to `less -S` to prevent line wrapping, because the lines on this
file can be quite long
head -n 20 genomic.gff | less -S
```
```output
##gff-version 3
#!gff-spec-version 1.21
#!processor NCBI annotwriter
#!genome-build R64
#!genome-build-accession NCBI_Assembly:GCF_000146045.2
#!annotation-source SGD R64-4-1
##sequence-region NC_001133.9 1 230218
##species https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=559292
NC_001133.9     RefSeq  region  1       230218  .       +       .       ID=NC_001133.9:1..230218;Dbxref=taxon:559292;Na>
NC_001133.9     RefSeq  telomere        1       801     .       -       .       ID=id-NC_001133.9:1..801;Dbxref=SGD:S00>
NC_001133.9     RefSeq  origin_of_replication   707     776     .       +       .       ID=id-NC_001133.9:707..776;Dbxr>
NC_001133.9     RefSeq  gene    1807    2169    .       -       .       ID=gene-YAL068C;Dbxref=GeneID:851229;Name=PAU8;>
NC_001133.9     RefSeq  mRNA    1807    2169    .       -       .       ID=rna-NM_001180043.1;Parent=gene-YAL068C;Dbxre>
NC_001133.9     RefSeq  exon    1807    2169    .       -       .       ID=exon-NM_001180043.1-1;Parent=rna-NM_00118004>
NC_001133.9     RefSeq  CDS     1807    2169    .       -       0       ID=cds-NP_009332.1;Parent=rna-NM_001180043.1;Db>
NC_001133.9     RefSeq  gene    2480    2707    .       +       .       ID=gene-YAL067W-A;Dbxref=GeneID:1466426;Name=YA>
NC_001133.9     RefSeq  mRNA    2480    2707    .       +       .       ID=rna-NM_001184582.1;Parent=gene-YAL067W-A;Dbx>
NC_001133.9     RefSeq  exon    2480    2707    .       +       .       ID=exon-NM_001184582.1-1;Parent=rna-NM_00118458>
NC_001133.9     RefSeq  CDS     2480    2707    .       +       0       ID=cds-NP_878038.1;Parent=rna-NM_001184582.1;Db>
NC_001133.9     RefSeq  gene    7235    9016    .       -       .       ID=gene-YAL067C;Dbxref=GeneID:851230;Name=SEO1;>
```

The start of the gff is a header, marked by the "#". It tells you which specific gff version you have and where it came from.
The first few columns tell us:
1) Chromosome or contig
2) Source of the feature
3) What type of feature
4) The start of the feature
5) The end of the feature
6) The score for the feature (in this case blank)
7) The strand the feature is on
8) The phase of the feature (in this case, what codon frame the CDS is in)
9) Additional info
You can read more about how gff format works here: https://github.com/The-Sequence-Ontology/Specifications/blob/master/gff3.md

In many cases, you'll want to know which features are in a particular part of the genome. Since a gff file is 
actually just text, we can use some basic unix tools to filter it. A convenient option is "awk".

```bash
awk '$1 == "NC_001133.9" && $4 >= 10290 && $5 <= 27968' genomic.gff
```
This filter for features on chromosome NC_001133.9 that start after 10290 and end before 27968. 

### Activity
- Find all the features on NC_001148.4 from 1 to 10000 bp.
- Find all the origin of replications on NC_001133.9. How many are there?
- The previous awk command can sometimes miss features that overlap with your window. Here's an
improved version of the command
```bash
chromosome="NC_001133.9"
start_position=10290
end_position=27968

awk -v chrom="$chromosome" -v start="$start_position" -v end="$end_position" \
  '($1 == chrom && ($4 <= end && $5 >= start)) || \
    ($1 == chrom && $4 >= start && $4 <= end) || \
    ($1 == chrom && $5 >= start && $5 <= end) || \
    ($1 == chrom && $4 <= start && $5 >= end)' genomic.gff
```
Generally speak, which features would this command capture that the previous command would miss?

***







