# Data Acquisition

Let's go ahead and log into [ALPINE](https://ondemand-rmacc.rc.colorado.edu) if you're not already logged in:

Go ahead and load the sra-toolkit module.

**Once there**

```
$ acompile --time=2:00:00
$ module load sra-toolkit
```

How do we obtain RNA-seq data?

Data for different projects are stored/housed in repositories. The most common repositories are **NCBI GEO** (National Center for Biotechnology Information, [Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/), organized by the NIH) and the [NCBI SRA (Short Read Archive)](https://www.ncbi.nlm.nih.gov/sra).

Not every project uses the NCBI repositories. Individual projects (NASA, ENCODE, etc) sometimes design their own repositories as part of their research project.

When you publish your own RNA-seq data, you will be required by the journal to deposit all your data in a repository, sometimes prior to submitting the manuscript for review. You can submit the data to the repository in an **embargoed** format. That is, the data will be hidden and password protected so that manuscript reviewers can access it, and then the password protection will be lifted once the manuscript is published.

- It is your responsibility as an author to upload the data to the repository and provide the **reviewer link** to the journal when you submit your manuscript.
- It is your responsibility as the author to contact NCBI when your paper is **published** and assist them in lifting the embargo and linking the repository to the published work.

## Resources

[SRA-Tools - fasterq-dump](https://github.com/ncbi/sra-tools/wiki/HowTo:-fasterq-dump)

## Obtain the Gomez-Orté Dataset Information

**!!! Exercise:** Let's download one of the Gomez-Orté files together.

- **First**, consult the Gomez-Orté manuscript: [Gomez-Orté et al., 2017](https://pmc.ncbi.nlm.nih.gov/articles/PMC5839384/)
- **Second**, search for **GEO** or **GSE** in the document and find this reference in the materials and methods section: 

<p align="center">
<img width="700" alt="GEO info" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/GEO_info.png">
</p>

- **Third**, google search **GSE101524** to find its NCBI GEO Entry: [GSE101524](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE101524).
  - Go ahead and navigate to that page and explore the content.
  - There are 18 samples to download (each link to their own description page)
  - There is additional information about the experiment.
- **Fourth**, we want to link to the SRA repository data by clicking on SRA Run Selector at the bottom of the page, here:

<p align="center">
<img width="700" alt="data aquisition" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/data_acquisition.png">
</p>

## Test the SRA-Toolkit

Let's test that sra-tools is working

```
$ fasterq-dump --help #test that it works
```

## Download a single file

Say we want to download a single .fastq file from a dataset. We'll use **fasterq-dump** within the **sra-tools** suite of tools to do this.

First, look up the SRR number of the .fastq file you want to download. Let's pick the first on this SRA Run Selector page.

<p align="center">
<img width="700" alt="SRA" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/SRA.png">
</p>

It's SRR5832182.

**!!! Exercise:** Let's do this together

```
$ mkdir test_download   # Create anew directory within /scratch/alpine/<eID>@colostate.edu/DSCI512/
$ cd test_download      # Navigate into that directory
$ fasterq-dump -e 1 --split-files --progress SRR5832182 # Execute this line of code to download one sample (2x files). This took me 5 minutes for two 1.8 GB files.
spots read      : 5,357,669
reads read      : 10,715,338
reads written   : 10,715,338
 
$ vdb-validate SRR5832182   # check that the files are intact and weren't corrupted during transit.
2025-11-16T11:43:01 vdb-validate.2.11.0 info: Validating '/scratch/alpine/jesshill@colostate.edu/ncbi/sra/SRR5832182.sra'...
2025-11-16T11:43:01 vdb-validate.2.11.0 info: Table 'SRR5832182.sra' metadata: md5 ok
2025-11-16T11:43:01 vdb-validate.2.11.0 info: Column 'ALTREAD': checksums ok
2025-11-16T11:43:02 vdb-validate.2.11.0 info: Column 'QUALITY': checksums ok
2025-11-16T11:43:02 vdb-validate.2.11.0 info: Column 'READ': checksums ok
2025-11-16T11:43:02 vdb-validate.2.11.0 info: Column 'X': checksums ok
2025-11-16T11:43:02 vdb-validate.2.11.0 info: Column 'Y': checksums ok
2025-11-16T11:43:02 vdb-validate.2.11.0 info: Table 'SRR5832182.sra' is consistent
 
$ ls
SRR5832182_1.fastq  SRR5832182_2.fastq
```

The line **vdb-validate** automates the process of checking **md5-sums**

**!! Common pitfall:** the number one reason vdb-validate doesn't come out ok - Error on SRA's side. So stay calm.

## Start a downloads project

**!!! Exercise:** Let's download all the files!

To do this, we can AUTOMATE the process of pulling .fastq files using **fasterq-dump**. Let's do the following...

1. Set up a folder to download the files into
2. Obtain a list of SRR files you want to download from the SRA page
3. Obtain a looped script to automate the download
4. Download!

**1. Set up a folder to download the files into**

```
# Let's go here...
 
$ cd /scratch/alpine/<eID>@colostate.edu # Replace this with your eID
$ cd DSCI512
$ cd PROJ01_GomezOrte # If you have this directory, navigate into it. If you don't have this, go ahead and create it with mkdir
$ cd 01_input
```

**2. Obtain a list of SRR files you want to download from the SRA page**

<p align="center">
<img width="700" alt="SRA pic2" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/SRA_pic2.jpg">
</p>

- Navigate to the Gomez Orte SRA page.
- Click on **Accession List**
- Download it somewhere on your computer
- Open the file
- Copy and paste the contents of that file into a new file on ALPINE called `SRR_Acc_List_GomezOrte.txt` (you can use nano to do this).

```
$ more SRR_Acc_List_GomezOrte.txt
SRR5832182
SRR5832183
SRR5832184
SRR5832185
SRR5832186
SRR5832187
SRR5832188
SRR5832189
SRR5832190
SRR5832191
SRR5832192
SRR5832193
SRR5832194
SRR5832195
SRR5832196
SRR5832197
SRR5832198
SRR5832199
```

## Two ways to automate downloading

There are two ways to automate downloading.

**1) LOOPS:** The first is to **loop** over the list of samples and download each, in succession. In this model, when the first sample completes its download, the download of the next will start. This can be done using a `while`, `for`, or `while read` loop structure. Because of the way fasterq-dump is written, we can use multiple tasks for this process, improving its efficiency. If each file takes 4 minutes to download using 12 tasks, the whole job of downloading all 18 samples will take (4 min x 18 samples) 72 minutes, or an hour and 12 minutes.

<details>
  <summary>To see an example of 'Data acquisition using loops' type of code click here</summary>

---

### Data acquisition using loops

Using a `for`, `while`, or another type of loop can help to automate the downloading process. Using these loops, the download of each sample will happen in the same way, one after another.

Here is one method:

**Step 1:** Make a file that lists all the samples to download. Create a file called `SRR_Acc_List_GomezOrte.txt`

```
$ more SRR_Acc_List_GomezOrte.txt
SRR5832182
SRR5832183
SRR5832184
SRR5832185
SRR5832186
SRR5832187
SRR5832188
SRR5832189
SRR5832190
SRR5832191
SRR5832192
SRR5832193
SRR5832194
SRR5832195
SRR5832196
SRR5832197
SRR5832198
SRR5832199
```

**Step 2:** Copy and paste the following code into a file called `automateSRA_wLoops.sbatch`.

```
#!/usr/bin/env bash

# =====================================================================
#  Usage:      sbatch automateSRA.sbatch <file_listing_SRR_files.txt>
#
#  Input:      A text file where each line contains an SRR accession
#               (e.g., SRR12345678). The script loops through each
#               accession and:
#                   1. Downloads FASTQ files using fasterq-dump
#                   2. Splits paired-end reads into separate files
#                   3. Validates the resulting SRA data with vdb-validate
#
#  Notes:
#               - $SLURM_NTASKS is set automatically based on the
#                 --ntasks value in the SBATCH header.
#               - Runtime depends on dataset sizes and number of threads.
# =====================================================================

#SBATCH --nodes=1
#SBATCH --ntasks=12
#SBATCH --time=02:00:00
#SBATCH --partition=amilan
#SBATCH --qos=normal
#SBATCH --mail-type=end
#SBATCH --mail-user=tstark@colostate.edu #REPLACE THIS E-MAIL WITH YOUR E-MAIL
#SBATCH --output=log-download-%j.out

# EXCUTE code with: $ sbatch automateSRA.sbatch <file_listing_SRR_files.txt>
 
# INSTALL software
module purge
module load sra-toolkit
 
# RUN SCRIPT
  # Loop over each SRR file and import each fastq file:
  # Note - $SLURM_NTASKS is an environmental variable set by the #SBATCH line with --ntasks. So it dereferences the number that is specified in that line of code. In this case, it dereferences the number 12. 
 
while read line
do
   echo -e $line
   # Download and split FASTQ files using multiple threads
   echo "fasterq-dump -e $SLURM_NTASKS --split-files $line"
   time fasterq-dump -e $SLURM_NTASKS --split-files $line
   # Validate SRA data integrity
   echo "vdb-validate $line"
   vdb-validate $line
done < $1
```

**Step 3:** Execute the code with

```
$ sbatch automateSRA_wLoops.sbatch SRR_Acc_List_GomezOrte.txt 
```

---

</details>

**2.) ARRAYS:** The second way to automate downloading is to use an **array** structure to download each file simultaneously. In this model, we start all the downloads at the same time. We probably don't need as many tasks per download, so let's go with 4 tasks and it takes 10 minutes to download each sample. Because all the downloading will happen, simultaneously, or **in parallel**, the entire job will take 10 minutes. This sounds way better. Let's do it...

Copy and paste this following short script into the same directory and call it `automateSRA.sbatch`. This will automate the process of downloading every sample in the SRR list using **fasterq-dump**.

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --time=00:20:00
#SBATCH --partition=amilan
#SBATCH --qos=normal
#SBATCH --output=log-download-%A.%a.out
 
# Execute code with: $ sbatch --array=1-n automateSRA.sbatch <file>
# Make sure that in --array=0-n, the number n matches the number of lines in <file>
# The format of <file> is a list of SRR#s with each SRR# on its own line.
 

# Read input file containing SRR accessions.
# Convert the entire file into an array (ARR[]) where:
#   ARR[0] = first line (SRR#)
#   ARR[1] = second line (SRR#) 
myinfile=$1
ARR=($(cat $myinfile))
#
# Select the SRR accession for this SLURM array task.
# $SLURM_ARRAY_TASK_ID determines which line to process. 
line=${ARR[ $SLURM_ARRAY_TASK_ID ]}
echo -e $line
#
# Download FASTQ files using fasterq-dump:
#   -e $SLURM_NTASKS : number of threads
#   --split-files   : outputs *_1.fastq and *_2.fastq for paired-end
echo "fasterq-dump -e $SLURM_NTASKS --split-files $line"
time fasterq-dump -e $SLURM_NTASKS --split-files -O ../01_input/ $line         # The -O <out/put/dir> specifies where to save the .fastq files 
time fasterq-dump -e $SLURM_NTASKS --split-files $line
#
# Validate the downloaded SRA data for integrity.
echo "vdb-validate $line"
vdb-validate $line
```

Now, just execute the program like so... 

```
$ sbatch --array=0-17 automateSRA.sbatch SRR_Acc_List_GomezOrte.txt 
```

**NOTE:** We should be signed up for a special “reservation”. Meaning that because our class is an official sanctioned ALPINE class, we have been given priority on our jobs during our classtime. To take advantage of this during classtime, run your job with our special priority access code like so:

```
$ sbatch --account=csu99_alpine1 --array=0-17 automateSRA.sbatch SRR_Acc_List_GomezOrte.txt 
```

**NOTE:** 
- You can also download just a subset of SRR files.
- What does that term `$SLURM_NTASKS` mean? This is an environmental variable that dereferences whatever number is written in the line `#SBATCH –ntasks=4`. So, in this case `$SLURM_NTASKS` dereferences to 4. And this is how you'll need to sync up the resources you request with the resources you tell the software you have available to use. These numbers must match. You can do it by hand, or you can use the `$SLURM_NTASKS` variable.

## Finish it up

Best practices are to write a README.txt file to keep track of what you did. Make sure you include...

- The date
- Your name
- The purpose for downloading the dataset (believe me, it's easy to forget)
- Also consider downloading the metadata file from SRA Selector page and saving that in the same directory.

## Explore the data from the Gómez-Orte, et al., 2017 study

We should all have successfully obtained the data for this class. **YAY!** 

#### Exploring the data: 

- Let's explore what we obtained
- Using `more`, peek into some of the .fastq files
- Using `wc`, explore how big these files are

#### FASTQ Data

Sequencing data is stored in the fastq standardized data format:

```
@SRR5832182.1 HISEQ-MFG:180:hw5f3bcxx:2:1101:1205:2195 length=101                                       # Header Line
CGCGAACAGTCCTCAAATCGGGGATCGAAAAAGGATTCGTGGAATCACAGTATTTTTATGTATAATTTCTTCCACCGAAGAGGGAGCAACAAGAGAGCGGC   # Sequence Line
+SRR5832182.1 HISEQ-MFG:180:hw5f3bcxx:2:1101:1205:2195 length=101                                       # Seperator Line
DDDDDHHIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIGHHHIEHIIIIIIIIIIFHIIIHIIIIIHIIIIIIIIIIIIIIIIGHHIIIIIHIIHIIIIH   # Quality Score Line
```

In this format, each molecule sequenced on the sequencing instrument contains four rows of information. Each molecule sequenced is referred to as a **read**.

| Row # | Information |
| ----- | ----------- |
| 1 | Read information (sample name, instrument, coordinates or name, read length |
| 2 | ATGC DNA sequence obtained |
| 3 | Read information repeated |
| 4 | Quality scores |

All the reads (tens of millions) are concatenated together in groups of 4-rows each in the .fastq file.

For paired-end sequencing, two matched .fastq files are obtained. Typically, they are matched together based on their file names... `Sample1_1.fastq` & `Sample1_2.fastq`.

#### Quality Scores

OK, those quality scores look weird. What do they mean? These are **Phred quality scores (Q)** that were originally developed by the program Phred to measure base-calling reliability from Sanger sequencing chromatograms.

If Phred assigns a quality score of 30 to a base, the chances that this base is called incorrectly are 1 in 1000.

Phred quality scores are logarithmically linked to error probabilities

| Phred Quality Score | Probability of incorrect base call | Base call accuracy | 
| ----- | ----------- | --- |
| 10 | 1 in 10 | 90% |
| 20 | 1 in 100 | 99% |
| 30 | 1 in 1000 | 99.9% |
| 40 | 1 in 10,000 | 99.99% |
| 50 | 1 in 100,000 | 99.999% |
| 60 | 1 in 1,000,000 | 99.9999% |

Quality scoring range:
- **< 20**: poor quality
- **20 - 30**: moderate quality
- **> 30**: high quality

Each possible quality score is encoded by an individual character. [This is the code table](https://www.drive5.com/usearch/manual/quality_score.html)

Continue on to [Pre-Processing & QC](2_7_Pre-processing_and_QC.md) 
