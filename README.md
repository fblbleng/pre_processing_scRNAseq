# scRNAseq_preprocessing_workflow
The repository contains the workflow for the scRNAseq pre-processing workflow: from package installation until QC af Seurat Object


This is an example of a workflow to process data in Seurat v5. Here the steps:

1. Load in the data

2. Quality Control and Filtering

3. Normalization, Scaling & Variable Features selection

4. Perform dimensionality reduction

5. Detect clusters within the data

6. Find genes which define the clusters

7. Examine how robust the clusters and genes are

#Loading Data
We have the SevenBridge output files already produced. The files we’re working with here are the .Rds files from the SevenBridge pipeline. Data were loaded as object list using Merge_Seurat_List function in Seurat.

#QC

Seurat automatically creates two metrics we can use:

nCount_RNA the total number of UMIs: cells with unusually high UMI counts might be multiplets, whereas cells with low UMI counts might be droplets containing ambient RNAs but not real cells. 

nFeature_RNA the number of observed genes (anything with a nonzero count):Any cell with very few expressed genes is likely to be of poor quality as the diverse transcript population has not been successfully captured.


% of Mithocondrial genes: Low-quality / dying cells often exhibit extensive mitochondrial contamination. The expression level of mt genes can vary among samples. For some cells types (e.g. cardiomyocytes), expression of mt genes may have biological meanings and filtering cell barcodes based on this may lead to bias in the analysis.

Amount of Ribosomal Genes
Ribosomal genes also tend to be very highly represented, and can vary between cell types, so it can be instructive to see how prevalent they are in the data. Ribosomial genes represent a measure of the translational activity of the cell.

Cell Cycle Scoring
To understand if the cell cycle is having any effect on the data, Seurat comes with a bunch of marker genes for different cell cycle stages that can be used to predict classification of each cell in either G2M, S or G1 phase.

Based on these plots, one could now also define manual thresholds for filtering cells. For example, we might consider cells to be low quality if they have library sizes below 100,000 reads; express fewer than 5,000 genes; have spike-in proportions above 10%; or have mitochondrial proportions above 10%.

#Normalization, Scaling & Variable Features selection
Normalisation
Before we do any analysis with the data we needs to normalise the raw counts.
The default normalisation in Seurat is pretty simple - it simply scales the counts by the total counts in each cell, multiplies by 10,000 and then log transforms.

