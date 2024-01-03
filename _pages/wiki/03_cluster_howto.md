---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/cluster_howto/
---

# Working with the Cluster

**Please contact us to get a full documentation about the Cluster** (documentation is internal, hence cannot be published on the wiki...)

## Working with tmux sessions
Losing connection to the server can result in the loss of progress. To avoid this, always work in tmux sessions. Tmux allows you host your session in the server, so if you terminal is killed or you disconnect, you can access your session after reconnecting. 

To initialise a session, run the following line:  

`tmux new -s mysessionname`

to leave the session, press ctrl + b and then press d. To rejoin a session, run:  

`tmux attach -t mysessionname`

If you can’t remember your session’s name, you can run  

`tmux list-sessions`  

to display all active sessions. To delete a session, if you don’t need it anymore, run:

`tmux kill-ses -t mysessionname`  

For more help, you can use the following cheatsheet: https://tmuxcheatsheet.com/



## Submitting jobs

During your work, you will have to process computationally expensive tasks. For more demanding tasks, you might need to submit a job rather than working interactively. These tasks will be simply unfeasible to run locally on your laptops, notebooks or desktop machines due to limitations in memory size (RAM). Some examples of such tasks might be -

* Indexing the genome ~70gb
* Running alignments  ~5-10gb
* Compiling very large Markdown ~20-50gb
* Running NMF ~20gb etc

For such situations you have a dedicated cluster system to your rescue where you will have access to lots of memory, if not infinite ;-)

Some of the task will be quite simple in terms of memory requirements say < 16GB, which means it can easily run in your local machine, laptops etc (given your machine has at least 16 GB of RAM). However, the catch is that you have to repeat this task say 10,000 times. Even if executing one task takes just 1 sec running all the 10K tasks one after another will take ~ 3hrs. In such a situation one can use the cluster to facilitate parallel execution. This means that instead of executing your tasks one after another, you can simultaneously submit a batch of tasks together. Some examples of such tasks might be -

* Permutation
* Bootstrap analysis
* Pairwise correlation computation on large matrix

### Using the BioQuant cluster

This cluster system is a common infrastructure for the entire BioQuant. It is highly recommended that you use either the Mac OS or Linux environment in your local machine. Once you have the login access details, simply open up the terminal on your machine and type the Bioquant cluster SSH command. When logging in the cluster for the first time, immediately change the password.  

The Bioquant cluster includes dedicated nodes for job submission. The queue system is based on SLURM/Sbatch. You can request a full document to the Bioquant IT on the used commands for job submission, inspection, among others.  

To send a job to the queue, you can use this format in the beginning of your job submission script:

```(bash)
#!/bin/bash  
#SBATCH --job-name=JOBNAME  
#SBATCH −W 00:10 		          # walltime ( hours : mins )  
#SBATCH −n 4 			          	# number of tasks  
#SBATCH −-mem 4g 		        	# memory  
#SBATCH −N 2 			           	#reserves 2 cluster nodes  
#SBATCH −−output output.job 	#output filename   
#SBATCH --mail-user=youremail@bioquant.uni-heidelberg.de  
```

A little trick: sometimes you might only have a very simple task that requires more resources. A one-liner wrap also works for job submission:
```(bash)
sbatch --job-name=JOBNAME −W 00:10 −n 4 −-mem 4g −N 2 −−output output.job --wrap=”Rscript [your R script]”
```

Interactive jobs can be created using:
```(bash)
srun [options] --pty /bin/bash
```
You can inspect the status of your job using `squeue` for example.  


### Using the Curry cluster

This cluster system is shared between Rippe, Hoefer and Herrmann labs. Try this only after getting the BioQuant account. The curry cluster can be access from inside the BioQuant (see section above, Using the BioQuant cluster) using the corresponding SSH command. 

After logging the terminal will show curry0 as the node being used. The BioQuant cluster uses a hierarchical jobs system. Curry0 corresponds to the node which sends jobs to all other nodes. We should not use curry0 to work but instead send jobs to other nodes. In order to send jobs we use the qsub command as described below:

```(bash)
#!/bin/bash
SCRIPTDIR="/location/of/the/folder/containing/your/scripts/"
LOGDIR="/location/of/the/folder/to/store/your/error_output/logfiles/"
SCRIPT="NameOfYourScript.R"
```
Specifics to the qsub command can be found here:  

```(bash)
qsub 
-N myCoolJobName            #name of the job
-M myself@email.com         #email id for sending notifications
-m bea                      #send an email when the job begins (b), ended (e) and aborts (a)
-l walltime=00:15:00,mem=1g #launch with these resources - time and memory required
-l nodes=1:ppn=10           #launch with these resources - node and cores required
-o $LOGDIR                  #save output logfile here
-e $LOGDIR                  #save error logfile here
$SCRIPTDIR$SCRIPT           #execute this script
```

Alternatively, interactive jobs can be queued using the following command:  
```(bash)
qsub -I -l walltime=10:00:00,nodes=1:ppn=1,mem=10g
```


## Using Miniconda

### What is Miniconda ?

While working on any long term project, you will soon realize that the softwares, packages etc that you will use for your daily tasks are dependent on one other and with other softwares/packages. For your scripts to run smoothly, all these software and package inter-dependencies much match exactly. Controlling for all these software versions while running a large project will soon become cumbersome.

Conda environments are an elegant solution to such issues. You may consider Conda environment as a box containing all the softwares and packages you need. Furthermore, within this box the correct compatibility of all packages and softwares are ensured. Also, while updating or changing any particular software, the backward compatibility of other depending softwares is ensured. Any compatibility issues will be immediately flagged and you can then rectify this. Fore more information on Conda [see here](https://conda.io/docs/)

Also, in our cluster you will not have the latest version of the softwares you might need. Using Conda you can predefine all the packages and softwares you require to run your script and ensure smooth execution of your code. Together with the Conda environment and your scripts, you will create an independently reproducible module for your analysis. Anyone will be able to replicate your analysis exactly using this module.

### Installing Miniconda

Firstly you will need to download [Miniconda](https://docs.conda.io/en/latest/miniconda.html). Chose the 64 bit version for Linux/Mac OS and the correct Python version. If in doubt select Python 3.7. Follow the steps [given here](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) for installation. Right click on the download link and copy the link address. Now execute the commands below one at a time, step by step.

```(bash)
cd /net/data.isilon/ag-cherrmann/jdoe/ # Go to your personal directory
mkdir miniconda # make this folder, you will download Conda inside this
cd miniconda # enter the folder
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh # Type wget and paste the link you copied above which should be the one shown here
sh Miniconda3-latest-Linux-x86_64.sh # Execute the ***.sh file you downloaded above and follow the instructions. 
# Also install miniconda in this directory and not your home directory
```

### Creating environments

Once you have installed, Conda. You will need to create an environment and fill it with softwares and packages you might need like R, Rstudio, CRAN packages and Bioconductor packages. [Anaconda](https://anaconda.org/anaconda/repo) hosts all the available packages for you Bioinformatics analysis. You can create an environment by following the example below. 

```(bash)
conda create --name myProjectEnvironment

conda activate myProjectEnvironment

# Here I have listed a bunch of packages which is almost always needed change and alter this as required
conda install r-base=3.4.1 r-pheatmap r-dplyr r-data.table r-rcurl r-ggplot2 r-gridextra r-ppcor r-pvclust r-readxl r-writexls r-reshape2 r-rcolorbrewer r-igraph r-dendextend r-tsne bioconductor-consensusclusterplus bioconductor-ensdb.hsapiens.v75 bioconductor-ensembldb bioconductor-genomicfeatures bioconductor-rtracklayer bioconductor-genomicranges bioconductor-interactionset bioconductor-rgraphviz

conda deactivate
```

You can also find a list of useful Conda commands [here](https://docs.conda.io/projects/conda/en/latest/_downloads/1f5ecf5a87b1c1a8aaf5a7ab8a7a0ff7/conda-cheatsheet.pdf)

## Mounting the cluster locally  

Sometimes you might need to access and modify files from the cluster in the same way you do locally. 
VScode allows this through the **Remote - SSH extension**. After installing and setting it up, you can log in remote and your preferred location (along with all the files in it) will become accessible in the Explorer. 

In alternative, to open files from the SSH remotes in the Finder/File Explorer, you can either use **FTP Mounter Lite** (MacOS) or a software like **MountainDuck**.  

## Using the Windows Remote Server 

If you need to use Affinity or any other software, sometimes this can be found through the Bioquant's [Windows Remote Server](quant-ts1.bioquant.uni-heidelberg.de). 

The server includes: -

* 7-zip
* Affinity Designer
* Affinity Photo
* Affinity Publisher
* MS Office 2016 (incl. Access 2016)

The terminal server is reachable from the UniHD network. The server supports RDP, please use the Microsoft Remote Desktop client or any other software supporting RDP to connect to the terminal server.

In the Microsoft Remote Desktop, add pc with the host name/IP address `quant-ts1.bioquant.uni-heidelberg.de`. After, you can log in using bq_username and password. 
You can add specific folders from your local computer to make exporting/importing figures or other files easier by editing PC and adding the desired folder in the Folders tab. 


## Troubleshooting for cluster issues

<figure>
<img src="{{ site.url }}{{ site.baseurl }}/images/issue.png" width="95%">
</figure>
