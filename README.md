# Best practices for Data Management

This tutorial was created to share some data management tips with other Hargreaves/Schoen lab members. 

## Data storage: Using GitHub for scripts

I use GitHub to share code with collaborators. It makes it easier to edit scripts together, but also to visualize your code without having to download your scripts from online. Since we're already on GitHub, I recommend you take a look around to see how it works. I'm sure there's a tutorial somewhere.
As an example, you could check out my GitHub page on [Sundial lupine population genetics stuff](https://github.com/socameron/lcwgs-lupine). 



## Data storage: Using OneDrive for data

I use McGill's OneDrive to store my data because it's FREE. McGill provides 25GB (?) of free storage for all students, however this can be bumped to a maximum of 1TB if you provide appropriate reasoning (e.g research). Remember that your access to OneDrive will be removed once you graduate, so be sure to store your data elsewhere. If your lab group has a SharePoint server (different from OneDrive!), then you can store up to 25TB of data. This data can also be accessed for all lab members. Currently, Dan and Anna have a SharePoint set-up to store large bioinformatic data plus student data respectively.



## Data storage: Using Borealis for publication data and scripts

[Borealis](https://borealisdata.ca/) provides data storage services and is the preferred host rather than for-profit hosts such as Dryad. It is also used widely by other Canadian Universities and the physical data storage units are located in Canada at the University of Toronto, which is less sketchy than private companies that house data servers in off-shore countries. *Ahem Norway*. I've seen people also post their scripts there, so it appears that you no longer need to post scripts on Zenodo (i.e the equivalent to Dryad). Borealis is also free for Canadian researchers.

## Data anlyses: Using R

I often use `.Rproject` files to ensure accessibility among different compute networks. For instance, they might be useful when working on a laptop in one location and then running scripts on another machine (e.g work vs home). This means you *do not need to use `setwd`*. 

In my Rprojects folder, I often have 3 folders:

1. Rscripts   - for storing R script
2. Rdata      - for storing raw data
3. Routput    - for storing analysis outputs

Within each folder, I often name files with the naming convention of '01_filename' so I can keep track of the file purpose and number. This makes archiving analyses during publication MUCH easier.

*If you'd like to maintain specific R versions and R package versions, then there are multiple ways around this*. Generally, you can use `renv` files which specific the R package versions required. These are stored in a `renv.lock` file. However, `renv` doesn't save the R version so I recommend documenting that piece of information within your scripts. R versions can then be swapped around in the 'Global Options' within RStudio. Here's how to use `renv` : 

```
# After creating an R Project :
install.packages("renv")   # install 'renv' package
library(renv)              # call 'renv' package
renv::init()               # initialize the package in the current working directory

install.packages("tidyverse") # install necessary packages now as in your normal script

renv::snapshot()              # save all R package dependency versions to a `renv.lock` file
renv::restore()               # when operating on a new computer, run to restore the environment with all versions required
renv::dependencies()          # check which dependencies are used for this Rproject

# Tips: To update packages within the renv environment, install the newer version and then run renv::snapshot() again to update renv.lock.

```


*Alternatively, you can use Docker as a way to manage R versions AND R package versions*. I haven't used `Docker` before, but you can read more into it [here](https://colinfay.me/docker-r-reproducibility/).

For each R Script, I will have the following as my header:

```
## Title: Lupinus perennis transplant data (2022 transplants)
## Purpose: cleaning and data prior to running analyses
## Authors: Cameron So
```

For each line of code, I will also provide annotations. It is *extremely* important that you annotate your code. Not really for your collaborators, but moreso for yourself when you return 2 months after fieldwork and you're super confused about what you did. Never forget to annotate!!!! For more information, you can check out Anna's best practices file [here](https://12c7dc14-9b05-8320-8644-794d1c8f6407.filesusr.com/ugd/9e5dfc_93c3b3c3579a4a28a282fd382bafed98.pdf).



## Data analyses: Using Compute Canada / DRAC clusters

DRAC clusters provide high-performance compute resources which may be useful for running large models such as SDMs, etc. It is especially beneficial for bioinformatics, which often uses multiple CPUs and lots of RAM (>40GB) and requires multiple pieces of software to run. You should be familiar with `linux` commands but here's a couple for your interest:

```
cd      # directs you to a folder
ls      # gives you list of files in the working directory. invoke -lh to make it human-readable and listed; this will give you file sizes, permissions, and dates of last edit
mkdir   # creates a folder
pwd     # prints working directory
sbatch  # sends an executable .sh script to SLURM 
squeue  # checks the status of your jobs in SLURM
```

To login into a cluster, use your terminal or powershell client (depends on Mac vs Windows). Use `ssh username@clustername.computecanada.ca` to connect with your cluster. For instance, I would use `ssh socamero@beluga.computecanada.ca`. You will need your password and 2-factor authentication with Duo. See the DRAC website on how to set that up.

Once you've logged into, you'll notice 3 folders: `scratch`, `nearline`, and `projects`. The `scratch` folder is where you'll typically run all of your analyses. This means you will need to enter that folder (with `cd scratch`) and run `sbatch` there. This folder terminates files not in use for 2+ months. Hence, you should not use it for long-term storage. The `projects` folder is used for longterm storage and can be accessed by other members in your lab group. Note that the storage capacity for `scratch` and `projects` respectively is 20TB and 1TB. However, the group manager (i.e Anna Hargreaves) can request additional storage in the `projects` folder given appropriate reasoning.

Once you've noticed how annoying it is to use `mkdir` and `sbatch` constantly, you might be interested in learning how to setup *Snakemake* on your cluster. Snakemake allows you to parallelize jobs without ever having to create directories. It's human readable and improves reproducibility in research. For more information, see my [Snakemake tutorial here](https://github.com/socameron/snakemake-tutorial).


### Notes:

... Eventually I'll post scripts here for example .sh executables and R scripts (for data cleaning) ...

