# Alternative Polyadenylation Site Usage Prediction with Deep Learning

<br>

<img src="figure/polyadb4_lr.png" margin-left: auto margin-right: auto >


<br>

## Table of Contents

I. Introduction

II. Schema

III. Column Description

IV. Database Interaction via Web

V. Technologies

VI. Abbreviation

VII. Acknowledgements

VIII. References


<br>

## I. Introduction

PolyADB4-LR database (v4.01-LR) stores information for an extended set of polyadenylation (polyA) sites found in human genome (hg38) using 3’READS+ deep sequencing.  The polyA sites are additionally supported by long read sequencing and data generated from machine learning, and are annotated via NCBI database and by IsoQuant-assigned long reads of ENCODE and GTEx.  This database contains 658,880 entries, 638,089 unique polyA sites, and 33,614 unique genes, and is based on ~2.3 billion 3’READS+ sequencing reads and ~600 million long reads.  With a large-scale data collection, the database can be utilized for data mining to study the role of alternative polyadenylation in various models including human diseases.


<br>

## II. Schema




<br>

Table 1.  Column Description  Also see polyADB4-LR database for more information: https://github.com/wcjohnchen/polyadb4_lr

| Column | Description |
| ---- | ---- |
|Key |Unique identification for PAS (gene symbol : chromosome : strand : position : type). |
|Gene Symbol |Abbreviation for gene name. |
|PasID |PAS identification (chromosome : strand : position). |
|Type |Long-read-TES-supported polyA site category: TR: PAS found in terminal regions, in which the regions are identified as aggregated overlapping 3’-most exon of isoform of each gene by RefSeq TES annotation; UR: upstream regions of TR;  DR: downstream regions of TR. |
|PSE |Percentage of samples which PASs were expressed. |
|AvgRPM |Mean RPM of PAS. |
|mm10_pAid |Conserved sites in mouse genome (mm10). Non-conserved sites are labeled as "nc". |
|NumRefSeq |Number of the PAS reads annotated by RefSeq TESs. |
|NumLENCODE |Number of long-read TESs (from the ENCODE4 PacBio IsoSeq dataset) that were matched to the PAS location. |
|NumLRGETx |Number of long-read TESs (from the GTEx V9 ONT cDNA dataset) that were matched to the PAS location. |
|polyAID |Classification probability for putative PAS within a sequence expected to occur (https://github.com/zhejilab/PolyaModelsHuman). |
|polyAStregth |Score for the usage level of PAS (https://github.com/zhejilab/PolyaModelsHuman). |
|SVM |Predicted PAS probability by support vector machine (polya_svm v1.1: https://exon.apps.wistar.org/polya_svm/). |

<br>

## IV. Database Interaction via Web

Figure 1.  Viewing a list of available genes in the database.  User database interaction via web is supported by Flask.

<img src="figure/gene_list.png" >

<br>

Figure 2.  Querying for a specific gene.

<img src="figure/query_gene.png" >


<br>

## V. Technologies

Bioinformatics, Database, PostgreSQL, SQL, pgAdmin4, Jupyter Notebook, Python, Flask, VS Code, Git, Linux, Machine Learning, Deep Learning


<br>

## VI. Abbreviation

PAS: polyA site <br>

TES: Transcription end site


<br>

## VII. Acknowledgements

I would like to thank Dr. Bin Tian’s lab for data availability and contribution.


<br>

## VIII. References


