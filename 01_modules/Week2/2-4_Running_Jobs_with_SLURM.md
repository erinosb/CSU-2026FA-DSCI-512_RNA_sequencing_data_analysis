# Overview of Running Jobs with SLURM 

## Resources

- Research Computing at Boulder - [Guide to running batch jobs](https://curc.readthedocs.io/en/latest/running-jobs/batch-jobs.html)
- Research Computing at Boulder - [Guide to SLRUM directives](https://curc.readthedocs.io/en/latest/running-jobs/job-resources.html)
- Research Computing at Boulder - [Alpine Hardware](https://curc.readthedocs.io/en/latest/clusters/alpine/alpine-hardware.html)
- SCHEDMD - [Guide to SLURM](https://slurm.schedmd.com/reservations.html#:~:text=Reservation%20Use&text=This%20name%20is%20automatically%20generated,completely%20within%20the%20named%20reservation.)

## A review of vocabulary words

- **High-Performance Computing (HPC):** this is the field of computing that uses **supercomputers**, **compute clusters**, or **servers** to solve computational tasks. It is most often used in STEM research fields but has also expanded to business and industry fields.
- **Supercomputers** and **compute clusters** are both architectures of computers in which multiple, small computers are stitched together so they function as a single, big computer. They differ in their scale, with the supercomputer term being used for larger systems.
- **Servers:** this is another catch-all term that can also refer to a compute cluster (of varying sizes, some are small) but implies that the system is hosting some function for users.
- **Nodes** are the individual, small computing units of which a supercomputer is comprised. Individual nodes are like mini-computers, each with its own CPUs, memory, and a bit of storage space.
  - **Types of Compute Nodes:** Compute nodes are the nodes on the ALPINE system where we run jobs. They come in a few different types: **GPU** (graphical processing units), **HighMEM** (high memory nodes), and **Haswell** (typical, garden-variety, CPU)
  - **Node design:** Each compute node can support multiple tasks. These tasks are referred to as either **processes** or **threads** or **cores**. This is similar to having a quad-CPU in your laptop that can support 4x processes at a time. The Haswell Nodes on ALPINE can support up to 24 processes. On ALPINE, each compute node will support 64 processes.

## The ALPINE System: 

<p align="center">
<img width="700" alt="alpine map" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/alpine_map.jpg">
</p>

### Where do we run jobs when we are working on ALPINE?

- Try to avoid running jobs on the **login node!**
- For small jobs and testing (single node) - these can be run directly from the command line on a **compile node**.
  - Use `acompile` to switch from a **login node** to a **compile node**. 
- For larger jobs, we submit the job to a **compute node**

### How do we submit a job to the compute node?

- We use the **job scheduler** called **SLURM**
- A [job scheduler](https://en.wikipedia.org/wiki/Job_scheduler) is a specialized piece of software used in **High Performance Computing** to send jobs to specialized nodes. 

## Quick SLURM Intro 

The job sharing utility that is installed on ALPINE is called **SLURM** and we use it by writing and executing **batch scripts**. Batch scripts are just specialized bash scripts that are executed using the command `sbatch`. Batch scripts do everything that bash scripts do, but in addition, **they request a certain amount of the supercomputer for use and request a certain amount of time on that hardware**. When we execute batch scripts, they don't run right away. Instead, they get in the queue and are assigned to a node based on the requested hardware, requested time, and availability.

**!!! Exercise:** Follow along. Let's do some SLURM practice.

- First, navigate to the space `/scratch/alpine/$USER`
- Let's make a new directory called `test_slurm`
- `cd` into the `test_slurm` sub-directory
- Next, let's make a practice shell script.
- Start a file called `testscript.sh` using `touch` or `nano`.
- Open `testscript.sh` by navigating to DASHBOARD, FILES, and editing the file. OR you can use nano.

### Write the following code into `testscript.sh`

```
#!/usr/bin/env bash
 
echo "hello"
sleep 30
echo "world"
sleep 30
```

This script is run with the command:

```
$ bash testscript.sh
```

The script prints Hello, waits 30 seconds, then prints world, then waits 30 seconds.

To run jobs using SLURM, we take a typical `.sh` script and convert it to an `.sbatch` script. To turn a bash script into an sbatch script we just need to do a few things...

1. change the file extension
2. add directives
3. execute with the command `$ sbatch <yourscript.sbatch>`

Sbatch scripts look like so...
```
#!/bin/usr/env bash
 
## Directives go here...
#SBATCH <SLURM commands>
#SBATCH <SLURM commands>
#SBATCH <SLURM commands>
#SBATCH <SLURM commands>
#SBATCH <SLURM commands>
#SBATCH <SLURM commands>
 
## Software is loaded here...
module purge
module load <desired software>
 
## Your regular old shell script goes here...
echo "hello"
sleep 30
echo "world"
sleep 30
```

**!!! Exercise:** Copy the `.sh` script into an `.sbatch` script called `testscript.sbatch`

```
$ cp testscript.sh testscript.sbatch
```

**!!! Exercise:** open `testscript.sbatch` in a text editor and convert it to an `.sbatch` script by adding the following directives:

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --time=0:1:30
#SBATCH --partition=atesting
#SBATCH --qos=testing
#SBATCH --output=sample-%j.out
 
## Software is loaded here...
module purge
 
## Code
echo "hello"
sleep 30
echo "world"
sleep 30
```

Submit this script to the SLURM job scheduler using this command:

```
$ sbatch testscript.sbatch
```

Use the following commands to interact with the progress of your script:

```
$ squeue -u $USER    # to see all the scripts you, the user, are running
$ sacct -u $USER              # See some stats about running or previously run jobs
$ sacct -X --format JobID, JobName,AllocCPUS,State,ExitCode,Elapsed,TimeLimit,Submit,Start,End  # A nicely formatted status output
$ scancel <jobID#>   # to cancel a script
```

Do you see a log file that was created? Peek into it to explore it.

**??** What just happened? 

**??** What do all the #SBATCH lines mean? 

Everything after an `#SBATCH` is a **SLURM directive**. Think of these like **options**. You're telling SLURM details about how you want your script to be run.

```
#SBATCH --nodes=1   <-- Requests 1 node
#SBATCH --ntasks=1  <-- Requests 1 core (can be up to 64 per single node)
#SBATCH --time=0:1:30   <-- Requests 1 minute & 30 seconds of time. Job will stop then
#SBATCH --partition=atesting  <-- Requests a "testing partition"
#SBATCH --qos=testing  <-- Used for all testing partitions
#SBATCH --output=sample-%j.out <-- saves the output in a file named sample-%j.out where %j is a variable where the job number will output
```

More info: 
- Research Computing at Boulder - [Guide to running batch jobs](https://curc.readthedocs.io/en/latest/running-jobs/batch-jobs.html)
- Research Computing at Boulder - [Guide to SLURM directives](https://curc.readthedocs.io/en/latest/running-jobs/job-resources.html) 

## Making SLURM easier to use

Because the `squeue -u $USER` command is cumbersome to type, let's make some aliases of shortcut commands.

**!!! Exercise:** Let's navigate to our home directory and add some aliases to our `.bashrc` file

```
# cd with no argument returns you to your home directory
$ cd 
$ pwd
```

Go to the DASHBOARD. Select FILES. Edit the file `.bashrc`

Copy and paste the following lines of code to the end of the `.bashrc` file:

```
#Alises
alias scheck="squeue -u $USER"
alias sa='sacct -X --format JobID,JobName,AllocCPUS,State,ExitCode,Elapsed,TimeLimit,Submit,Start,End'
alias suser="sacct -u $USER"
```

- Save the file
- Close it
- Return to the prompt and the home directory
- To activate your new `.bashrc`, source it...

```
$ source .bashrc
```

Now test your new aliases: 

```
$ scheck
$ sa
$ suserc
```

**!!! Exercise:** Try running `testscript.sbatch` again, this time with your new command aliases!

## Some hackable sbatch scripts

The example script above is great for a nice short, quick run. It's capabilities will be similar to a laptop but allow for a 2 hour job to complete.

#### The intensive run

This run takes advantage of a full node but may take a while to start. (Remember: Max cores per Haswell node = 24; Max time = 24 hours)

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=24
#SBATCH --time=12:00:00
#SBATCH --partition=amilan
#SBATCH --qos=normal
#SBATCH --mail-type=end
#SBATCH --mail-user=tstark@colostate.edu # replace this with your own e-mail to receive an e-mail update when this job is complete
#SBATCH --output=log-download-%j.out
 
echo "hello"
sleep 30
echo "world"
sleep 30
```

Real runs or intensive runs: 

- **amilan** nodes have either 32, 48, or 64 cores. However, there aren't many with 64 cores, so if you run a job with `–nodes=1 and –ntasks=64` you will need to wait until that resource is available. It might be a long wait
- Try requesting **amilan** nodes with a max of 32 ntasks for best performance
- The max time you can request in 24 hours
- If you're waiting for your job to start and want to get an estimate of when it will start, try your `sa` command alias

#### The tester run

This will run right away and is great for testing if things work. (Remember: Max nodes for a testing run = 2; Max processes for a testing run = 24 total; Max time is 30 min)

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=2
#SBATCH --time=00:10:00
#SBATCH --partition=atesting
#SBATCH --qos=testing
#SBATCH --output=log-download-%j.out
 
echo "hello"
sleep 30
echo "world"
sleep 30
```

Continue on to [Overview of R](2_5_Overview_of_R.md) 
