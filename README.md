# Alternative Polyadenylation Site Usage Prediction with Deep Learning

<br>

<img src="figure/cnn_image.png" margin-left: auto margin-right: auto >


<br>

## Table of Contents

I. Introduction

II. Methods

III. Results

IV. Conclusion

V. Technologies

VI. Abbreviation

VII. Acknowledgements

VIII. References


<br>

## I. Introduction

Alternative polyadenylation has been implicated in association with human diseases.  To further understand the underlying mechanism, the main goal of this study is to use deep learning models PolyAID, PolyAStrength, and APARENT2 to characterize the usage level of polyadenylation sites in the long-read-annotated PolyADB-LR database (v3x-LR).


<br>

## II. Methods

Input.  Data were originally sourced from PolyADB-v3x-LR database, reference to PolyA_DB V3.2 (https://exon.apps.wistar.org/polya_db/v3/).  Surrounding nucleotide sequences for PAS genomic location were extracted from BSgenome.Hsapien.UCSC.hg38 package using R.

Models.  <i>PolyAID</i>: a deep learning model that consists of a single 1-D convolutional layer and a single bidirectional long short-term memory (LSTM) layer (Stroup <i>et al.</i>, Nature Commun, 2023).  Input sequence is one-hot encoded.  And output generates a classification probability for putative PAS within the sequence.  <i>PolyAStrength</i>: a deep learning model with a 1-D convolutional layer and a LSTM layer that calculates a strength score for PAS usage level based on the one-hot encoded input sequence (Stroup <i>et al.</i>, Nature Commun, 2023).  <i>PolyA_SVM</i>: a classical machine learing model, support vector machine, that predicts putative PAS based on surrounding <i>cis</i> element motifs (Cheng <i>et al.</i>, Genome Res, 2005).  <i>APARENT2</i>: a deep learning model that is composed of an architecture of residual blocks with input sequence represented by one-hot encoding (Bogard <i>et al.</i>, Cell, 2019; Linder <i>et al.</i>, Genome Biol, 2022).  The model is used for determining gene's preference between short and long isoforms.

<br>

Table 1.  Column Description.  Also see PolyADB-v3x-LR database for more information: https://github.com/wcjohnchen/database

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

## III. Results

PASs in the terminal regions, i.e. the 3'UTR regions, were examined in this study.  There are 247,852 unique TR PASs, which are associated with 29,434 unique genes.  Correlations between PAS features were compared (Figure 1).  The correlations bewtween the three models were as follow: PolyAID vs PolyAStrength: 0.61; PolyAID vs PolyA_SVM: 0.55; and PolyAStrength vs PolyA_SVM: 0.52.  

There are many diseases associated with cleavage and polyadenlyation activites, including cancer, aging, and skin-related diseases.  For example, FIP1L1 gene (a component of cleavage and polyadenylation specificity factor complex), which plays a role in leukemia (Ali <i>et al.</i>, 2023), enhances usage of proximal PASs (global 3'UTR shortening), while knockdown of FIP1L1 expression leads to usage of distal PASs (global 3'UTR lengthening) (Li <i>et al.</i>, 2015; Goering <i>et al.</i>, 2021; Davis <i>et al.</i>, 2022).  By profiling FIP1L1 based on expression level and modeling, PAS genomic locations chr4:+:53459611 (PolyAID: 0.9986, PolyAStrength: 0.5618, PolyA_SVM: 0.9911) and chr4:+:53459667 (PolyAID: 0.9768, PolyAStrength: -7.8462, PolyA_SVM: 0.9750) ranked at the top two (Figure 2).  The result from APARENT2 shows that FIP1L1 has a preference for proximal PASs (delta log-odds-narrow < 0) (Figure 5), which confirms with the previous findings.  Similarly, CSTF2 (cleavage stimulation factor 2) gene, which involved in cancer, and proliferating cells (Xia <i>et al.</i>, 2014, Goering <i>et al.</i>, 2021), favors 3'UTR shortening and is in accordance with the APARENT2 result (preference for proximal PASs; genomic locations: chrX:+:100840916 (polyAID: 0.9984, polyAStrenght: -4.5279, PolyA_SVM: 0.6831); chrX:+:100841518 (polyAID: 0.9908, polyAStrenght: -6.7179, PolyA_SVM: 0.7773); delta log-odds-narrow < 0) (Figure 2, 5).  In addition to cancer, RRAS2 gene (encoding RAS-relasted small GTPases) is also associated with aging.  It is reported that RRAS2 promotes 3'UTR lengthening in aging process, as in senescent cells (Chen <i>et al.</i>, 2018).  Profiling of RRAS2 and comparison of proximal (chr11:-:14278745, polyAID: 0.7174, polyAStrenght: -6,5240, PolyA_SVM: 0.8469) and distal PASs (chr11:-:14277918, polyAID: 0.7682, polyAStrenght: -5.1554, PolyA_SVM: 0.8949) shows that RRAS2 has the same preference for distal PASs (delta log-odds-narrow > 0) (Figure 3, 5).  In skin-related diseases, such as systemic sclerosis, NUDT21 gene (also known as CFlm25) acts as an important regulator of alternative polyadenylation, and reduced exprssion of NUDT21 leads to predominant global 3'UTR shortening (Weng <i>et al.</i>, 2019).  Profiling of NUDT21 shows a preference for distal PASs (proximal PAS (chr16:-:56431674, polyAID: 0.9889, polyAStrenght: -5.3467, PolyA_SVM: 0.9730); distal PAS (chr16:-:56429141, polyAID: 0.9997, polyAStrenght: -1.3675, PolyA_SVM: 0.9925); delta log-odds-narrow > 0) (Figure 4, 5).


<br>

Figure 1.  Feature correlation of TR PASs.  Pearson correlation was used for comparison.

<img src="figure/tr_corrmatrix.png" style="width: 50%; height: 50%;">


<br>

Figure 2.  Profiling of cancer-associated genes, FIP1L1 and CSTF2.

<img src="figure/genes_FIP1L1_CSTF2.png" style="width: 50%; height: 50%;">


<br>

Figure 3.  Profiling of age-related gene, RRAS2.

<img src="figure/gene_RRAS2.png" style="width: 50%; height: 50%;">


<br>

Figure 4.  Profiling of skin-related gene, NUDT21.

<img src="figure/gene_NUDT21.png" style="width:75%; height:75%;">


<br>

Figure 5.  PAS usage prediction using APARENT2.  3'UTR Shortening: delta log-odds-narrow < 0.  3'UTR lengthening: delat log-odds-narrow > 0.

<img src="figure/plot_APARENT2.png" style="width:25%; height:25%;">



## IV. Conclusion

The present study used deep learning (PolyAID, PolyAStrength, APARENT2) and machine learning (PolyA_SVM) methods to further characterize the profile of long-read-annotated 3'UTR PASs in the PolyADB-v3x-LR database.  By acquring the PAS usage statistics through advanced learning, the database may serve as an additional insight for potential therapeutics biomarkers and targets for disease models.



<br>

## V. Technologies

Bioinformatics, Machine Learning, Deep Learning, Jupyter Notebook, Python, R, VS Code, Git, Linux


<br>

## VI. Abbreviation

PAS: polyA site <br>

TES: Transcription end site


<br>

## VII. Acknowledgements

I would like to thank Dr. Bin Tian’s lab for data availability and contribution.


<br>

## VIII. References

Ali S, Al-Qattan Y, Awny W, Hamadah A, Pinto K, and AlShemmari S.  2023.  FIP1L1-PDGFRA fusion gene in T-lymphoblastic lymphoma: A case report. Cancer Rep (Hoboken), 6(1):e1769.  doi: 10.1002/cnr2.1769.

Bogard N, Linder J, Rosenberg AB, and Seelig G. 2019.  A Deep Neural Network for Predicting and Engineering Alternative Polyadenylation.  Cell, 178(1):91-106.e23.  doi: 10.1016/j.cell.2019.04.046.

Chen M, Lyu G, Han M, Nie H, Shen T, Chen W, Niu Y, Song Y, Li X, Li H, Chen X, Wang Z, Xia Z, Li W, Tian XL, Ding C, Gu J, Zheng Y, Liu X, Hu J, Wei G, Tao W, and Ni T.  2018.  3' UTR lengthening as a novel mechanism in regulating cellular senescence.  Genome Res, 28(3):285-294.  doi: 10.1101/gr.224451.117.

Cheng Y, Miura RM, and Tian B.  2006.  Prediction of mRNA polyadenylation sites by support vector machine.  Bioinformatics, 22(19):2320-5.  doi: 10.1093/bioinformatics/btl394.

Davis AG, Johnson DT, Zheng D, Wang R, Jayne ND, Liu M, Shin J, Wang L, Stoner SA, Zhou JH, Ball ED, Tian B, and Zhang DE.  2022.  Alternative polyadenylation dysregulation contributes to the differentiation block of acute myeloid leukemia.  Blood, 139(3):424-438.  doi: 10.1182/blood.2020005693.

Goering R, Engel KL, Gillen AE, Fong N, Bentley DL, and Taliaferro JM.  2021.  LABRAT reveals association of alternative polyadenylation with transcript localization, RNA binding protein expression, transcription speed, and cancer survival.  BMC Genomics, 26;22(1):476.  doi: 10.1186/s12864-021-07781-1.

Li W, You B, Hoque M, Zheng D, Luo W, Ji Z, Park JY, Gunderson SI, Kalsotra A, Manley JL, and Tian B.  2015.  Systematic profiling of poly(A)+ transcripts modulated by core 3' end processing and splicing factors reveals regulatory rules of alternative cleavage and polyadenylation.  PLoS Genet, 11(4):e1005166.  doi: 10.1371/journal.pgen.1005166.

Linder J, Koplik SE, Kundaje A, and Seelig G. 2022.  Deciphering the impact of genetic variation on human polyadenylation using APARENT2.  Genome Biol, 23(1):232.  doi: 10.1186/s13059-022-02799-4.

Stroup EK, and Ji Z. 2023. Deep learning of human polyadenylation sites at nucleotide resolution reveals molecular determinants of site usage and relevance in disease.  Nature Commun, 14(1):7378:1-17.  doi: 10.1038/s41467-023-43266-3.

Wang R, Nambiar R, Zheng D, and Tian B.  2017.  PolyA_DB 3 catalogs cleavage and polyadenylation sites identified by deep sequencing in multiple genomes.  Nucleic Acids Res, 46(D1):D315-D319.  doi: 10.1093/nar/gkx1000.

Weng T, Huang J, Wagner EJ, Ko J, Wu M, Wareing NE, Xiang Y, Chen NY, Ji P, Molina JG, Volcik KA, Han L, Mayes MD, Blackburn MR, and Assassi S.  2020.  Downregulation of CFIm25 amplifies dermal fibrosis through alternative polyadenylation.  J Exp Med, 217(2):e20181384.  doi: 10.1084/jem.20181384.

Xia Z, Donehower LA, Cooper TA, Neilson JR, Wheeler DA, Wagner EJ, and Li W.  2014.  Dynamic analyses of alternative polyadenylation from RNA-seq reveal a 3'-UTR landscape across seven tumour types.  Nat Commun, 5:5274.  doi: 10.1038/ncomms6274.

Zheng D, Liu X, and Tian B.  2016.  3'READS+, a sensitive and accurate method for 3' end sequencing of polyadenylated RNA.  RNA, 22(10):1631-9.  doi: 10.1261/rna.057075.116.

