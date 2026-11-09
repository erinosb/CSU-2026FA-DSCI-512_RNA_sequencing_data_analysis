# Pre-Processing & Quality Control with fastp

Let's go ahead and log into [ALPINE through OnDemand](https://ondemand.rc.colorado.edu) if you're not already logged in.

Then go ahead and activate our conda environment.

### Once there:

```
$ acompile --time=2:00:00
$ module load anaconda
$ conda env list
$ conda activate 2025dsci
$ cd /scratch/alpine/<youreID@colostate.edu>/DSCI512   # move to your directory
```

Today, we want to do several things in one step. We want to...

- Assess the overall quality of the data we have obtained
- Remove low Phred-scoring reads
- Remove any adapter sequence that has snuck into our reads
- Remove polyA tails or PolyX tracks. PolyA tails won't align to the genome. And PolyX tracks (homopolymer runs) sometimes arise erroneously through the sequencing process.

We can do all of these tasks with the toolkit **fastp**

### Helpful fastp references

- [fastp github repository](https://github.com/OpenGene/fastp)
- [fastp publication](https://academic.oup.com/bioinformatics/article/34/17/i884/5093234?login=true)

```
$ fastp
```

To play around with fastp, let's make some small **testfiles** that we can use to test out how it works. The files we have obtained have 20 million lines in them. Let's make some small tester files to use instead. These will have 100,000 lines each and we can make them using the head command like so:

```
# First, navigate to the location where you copied and pasted the .fastq files for our demonstration project...

$ cd PROJ01_GomezOrte
$ cd 01_input
$ pwd # make sure you are in your input directory
$ head -n 100000 SRR5832182_1.fastq > tester_SRR5832182_1.fastq
$ head -n 100000 SRR5832182_2.fastq > tester_SRR5832182_2.fastq
```

Next, let's go ahead and start a script file within the script directory where we can play around with writing some fastp code:

- Within the onDemand Dashboard, navigate to `/scratch/alpine/eID@colostate.edu/`
- Navigate to the `~/DSCI512/PROJ01_GomezOrte/02_scripts` directory you created
- Click on `+New File`
- Type in the filename `preProcess_fastp.sbatch`
- Notice your new file. To open it, click on the three vertical dots to the right of the file name and select `Edit`

Go ahead and start your script by

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=2
#SBATCH --time=00:15:00
#SBATCH --partition=atesting
#SBATCH --qos=testing
#SBATCH --output=log-preprocess-%j.out
 
 
# Execute fastp
fastp
```

- Save the code by clicking on the SAVE button.
- Return to the command line, make sure you are in the proper directory (~/DSCI512/PROJ01_GomezOrte/02_scripts)
- Run it!

```
$ pwd # make sure you're in 02_scripts
$ ls # makes ure you can see your script
$ sbatch preProcess_fastp.sbatch
```

- You should see the instructions for fastp printed to the log file.
- This is a best practice! Always start your codes by testing that they are executable with the most basic instance of a command.

Next, let's write a fastp command

- Let's send one set of our tester .fastq files through fastp.
- We will write out the full command line in the script.

The default fastp usage is:

```
# Usage for paired end reads:
fastp -i <in_R1.fastq> -I <in_R2.fastq> -o <out_R1.fastq> -O <out_R2.fastq> [options]
```

The usage we will use today is:

| Option name | Information after | Description |
| ----------- | ----------------- | ----------- |
| -i | ../01_input/tester_SRR5832182_1.fastq | input file for one side of paired-end reads | 
| -I | ../01_input/tester_SRR5832182_2.fastq | input file for the other side of the paired end read | 
| -o | ../03_output/test_EG01_trim_1.fastq | trimmed output file #1 |
| -O | ../03_output/test_EG01_trim_2.fastq | trimmed output file #2 | 
| -h | ../03_output/test_EG01_report.html	| create a report |
| -j | ../03_output/test_EG01_report.json | create another report |
| --detect_adapter_for_pe |  | to switch the adapter removal into paired-end mode |
| --thread | 2 | Number of cores to use (parallel processing) |
| -g |  | this will remove poly-G sequences |
| -x |  | this will remove poly-X sequences |
| -p |  | this will perform over-represented sequence analysis |

By default, fastp tries to discern the adapter and primer sequences based on over-represented sequences. This works pretty well. For future use, you may also want to include a .fasta file containing the list of primer and adapter sequences you used in your library prep. These are available on the company's website.

```
-a, --adapter_sequence               the adapter for read1. For SE data, if not specified, the adapter will be auto-detected. For PE data, this is used if R1/R2 are found not overlapped. (string [=auto])
      --adapter_sequence_r2            the adapter for read2 (PE data only). This is used if R1/R2 are found not overlapped. If not specified, it will be the same as <adapter_sequence> (string [=auto])
      --adapter_fasta                  specify a FASTA file to trim both read1 and read2 (if PE) by all the sequences in this FASTA file (string [=])
      --detect_adapter_for_pe          by default, the auto-detection for adapter is for SE data input only, turn on this option to enable it for PE data.
```

| Option name | Information after | Description |
| ----------- | ----------------- | ----------- |
| --adapter_fasta | <path to a fasta file> | a fasta file containing a the adapter sequences | 

**!!! Exercise:** Go ahead and try to write a line of fastp code using the table above to help you.

- Execute your code using `sbatch preProcess_fastp.sbatch`
- Check for your output in the output directory

<details>
  <summary>Click to see the answer</summary>

---

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=2
#SBATCH --time=00:15:00
#SBATCH --partition=atesting
#SBATCH --qos=testing
#SBATCH --output=log-preprocess-%j.out
 
 
# Execute fastp
fastp -i ../01_input/tester_SRR5832182_1.fastq -I ../01_input/tester_SRR5832182_2.fastq -o ../03_output/tester_EG01_trim_1.fastq -O ../03_output/tester_EG01_trim_2.fastq -h ../03_output/test_EG01_report.html -j ../03_output/test_EG01_report.json --detect_adapter_for_pe --thread 2 -gxp
```

**Hot tip!** You can wrap your long line of code into multiple lines that are interpretted as one line by the shell using `\`.

- This doesn't always work for all browsers.
- You need to make sure that you don't have any white space after the `\`.

```
#!/usr/bin/env bash
 
#SBATCH --nodes=1
#SBATCH --ntasks=2
#SBATCH --time=00:15:00
#SBATCH --partition=atesting
#SBATCH --qos=testing
#SBATCH --output=log-preprocess-%j.out
 
 
# Execute fastp
fastp -i ../01_input/tester_SRR5832182_1.fastq \
-I ../01_input/tester_SRR5832182_2.fastq \
-o ../03_output/tester_EG01_trim_1.fastq \
-O ../03_output/tester_EG01_trim_2.fastq \
-h ../03_output/test_EG01_report.html \
-j ../03_output/test_EG01_report.json \
--detect_adapter_for_pe --thread $SLURM_NTASKS -gxp
```

---

</details>


### Let's review our goals

- Our goal was to **filter out low-quality reads**
- Our goal was to **assess the quality** of the reads before & after filtering
 - Overall, this data looks very good. The quality is quite high throughout, both before and after filtering
- Our goal was to remove **adapters** and other **sequences** that will not align to the genome.
  - **adapters and primers** introduced during library prep
  - **polyA tails**
  - **polyG sequences** - this one is weird. In some Illumina sequencing machines, “G” is denoted by no fluorophore. So these inaccurate polyG reads can accumulate if polymerase fails on a given cluster.

### Adapters can be sequenced on the opposite end of a short insert

<p align="center">
<img width="700" alt="adapter removal" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/adapter_removal.jpg">
</p>



<p align="center">
<img width="700" alt="index and read sequencing" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/index_and_read_sequencing.png">
</p>

Concatamers of primers and adapters can also occur.

### Adapters can be sequenced on the opposite end of a short insert

**!!! Exercise:** Explore your results

- Within the terminal, navigate to `~/PROJ01_GomezOrte/03_output`
- Explore the output
- Using the file navigator panel, navigate to the same output directory.
- **Right-click on the .html file** and download it to your local computer.
- Open the .html file to explore it.

*Note: All these metrics look spectacular in these samples. It appears that the adapters were removed previous to the authors uploading them (as noted in the fastp output). I don't typically do this. I submit my sequences to the repositories unprocessed.*

## Running fastp a second time with looping

OK, that was just the first run of the tester files. What if we want to do the whole list of files?

> make the tester_SRR5832183 files "head -n 100000 SRR5832183_1.fastq > tester_SRR5832183_1.fastq

We'll need to incorporate a loop into our bash script. Let's make a new script called:

**preProcessingWithLooping.sbatch**

```
#!/usr/bin/env bash
 
 
#SBATCH --nodes=1
#SBATCH --ntasks=12
#SBATCH --time=06:00:00
#SBATCH --partition=amilan
#SBATCH --qos=normal
#SBATCH --mail-type=end
#SBATCH --mail-user=<eID>@colostate.edu
#SBATCH --output=log-download-%j.out
 
 
# execute fastp on two paired-end reads of tester files
 
# Initiate a bash array called SRRIDs with two elements
SRRIDs="tester_SRR5832182 tester_SRR5832183"
 
# Loop through the SSRID array and run fastp on each:
for SRRID in $SRRIDs
do
     echo -e $SSRID
 
     fastp -i ../01_input/${SRRID}_1.fastq \
     -I ../01_input/${SRRID}_2.fastq \
     -o ../03_output/${SRRID}_trim_1.fastq \
     -O ../03_output/${SRRID}_trim_2.fastq \
     -h ../03_output/${SRRID}_report.html \
     -j ../03_output/${SRRID}_report.json \
     --detect_adapter_for_pe \
     --thread $SLURM_NTASKS \
     -g -x -p
done
```

- Depending on how you copy-and-paste this script, the slashes may not incorporate properly. A back slash as it is written is just a way to wrap a single line of a command around a carriage return. It can make it easier to read. However, if it is causing problems, remove the back slashes
- So, now you can see that we can use bash arrays and loop over them to make this process more efficient and prevent errors in coding.
- Review and learn more about loops here:
  - [DSCI510 for loops](https://github.com/jesshill/CSU-2025FA-DSCI-510-001_LINUX_as_a_computational_platform/blob/main/Modules/Week4/4-3_Loops.md)
  - [Ryan's tutorials on loops](https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php)
  - [Bash guide for beginners (but is more advanced)](https://tldp.org/LDP/Bash-Beginners-Guide/html/chap_09.html)
- Note that this script will run a little bit different than our first by recycling the SRR ID in the output file.

Continue on to [Automation](../Week_3/3_1_Automation.md)
