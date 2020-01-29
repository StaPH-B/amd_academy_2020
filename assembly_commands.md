# Assembly Commands
This document outlines the commands used during the assembly exercise.

## Read Mapping
In this section we will use bwa and minimap2 to map reads to a couple reference genomes. Then we will evaluate the read mapping.

#### Mapping with BWA
* First we have to index the reference sequence  
`staphb_toolkit bwa index <reference_sequence>`  

* Then we can map the reads to the reference sequence  
`staphb_toolkit bwa mem <reference_sequence> <read1> <read2> -o genome_mapping.sam`  

* Once the reads are mapped we need to convert it to a bam to look at the mapping statistics
`staphb_toolkit samtools view -b genome_mapping.sam -o genome_mapping.bam`  

* Then we need to sort the bam file  
`staphb_toolkit samtools sort genome_mapping.bam -o genome_mapping_sorted.bam`

#### Mapping Quality
* Let's first get the average depth  
`staphb_toolkit samtools depth genome_mapping_sorted.bam | awk '{sum+=$3} END { print "Average = ",sum/NR}'`

* Now let's get some more information about the read mapping  
`staphb_toolkit samtools flagstats genome_mapping_sorted.bam`



## Assembling with Spades
To see the effects of kmer size on genome assembly we will assemble the raw reads using Spades while specifying the kmer size. We do this by supplying the kmer size using the `-k` flag.

#### 17-mer Assembly
`staphb_toolkit spades -k 17 -1 <read1> -2 <read2> -o k17_assembly`

#### 99-mer Assembly
`staphb_toolkit spades -k 99 -1 <read1> -2 <read2> -o k99_assembly`

#### Normal Assembly
`staphb_toolkit spades -1 <read1> -2 <read2> -o spades_assembly`

## Checking Assembly Quality
To generate the assembly quality statistics you can use the program QUAST with the following command:  
`staphb_toolkit quast <assembly.fasta> -o quast_results`

You can then view the results in the terminal using the following command:  
`cat quast_results/report.txt`
