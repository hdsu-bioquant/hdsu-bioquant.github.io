---
title: "Components Interactions and Networks 2022/2023"
layout: textlay
excerpt: "CIN"
sitemap: false
permalink: /teaching/CIN
---
## Medizininformatik - Components Interactions and Networks

### Introduction into Regulatory Genomics

#### Teachers

* **Carl Herrmann** (carl.herrmann (at) uni-heidelberg.de)

#### Dates

- 14.11.2024 : [lecture 1]({{ site.url }}{{ site.baseurl }}/teaching/downloads/CIN2223_lecture1.pdf)
- 21.11.2024 : [lecture 2]({{ site.url }}{{ site.baseurl }}/teaching/downloads/CIN2223_lecture2.pdf) and [practical]({{ site.url }}{{ site.baseurl }}/teaching/downloads/CIN-WiSe2223_RegGen.json)

#### Content

The purpose of this lecture is to provide an introduction to the concepts of regulatory genomics in the age of high throughput sequencing. We will introduce 

* the general concepts of transcriptional regulation
* the data of sequencing data that is available (ChIP-seq, RNA-seq, Hi-C, ATAC-seq, ...)
* the role of transcription factors (TFBS binding models, position weight matrices,...)
* the integration of different data layers (example of ChromHMM)
* some insights into bayesian networks modelling


#### Practical application (21.11.2024)

* Download the json file [here]({{ site.url }}{{ site.baseurl }}/teaching/downloads/CIN-WiSe2223_RegGen.json)
* Load it into the [IGV App](https://igv.org/app/)
* Try to answer the following questions

    * What kind of data is displayed in the app?
    * For each histone mark, try to characterize its shape/localization
    * What is displayed at the bottom of the window?
    * What is the difference between the two RNA-seq tracks at the bottom?
    * Search for the MYC gene in the search window (top left). Can you identify a regulatory element based on the Hi-C track (you might need to zoom out...). Is there something special at the locus of the regulatory element (histone mark,...)

