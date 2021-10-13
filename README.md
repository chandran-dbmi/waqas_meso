# Mesothelioma

The mesothelioma project consists of both DNA-Seq and RNA-Seq data. The data was sequenced and analyzed by UGC.

## 1. Data
The data for this project can be found on Bridges2. The data folders for DNA-Seq and RNA-Seq are available in the following folders:
1. DNA-Seq: `/ocean/projects/bio140004p/achakka/Variant_Calling/Waqas/Meso_2021`
2. RNA-Seq: `/ocean/projects/bio140004p/achakka/RNA-Seq/Waqas/`

### 1.1. DNA-Seq files summary

| File | Number of files| File type|
| -------- | -------- | -------- |
| Fastq     |  80   |  .fastq    |
| BAMs     |  40   |   .bam   |
| Somatic BAMs     |  18   |   .bam   |
| Germline VCFs (HC)    |  40   |   .vcf   |
| Somatic VCFs     |  18   |   .vcf   |
| Germline MAFs (HC)    |  40   |   .maf   |
| Somatic MAFs     |  18   |   .maf   |
| Tumor purity (BEST.results) | 19 | text file |
| CNV Allelic counts    |  40   |   .tsv   |
| CNV Denoised CR  |  40   |   .tsv   |
| CNV Modeled    |  500   |   .png   |
| CNV Allelic counts    |  20   |   .vcf  |

### 1.2. RNA-Seq files summary
| File | Number of files| File type|
| -------- | -------- | -------- |
| Raw Fastq     |  64   |  .fastq    |
| Trim Fastq     |  64   |  .fastq    |
| BAMs     |  32   |   .bam   |
| Splice Junctions     |  32   |   .tsv   |
| Counts | 32 | .ct.short.txt |

## 2. Methods
The methods as provided by UGC for DNA-Seq and RNA-Seq analysis is below:

### 2.1. DNA-Seq Methods
#### 2.1.1. Material and Methods
The samples were processed following standard Whole Genome Sequencing (WGS) pipeline in UPMC Genome Center (UGC). Genomic DNA was isolated from tumor tissue or PBMC on the automated Chemagic 360 (Perkin Elmer) instrument according to the manufacturer’s instructions. Extracted DNA was quantitated using Qubit™ dsDNA BR Assay Kit (Thermo Fisher Scientific). DNA libraries were prepared using the KAPA Hyper Plus Kit (KAPA Biosystems). 500 ng of genomic DNA was processed through fragmentation, enzymatic end-repair and A-tailing, ligation, followed by quality check using Fragment Analyzer (AATI). Libraries with an average size of 450 bp (range: 300-600bp) were quantified by qPCR on the LightCycler 480 (Roche) using the KAPA qPCR quantification kit (KAPA biosystem). The libraries were normalized and pooled as per manufacturer protocol (Illumina). Sequencing was performed using NovaSeq 6000 platform (Illumina) with 151 paired-end reads to an average target depth of 30X for germline and 70X coverage for somatic samples.

#### 2.1.2. Bioinformatic Analysis
The samples were mapped with Sentieon v1.3.4 (sentieon-genomics-201808.08). The germline variants also called by Sentieon Haplotyper. Somatic variants were called by TNhaplotyper2 at tumor-normal mode with best-practice recommended WGS setting. We annotated the variants with VEP(v102) and converted to MAF format with vcf2maf and annotated. All computation process are carried on linux based AWS ec2 instances on DNAnexus platform.
We called the CNV with an in-housed developed ensemble method (CNVsenate) with mapped bam and somatic SNV vcf. The CNVsenate gathers calling results from GATK(4.0.5), CNVkit(0.9.5), CNVnator(0.2.7), Manta(1.3.2), Sentieon CNV(sentieon-genomics-201911) and combine calling with SURVIOR2(1.0.3), then used Machine Learning (trained with 1000G samples) and event size for filtering. The filtered results were annotated with ANNOSV(1.1.1) for affected genes. 
The SNV and CNV results were used for estimating the tumor purity with THetA2 (v0.7).

### 2.2. RNA-Seq Methods
#### 2.2.1. Material and Methods
Extracted RNA were quantitated Qubit™ RNA BR Assay Kit (Thermo Fisher Scientific) followed by the RNA quality check using Fragment Analyzer (Agilent). For each sample, RNA libraries were prepared from 500ng RNA using the KAPA mRNA HyperPrep Kit (Kapa Biosystems) according to manufacturer’s protocol, followed by quality check using Fragment Analyzer (Agilent) and quantification by qPCR (Kapa qPCR quantification kit, Kapa biosystem). The libraries were normalized and pooled, and then sequenced using NovaSeq6000 platform (Illumina) to an average of 50M 101PE reads.

#### 2.2.2. Bioinformatic Analysis
The fastq files are pre-processed and the sequence quality is observed using FastqQC and adapters are removed with TrimGalore. The paired-end reads are mapped to the reference genome using STAR aligner (Spliced Transcripts Alignment to a Reference) and this tool writes several output files such as the binary version of the sequence alignment files, high confidence splice junctions and unmapped reads in fasta file format. The RNA metrics is the log file by qualimap and samtools that provides with the total reads mapped, percent reads mapping to exon/intron/intergenic region. The read quantification of RNAseq data is done by Subread FeatureCount. Each read is mapped to a genomic feature. This tool has a read summarization program that counts mapped reads for genomic features such as genes, exon etc. The feature count files provide genes and the corresponding read count against the feature along with the co-ordinates.








