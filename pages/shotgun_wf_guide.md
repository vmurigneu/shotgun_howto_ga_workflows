---
title: Shotgun metagenomics Workflow
type: guide
description: How-to guide for Shotgun metagenomics workflow on Galaxy Australia
affiliations: QCIF
toc: false
---
  - [Background](#background-and-tutorials)
  - [Overview](#analysis-overview)
  - [User guide](#user-guide)
      + [Running a multi sample experiment](#running-a-multi-sample-experiment)
  - [Example output](#example-output)
  - [Licenses](#licenses)

---

# Background and Tutorials 

This document describes how to use a shotgun metagenomics workflow on Galaxy Australia. The aim of this workflow is to handle the routine part of shotgun metagenomics data processing. The workflow is using the tools MetaPhlAn2 for taxonomy classification and HUMAnN2 for functional profiling of the metagenomes.  

The workflow is based on the Galaxy Training tutorial [Analyses of metagenomics data](https://training.galaxyproject.org/training-material/topics/metagenomics/tutorials/general-tutorial/tutorial.html#shotgun-metagenomics-data). 

---

# Analysis overview

<p align="center"><img src="./images/overview_shotgun.png" alt="Logo" width="45%"></p>

## 1. Extraction of taxonomic information ([MetaPhlAn2](https://huttenhower.sph.harvard.edu/metaphlan2/))

Assignation of taxonomy on the whole sequences using databases with marker genes. MetaPhlAn2 is using a database of ~1M unique clade-specific marker genes (not only the rRNA genes) identified from ~17,000 reference (bacterial, archeal, viral and eukaryotic) genomes.

MetaPhlAn2 generates 3 files:

* A tabular file with the community structure. Each line contains a taxa and its relative abundance found for our sample. The results are visualised with Krona plots.

* A BIOM file with the same information as the previous file but in BIOM format

* A SAM file with the results of the mapping of the sequences on the reference database

## 2. Extraction of functional information ([HUMAnN2](https://huttenhower.sph.harvard.edu/humann2/))

In the shotgun data, we have access to the gene sequences from the full genome. We use that to identify the genes, associate them with a function, build pathways, etc., to investigate the functional part of the community.

HUMAnN2 generates 3 files:

* A file with the abundance of gene families. Gene family abundance is reported in RPK (reads per kilobase) units to normalize for gene length. It reflects the relative gene (or transcript) copy number in the community. The RPKs of reads that map to gene families that lack UniRef90 IDs are captured as UniRef90_unknown, and unmapped read abundances as UNMAPPED. The “UNMAPPED” value is the total number of reads which remain unmapped after both alignment steps (nucleotide and translated search). Since other gene features in the table are quantified in RPK units, “UNMAPPED” can be interpreted as a single unknown gene of length 1 kilobase recruiting all reads that failed to map to known sequences.

* A file with the coverage of pathways. Pathway coverage provides an alternative description of the presence (1) and absence (0) of pathways in a community, independent of their quantitative abundance.

* A file with the abundance of pathways

---

# User guide
## Running a multi sample experiment

### 1. Upload the raw datasets (fastq files) in a Galaxy history

### 2. Create a dataset collection containing the fastq files 
The goal of a collection is to process all the samples at once.  

The workflow will take one fastq file per sample as input.     
* If the dataset are single-end reads, include all the files in the collection
* If the dataset are paired-end reads, only include the forward reads (R1) for each sample

Figure: Creating a collection

<p align="center"><img src="./images/create_collection.png" width="99%"></p>

A. Click the checkbox icon. This will reveal checkboxes to the left of all datasets in the history.  
B. In this case we want to select all datasets, so press "Select All" button (alternatively datasets can be filtered). This will put a check mark into all checkboxes.  
C. Click "All 10 selected..." button. This will reveal a dropdown.  
D. Since this is not paired-end (or mate-pair) data we will choose to "Build Dataset List". This will open a dataset collection creator interface.  
E. Within the dataset collection creator interface, use the "Name" box to name the collection. "Hide original elements" checkbox ensures that upon creating the collection the original datasets will be hidden from the history as shown in the next figure. Click "Create collection".  
F. A collection named "10 samples" is now added to the history and original datasets are hidden, so that the history only has one item.  
G. Clicking on a collection reveals its content.

### 3. Run the shotgun workflow

  Search for the shotgun metagenomics workflow under "workflow" and run it. The "input fastq collection" should be the collection you just created ("10 samples"). 
<p align="center"><img src="./images/run_workflow.png"></p>

<p align="center"><img src="./images/run_workflow_collection.png" width="100%"></p>

[Link](https://usegalaxy.org.au/u/valentine_murigneux/w/shotgun-metagenomics-ga-workflow-for-collection) to the shotgun metagenomics workflow for collection  

---

# Example output
 
When run in full, the workflow produces the following outputs:

* Output Dataset: Krona pie plot (MetaPhIAn2 taxonomy abundance)
* Output Dataset: Merged MetaPhIAn2 taxonomy abundance table with sample ID
* Output Dataset: Merged table for HUMAnN2 gene families abundance
* Output Dataset: Merged and normalised table for HUMAnN2 gene families abundance
* Output Dataset: Merged, regrouped and normalised table for HUMAnN2 gene families abundance
* Output Dataset: Merged table for HUMAnN2 Pathways and their abundance
* Output Dataset: Merged table for HUMAnN2 Pathways and their coverage
* Output Dataset Collection: MetaPhIAn2 SAM file (one file per sample)
* Output Dataset Collection: MetaPhIAn2 biom file (one file per sample)

This is an example of [Galaxy history](https://usegalaxy.org.au/u/valentine_murigneux/h/shotgun-workflow-forward-reads-15-samples) containing the results for 15 samples and the corresponding [workflow report](  
https://usegalaxy.org.au/workflows/invocations/report?id=78f98f14bf4a7a20) generated by Galaxy. 

---

# Licenses

Usage of this workflow is dependant on the (generally permissive) licences of the underlying tools and platforms; including;

* [MetaPhlAn](https://github.com/biobakery/MetaPhlAn/blob/master/license.txt)
* [HUMAnN](https://github.com/biobakery/humann/blob/master/LICENSE)
* [Krona](https://github.com/marbl/Krona/blob/master/KronaTools/LICENSE.txt)
* [Galaxy](https://galaxyproject.org/admin/license/)
* [Galaxy Australia terms of service](https://site.usegalaxy.org.au/about#terms-of-service)
