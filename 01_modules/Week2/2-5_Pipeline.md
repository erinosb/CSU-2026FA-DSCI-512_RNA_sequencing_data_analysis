<p align="center">
<img width="90%" alt="icons" src="../../05_images/headers_pipes.jpg">
</p>

# What is a Pipeline?

A **pipeline** is series of data processing operations in which the output of one process is the input for the next process

<p align="center">
<img width="80%" alt="pipeline" src="../../05_images/pipeline.jpg">
</p>

## A Typical RNA-Seq Data Analysis Pipeline

  1. Align sequenced fragments (called reads) to the genome
  2. Quantify the number of reads associated with each gene
  2. Normalize the samples
  4. Use modeling and statistics to identify differentially expressed genes

<p align="center">
<img width="80%" alt="RNA-seq Data Analysis" src="../../05_images/data_analysis_recap.png">
</p>

Each step will involve different software. There are many options for each step:

<p align="center">
 <img width="90%" alt="software for RNA-seq" src="../../05_images/StarkEtAl_Fig.png">
 <br>
 <em><b>Figure:</b> Adapted from Stark et al., 2019</em>
</p>

**For our pipeline, we will use:**

  * **Hisat2** for alignment
  * **featureCounts** for quantification
  * **DESeq2** for normalization
  * **DESEq2** for differential expression.

In addition, we will need to do some **pre-processing** of our reads. This will remove low quality reads and primer or  adapter sequences that were not from our original sample.

  * **fastp** for pre-processing

All of these software packages, except one, are loaded up in your conda environment `2026dsci`. The one exception is **DESeq2** that will be run on your local laptop computer within `R`.

## Our RNA-seq Data Analysis Roadmap (short version):

<p align="center">
 <img width="80%" alt="our pipeline for RNA-seq" src="../../05_images/our_pipeline_short.png">
</p>

## Our RNA-seq Data Analysis Roadmap (long version):

<p align="center">
 <img width="80%" alt="our full pipeline for RNA-seq" src="../../05_images/our_pipeline_long.png">
</p>

Notice what goes in and what comes out:

  * **Input:** `file_R1.fastq` and `file_R2.fastq`
  * **Input:** Genome index files
  * `file_counts.txt` is a major intermediary file
  * **Output:** Differentially abundant genes
  * **Output:** Pretty browser shots
  * **Output:** Plots

Continue on to [2-6 The Pipeline](2-6_Building_Indexes.md) 
