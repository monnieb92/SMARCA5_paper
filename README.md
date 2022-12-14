# **Human SMARCA5 Is Continuously Required to Maintain Nucleosome Spacing**
### *Software Required for each Figure to remove adapters, align, and clean up .sam files:*
```{bash}
#Remove adapter and align
trimmomatic-0.32.jar
bowtie2 #version 2.2.2
samtools #Version: 0.1.19-44428cd
```
### *Any motif analysis:* 
```{bash}
# Homer
findMotifsGenome.pl file.bed hg19 OUTPUT_DIR/ -size given -mask 
```
```{r}
#Motif Enrichment from Emily Hodge's Lab
#function
format_motif_results <- function(df){
  df <- mutate(df, percentTargetNum = (as.numeric(gsub("%", "", percentTarget, fixed = TRUE))/100))
  df <- mutate(df, percentBackgroundNum = (as.numeric(gsub("%", "", percentBackground, fixed = TRUE))/100))
  df <- mutate(df, percentFold = (percentTargetNum/percentBackgroundNum))
  df <- mutate(df, Motif = gsub('\\(.*', "", MotifName))
  df <- dplyr::filter(df, percentFold != "Inf")
  df$Motif <- factor(df$Motif,levels=rev(unique(df$Motif)))
  # Plot with GGPlot
  return(df)
}
```
### *Overlapping peak files:* 
```{bash}
# Homer
mergePeaks -d given peakfile1.bed peakfile2.bed > output_overlappedpeaks_file.txt
```
### *Any annotated peak analysis:*
```{bash}
# Homer
annotatePeaks.pl peakfile.bed hg19 > output_annotated_file.txt
```
### *Creating deeptools heatmaps:* 
```{bash}
#deeptools 3.4.1
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
#Callpeaks macs2 2.2.6
macs2 callpeak -q 0.001 -f BAMPE --keep-dup all
```
```{r}
# R packages
dplyr #1.0.6
tidyverse #1.3.1
Diffbind #3.0.15
DESeq2 #1.30.1
```
## Figure 3: CUT&RUN CTCF, RUNX1, AML1-ETO
### Necessary packages: 
```{bash}
#Callpeaks macs2 2.2.6
macs2 callpeak -q 0.05 -f BAMPE --keep-dup all
```
```{r}
# R packages
dplyr #1.0.6
tidyverse #1.3.1
Diffbind #3.0.15
DESeq2 #1.30.1
```


## Figure 4: ATAC-Seq
### Necessary packages: 
```{bash}
#Install Genrich into conda envrionment (https://anaconda.org/bioconda/genrich)
conda install -c bioconda genrich
# Call peaks Genrich
conda activate Genrich 

Genrich -t file.sortn.bam -j -r -E {ENCODE_download}/hg19.blacklistpeaks.bed \
-q 0.05 -o FILENAME.j.r.Eblacklist.q0.05.narrowPeak
```
```{r}
# R packages
dplyr #1.0.6
tidyverse #1.3.1
Diffbind #3.0.15
DESeq2 #1.30.1
```
## Figure 5: PRO-Seq and RNA-seq
### Necessary packages: 
```{bash}
#NRSA (http://bioinfo.vanderbilt.edu/NRSA/)

```
```{r}
# R packages
dplyr #1.0.6
DESeq2 #1.30.1
tidyverse #1.3.1
ComplexHeatmap #2.7.4
circlize #0.4.12
```

## Figure 6/7: ATAC-seq and MNase-seq nucleosome repeat length calculations (NRL)
### Necessary packages: 
```{r}
#R packages #version
BSgenome.Hsapiens.UCSC.hg19 #1.4.3
Rsamtools #2.6.0
AnnotationDbi #1.52.0
GenomicRanges #1.42.0
tidyverse #1.3.1
swissknife #version: 0.28
devtools #2.4.2
```

## Figure 7: MNase-seq Phasing around CTCF motifs
### Necessary packages: 
#### deeptools
```{}
alignmentSieve -b file.bam --minFragmentLength 120 --maxFragmentLength 210 -p 6 \
-o file.alignsieve140-200.BLfiltered.bam -bl {ENCODE}/hg19.blacklistpeaks.bed

bamCoverage -b file.bam -of 'bigwig' -bs 1 -p 6 --normalizeUsing CPM --MNase -o filename.bw --minFragmentLength 130 --maxFragmentLength 210
```
#### FIMO 4.12.0
 Determine CTCF motif center 
#### bedtools v2.30.0
```{r}
#R packages
Rsamtools #2.6.0
AnnotationDbi #1.52.0
GenomicRanges #1.42.0
tidyverse #1.3.1
```
