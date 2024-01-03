---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/cluster_access/
---

# Getting started  

## Getting access to the cluster

There are 2 clusters available for working:

1. **BioQuant Cluster**: this is the general cluster of the institute;
2. **Curry cluster**: this is the cluster we share with the Rippe group, which is dedicated to single-cell analysis and other tasks. It comes equipped with an RStudio Server and a Jupyter server.

In addition, we have a **DGX GPU workstation** on which you can run applications optimized for GPU architecture. For access to the workstation, contact Daria.

You might also need to access other clusters which we use mostly for heavier data storage or more computationally demanding tasks.
1. **BQ-storage:** registration with the Bioquant IT and login with Kerberos-ticket is required. We mostly use it for large datasets. 
2. **DKFZ cluster:** Part of the DKFZ infrastructure. You will only have access if you are taking part in a project together with DKFZ research groups. 
3. **BW-cluster:** part of a state-wide initiative of Baden-Württemberg aimed for cooperative provision of resources and service structures. You can find more information here.
4. **SDS@hd:** part of the same initiative as BW-cluster, but more focused on data storage. More information here.

In order to safely access a document containing SSH commands for the clusters you will use, refer to [this](https://docs.google.com/document/d/1xTSGF7gOYrq-1-SxJcBTAbc3K4H1yOZELgM_iUwFrNM/edit?usp=sharing) Google document.



## Creating your project folder

### Group storage area

After getting access and changing your password, the next step will be to create your working directory in our group folder. You are already logged into the cluster (Bioquant), now simply type the following commands in your terminal -

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

## The ```hdsu``` folder

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

## Public data downloads

Throughout your stay with our group, you might use public data in your project. 
Depending on the nature of your project, here are some repositories which can help you find datasets: 

1. GEO (largest functional genomics repository with raw and processed data published on all sorts of technologies - from microarray to single-cell);
2. ENCODE (large repository specialised on gene regulation containing data from undisturbed cell lines and tissues - raw and processed using the same methodologies);
3. TCGA (standard cancer genomics database).

Smaller databases but specialised for single-cell datasets (mainly already processed):  
1. UCSC Cell Browser (serves both as repository and explorer of processed single-cell dataset);
2. Single Cell Portal (repository to processed data on ~600 single-cell studies);
3. CZ CELLxGENE (single-cell, includes processed datasets).  

If you think a dataset you downloaded could be useful to others and/or is relatively large (more than ~100GB), you can add it to the [groups’ dataset sheet](https://docs.google.com/spreadsheets/d/1ildocNb9Bi8girF1vFy6kn9aARzHem3KJo-RegT1cOU/edit?usp=sharing).  

Such datasets should ALWAYS be accompanied by a READme indicating when they were downloaded, and any differences in version(s), among other features which can help anyone else using them.  

On the other hand, you should also refer to this sheet to verify if the data you are downloading is already stored in our cluster. 




