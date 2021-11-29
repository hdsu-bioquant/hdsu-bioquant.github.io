---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/cluster_howto/
---

# Working with the Cluster

**Please contact us to get a full documentation about the Cluster** (documentation is internal, hence cannot be published on the wiki...)

## Jobs with huge memory requirements

During the course of your work, you will have to process computationally expensive tasks. These tasks will be simply unfeasible to run locally on your laptops, notebooks or desktop machines due to limitations in memory size (RAM). Some examples of such tasks might be -

* Indexing the genome ~70gb
* Running alignments  ~5-10gb
* Compiling very large Markdown ~20-50gb
* Running NMF ~20gb etc

For such situations you have a dedicated cluster system to your rescue where you will have access to lots of memory, if not infinite ;-)

## Repetitive tasks

Some of the task will be quite simple in terms of memory requirements say < 16GB, which means it can easily run in your local machine, laptops etc (given your machine has at least 16 GB of RAM). However, the catch is that you have to repeat this task say 10,000 times. Even if executing one task takes just 1 sec running all the 10K tasks one after another will take ~ 3hrs. In such a situation one can use the cluster to facilitate parallel execution. This means that instead of executing your tasks one after another, you can simultaneously submit a batch of tasks together. Some examples of such tasks might be -

* Permutation
* Bootstrap analysis
* Pairwise correlation computation on large matrix

