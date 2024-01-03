---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/rstudio_server/
---

# RStudio Server & Jupyter

You can access an RStudio Server and a Jupyterhub instances on the Curry cluster on the following links:
- [RStudio Server](https://curry0.bioquant.uni-heidelberg.de/rstudio/)
- [Jupyterhub](https://curry0.bioquant.uni-heidelberg.de/jupyter/)



## RStudio Server on Curry

By default, the Rstudio Server instance has many packages installed and can be readily used for many basic analyses. However, as new versions of packages are released and the content of your package library is highly dependent on every project, it is recommended to create a self contained library for your projects.

### Configure renv in the RStudio Server

renv is an package that helps to install a self-contained library of R packages, independently from the system R library. You can check the path of the current libraries with the command:

```(R)
.libPaths()
```

The RStudio server configuration is recorded on the **Rprofile.site** config file used by the system. However, the default configuration loads the package _devtools_ at the start of the R session. This forces R to use the devtools version installed for the whole system, and ignore a new version installed with renv.

To solve this problem we have to create our own Rprofile.site config file. Please follow these instructions to correctly configure _renv_ for your user: 

1. Check if you have an Rprofile.site file in your home directory **~/Rprofile.site**, if you don't create a new one and add the following lines:

    ```(R)
    .First <- function() {
    options(
        repos = c(CRAN = "https://cran.rstudio.com/"),
        browserNLdisabled = TRUE,
        deparse.max.lines = 2)
    }
    ```

2. To tell R which profile file to use we have to modify **~/.Renviron** to look like this:

    ```(R)
    RENV_CONFIG_USER_PROFILE = FALSE

    # Set R profile to avoid loading devtools
    R_PROFILE="/home/bq_aquintero/Rprofile.site"

    ```

### Using renv

After correctly configuring your user to use _renv_, you can initialize and mange your R libraries with just a few commands:

1. Call renv::init() to initialize a new project-local environment with a private R library,
2. Install any needed packages, these will be saved on the private R library.
3. Call renv::snapshot() to save the state of the project library.
4. Call renv::restore() to revert to the previous state if a problem emerged after installing a new package.

For more instruction on how to use _renv_, please read the [official documentation](https://rstudio.github.io/renv/articles/renv.html).


## JupyterHub

On the Curry cluster, you can use a Jupyter server to code and run your programs. The difficult part of working on a cluster is often the installation of the tools in an environment where you are not an administrator. To avoid the potential troubles you can use conda (see the dedicated part on how to use conda on the curry cluster) to create environments for your programs.

Those environments, once created, can be used as kernels by Jupyter to run your programs. To use an environment as a kernel, follow the steps as described below (two different methods, one for Python kernels and one for R kernels).  

The first step for both methods is to connect via a terminal to the Curry cluster (see the section about cluster connection).

### Python kernel

Use conda environment as kernel for python: 

```(bash)
conda create -n my-conda-env # creates new virtual env 
conda activate my-conda-env # activate environment in the terminal 

conda install ipykernel # install Python kernel in new conda env 
ipython kernel install --user --name=my-conda-env-kernel # configure Jupyter to use Python kernel
```

Now, when connecting to Jupyter (https://curry0.bioquant.uni-heidelberg.de/jupyter) you can see your kernel in the top right corner kernel selection menu of your script.

### R kernel 

Due to the old installation of system packages on the cluster, the process for an R kernel is different. Nevertheless, the same principles are applied here, we create a conda environment with some specific packages, install what we need, and then declare the environment as a kernel:

```(bash)
conda create --name seurat_env
conda activate seurat_env
conda install -c conda-forge r-base
conda install r-irkernel
conda install -c anaconda ipykernel
conda install nb_conda_kernels
conda install -c conda-forge r-htmltools
conda install -c conda-forge r-rlang
conda install -c conda-forge r-pillar

R -e "install.packages('lifecycle',repos='http://cran.us.r-project.org')"
R -e "install.packages('vctrs',repos='http://cran.us.r-project.org')"
conda run Rscript -e 'IRkernel::installspec(name="irS", displayname="R seurat")'
```


