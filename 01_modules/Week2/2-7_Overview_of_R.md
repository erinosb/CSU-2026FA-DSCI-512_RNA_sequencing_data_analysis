# Upcoming Homeworks 

Homework assignments 2 & 3 focus on learning R via **Swirl**. They are due after fall break on **December 4th**!

## Learning R with swirl

<p align="center">
<img width="400" alt="swirl" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/swirl.png">
</p>

First, you'll need to ensure that you have installed **R** and **R-studio**. Please see [Computer Requirements](../../Computer_Requirements.md) for instructions on how to install those applications.

The homework assignments will be done in R-studio using [swirl](https://swirlstats.com/). So you will need to familiarize yourself with the R-Studio layout. 

<p align="center">
<img width="800" alt="R-studio" src="https://github.com/jesshill/CSU-2025FA-DSCI-512-001_RNA-Sequencing_Data_Analysis/blob/main/Images/r_tutorial_image.jpg">
</p>

Then you will need to **install swirl**! You will only need to do this step once. To install swirl, execute the following command in R

```
> install.packages("swirl")
```

Once installed, you will need to **load the swirl library**. You'll need to do this every time (in an R session) you want to work with swirl. In R, execute... 

```
> library("swirl")
# That loads up the base swirl lesson set. 
# Let's also acquire some more advanced swirl lessons
> install_course("R Programming")
> install_course("Getting and Cleaning Data")
> install_course("Exploratory Data Analysis")
```

If you want to browse different courses to load, check them out...
- Here are some on [Github](https://github.com/swirldev/swirl_courses), which include instructions for lesson installation.
- Here are some on the [swirl website](https://swirlstats.com/scn/title.html#). These are the actual names you'll use to install.

Finally, go ahead and **launch swirl**!

```
> swirl()
```

From here, your interactions with swirl will be fully interactive. Go ahead and follow the instructions within swirl and within the homework assignments to add to your R knowledge.

see the swirl website for more [swirl online resources](https://swirlstats.com/).

Continue on to [Data Aquisition](2_6_Data_Aquisition.md)
