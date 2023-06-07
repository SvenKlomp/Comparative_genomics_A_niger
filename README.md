# Comparative_genomics_A_niger
Comparative genomics of a 40 year old lineage of Aspergillus niger 
The goal of this project is a SNP and overall genome analysis of _Aspergillus niger_ mutants by using Illumina and Nanopore seqeuncing data. Besides using own sequenced data publicly available data is used as well. 

Analysis:
Step 1: 
We have done an initial quality controll of our Nanopore data with NanoPlot.
The usage is: 2023MBI_11@isengard:~ NanoPlot --fastq input_data.fastq -o nanoplot/input_data/

Step 2: 
We have filtered the Nanopore data with NanoFilt.
With this command all the reads below quality 15 and all the reads smaller than 1000 gets filtered out. 
The usage is: 2023MBI_11@isengard:~ NanoFilt -q 15 -l 1000 -o nanofilt/output_data_after_filtering.fastq -f input_data.fastq

Step 3: 
We have done a quality controll of our filtered Nanopore data with NanoPlot.
The usage is: 2023MBI_11@isengard:~ NanoPlot --fastq nanofilt/output_data_after_filtering.fastq -o nanoplot/after_filtering_input_data/

Step 4: 
We have done an intial quality controll of our Illumina data with Fastqc.
The usage is: 2023MBI_11@isengard:~ fastqc 
