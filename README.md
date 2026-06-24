# Scripts and Data for Analyzing Deep Mutational Scanning of WNV E

## Overview

In this directory are the input files and code to reproduce the results from [Dorman et al 2025](https://academic.oup.com/ve/article/11/1/veaf087/8307679). 
- Calculate_Fitness_Scores: directory containing the counts files used as input for Enrich2
- Generate_Figures: directory containing the csv with the fitness score for each variant in each cell as well as the Rmarkdown for analyzing the fitness scores and generating the figures
- Natural_Diversity: directory containing the input alignments and blast outputs for comparing DMS results to natural diverstity (figrues 5 and 6)

## Calculate Fitness Scores

### Analyzing Sequencing Output

To recapitulate analysis from the Illumina sequencing output, the raw fastq files can be accessed from NCBI Sequence Read Archive under BioProject accession number PRJNA1333558

First, align fastq files to the WT NY99 reference sequence using minimap2 with the following parameters:
` minimap2 -ax sr $ref/pdonr221_gfp_ins-wnv-bsai-and-bsmbi-free.fasta $read1 $read2 > $aligned_output `

Then run GATK on the aligned outputs:
` gatk AnalyzeSaturationMutagenesis -I $aligned_output --orf 56-2401  -R $ref/pdonr221_gfp_ins-wnv-bsai-and-bsmbi-free.fasta -O $gatk_output `

### Enrich2 

The starting point for this step is the amino acid counts produced by GATK. See the [Enrich2 documentation](https://github.com/fowlerlab/enrich2) for installation and usage instructions.

First, convert the aaCounts output into a format that is compatible with Enrich2 with the countProcessing.py script. The WTNorm experiment will require the codonCounts output and the synonymous mutation will need to be designated '_wt'

Then, run Enrich2 with 'Log Ratios (Enrich2)' and 'Library Size (Complete Cases)' for the full expeirment or 'Wild Type' for the WTNorm experiment. The config files used are present in this directory. 

## Generate Figures

With the outputs from the previous steps, all necessary inputs should be present to follow the Rmarkdown file. This file has all necessary R package dependencies for each analysis. For convenience, the fitness scores as calcualted and normalized by following this analysis are present as their own csv (DMS_P1_CG_C6_DC_RepAB_normEfit_060425.csv).

## Natural Diversity

To recapitulate the comparisons of fitness measurements to natural diversity, all necessary input alignments and BLAST outputs are present. Follow the Rmarkdown and publication methods to use these input files.

