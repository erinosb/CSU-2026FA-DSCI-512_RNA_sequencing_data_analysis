<p align="center">
<img width="90%" alt="icons" src="../../05_images/headers_conda.png">
</p>


# Welcome to conda

Another way of installing software is through a **conda virtual environment**. Virtual environments can contain their own combinations of installed software of different versions. In this way, you can switch between different environments for different projects. This is one of the many ways the community is trying to make research more **reproducible**.

Today, we will walk you through how to install software on SUMMIT using **conda**.

<p align="center">
<img width="60%" alt="conda" src="../../05_images/conda.png">
</p>

------

# Resources

- [CURC Basic Conda Commands to Get You Started](https://curc.readthedocs.io/en/latest/software/python.html#basic-conda-commands-to-get-you-started)
- [Anaconda Resources](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
- [bioconda](https://bioconda.github.io/index.html)

-----

## What is conda?

- conda is an open-source **software** (package) management system
- conda spans multiple different repositories for software
  - **Bioconda**. We will use the computational biology specific version called [bioconda](https://anaconda.org/bioconda/repo)
  - **conda-forge**. This is another popular software repository
- conda assists in downloading, compiling, and installing software
- when a piece of software is downloaded, all dependent packages will also be obtained
- conda helps users create different **environments** where software collections can be collected and curated.

**packages** - small units of software

**environment** - a virtual space where a user can contain a collection of installed software. Users can have environments associated with different research projects.

**anaconda** - a large version of conda

**miniforge** - a small version of conda

---

## What is the meaning of the name?

- python, anaconda, sense a theme? hint - there is also a boa

## What is available? 


| Repository | # of modules | 
|-------------------|--------------|
| bioconda | ~10,000 |
| conda-forge | ~34,000 |
| anaconda | ~ 19,000 |

Basically, everything is here!

----

# Getting started on conda

I know this is going to feel a little circular, but **conda** is already installed as a module. Our first task is to **load** conda and then we can build our own environment.

:hammer_and_wrench: **GROUP EXERCISE** - follow along on your own computer

First, go ahead and open a ALPINE Shell using **CLUSTERS** -> **>_ALPINE SHELL ACCESS**

```bash
# Next, make sure you're on an acompile node. If not, acompile
hostname
module avail
module load anaconda
```

----


# First Time Use - Configuring conda with `.condarc`

Today's lesson is adapted from: [CURC - Basic Conda Commands to Get You Started](https://curc.readthedocs.io/en/latest/software/python.html#basic-conda-commands-to-get-you-started).

Our first step before we can use conda is to configure our startup files to customize how conda will perform. We will only need to do this once. 

Like `.bash_profile`, `.bashrc` and `.zshrc`, other programs use “.rc” files to start up files that spell out configuration settings. These settings are not BASH scripts, though, just lists of values.

For conda, the startup file is called `.condarc` and it is located in your `$HOME` directory.

:hammer_and_wrench: **GROUP EXERCISE** - Let's work together to customize our conda settings.

Let's navigate through our files in the OnDemand GUI. 

  * Click on **Home Directory**
  * Click on **Show Dotfiles**

Do you have a file called `.condarc`? 

  * If yes, edit it in a new tab by clicking on the three vertical dots alongside it.
  * If not, create this file by clicking on **+ New File**. Make sure to name the file starting with a . character: `.condarc`

We are going to check that your `.condarc` file was created with no errors

Does your `.condarc` files look like this:


```bash
pkgs_dirs:
  - /projects/$USER/.conda_pkgs
envs_dirs:
  - /projects/$USER/software/anaconda/envs
```

If not, copy and paste the above code to your file. 

Save and exit the file. You won’t need to perform this step again – it’s permanent unless you modify `.condarc` later.

The `.condarc` file provides a variety of settings that can be detailed to speed up your workflows. For more information on `.condarc`, check out the [Anaconda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html). 

Yay! If you now have a `.condarc` file and see the four lines of code within it specifying your package and environmental paths, you are good to go! You won't need to do this step again on ALPINE.

----

# Start a new conda environment

Next, we will start the new conda environment that we will use throughout this class. 

Notice that when we loaded the anaconda module, a little `(base)` popped up before our prompt? That is our base environment. It is the default environment you land in when you start conda. 

:hammer_and_wrench: **GROUP EXERCISE**. Let's explore our environments

```bash
hostname # double check you're on a compile node
conda env list # list all the virtual conda environments we can currently load

# conda environments:
#
base                   *  /curc/sw/anaconda3/2023.09
ATOC_NWP                 /curc/sw/anaconda3/2023.09/envs/ATOC_NWP
bash_spr23               /curc/sw/anaconda3/2023.09/envs/bash_spr23
pyomp_2022               /curc/sw/anaconda3/2023.09/envs/pyomp_2022
synoptic_f23             /curc/sw/anaconda3/2023.09/envs/synoptic_f23
DSCI510                  /projects/.colostate.edu/jesshill/software/anaconda/envs/DSCI510
FuSeq                    /projects/.colostate.edu/jesshill/software/anaconda/envs/FuSeq
Picard                   /projects/.colostate.edu/jesshill/software/anaconda/envs/Picard
RNAseq                   /projects/.colostate.edu/jesshill/software/anaconda/envs/RNAseq
py3.7                    /projects/.colostate.edu/jesshill/software/anaconda/envs/py3.7
scRNAseq                 /projects/.colostate.edu/jesshill/software/anaconda/envs/scRNAseq
                         /projects/jesshill@colostate.edu/software/anaconda/envs/DSCI510
                         /projects/jesshill@colostate.edu/software/anaconda/envs/RNAseq
                         /projects/jesshill@colostate.edu/software/anaconda/envs/py3.7
```

The output shows us the default environments that the personnel at CU Boulder have kindly initiated for us to use. It also shows us the environments that we have created. The one we are currently using is marked by an asterisk. 

:hammer_and_wrench: **GROUP EXERCISE**. Lets build our own custom environment

```bash
hostname      # Ensure first that you're on a compile node. acompile if not
conda create -n 2026dsci

# You will be asked if you want to proceed. Type y
# When your environment is created, check that it exists:
conda env list

```

You should now see a new virtual environment has appeared called `2026dsci`

To navigate into your new environment, do this
```bash
conda activate 2026dsci
conda env list # This shows you which environments are available and selected
conda list  # This shows the software currently installed in your active environment

# Another key you're on the right track... Your prompt should now start with your conda environment when active
(2026dsci) [erinnish@colostate.edu@c3cpu-a2-u32-1 ~]$ 

```

Yay! You should now have your environment `2026dsci` installed and activated and updated

----

# Install software in your conda environment

:hammer_and_wrench: **GROUP EXERCISE**. Let's install software! For this class, we will need the software packages: 

| Software Package  | What it does | How we'll install it |
|-------------------|--------------|----------------------|
| sra-toolkit | downloads genomics data from online databases	 | modules |
| ncbi-datasets-cli | downloads genomes and .gtf files from online databases | conda |
| fastp | quality control | conda |
| bwa (bowtie) | alignment | conda |
| hisat2 | alignment | conda |
| bedtools | genomics file conversion | conda |
| samtools | genomics file conversion | conda |
| subread | tabulation | conda |
| deeptools | heatmaps & file conversion | conda |

```bash
# First double, triple check that you're in the 2026dsci environment
conda env list

# Install the conda-forge tools:
conda install -c conda-forge ncbi-datasets-cli

# Install the bioconda tools:
conda install -c bioconda fastp bwa hisat2 bedtools samtools subread deeptools

# You may need to press y to install dependencies. Type "y"
# Some things will say they are failing. That's ok. 
# This whole install took ~20 minutes for me
```

> [!WARNING]
> If you see a KILLED message and it looks like it stopped installing, try installing the software one at a time.

Now, let's test and see whether the software you requested was installed successfully. If they weren't installed successfully, you will get an error message

```bash
fastp
bwa
hisat2       #Note: hisat2 will say Error at the bottom because you didn't give it a sequence file. That's ok.
bedtools
samtools
featureCounts
bamCoverage -h
datasets
```

Yay! You have successfully created a conda environment with all the software you'll need for the course! 

---- 

# Summary of Conda 

To summarize: 

  * We started a new conda environment
  * We installed software in it
  * Next time, we will just load conda, activate our environment, and we can use any of this software!

What will we need do we need to do routinely?

1. Switch to the compile node 
2. Load the anaconda module 
3. Activate your 2026dsci environment

```bash
acompile
module load anaconda
conda activate 2026dsci
```

What if you want to load more software into this environment?

```bash
conda install -c <SOFTWARE1> <SOFTWARE2>
conda install -c conda-forge <SOFTWARE1> <SOFTWARE2>
conda install -c bioconda <SOFTWARE1> <SOFTWARE2>
```

Just make sure you are actively in the environment when you are installing new stuff!

