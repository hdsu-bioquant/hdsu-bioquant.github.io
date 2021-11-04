---
title: "HDSU - Teaching"
layout: textlay
excerpt: "HDSU -- Teaching"
sitemap: false
permalink: /teaching/
---

# Teaching


Members of the lab are involved in teaching in the [Molecular Biotechnology Bachelor and Master Program](https://www.uni-heidelberg.de/courses/prospective/academicprograms/Molecular_Biotechnology_en_ba.html) at the university Heidelberg

___


## Courses

### Winter semester 2020 / 2021
- MoBi Bachelor 1. FS : [Mathematik A](http://bioinfo.ipmb.uni-heidelberg.de/crg/mathea/)
- MoBi Bachelor 3. FS : [Data Analysis Course](http://bioinfo.ipmb.uni-heidelberg.de/crg/datascience3fs/)
- MoBi Bachelor 5. FS : [Bioinformatics 1](http://bioinfo.ipmb.uni-heidelberg.de/crg/bioinfo1/)
- MoBi Master : [Meet-EU project module](http://bioinfo.ipmb.uni-heidelberg.de/crg/master-meetu/)

### Summer semester 2020
- MoBi Bachelor 2. FS : Mathematik B
- MoBi Bachelor 4. FS : [Data Analysis Project Module](https://datascience-mobi.github.io/)

### Winter semester 2019 / 2020
- MoBi Bachelor 1. FS : [Mathematik A](http://bioinfo.ipmb.uni-heidelberg.de/crg/mathea/)
- MoBi Bachelor 3. FS : [Data Analysis Course](http://bioinfo.ipmb.uni-heidelberg.de/crg/datascience3fs/)
- MoBi Bachelor 5. FS : [Bioinformatics 1](http://bioinfo.ipmb.uni-heidelberg.de/crg/bioinfo1/)

___

## Bachelor thesis projects 2021

Here are some possible topics/projects for students wanting do to their bachelor thesis in our group during the summer semester 2021

### Topic 1 : Building gene-regulatory networks
Gene regulatory networks (GRN) are networks between transcription factors (TF) and target genes, indicating which genes are regulated by which transcription factor. Currently, several methods enable to build (or “infer”) gene regulatory networks from gene expression data: if a gene is regulated by a TF, then the expression profiles of the two should be related. However, such methods require a large number of samples in order to work. Often however, we have a small collection of samples (e.g. patients) for which we have expression data, but insufficient to reliably infer GRNs. One idea is to use **transfer learning**, which means using a large, related dataset to predict a GRN, and fine-tune the GRN using the specific small dataset of interest. I doing so, we can build specific gene regulatory networks for small sets of patients.

The goal of the bachelor thesis will be to test several approaches towards transfer-learning on GRNs, for example Random Forest approaches, and to use different test datasets (e.g. neuroblastoma, glioblastoma,…) to validate the approach, using pre-existing knowledge on the gene regulatory mechanisms

Main aspects:
- data analysis of expression data
- statistical concepts of machine-learning

References
- [Inferring Regulatory Networks from Expression Data Using Tree-Based Methods](https://dx.plos.org/10.1371/journal.pone.0012776)


### Topic 2 : comparison of Type 1 vs Type 2 diabetes with respect to COVID19 comorbidities
Diabetes has been recognized as a major comorbidity of COVID19, and it is established that Sars-Cov-2 can infect pancreatic cells. However, little is known about the difference between Type 1 and Type 2 diabetes. Given the very different ethiology between these 2 diseases, we expect different molecular mechanisms which could play a role towards comorbidity with COVID19. Here, we want to use recently obtained single-cell datasets from Type 2 patients, as well as further datasets of Type 1 patients to compare the perturbed pathways in pancreatic cells of both diseases and identify shared or specific mechanisms which could explain the higher susceptibility. The goal of the bachelor thesis will be to use COVID19 gene signatures that we have previously identified, and compare the expression of these signatures across multiple diabetes datasets.

Main aspects
- data processing of single-cell data; deconvolution of bulk RNA-seq datasets
- representation of the results
- biological interpretation of identified activated pathways

References
- see for example this paper: [Autoantibody-negative insulin-dependent diabetes mellitus after SARS-CoV-2 infection: a case report](http://www.nature.com/articles/s42255-020-00281-8)
- [COVID-19, diabetes mellitus and ACE2: The conundrum](https://doi.org/10.1016/j.diabres.2020.108132)

### Topic 3 : differential “in-silico phenotyping” of tumor and normal tissues
The existence of large RNA-seq datasets of tumor tissue and matching normal tissue allows to conduct comparative studies. In particular, recent approaches allow to determine the activity of pathways and transcription factors from the transcriptomic data, which can be used to understand how pathways and master regulators are jointly activated or seem to have mutually exclusive patterns. In recent projects, we have for example described how mesenchymal phenotypes appear to be tightly related to pathway activation, for example the RAS pathway. The goal of the project is to conduct a large scale analysis of the activity patterns of pathways and master regulators, and to understand how these patterns are perturbed in tumor tissues compared with normal counterpart. We will in particular focus on processes related to ferroptosis across various tumor types to describe how this process is related to other pathways.

Main aspects
- large scale processing of transcriptomic data from TCGA
- implementation of statistical methods to study differential correlation
- visual representation of the data and interactive data mining.
