### Introduction
\# Here are some basic commnad you could follow to perform the iterative hybrid assembly/binning method

\# If you have too much long/short reads, please subsampling the dataset, this strategy, on one side, will short the running time, on the other side, lower the complexity of the system to some degree.

#### Step 01: Data preparation
\# Subsampling datasets using seqkit; if not too much reads, you might skip this step
```
cat Raw_1.fastq | seqkit sample -s 11 -n 20000000 > Raw_sub_1.fastq
cat Raw_2.fastq | seqkit sample -s 11 -n 20000000 > Raw_sub_2.fastq
cat Raw-lr.fastq | seqkit sample -s 11 -n XXXXXXXX > Raw_sub-lr.fastq
```
\# Also, please prepare the all seq ID from the long reads and sort, named LRs.ID.sorted

#### Step 02: Hybrid assembly using Unicycler
```
unicycler-runner.py --no_correct -1 Raw_sub_1.fastq -2 Raw_sub_2.fastq -l Raw_sub-lr.fastq -t 40 -o H1-unicycler --min_fasta_length 1000
```
#### Step 03: Binning using metaWRAP, other binners could also be options
```
metawrap binning -o INITIAL_BINNING -t 40 -a H1-unicycler/assembly.fasta --metabat2 --maxbin2 Raw_sub_*fastq
metawrap bin_refinement -o BIN_REFINEMENT -c 50 -x 10 -t 40 -A INITIAL_BINNING/metabat2_bins/ -B INITIAL_BINNING/maxbin2_bins
```
#### Step 04: Qualified MAGs selection
\# High-quality and High-contiguity MAGs are recommented to be collected. e.g., completeness > 90% && contamination < 5% && contig count <=5
\# cat all the qualified MAGs as one fasta file, e.g., named H1-qualified.fasta

* #### After collection of the qualified MAGs. Data preparation in the next cycle should be performed
#### Step 01: Data preparation
\# Long reads mapping and filtration
```
minimap2 -x map-ont -t 40 H1-qualified.fasta Raw_sub-lr.fastq > H1-qualified-mapping.lr.paf
awk -F'[\t:]' '($4-$3+1)/$2 >=0.80 && $27<0.20 {print $1}' *.lr.paf > filtered_80_80-lr.ID
cat filtered_80_80-lr.ID | sort > filtered_80_80-lr.ID.sorted ## use --parallel=XX to short the running time if many sequences generated
comm -13 filtered_80_80-lr.ID.sorted LRs.ID.sorted > H2-LRs.ID
seqtk subseq Raw-lr.fastq H2-LRs.ID > H2-LRs.fastq
```
\# Short reads mapping and output unaligned paired reads
```
bowtie2-build H1-qualified.fasta H1-qualified.fasta --threds 20
bowtie2 -x H1-qualified.fasta -1 Raw_1.fastq -2 Raw_2.fastq -p 20 --very-sensitive -S H1.tmp.sam --un-conc H2-pairs.fastq && rm *sam
```
* #### then follow the same Step 02 - Step 04 to reconstruct more (near-) complete genomes from the ecosystem.
[Back to Top](#Introduction)

