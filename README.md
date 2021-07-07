# **SMARCA5_paper**
### *Software Required for each Figure to remove adapters, align, and clean up .sam files:*
```{bash}
#Remove adapter and align
trimmomatic-0.32.jar
bowtie2
samtools
```
### *Any motif analysis:* 
```{bash}
# Homer
findMotifsGenome.pl file.bed hg19 OUTPUT_DIR/ -size given -mask
```
### *Any annotated peak analysis:*
```{bash}
# Homer
mergePeaks -d given peakfile1.bed peakfile2.bed > output_overlappedpeaks_file.txt
```
### *Overlapping peak files:* 
```{bash}
# Homer
annotatePeaks.pl peakfile.bed hg19 > output_annotated_file.txt
```
### *Creating deeptools heatmaps:* 
```{bash}
#First Create bigwig file (make sure .bam file is indexed)
bamCoverage -b file.bam -of 'bigwig' -bs 5 -p 6 -o filename.bw

#Second Create Matrix
computeMatrix reference-point --referencePoint center -R peakfile.bed -S filename1.bw filename2.bw \
-a 1000 -b 1000 -bs 5 -p 6 -o matrix_filename.gz

#Third plotHeatmap
plotHeatmap -m matrix_filename.gz --colorList 'white,purple' -o heatmap_filename.pdf

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
## Figure 3: PRO-Seq
```{bash}
#NRSA (http://bioinfo.vanderbilt.edu/NRSA/)

```
```{r}
# R packages
dplyr
tidyverse
ComplexHeatmap
circlize
```
## Figure 4: ATAC-Seq
```{bash}
#Install Genrich into conda envrionment (https://anaconda.org/bioconda/genrich)
conda install -c bioconda genrich
# Call peaks Genrich
conda activate Genrich 

Genrich -t file.sortn.bam -j -r -E {ENCODE_download}/hg19.blacklistpeaks.bed -q 0.05 -o FILENAME.j.r.Eblacklist.q0.05.narrowPeak
```
```{r}
# R packages
dplyr
tidyverse
Diffbind
DESeq2
```
## Figure 5
## Figure 6
## Figure 7
