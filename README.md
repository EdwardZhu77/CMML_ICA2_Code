# A Comparative Benchmark of totalVI, Seurat WNN, and PCA for CITE-seq Data Integration 
This repository contains the code, processed data, and key results for the comparative benchmark study evaluating totalVI, Seurat WNN, and Principal Component Analysis (PCA) for integrating single-cell CITE-seq RNA and protein data. The primary analysis was performed on the 10x Genomics 10k PBMCs from a Healthy Donor CITE-seq dataset.

## Abstract

Integrating the complementary RNA and surface protein data of CITE-seq presents unique challenges. This study benchmarks three widely used methods on PBMC10k data. PCA and totalVI excelled in aligning unsupervised clusters with ground truth annotations. totalVI uniquely enhanced RNA-protein Spearman correlations, crucial for biological insight. Seurat WNN provided better UMAP visual separation and cluster stability. Our findings reveal method-specific strengths, offering guidance for selecting appropriate tools based on analytical objectives.

## Project Structure

The repository is organized as follows:

CMML_ICA2_Code/  
│  
├── Data_Preprocessing_Outputs/  
│ ├── figures_pbmc10k_mtx/ # QC figures from preprocessing (before/after filtering)  
│ └── data_pbmc10k_mtx/  
│ │ ├── raw/ # Original downloaded 10x Genomics data and ground truth  
│ │ └── processed/ # Data after unified Python preprocessing   
│ │ │ ├── pbmc10k_for_SeuratPCA_preprocessed/ # Data for Seurat & PCA after unified Python preprocessing   
│ │ │ └── pbmc10k_for_totalVI_preprocessed/ # Data for totalVI after unified Python preprocessing   
│  
├── Model_Outputs/  
│ ├── totalVI_pbmc_model/ # Results and key outputs from the totalVI model run   
│ ├── Seurat_pbmc_model/ # Results and key outputs from the Seurat WNN model run  
│ └── PCA_pbmc_model/ # Results and key outputs from the PCA model run   
│  
├── Benchmark_Analysis/ # Notebook/script for final benchmark metric calculations, figure generation, and summary tables  
│ └── Figure/ # Figures generated by the benchmark analysis (Fig1, Fig2, Sub Fig1, Sub Fig2)  
│ └── Data/ # Data generated by the benchmark analysis  
│  
└── Notebooks_and_Scripts/  
│ ├── Data_Preprocessing.ipynb # Python notebook for data download, QC, and initial processing  
│ ├── totalVI.ipynb # Python notebook for running totalVI (Pure code, without output)  
│ ├── totalVI_with_output.ipynb # Python notebook for running totalVI (Visual Issue because of format of output by Github)  
│ ├── Seurat_WNN.Rmd # R Markdown script for running Seurat WNN  
│ ├── PCA_Integration.ipynb # Python notebook for running the PCA-based integration  
│ └── Benchmark_Analysis.ipynb  # Python notebook for benchmark analysis  


**Note on Data:**
*   Raw data from 10x Genomics can be downloaded from [10x Genomics Datasets](https://support.10xgenomics.com/single-cell-gene-expression/datasets/3.0.0/pbmc_10k_protein_v3).
*   Ground truth annotations were obtained from the supplementary resource of Wang et al., 2020 (BREM-SC), accessible [here](https://github.com/tarot0410/BREMSC/blob/master/data/RealData/10X10k/truth_10X10k.csv).
*   Due to file size, only key processed data files necessary for reproducing the benchmark analysis might be included directly in this repository. Paths in scripts might need adjustment if running locally.

## Workflow Overview

1.  **Data Download and Preprocessing (`Notebooks_and_Scripts/Data_Preprocessing.ipynb`):**
    *   Downloads the raw 10x Genomics PBMC10k CITE-seq dataset.
    *   Performs quality control (QC) filtering on cells and genes for both RNA and ADT modalities.
    *   Normalizes RNA data, identifies highly variable genes (HVGs).
    *   Maps ground truth cell type labels.
    *   Saves the primary processed AnnData/MuData object (`pbmc10k_cite_seq_processed_for_totalVI.h5ad`).
    *   Exports data in formats suitable for totalVI, Seurat, and PCA inputs.

2.  **Integration Model Implementation:**
    *   **totalVI (`Notebooks_and_Scripts/totalVI.ipynb`):**
        *   Loads processed data into a MuData object.
        *   Configures and trains the totalVI model using raw HVG counts and raw protein counts.
        *   Generates a 20-dimensional joint latent space and denoised RNA/protein expressions.
        *   Outputs are saved in `Model_Outputs/totalVI_pbmc_model/`.
    *   **Seurat WNN (`Notebooks_and_Scripts/Seurat_WNN.Rmd`):**
        *   Loads exported raw HVG RNA and protein counts into R.
        *   Processes RNA data (SCTransform, PCA) and ADT data (CLR normalization, scaling, PCA).
        *   Constructs a Weighted Nearest Neighbor (WNN) graph.
        *   Performs UMAP embedding and Leiden clustering.
        *   Outputs are saved in `Model_Outputs/Seurat_pbmc_model/`.
    *   **PCA (`Notebooks_and_Scripts/PCA_Integration.ipynb`):**
        *   Log1p-transforms and scales raw HVG RNA and protein counts.
        *   Concatenates scaled matrices and performs PCA (50 components).
        *   Performs UMAP embedding and Leiden clustering.
        *   Outputs are saved in `Model_Outputs/PCA_pbmc_model/`.

3.  **Benchmark Analysis (`Notebooks_and_Scripts/Benchmark_Analysis.ipynb`):**
    *   Loads the outputs from each integration method.
    *   Calculates quantitative benchmark metrics:
        *   Clustering Concordance (ARI & NMI)
        *   Cell Type Separation (cASW)
        *   Clustering Stability (Mean Pairwise ARI)
        *   RNA-Protein Correlation (Spearman and Pearson)
    *   Generates figures and summary tables presented in the report.

## Key Software and Versions

A detailed list of software versions is available in the "Software Versions" section of the Supplementary Materials accompanying the main report. Key tools include:

*   Python (v3.11.12)
*   Scanpy (v1.11.1)
*   MuData (v0.3.1)
*   scvi-tools (v1.3.1)
*   Pandas (v2.2.2)
*   NumPy (v2.0.2)
*   scikit-learn (v1.6.0)
*   SciPy (v1.15.3)
*   R (v4.3.1)
*   Seurat (v4.3.0.1)
*   Matplotlib (v3.7)
*   Seaborn (v0.13.2)

## Results and Discussion

For a detailed presentation of the results, their interpretation, and discussion of method-specific strengths, limitations, and future directions, please refer to the main project report and its accompanying supplementary materials.

Key findings include:
*   PCA and totalVI excel at aligning unsupervised clusters with ground truth.
*   totalVI uniquely enhances RNA-protein Spearman correlations.
*   Seurat WNN provides superior UMAP visual separation and cluster stability.

The choice of method should be guided by the primary analytical objective.
