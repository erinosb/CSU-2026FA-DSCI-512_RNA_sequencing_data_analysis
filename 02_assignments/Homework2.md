# Homework #2

  - Due December 1st, 2026, 10:00 am
  - Turn in on canvas

----

# Obtain RNA-seq .fastq files

## Step 1: Choose a dataset

For this homework, you will download or upload the dataset you will use for your **Final Exam Project** onto ALPINE.

  - Remember, you can choose to use 1) your own data, 2) a published dataset in your field, 3) any of the suggested datasets below. These are also listed in [Reading #2](READIN2.md).
  - Download the dataset (or a portion of it) to your scratch space on ALPINE.
  - To do this, follow the instructions laid out in [Data Acquisition](../01_modules/Week2/2-4_Data_Acquisition.md)

-----

## Step 2: Start a Computational Lab Notebook

Start an entry for each day you work on this homework. This can be a physical noteobook, a OneNote directory, an ELN (Electronic Lab Notebook), or a space on your local computer. It should not be on ALPINE. 

  - Write the date
  - Write a few introductory sentences to state your goal
  - Write directory where you are working
  - State generally what your goal is for the working session. 
    - You don't need to copy and paste all commands, but you need to write where these commands are kept (script names, etc)
  - There should be some assessments and personal reflection - “This worked; This didn't work; etc”


## Step 2: Start a Project Directory

  * Consdier putting it in the same directory on your scratch spaced that we started called DSCI512
    * Consider naming it PROJ02_<titlehere>
  * Create subdirectories (`01_input`, `02_scripts`, `03_output`)
  * Start a **README.txt file**. Include the following
    * Date
    * General Goal
    * Any URL links that are noteworthy
    * This can be brief.

## Step 3: Download your .fastq files into the 01_input directory

  * Select your dataset.
  * Aim for 6 - 32 files. More files than that can be cumbersome. 12 - 24 is ideal.
  * This may mean that you will end up ignoring some of the files in your dataset
    * If it is not clear which samples are which, you can download the whole dataset and sort it out later.
  * To do the download, follow the instructions laid out in [Data Acquisition](../01_modules/Week2/2-4_Data_Acquisition.md)

## Step 4: Turn it in

  * :Exclamation: If you are using your own data that you already have, just upload your dataset to ALPINE and describe how you did this. 
    * [Files on CURC](https://curc.readthedocs.io/en/latest/open_ondemand/files_app.html)
  * :Exclamation: If your dataset is not accessible through an SRA link, but uses some other data distribution strategy, just follow the instructions to obtain the data and report what you did to get it onto ALPINE. This may require downloading data to your local computer and then uploading that to ALPINE.
  * :Exclamation:  More tips on uploading data to ALPINE. Sometimes, it can be frustrating to upload several large datasets to ALPINE using the upload icon. In these cases, using dedicated sFTP software can work much more smoothly.

**What to turn in:** Turn in your Computational Lab Notebook Entries. Be sure to state at the end whether you were successful in obtaining all the data or whether you need futhre help.

-----

## Tips

:exclamation: **NOTE:** The pipeline we are using works on paired-end sequencing. Please try to identify a dataset that uses paired-end sequencing. 

:exclamation: **NOTE:** microarray, ddPCR, minION, and pacBIO datasets will not work for this assignment.

-----


## Possible Projects

*Yeast*

[Dong Y, Hu J, Fan L, and Chen Q. (2016) RNA-seq-based transcriptomic and metabolomic analysis reveal stress responses and programmed cell death induced by acetic acid in Saccharomyces cerevisiae. Scientific Reports. 7:4.](https://www.nature.com/articles/srep42659)

- 12 files
- 8 - 12 minutes download per file

[Herranz, Vandenbrink, Villacampa, Manzano, Poehlman, Feltus, Kiss, and Medina. (2019) RNAseq Analysis of the Response of Arabidopsis thaliana to Fractional Gravity Under Blue-Light Stimulation During Spaceflight. Front Plant Sci. 2019 Nov 26;10:1529.](https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2019.01529/full)

- untested

*Arabidopsis* 

[Herranz, Vandenbrink, Villacampa, Manzano, Poehlman, Feltus, Kiss, and Medina. (2019) RNAseq Analysis of the Response of Arabidopsis thaliana to Fractional Gravity Under Blue-Light Stimulation During Spaceflight. Front Plant Sci. 2019 Nov 26;10:1529.](https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2019.01529/full)

- This is a pretty big dataset. Suggest just using a low-gravity & high gravity conditions.
- This download's from a NASA-managed site, not from SRA. You'll need to download the data to your local computer and then upload it to ALPINE manually.
- Go to “Study Files” and download “Raw Sequence Data” appropriate for the conditions you select to study.

[Park et al., (2019) The transcriptome analysis of the Arabidopsis thaliana in response to the Vibrio vulnificus by RNA-sequencing. PLoS One. 2019 Dec 16;14(12):e0225976.](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0225976)

- Roughly 15 minutes per sample
- 10 samples

*Rice*

[Ahn H, Jung I, Shin SJ, Park J, Rhee S, Kim JK, Jung W, Kwon HB, Kim S. Transcriptional Network Analysis Reveals Drought Resistance Mechanisms of AP2/ERF Transgenic Rice. Front Plant Sci. 2017](https://pubmed.ncbi.nlm.nih.gov/28663756/)

*Mouse*

[Nardou R, Sawyer E, Song YJ, Wilkinson M, Padovan-Hernandez Y, de Deus JL, Wright N, Lama C, Faltin S, Goff LA, Stein-O'Brien GL, Dölen G. Psychedelics reopen the social reward learning critical period. (2023) Nature. Jun;618(7966):790-798. doi: 10.1038/s41586-023-06204-3.](https://pubmed.ncbi.nlm.nih.gov/37316665/)

- untested

[Brady RA, Bruno VM, Burns DL. (2015) RNA-Seq Analysis of the Host Response to Staphylococcus aureus Skin and Soft Tissue Infection in a Mouse Model](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0124877)

- Suggest just focusing on the Day 7 data. Even that took about 8 hrs to download.

[Tuipulotu, Netzler, Lun, Mackenzie, and White1. (2017) RNA Sequencing of Murine Norovirus-Infected Cells Reveals Transcriptional Alteration of Genes Important to Viral Recognition and Antigen Presentation. Front. Immunol. 11 August](https://pubmed.ncbi.nlm.nih.gov/28848558/)

- This dataset is large. Just work on a subset of the samples
- Some samples may be organized into multiple small files that will need to be merged together
