# Installing Software II - Setting up SRA Toolkit

We are going to start to use the **SRA Toolkit (Short Reach Archive Tools)** to download large, raw RNA-seq data files from a **public data repository**. SRA Toolkit is a collection of software bundled together. It is written and maintained by [NCBI - National Center for Biotechnology Information](https://www.ncbi.nlm.nih.gov/), a division of the NIH (National Institutes of Health).

If your grant is supported by US government funds, you must deposit your data in NCBI prior to publication. If you are publishing in any major journal, you will be required to deposit your data at either NCBI or an equivalent public data repository and allow your reviewers to preview the data.

**Before we start, we'll need to just configure SRA Tools**. We will only need to do this once.

**!!! EXERCISE:** Let's load the SRAtoolkit module
```
$ acompile
$ module list             # this lists what you have installed already
$ module spider sra       # this searches for things that have 'sra' in them
$ module load sra-toolkit # this installs the pre-loaded sra-toolkit
$ module list             # this lists what you have installed
```

**!!! HINT:** If you're not seeing sra-toolkit, ensure that you are on the compile node:

```
$ hostname #should not say login
$ acompile # to switch to to a compile node
```

Great! Now, if you just executed that command, double check that you have the proper version with
```
$ fasterq-dump
```

**!!! EXERCISE:** Next, let's all work together to ensure that SRA Tools will use the proper temp directory for its download. This directory requires 100 G of space. However, the way that SRA Tools sets up by default, it lists sets the temp directory within your home directory. Because we only have 2G of space there, this is not going to work. Let's change the temp directory...

```
$ mkdir -p /scratch/alpine/$USER/ncbi/public
$ vdb-config -s /repository/user/default-path="/scratch/alpine/$USER/ncbi"
$ vdb-config -s /repository/user/main/public/root="/scratch/alpine/$USER/ncbi/public"
```

Let's double check that this worked using the vdb-config interactive browser:

```
$ vdb-config -i

# some of you might have needed to do... vdb-config --interactive if I remember correctly?
```

This will open up a window like so...

<p align="center">
<img width="500" alt="first screen" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/first_screen.png">
</p>

This little applet can be navigated by clicking on letters associated with any letters in red.

Type **Capital C** to navigate to the Cache menu

<p align="center">
<img width="500" alt="second screen" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/second_screen.png">
</p>

Check that your `/scratch/alpine` directory is listed

Type **lowercase x** to Exit

For more information on how to navigate vdb-config, check **Set up** of fasterq-dump

## Reviewing SRA-Toolkit Usage

We only need to specify SRA-Toolkit's temp space once. So you won't need to vdb-config any more.

Next time we want to use SRA-Toolkit, we only need to do the following to install it and it will work.
```
$ module load sra-toolkit
```

Next, we'll learn how to obtain raw RNA-seq files from the NCBI Short Read Database using the command ...
```
$ fasterq-dump
```

Continue on to [Project Initiation](2_3_Project_Initiation.md) 
