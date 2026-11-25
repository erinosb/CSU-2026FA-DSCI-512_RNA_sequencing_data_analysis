# DSCI 512 - Final Exam Project

- Due Tuesday, December 15, 2025, 11:59 pm
- If you need more time to complete this exam, please contact me ahead of time to make arrangements to turn the assignment in late.
- This exam accounts for 35 % of your final grade

----

## RULES

- NO Wheat! No hexaploid, tetraploid, or other complex genomes!
- NO uncharacterized genomes!
- Go for model systems if you can or at least organisms that have a .gtf file.
- RNA-seq data must be paired-end!

## Description

This exam will be a **project**. You will be asked to pick an RNA-seq dataset to analyze. This dataset can be either something **published** already in a paper. It can be something you **produced** yourself or that your lab has produced. If you don't have an idea for a project readily available, talk to your PI about it. If you are not associated with a lab and you aren't sure which dataset to use, please choose from the [Possible Projects](https://github.com/erinosb/CSU-2026FA-DSCI-512_RNA_sequencing_data_analysis/blob/main/02_assignments/Homework2.md#possible-projects).

You will be asked to formulate a **biological question** that the RNA-seq dataset could address. It can be a question addressed in the originally published paper or it can be a new question.

- What are the transcriptional differences between wild type or mutant? What categories of genes might you expect to change most dramatically?
- Which type of condition do you expect to have a dramatic effect on an organism (drug or mutant)?
- What are the top-most changing genes between two conditions?

Some of the tasks you will do started out as Homework and Reading Assignments.

For the Final Project, you will customize and run the pipeline RNAseq_PE_array. Basically, you will hack the code to run on your dataset. **You will use .fastq files as input to produce tabulated counts as output on ALPINE.** You will then **download the tabulated count file from ALPINE and analyze it on your own computer using DESeq2 to produce a few plots.**

**You will be asked to create some figures and figure legends pertaining to your analysis (Questions 2 - 4) and you will be asked to introduce (Question 1) and discuss (Question 5) your findings.**

----

## EXAM QUESTIONS

**Turn Questions 1 - 5 of your exam results as a single .pdf file.**

The exam will be graded out of 35 pts and represent 35 % of your final grade.

-----

### Question 1 - Introduction - 7 pts

Write an introduction to your RNA-seq final project. Introduce your topic in three short paragraphs with the following structure:

- What is the dataset you will use? Include a reference and GEO Accession Number if it is already published. Explain how the dataset was generated.
- What is the hypothesis you aim to test using your RNA-seq data analysis? Be sure to include an introductory description of the original study and describe whether you are choosing to confirm the results for yourself or take on a new analytical question. Note, you may not actually be able to address this as part of the exam project - that is ok. The goal is more to introduce the type of question that this data could resolve.
- Briefly state the greater relevance of this work. Why should people care about it?

-----

### Question 2 - Quality Control Supplemental Figure - 7 pts

Publications often contain a supplemental figure that illustrates information about the quality of each RNA-seq sample they ran. Devise a computational approach to mining your fastp or hisat2 output. Your computational approach should gather relevant information about each sample's quality. You can display the resulting statistics in a plot generated in R, matplotlib or plotly (python), or any other dedicated scientific plotting software. Excel will be accepted, too, but please only use excel if the other options are not working for you. You can choose any metric (# of reads, duplication rate of each sample, percentage of bases with >Q30, % mapped reads, before v. after filtering of any metric, or multiple metrics).

**Turn in the following:**

- Your plot
- A figure legend of your plot. By legend, I don't mean a square inset in the corner with “red = #reads; blue = % Q30”. I mean, a written paragraph describing the figure as you would see written in a paper. Your figure legend should describe what it is being shown, a general overview of what was done to generate the figure, and any statistics or axes should be explained. For more information on figure legends, [learn about them here](). Also, read some papers to get a sense of the type of information that is gathered into figure legends.
- An additional paragraph summarizing in casual terms, how you did this. How did you gather the statistics? How did you generate the figure (including language, commands used, version of software, etc)?

> [!TIP]
> Consider using bash scripting or python to parse through the output created by fastp. It can be as simple as a wc command or as complex as reading through the .json or .html output.

> [!TIP]
> For full credit you must have a datapoint for every sample in your dataset.

> [!WARNING]
> The legend is where most students lose points. Take the time to learn how to write a good figure legend. Use existing manuscripts as examples*

-----

### Question 3 - DESeq2 Plot - 7 pts

Create a plot using DESeq2. It can be an MA-plot, a plotCounts dot plot, volcano plot, a SampleDistMatrix, a PCA plot, a heatmap, whatever you like.

**Turn in the following:**

- Your plot
- A legend of your plot.
- Results. 1-3 sentences explaining (as if in the results section of a paper) what this plot illustrates and what you can learn from it.

-----

### Question 4 - Wildcard Plot - 7 pts

Create any other plot of your choosing. It can be a second DESeq2 plot. It can be a plot of your own devising (get creative).

- Your plot
- A legend of your plot.
- Results. 1-3 sentences explaining (as if in the results section of a paper) what this plot illustrates and what you can learn from it.

-----

### Question 5 - Discussion - 7 pts

Reflect on the project and write a single paragraph to address the following:
- Brief synopsis and conclusions including an assessment of whether you were able to answer the specific question you set out to address (it's ok if you didn't)? If you weren't able to address the question, explain why.
- Future directions. If you were to continue to work on this project, what analysis would you propose to do next?


**!!! AH! Nothing's working! Help! What do I do?** To avoid being in this situation, **START EARLY** on your final project. Get a head start on Assignments #4 and #5 as those will help you obtain the input files you need.

- Come to office hours if you are having trouble adapting the pipeline to your dataset, or need help with DESeq2.
- E-mail me with questions when you get stuck, but don't expect a lot of answers the night before the due date.
- Document in Question 5, "Discussion part" what you think is going wrong and what you might try to troubleshoot to fix it.
- If you can't get to your _counts.txt file, that is ok. E-mail me and I can send you a `_counts.txt` files associated with one of the *possible projects*. 

> [!WARNING]
> You can ask for a 48 hour extension but remember that ALPINE is often down on Wednesdays. 
