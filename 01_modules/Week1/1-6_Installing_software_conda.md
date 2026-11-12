<p align="center">
<img width="90%" alt="icons" src="../../05_images/headers_conda.png">
</p>


## Initiate a virtual conda environment 

Another way of installing software is through a **conda virtual environment**. Virtual environments can contain their own combinations of installed software of different versions. In this way, you can switch between different environments for different projects. This is one of the many ways the community is trying to make research more **reproducible**.

**!!! Exercise:** We first need to set up a configuration file in our home directory. We will start a new configuration file specifically for the conda software. This will tell conda where to save and install programs.

Follow along with these steps:

Ensure you are on a compile node
```
$ hostname
```

If you see anything with 'login', execute
``` 
$ acompile
```

See if you are not in your home directory
``` 
$ pwd 
```

If you are not in your home directory, use the `cd` command with no argument to go there.
```
$ cd 
```
 
Now, let's look and see if you already have a `.condarc` file. Some of you may
```
$ ls -alh
```

If you have a `.condarc` file, just look inside of it with `more` or `head` and see what it says.

If you do not have a `.condarc`, file, let's make one
```
$ pwd # make double triple sure you're in your home directory or this won't work
$ nano .condarc
```

Now copy and paste the following code into your file
```
pkgs_dirs:
  - /projects/$USER/.conda_pkgs
envs_dirs:
  - /projects/$USER/software/anaconda/envs
 
# Exit out of nano using
# CTRL + X
# Type Y
# Return
 
$ more .condarc     # do this to check your .condarc file
```

Yay! If you now have a `.condarc` file and see the four lines of code within it specifying your package and environmental paths, you are good to go! You won't need to do this step again on ALPINE.

**!!! Exercise:** Next, we will activate and explore **conda**. 

Conda is a module. To activate conda, use the `module load` command
```
$ hostname # double check you're on a compile node
$ module avail
$ module load anaconda
```

We can list all the virtual conda environments we can currently load
```
$ conda env list

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

The output shows us the default environments that the personnel at CU Boulder have kindly initiated for us to use. It also shows us the environments that we have created. The one we are currently using is marked by an asterisk. Note also, that **(base)** shows up before your prompt... another indication that conda is active and working. 

**!!! Exercise:** Lets build our own custome environment

We want to build a custom virtual environment for this class. To do so...
```
$ hostname      # Ensure first that you're on a compile node.
$ conda create -n 2026dsci python=3.11 # we need this version of python for deeptools and hisat2-inspect to work
 
# You will be asked if you want to proceed. Type y
# When your environment is created, check that it exists:
 
# Check it
$ conda env list
```

You should now see a new virtual environment has appeared called `2026dsci`

To navigate into your new environment, do this
```
$ conda activate 2026dsci
$ conda env list # This shows you which environments are available and selected
$ conda list  # This shows the software currently installed in your active environment
 
# Another key you're on the right track... Your prompt should now start with your conda environment when active
(2026dsci) [jesshill@colostate.edu@c3cpu-a2-u32-1 ~]$ 
```

Yay! You should now have your environment `2026dsci` installed and activated. 

**!!! Exercise:** Let's install software! For this class, we will need the software packages: `fastp` `bwa` `hisat2` `bedtools` `samtools` `subread` `deeptools`

```
# First double, triple check that you're in the 2026dsci environment
$ conda env list #Is it starred?

# To try and install all of the pieces of software at once do the following, however this doesnt always work and they often have to be installed individually. Try anyway. 
$ conda install -c bioconda fastp bwa hisat2 bedtools samtools subread deeptools 
 
# You may need to press y to install dependencies. Type "y"
# Some things will say they are failing. That's ok. 
# It will suggest you update conda. Ignore that.
# This whole install took ~20 minutes for me
```

**NOTE:** If you see a KILLED message and it looks like it stopped installing, try installing the software one at a time. I usually run into trouble when I try to install mulitiple packages at once! 

Now, let's test and see whether the software you requested was installed successfully. If they weren't installed successfully, you will get an error message. 
```
$ fastp
$ bwa
$ hisat2       #Note: hisat2 will say Error at the bottom because you didn't give it a sequence file. That's ok.
$ bedtools
$ samtools
$ featureCounts
$ bamCoverage -h
```

Yay! You have successfully installed and activated your conda environement!

## Summary of Conda 

To summarize, we configured our system to install our software in projects. We only need to do that once.

We also created an environment to use in this class. Again, this is something we just do once.

What will we need do we need to do routinely?

Next time you log into ALPINE, You'll do the following tasks:
1. Switch to the compile node
2. Load the anaconda module
3. Activate your 2026dsci environment

```
$ acompile
$ module load anaconda
$ conda activate 2026dsci
```

What if you want to load more software into this environment?

Use bioconda or conda 
```
$ conda install -c bioconda <SOFTWARE1> <SOFTWARE2>
#OR
$ conda install -c <SOFTWARE1> <SOFTWARE2>
```

Just make sure you are actively in the environment when you are installing new stuff!

## Resources 

- For more information: [Installing software](https://curc.readthedocs.io/en/latest/software/python.html)
- For a list of all avialable bioconda software: [Bioconda](https://bioconda.github.io/conda-package_index.html)
- For a list of all availble conda software: [Anaconda](https://anaconda.org/anaconda/repo)

