---
title: "Studienstiftung - Workshop 2025"
layout: textlay
excerpt: "scworkshop"
sitemap: false
permalink: /teaching/workshop_singlecell
---


# Pathways of Bioinformatics Workshop - Studienstiftung

## Introduction to Single-cell omics: computational dissection of the mouse brain

17.05.2025

Prof. Dr. Carl Herrmann - Bioinformatics Department - IPMB


### Course Overview

This hands-on laboratory introduces undergraduate students to single-cell and spatial transcriptomics analysis of the mouse brain cortex. Students will explore cell type diversity and spatial organization using both graphical user interfaces and basic coding in R. The course utilizes the "Shared and distinct transcriptomic cell types across neocortical areas" dataset from CellxGENE for single-cell analysis and the mouse cortex spatial transcriptomics dataset from the Seurat Visium vignette.


---

## SCHEDULE

**Morning Session (10:30 - 12:00)**
- Introduction to Single-Cell Transcriptomics and Mouse Cortex (30 min)
- [Hands-on Session 1](#lab1): CellxGENE Exploration of Mouse Cortex (1 hour)

**Afternoon Session (14:00 - 16:30)**
- [Hands-on Session 2](#lab2): Spatial Transcriptomics Analysis (1 hour)
- Introduction to R/Seurat (30 min)
- [Hands-on Session 3](#lab3): Integrative Analysis in R/Seurat (1 hour)

---

## MORNING SESSION (10:30-12:00)

### LECTURE: INTRODUCTION TO SINGLE-CELL TRANSCRIPTOMICS AND MOUSE CORTEX (30 MIN)

#### Single-Cell RNA Sequencing Technology
- Basic workflow: tissue dissociation → single-cell isolation → library preparation → sequencing
- Key concepts: UMIs (Unique Molecular Identifiers), droplet-based technology, cell barcoding
- Data processing steps: quality control, normalization, dimensionality reduction
- Visualization approaches: UMAP, t-SNE, heatmaps

#### Mouse Cortex Anatomy and Cell Types
- Overview of mouse cortex regions and layers
- Major cell types in the neocortex:
  * Excitatory neurons (layer-specific subtypes)
  * Inhibitory interneurons (PV+, SST+, VIP+, etc.)
  * Glial cells (astrocytes, oligodendrocytes, microglia)
  * Vascular cells and others
- Functional organization of the visual cortex
- Cross-area comparison of cortical regions

#### Introduction to CellxGENE
- Overview of CellxGENE as a resource for exploring single-cell data
- Features and functionality of the CellxGENE browser
- Dataset information: "Shared and distinct transcriptomic cell types across neocortical areas"

---

<a id="lab1"></a>
### HANDS-ON SESSION 1: CELLXGENE EXPLORATION OF MOUSE CORTEX (1 HOUR)

#### Part 0: Refresh your mouse brain anatomy knowledge (10 min)

1. Navigate to the [Allen Brain Mouse Atlas](http://atlas.brain-map.org/atlas?atlas=2)
2. Explore the different anatomical regions and subregions of the mouse brain.
3. Identify the localization of the layers of the visual cortex.

#### Part 1: Getting Started with CellxGENE (10 min)

1. Navigate to the CellxGENE website: https://cellxgene.cziscience.com/
2. Click on *Datasets* on the top, and use the filters to find the mouse dataset in *primary visual cortex* named *Shared and distinct transcriptomic cell types across neocortical areas* (it should have 22,375 cells)
3. Click on the description to understand what this dataset is about (Publication, single-cell protocol, ...)
4. Click on the dataset ('Explore') to open it in the CellxGENE browser. It might take a moment to load...
5. Take a moment to explore the interface:
   - Right panel: gene search and cell selection tools
   - Center: UMAP visualization of cells
   - Left panel: color legend and metadata information

> **Questions for part 1:**
> what is the Smart-seq protocol? Ask your favourite search engine or AI friend...

#### Part 2: Exploring Cell Types in Mouse Cortex (20 min)

1. **Understanding the dataset structure**
   - Look at the UMAP visualization where each dot represents a single cell
   - Use the dropdown menu to color cells by "cell_type"
   - Observe the distinct clusters representing different cell types
   
2. **Exploring cell type markers**
   - Search for and visualize key neuronal markers:
     * Slc17a7 (excitatory neuron marker, also known as Vglut1)
     * Gad1 (inhibitory neuron marker)
     * Sst, Pvalb, Vip (interneuron subtype markers)
   - Observe their expression patterns across the UMAP
   
3. **Exploring glial cell markers**
   - Search for and visualize glial markers:
     * Aldh1l1 (astrocyte marker)
     * Mobp (oligodendrocyte marker)
     * P2ry12 (microglia marker)
   - Compare expression patterns between neuronal and glial populations

> **Intermediate Questions for Part 2:**
>
> 1. Compare the expression patterns of Sst, Pvalb, and Vip. Do these interneuron markers show overlapping or distinct expression patterns?
> 2. Do the GABAergic neurons represent a homogeneous cell population?
> 3. Identify one additional marker gene for each major cell type by searching for correlated genes in CellxGENE. What genes did you find?
> 4. Using the differential expression feature, identify the top 3 genes that distinguish astrocytes from oligodendrocytes. (For this, check [these slides ('Find marker genes')](https://cellxgene.cziscience.com/docs/04__Analyze%20Public%20Data/4_1__Hosted%20Tutorials))

#### Part 3: Regional Differences in the Cortex (30 min)

1. **Examining regional identity**
   - Color cells by "brain_subregion" (if available); hover over the different subregions in the left panel.
   - Observe how cells from different cortical regions distribute in the UMAP; do the clusters in the UMAP corerspond to specific layers of the cortex?
   
2. **Layer-specific markers**
   - Search for and visualize layer-specific markers:
     * Cux2 (upper layer marker)
     * Rorb (layer 4 marker)
     * Fezf2 (deep layer marker)
   - Note how these markers highlight different excitatory neuron subtypes
   
3. **Comparative analysis**
   - Use the "Split by" feature to compare gene expression across cortical regions
   - Identify genes with region-specific expression patterns
   - Examine whether cell type proportions vary across regions

> **Intermediate Questions for Part 3:**
>
>1. Select two different cortical regions and use the comparison feature to identify at least 3 genes that show >differential expression between them.
>2. Do different inhibitory neuron subtypes (Pvalb+, Sst+, Vip+) show similar or different distributions across >cortical regions? Provide evidence.
>3. Can you identify any genes that show gradients of expression across cortical regions rather than discrete >regional patterns?
>4. Using the cell type proportions feature, determine which cell type shows the greatest variation in abundance >across cortical regions.
>
>5. **Worksheet Questions**
>   - What are the major cell types identified in the mouse cortex?
>   - How do excitatory neurons differ across cortical layers?
>   - Are there region-specific differences in cell type composition?
>   - Which genes show the strongest regional specificity?

---

## AFTERNOON SESSION (14:00-16:30)

<a id="lab2"></a>
### HANDS-ON SESSION 2: SPATIAL TRANSCRIPTOMICS ANALYSIS (1 HOUR)

#### Part 1: Introduction to Spatial Transcriptomics (15 min)

1. **Overview of Visium technology**
   - How Visium captures spatial gene expression data
   - Resolution considerations (55μm spots containing multiple cells)
   - Relationship between histological features and gene expression

2. **Introduction to the mouse cortex Visium dataset**
   - Origin of the dataset (Allen Brain Institute)
   - Overview of the cortical regions and layers captured
   - Connection to the single-cell dataset explored earlier

#### Part 2: Web-based Exploration using Vitessce (45 min)

1. **Getting started with Vitessce**
   - Navigate to [vitessce.io](https://vitessce.io/#?dataset=spatialdata-visium) in your web browser
   - Select the mouse brain spatial dataset from the demos section
   - Familiarize yourself with the interface components:
     * Image viewer panel
     * Gene expression visualization tools
     * Dataset information panel

2. **Exploring cortical layers in spatial context**
   - Examine the H&E image to identify cortical layers
   - Visualize layer-specific marker genes:
     * Cux2 (upper layer marker)
     * Rorb (layer 4 marker)
     * Fezf2 (deep layer marker)
   - Observe how gene expression aligns with visible histological layers

> **Intermediate Questions for Cortical Layers:**
>
> 1. Identify specific spots in the tissue where Cux2, Rorb, and Fezf2 show their highest expression. Do these spots correspond to specific cortical layers visible in the H&E image?
> 2. Find a gene that shows higher expression in the corpus callosum than in the cortex. What is the function of this gene?
   
3. **Cell type markers in spatial context**
   - Visualize cell type markers used in the single-cell analysis:
     * Slc17a7 (excitatory neurons)
     * Gad1 (inhibitory neurons)
     * Aldh1l1 (astrocytes)
     * Mobp (oligodendrocytes)
   - Compare their spatial distribution with your knowledge of brain anatomy

> **Intermediate Questions for Cell Type Markers:**
>
> 1. Using the dual gene expression feature, find spots where both Slc17a7 and Gad1 are expressed. What might these spots represent?
> 2. Compare the expression pattern of Mobp with the H&E image. Which anatomical structure shows the highest expression of this oligodendrocyte marker?
   
4. **Exploring pre-computed clusters**
   - View the spatial clustering results
   - Compare clusters with visible anatomical structures
   - Identify genes that define each spatial domain

**Additional Exploration Tasks:**
1. Use the cluster identification feature to select spots from a specific cortical layer and identify the genes most highly expressed in that layer.
2. Create a "gene signature" of 3-5 genes that together can help identify Layer 4 of the cortex.
3. Compare the spatial distribution of two different interneuron markers (e.g., Pvalb and Sst). Do they show different spatial patterns?
4. Identify a gene that shows distinct expression in the subplate region. What is the function of this gene?

5. **Worksheet Questions**
   - How do the spatial patterns of layer markers correspond to visible cortical layers?
   - Which cell types appear to be enriched in specific cortical regions?
   - How does the spatial data provide context that was missing from the single-cell data?

---

### LECTURE: INTRODUCTION TO R/SEURAT (30 MIN)

#### Basic R for Single-Cell Analysis
- Introduction to R syntax and data structures
- What is Seurat and why is it used for single-cell and spatial analysis?
- Understanding the Seurat object structure
- Main functions for visualization and analysis

#### Seurat for Spatial Analysis
- How Seurat handles spatial transcriptomics data
- Key visualization functions for spatial data
- Integration capabilities between single-cell and spatial data
- Analysis approaches for spatial transcriptomics

---

<a id="lab3"></a>
### HANDS-ON SESSION 3: INTEGRATIVE ANALYSIS IN R/SEURAT (1 HOUR)

You should have downloaded the following datasets

* [Single-cell mouse brain datasets: allen_mouse.rds](https://www.dropbox.com/scl/fi/kpzxif6sv9lskiywknx7q/allen_mouse.rds?rlkey=hbwi4vyn59mnkxpffuahfgt0q&dl=1) (about 180 Mb)
* [Spatial transcriptomics mouse brain: spatial_brain.rds](https://www.dropbox.com/scl/fi/s9qd2ptn6s1aew2m0vbk4/spatial_brain.rds?rlkey=0jmeonmun40114joj1snr633q&dl=1) (about 190 Mb)



#### Part 1: Exploring a mouse brain single-cell datasets (15 min)

We have selected a small dataset of mouse brain single-cell data. The corresponding publication is here [Tasic et al., Nature Neuroscience 2016](https://www.nature.com/articles/nn.4216). 

We want to introduce the basic steps in the analysis of this dataset.

```r
# Load required libraries
library(Seurat)
library(ggplot2)
library(dplyr)
library(patchwork)
```


```r
# Load the data
allen_reference <- readRDS("allen_mouse.rds")

# Examine the reference data; how many cells? how many genes?
allen_reference
```

We have preprocessed the data, and we can look at the different clusters in the data:

```r
DimPlot(allen_reference, group.by = "subclass", label = TRUE)
```

We can extract the number of cells for each cell subtype

```r
table(allen_reference@meta.data$subclass)
```

> **Intermediate questions Part 1**
>
> 1. Redo the `DimPlot` now using 'class' grouping; what can you observe?
> 2. can you count the number of GABAergic and Glutamatergic neurons?
> 3. Does the proportion between neuronal and non neuronal cells correspond to what is known from the literature?

#### Part 2: Finding differential genes (15 min)


We can check the expression of some markers that we have identified this morning. For example, Gad1 was identified as a marker of inhibitory neurons:

```r
p1 <- FeaturePlot(allen_reference,features=c('Gad1'))
p1
```

Unfortunately, Slc17a7 (a marker of excitatory neurons) is not in the reduced list of genes which we have selected in our reduced list of genes (try it out in the previous command!). 
However, we can try to identify differential genes between the 2 populations of neurons!

```r
# set the label to class
Idents(allen_reference) <- 'class'

# find the differential genes between the 2 populations
deg <-FindMarkers(allen_reference,ident.1 = 'GABAergic', ident.2 = 'Glutamatergic')

# Find the top markers for Inhibitory neurons
Inh.top <- deg %>% 
  filter(avg_log2FC >0) %>% 
  arrange(p_val_adj, desc(avg_log2FC)) %>%
  slice_head(n = 10)

# Find the top markers for Excitatory neurons
Exc.top <- deg %>% 
  filter(avg_log2FC <0) %>% 
  arrange(p_val_adj,avg_log2FC) %>%
  slice_head(n = 10)
```

> **Intermediate questions Part 2**
>
> 1. Test some of these genes in the `FeaturePlot` command and verify their expression.
> 2. By setting `Idents(allen_reference) <- 'subclass'` , can you find marker genes comparing Astrocytes and Oligodendrocytes? Check their expression!




#### Part 3: Loading and Exploring Spatial Data (15 min)

We will now work with a dataset representing a spatial transcriptomic dataset of the mouse brain. We will look at spatial expression patterns for different genes.

```r

# Load the spatial data (mouse cortex Visium dataset)
#brain <- readRDS(/path/to/spatial_brain.rds)
```

We can now visualize the tissue and the spots from the Visium assay.

```r
# Visualize the tissue image with different clusters
# Visualize the tissue image
Idents(brain) <- 'orig.ident'
SpatialDimPlot(brain)
```

We can also visualize the expression of specific genes:

```r
# Visualize specific gene expression on the tissue
p1 <- SpatialFeaturePlot(brain, features = "Hpca")
p2 <- SpatialFeaturePlot(brain, features = "Ttr")
p1 + p2
```

> **Intermediate Questions for Part 3:**
>
> 1. Can you identify the brain regions? You can have a look at this [brain atlas](http://atlas.brain-map.org/atlas?atlas=2&plate=100883867#atlas=2&plate=100883867&resolution=13.51&x=7767.795882686492&y=4024.102980090726&zoom=-3). In particular, identify the different cortical layers!
> 2. Modify the code to visualize the expression of Cux2, Rorb, and Fezf2 on the same plot. Which cortical layers show expression of these genes?
> 3. Use the `SpatialFeaturePlot` function with custom color scales to better visualize low-expression genes. Choose a gene and experiment with different visualization parameters.
> 4. Go to the [Allen Mouse Brain Atlas](https://mouse.brain-map.org) and check the spatial expression of these genes from in-situ hybridization (ISH).

------

#### Part 4: Analysing the spatial expression of gene signatures (15 min)

When we analysed the single-cell dataset, we have identified markers genes for Excitatory and Inhibitory neurons, as well as for Astrocytes and Oligodendrocytes. We can now map the aggregated expression of these signatures to the spatial data, to highlight these populations:

```r
# the AddModuleScore computes an aggregated expression of a list of genes for each spatial cell
brain <- AddModuleScore(brain, features =  list(oligo=rownames(Exc.top)), assay = "SCT", name = "Exc")

brain <- AddModuleScore(brain, features =  list(oligo=rownames(Inh.top)), assay = "SCT", name = "Inh")


p1 <- SpatialFeaturePlot(brain, features = 'Inh1')
p2 <- SpatialFeaturePlot(brain, features = 'Exc1')

p1 | p2
```


> **Intermediate Questions for Part 4:**
> 
> 1. Can you plot the signature scores for the Oligodendrocytes and Astrocytes?
> 2. Identify the region of expression and verify if this fits what is known...

------

#### Part 5: Integration with Reference scRNA-seq Data (if time permits!)

A cool feature is the possibility to map external single-cell datasets on top of the spatial data. This is useful if we have characterized well the single-cell dataset; we can then use the mapping to add additional information to the spatial data.

To map the single-cell dataset to the spatial data, we need to find "anchors", i.e. shared genes that will help to define the mapping. Remember that the cells from the single-cell dataset do not have spatial information!

```r
# Find anchors between the spatial data and scRNA-seq reference
anchors <- FindTransferAnchors(
  reference = allen_reference,
  query = brain,
  normalization.method = "SCT"
)

# Transfer cell type labels from reference to spatial data
predictions <- TransferData(
  anchorset = anchors,
  refdata = allen_reference$subclass,
  prediction.assay = TRUE,
  weight.reduction = brain[["pca"]],
  dims = 1:30
)

# Add cell type predictions to the spatial object
brain[["predictions"]] <- predictions

# Visualize cell type probability maps
DefaultAssay(brain) <- "predictions"

# Examine excitatory neuron layers
SpatialFeaturePlot(brain, 
                  features = c("L2/3 IT", "L4", "L5 IT", "L6 IT"), 
                  pt.size.factor = 1.6, 
                  ncol = 2)

# Examine inhibitory neurons
SpatialFeaturePlot(brain, 
                  features = c("Pvalb", "Sst", "Vip"), 
                  pt.size.factor = 1.6, 
                  ncol = 3)

# Return to standard assay
DefaultAssay(brain) <- "Spatial"
```

> **Intermediate Questions for Part 5:**
>
>1. Examine the prediction scores for different cell types across the tissue. Which cell type predictions show the highest confidence scores?
>2. Compare the spatial distribution of L2/3 IT and L4 excitatory neurons. Do they form distinct or overlapping layers?
>3. Identify a region in the tissue where the probability of finding inhibitory neurons is highest. Which specific inhibitory neuron subtype is most prevalent there?
>4. Using both the H&E image and cell type probability maps, trace the six cortical layers and compare your manual annotation with the computational predictions.

------



## WRAP-UP

### 5 Key Take-Home Messages

#### Biology Perspective

* The mouse cortex has a complex layered organization with specific distributions of excitatory and inhibitory neurons, where inhibitory "hot zones" exist in layers 2 and 5A despite inhibitory neurons making up only ~11.5% of total neurons.
* Specific marker genes reliably identify cell types across the cortex: Slc17a7 for excitatory neurons, Gad1 for inhibitory neurons, and layer-specific markers like Cux2 (upper layers), Rorb (layer 4), and Fezf2 (deep layers).
* The balance between excitation and inhibition varies across cortical layers and regions, creating unique computational environments crucial for proper cortical function.

#### Data Analysis Perspective

* Integrating single-cell and spatial transcriptomics provides complementary information: single-cell data offers high-resolution cell type identification while spatial data preserves anatomical context critical for understanding tissue organization.
* Modern computational approaches allow transfer of information between datasets, enabling the mapping of detailed single-cell profiles onto spatial coordinates to better understand the architectural principles of brain organization.

### Follow-up Resources

- CellxGENE datasets: https://cellxgene.cziscience.com/
- Seurat spatial vignette: https://satijalab.org/seurat/articles/spatial_vignette.html
- Allen Brain Atlas: https://portal.brain-map.org/
- 10x Genomics Visium resources: https://www.10xgenomics.com/products/spatial-gene-expression

---

## TECHNICAL NOTES 

### Troubleshooting Tips
- Common CellxGENE issues:
  - Slow loading: Reduce the number of genes displayed simultaneously
  - Browser compatibility: Works best with Chrome or Firefox
- Common R issues:
  - Memory limitations: Use smaller dataset subsets if needed
  - Package installation errors: Check for dependencies
  - Plotting errors: Ensure proper feature names are used

