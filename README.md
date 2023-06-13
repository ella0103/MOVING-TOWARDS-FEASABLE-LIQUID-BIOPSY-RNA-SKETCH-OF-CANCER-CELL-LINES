# MOVING-TOWARDS-FEASABLE-LIQUID-BIOPSY-RNA-SKETCH-OF-CANCER-CELL-LINES
Analysis of cancer and healthy cell lines using long-read RNA-seq, conducted with the ONT MinION device.

The objective of the study was set to assess the feasibility of RNA sketching with MinION for efficient differentiation between liquid biopsy samples of cancer cell lines and healthy cell lines. 


The full report of the analysys can be found here

## Table of contents
- [Flowchart](#flowchart)
- [Requirements](#requirements)
- [Galaxy workflow](#galaxy-workflow)
- [Input files](#input-files)
- [Reference genome](#reference-genome)
- [Tools and settings](#tools-and-settings)
- [Output interpretation](#output-interpretation)
- [Acknowledgements](#acknowledgements)

## Flowchart
![flowchart_pp](https://github.com/ella0103/MOVING-TOWARDS-FEASABLE-LIQUID-BIOPSY-RNA-SKETCH-OF-CANCER-CELL-LINES/assets/121402109/eed5c459-ba85-45e4-a05b-4544908dce46)

## Requirements
- Galaxy server
- Python 3.10.10

## Galaxy Workflow
The Galaxy Workflow can be accessed [here](https://github.com/ella0103/MOVING-TOWARDS-FEASABLE-LIQUID-BIOPSY-RNA-SKETCH-OF-CANCER-CELL-LINES/blob/a939c148217ea633a306f9f73b5260881cd6e752/Galaxy-Workflow-Workflow_constructed_from_history__MinION_data_imported_%20(2).ga).

## Input files 
Input files can be accessed on the [Galaxy history](https://galaxy.atgm.avans.nl/u/mirela_minkova/h/minion-rna-seq-input-data).


## Reference genome
The reference sample, T2T-CHM13v2.0 was downloaded locally from the [official site of the National Library of Medicine](https://www.ncbi.nlm.nih.gov/assembly/GCF_009914755.1/). 
The files can be accessed in the local history in a fna and gtf file format [here](https://galaxy.atgm.avans.nl/u/mirela_minkova/h/reference-genome).


## Tools and settings
All tools were used directly on the Galaxy server provided by Avans University of Applied Sciences.

### Quality control 
For assessing the quality of the data generated from MinION, the Nanoplot tool is used. The input files in FASTQ file format, keeping all settings at default. 

### Mapping analysis 
Minimap2 is tool designed specifically for MinION data. From the tool settings, single end reads was chosen, since MinION produces single-end reads. All other settings were set on default, producing a BAM output file. 

### Gene counting 
FeatureCounts was used for the read count generated from Minimap2. All settings were kept at default, selecting as annotation file the T2T-CHM13v2.0.gtf.gz from the own history. Featurecounts generates two output files, one containing the counts and the other a summary of the counts. Since single-ended reads are used, the stand information is set at unstanded, selecting the gene annotation file from the local history. All other settings are kept at default. Further, featurecounts was run on filtered alignments on the samtools view output, keeping the default settings once again.

### Statistical summary 
- Samtools stats was used to generate a statistical report of one of the most important features including the raw total sequences, the amount of filtered sequences, reads mapped, reads unmapped, total length, bases mapped, error rate and the average rate. The tool was used on the output from minimap2, as well as on the output from Samtools view. The settings were kept at default.
- Samtools view was performed on the output from minimap2, where filtering per sex chromosomes was performed. For each of the samples, samtools view was run two times, once filtering per chromosome Y and second time by filtering per chromosome X. This produced a BAM file format, that was further analysed with samtools stats and featurecounts. All setings were kept at default, except “Filter by region”, where chrY and chrX were specified.



## Output interpretation
- The gene location per chromosome was determined using the output files from FeatureCounts and the reference T2T-CHM13v.2.0, applying the VLOOKUP function in Excel. This way validation of the number of genes presented on the X and Y chromosome was performed. 
- Validation of the cell lines was performed using lists with reference genomes specific for a certain cell line available on the [Harrmonize 3.0 website](https://maayanlab.cloud/Harmonizome/). Genes with the highest expression levels per cell line were chosen and compared throughout the different cell lines.

## Acknowledgements
[Nanoplot](https://github.com/wdecoster/NanoPlot)
[Minimap2](https://github.com/lh3/minimap2)
[FeatureCounts](https://rnnh.github.io/bioinfo-notebook/docs/featureCounts.html)
