# Comparative_genomics_A_niger
Comparative genomics of a 40 year old lineage of Aspergillus niger. The goal of this project is a SNP and overall genome analysis of _Aspergillus niger_ mutants by using Illumina and Nanopore seqeuncing data. Besides using own sequenced data publicly available data is used as well. 

![image](https://github.com/SvenKlomp/Comparative_genomics_A_niger/assets/127952914/ffac3bea-0bc8-44e9-bd31-952a9d21c531)

All tools except Trimmomatic were installed with Bioconda.

# Analysis

Step 1: 

We have done an initial quality controll of our Nanopore data with NanoPlot.
The usage is: `NanoPlot --fastq input_data.fastq -o nanoplot/input_data/`

Step 2:

We have filtered the Nanopore data with NanoFilt.
The usage is: `NanoFilt -q 15 -l 1000 input_data.fastq > nanofilt/output_data_after_filtering.fastq`

Step 3: 

We have done a quality controll of our filtered Nanopore data with NanoPlot.
The usage is: `NanoPlot --fastq nanofilt/output_data_after_filtering.fastq -o nanoplot/after_filtering_input_data/`

Step 4: 

We have done an intial quality controll of our Illumina data with Fastqc.
The usage is: `fastqc input_data_R1.fastq.gz input_data_R2.fastq.gz -o fastqc/input_data/` 

Step 5: 

The output of the Fastqc is pooled using multiqc.
The usage is: `multiqc input_data_R1_fastqc.html input_data_R2_fastqc.html -o multiqc/`

Step 6: 

We have trimmed the Illumnia reads with Trimmomatic.
The usage is: `java -jar trimmomatic-0.39.jar PE -threads 8 -phred33 input_data_R1.fastq.gz input_data_R2.fastq.gz trimmomatic_out/R1_paired.fastq.gz trimmomatic_out/R1_failed.fastq.gz trimmomatic_out/R2_paired.fastq.gz trimmomatic_out/R2_failed.fastq.gz LEADING:5 TRAILING:5 SLIDINGWINDOW:4:25 MINLEN:50`

Repeat step 4 and 5 for the trimmed reads

Step 7:

We have assembled the reads with Flye.
The usage is: `flye -t 8 -g 36m --nano-raw nanofilt/output_data_after_filtering.fastq trimmomatic_out/R1_paired.fastq.gz trimmomatic_out/R2_paired.fastq.gz -o flye/assembly/`

Step 8:

We polished the long read assemlby with the paired-end short reads using Polypolish. 
First the short reads need to be aligned to the draft assemlby with BWA: 

`bwa index assembly.fasta`

`bwa mem -a assembly.fasta reads_1.fastq.gz > alignments_1.sam`

`bwa mem -a assembly.fasta reads_1.fastq.gz > alignments_1.sam`

Then the alignments were filtered using the Polypolish's insert size filter script: `polypolish_insert_filter.py --in1 alignments_1.sam --in2 alignments_2.sam --out1 filtered_1.sam --out2 filtered_2.sam`
Finally the polishing was done: `polypolish draft.fasta filtered_1.sam filtered_2.sam > polished.fasta`

Step 9: 

We have assesed the initial quality of the assembly using Quast.
The usage is: `quast -t 6 -e --fungus -o quast/ flye/assembly/assembly.fasta`

Step 10:

We have further assesed the quality of the assembly with Bandage.
The usage is: `Bandage`
This opens an GUI
Select "File" and open "Load Graph"
Open flye/assembly/assembly.gfa
Save image by selecting "File", "Save image"
Save image in bandage folder as .png

Step 11: 

To asses the completeness of the assembled genome we have used BUSCO.
The usage is: `busco -m genome --auto-lineage-euk -i flye/assembly/assembly.fasta -o busco/`

# Data storage

The original files are stored at isengard.bioinformatics-atgm.nl, the location /mnt/studentfiles/2023/2023MBI_11/comparative_genomics_A_niger

# Acknowledgments

**NanoPlot**

**NanoFilt**

**FastQC**

**MultiQC**

**Trimmomatic**

**Flye**

**Polypolish**
Wick, R. R., & Holt, K. E. (2022). Polypolish: Short-read polishing of long-read bacterial genome assemblies. PLoS Computational Biology, 18(1). 
[paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1009802)

[GitHub](https://github.com/rrwick/Polypolish/wiki/How-to-run-Polypolish)

**Quast**

**Bandage**

**BUSCO**
