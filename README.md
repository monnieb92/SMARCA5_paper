# SMARCA5_paper
## Software Required for each Figure to remove adapters, align, and clean up .sam files:
```{bash}
#Remove adapter and align
trimmomatic-0.32.jar
bowtie2
samtools
```
## Any motif analysis: 
```{bash}
# Homer
findMotifsGenome.pl file.bed hg19 OUTPUT_DIR/ -size given -mask
```
## Any annotated peak analysis: 
```{bash}
# Homer
mergePeaks -d given peakfile1.bed peakfile2.bed > output_overlappedpeaks_file.txt
```
## Overlapping peak files: 
```{bash}
# Homer
annotatePeaks.pl peakfile.bed hg19 > output_annotated_file.txt
```
## Figure 2: CUT&RUN SMARCA5 (FLAG)
### Necessary packages: 
```{bash}
#Callpeaks
macs2 callpeak -q 0.001 -f BAMPE --keep-dup all
```
```{r}
# R packages
dplyr
tidyverse
Diffbind
DESeq2
```
## Figure 3
## Figure 4
## Figure 5
## Figure 6
## Figure 7
