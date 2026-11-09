# Project Initation

We will now start a project. Quick review of some aspects of the ALPINE architecture.

## Start an RNA-seq data analysis project

We will start a data analysis directory for this class and initiate a project within that directory.

Currently, we are in our home directory `/home/jesshill@colostate.edu/`. The ALPINE staff have set up directories for each user. Let's review the general ALPINE Architecture:

### The Alpine System: 

<p align="center">
<img width="700" alt="alpine map" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/alpine_map.jpg">
</p>

### Alpine File Storage: 

[Detailed information about the file storage on ALPINE](https://curc.readthedocs.io/en/latest/compute/filesystems.html)

Whereas the nodes are analogous to CPU and memory on your home laptop, the filesystem is analogous to your hard drive. This is where data will be stored. The ALPINE team has pre-organized several nice directories where each user can house their data. Each space has a designated purpose.

Here are the different file storage spaces available:

<p align="center">
<img width="700" alt="file storage" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/file_storage.png">
</p>

**!!! Exercise:** Let's start our project.

We will be using our scratch space to perform the projects in this class. That means, everything we do will be deleted 90 days from now!

```
# Go to your scratch directory.
$ cd /scratch/alpine/<eID>@colostate.edu   # Substitute <eID> with your eID
 
# Create a directory for this class. Call it **DSCI512**
$ mkdir DSCI512
 
# Move into that directory
$ cd DSCI512
 
# Within that directory start a new project
$ mkdir PROJ01_GomezOrte
 
# Move into that directory
$ cd PROJ01_GomezOrte
 
# Create three new subdirectories as follows
$ mkdir 01_input 02_scripts 03_output
```

**YAY!** Now we have a place to do our work!

Continue on to [Running Jobs with SLURM](2_4_Running_Jobs_with_SLURM.md)
