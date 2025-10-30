---
title: "Components Interactions and Networks WiSe 2024/2025"
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

- 23.10.2025 : <a href='{{ site.url }}{{ site.baseurl }}/downloads/teaching/Transk_Regulation_WiSe2526_Teil1.pdf'>[Lecture 1]</a>
- 30.10.2025 : <a href='{{ site.url }}{{ site.baseurl }}/downloads/teaching/Transk_Regulation_WiSe2526_Teil2.pdf'>[Lecture 2]</a> and practical (see below).

#### Content

The purpose of this lecture is to provide an introduction to the concepts of regulatory genomics in the age of high throughput sequencing. We will introduce 

* the general concepts of transcriptional regulation
* the data of sequencing data that is available (ChIP-seq, RNA-seq, Hi-C, ATAC-seq, ...)
* the role of transcription factors (TFBS binding models, position weight matrices,...)
* the integration of different data layers (example of ChromHMM)
* some insights into bayesian networks modelling


#### Practical application (30.10.2025)

* Download the json file <a href='{{ site.url }}{{ site.baseurl }}/downloads/teaching/CIN-RegGen.json'>[here]</a>
* Load it into the [IGV App](https://igv.org/app/) using the **Session** menu on the top.
* Try to answer the following questions

    * What kind of data is displayed in the app?
    * For each histone mark, try to characterize its shape/localization
    * What is displayed at the bottom of the window?
    * What is the difference between the two RNA-seq tracks at the bottom? (Hint: look exactly at the genes below...)
    * Search for the MYC gene in the search window (top left). Can you identify a regulatory element based on the Hi-C track (you might need to zoom out...). Is there something special at the locus of the regulatory element (histone mark,...)
    * IN the Menu, select "Tracks > ENCODE Other... > " and search for "MCF 10A HiC"; load the "loops" track; this is another cell line, do you note differences to the K562 cell line?

