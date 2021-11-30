---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/cluster_access/
---

# Getting access to the cluster

There are 2 clusters available for working:

1. **BioQuant Cluster**: this is the general cluster of the institute;
2. **Curry cluster**: this is the cluster we share with the Rippe group, which is dedicated to single-cell analysis, and comes equiped with an RStudio server and a Jupyter server

In addition, we have a **DGX GPU workstation** on which you can run applications optimized for GPU architecture.

## BioQuant cluster

This cluster system is a common infrastructure for entire BioQuant. It is highly recommended that you use either the Mac OS or Linux environment in your local machine. Once you have the login access details, simply open up the terminal on your machine and type -

```{bash}
ssh -X username@login2.bioquant.uni-heidelberg.de
# OR simply
ssh -X username@login2

If your name is Jane Doe, username should be ```bq_jdoe```


# Once you hit enter, you might be asked - 
#-----------------------------------------------------------------------------------------------
# the authenticity of host can't be established, and would you still want to connect. Type yes.
#-----------------------------------------------------------------------------------------------
# Next you will need to provide the password provided in the login access sheet
# Voila !! you are in the cluster
```

When logging in the cluster for the first time, immediately change the password. Simply type -

```{bash}
passwd

# Provide your current password, followed by a new password twice as asked
```

## Curry cluster

This cluster system is shared between Rippe and Herrmann labs. Try this only after getting the BioQuant account. 

**Access to this cluster requires a specific registration: talk to Andres Quintero for this!**

Note that access to this cluster requires to be within the university network; if you are working from home, you need a vpn access (every student of Heidelberg University has a VPN access through its Uni-ID)

```{bash}
ssh -X username@curry0.bioquant.uni-heidelberg.de
# OR simply
ssh -X username@curry0

# Username and Password is the same as for Bioquant
```

Once you login, this is your home folder, you can now create your project folder, install conda, submit jobs etc directly here. More on how to do all this in the next sections. In this tutorial, we will assume that you are using the BioQuant cluster. But in principle all the next steps can also be done in the Curry cluster.



## Creating your project folder

### Group storage area

The next step will be to create your working directory in our group folder. You are already logged into the cluster (Bioquant), now simply type the following commands in your terminal -

```{bash}
cd /net/data.isilon/ag-cherrmann/
ls 
```

Here all the users (members of the lab) will create their personal work directories. 

> Kindly follow the nomenclature of **f**irst name **last name** (all in small caps) while creating your personal work directories. So for instance **Jane Doe** will be **jdoe**.

```{bash}
mkdir jdoe # NOTE - use your own naming scheme instead of jdoe
```

Next you should go into your personal directory and create your project folder. Use an easily understandable project folder name.

> Never have spaces in your folder name, use a combination of small and upper caps for easily readable folder names !!

```{bash}
mkdir myProjectName
```

On the curry cluster, the same storage area is mounted under

```{bash}
/media/ag-cherrmann
```



### Your folder structure

For simplicity and future interpretability we encourage that every project folder follows the following basic sub-folder structure. Of course you can add more folders or sub-sub folders as required but try to maintain this basic internal structure for improving readability for third persons.

```{bash}
-myProjectName
  ├── analysis    # for storing all the results generated from the analysis
  ├── data        # for storing all the input data required for the analysis
  ├── src         # for storing all your scripts
  ├── logs        # for storing all the intermediate log files generated while running your scripts
  ├── temp        # a temporary buffering folder to be used as you please
  ├── docs        # for storing all the papers relevant to this project, presentations, project report etc
  └── README.txt  # detailed description of the project, scripts etc
```

> PROTIP - Name the scripts in your src folder as 1_script.R, 2_script.py, 3_script.sh ... The numbering should follow the ordering of script execution. This will be very helpful for others trying to execute/understand your scripts later on. Similarly you could names files/folders in the analysis folder as 1_result.pdf, 2_result.xlsx, 3_result.jpeg ...

### The ```hdsu``` folder

At our parent group storage location ```/net/data.isilon/ag-cherrmann/```, we have a special ```hdsu``` folder created. This is not a user specific folder but a lab specific folder and will be used by all. Its structure is as follows -

```{bash}
cd /net/data.isilon/ag-cherrmann/ # Go to group root directory
ls -al                            # List all available folders
cd hdsu                           # Enter hdsu folder
tree -L 1                         # Explore its structure

#-hdsu
#├── condaenvs
#│   └── yamls
#├── data
#├── softwares
#└── tmpshare
```

__condaenvs__ will have the soft links to Conda environments for various projects. If you want to share your Conda environment just create a **soft link** here and others can simply use it by -

 ```conda activate /net/data.isilon/ag-cherrmann/hdsu/condaenvs/<env name>``` 
 
Every shared condaenv should also have its corresponding **.yaml** file copied in ```/conda/yamls/``` so that in case someone wants to build the environment from scratch, will be able to do so.

> We will talk about what is Conda in the next section !!

__data__ will contain all the common set of data that everyone can use like TCGA, GTex, Roadmap, Depmap, Genome index files, Gencode data (gtf files) etc. Whenever you download any public data, consider using this folder. Always create a directory for the data downloaded along with a README file (containing date of download, Version of the file, download URL etc) and a script for how to download the data (R/bash scripts with wget commands etc).

__softwares__ will have common set of software that everyone uses but is not available in the cluster. Note use this only if something is not available via Conda. Raise this issue in lab meetings discuss with others before putting up a software here. Once a software is here inform others about this.

__tmpshare__ sometime we quickly need to copy some data, files, docs etc for others to read. Use this folder for that. Keep in mind this folder is just a buffering location and will be cleaned periodically. Don’t keep important stuff here !!

## Using Miniconda

### What is Miniconda ?

While working on any long term project, you will soon realize that the softwares, packages etc that you will use for your daily tasks are dependent on one other and with other softwares/packages. For your scripts to run smoothly, all these software and package inter-dependencies much match exactly. Controlling for all these software versions while running a large project will soon become cumbersome.

Conda environments are an elegant solution to such issues. You may consider Conda environment as a box containing all the softwares and packages you need. Furthermore, within this box the correct compatibility of all packages and softwares are ensured. Also, while updating or changing any particular software, the backward compatibility of other depending softwares is ensured. Any compatibility issues will be immediately flagged and you can then rectify this. Fore more information on Conda [see here](https://conda.io/docs/)

Also, in our cluster you will not have the latest version of the softwares you might need. Using Conda you can predefine all the packages and softwares you require to run your script and ensure smooth execution of your code. Together with the Conda environment and your scripts, you will create an independently reproducible module for your analysis. Anyone will be able to replicate your analysis exactly using this module.

### Installing Miniconda

Firstly you will need to download [Miniconda](https://docs.conda.io/en/latest/miniconda.html). Chose the 64 bit version for Linux/Mac OS and the correct Python version. If in doubt select Python 3.7. Follow the steps [given here](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) for installation. Right click on the download link and copy the link address. Now execute the commands below one at a time, step by step.

```{bash}
cd /net/data.isilon/ag-cherrmann/jdoe/ # Go to your personal directory
mkdir miniconda # make this folder, you will download Conda inside this
cd miniconda # enter the folder
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh # Type wget and paste the link you copied above which should be the one shown here
sh Miniconda3-latest-Linux-x86_64.sh # Execute the ***.sh file you downloaded above and follow the instructions. 
# Also install miniconda in this directory and not your home directory
```

### Creating environments

Once you have installed, Conda. You will need to create an environment and fill it with softwares and packages you might need like R, Rstudio, CRAN packages and Bioconductor packages. [Anaconda](https://anaconda.org/anaconda/repo) hosts all the available packages for you Bioinformatics analysis. You can create an environment by following the example below. 

```{bash}
conda create --name myProjectEnvironment

conda activate myProjectEnvironment

# Here I have listed a bunch of packages which is almost always needed change and alter this as required
conda install r-base=3.4.1 r-pheatmap r-dplyr r-data.table r-rcurl r-ggplot2 r-gridextra r-ppcor r-pvclust r-readxl r-writexls r-reshape2 r-rcolorbrewer r-igraph r-dendextend r-tsne bioconductor-consensusclusterplus bioconductor-ensdb.hsapiens.v75 bioconductor-ensembldb bioconductor-genomicfeatures bioconductor-rtracklayer bioconductor-genomicranges bioconductor-interactionset bioconductor-rgraphviz

conda deactivate
```

You can also find a list of useful Conda commands [here](https://docs.conda.io/projects/conda/en/latest/_downloads/1f5ecf5a87b1c1a8aaf5a7ab8a7a0ff7/conda-cheatsheet.pdf)

