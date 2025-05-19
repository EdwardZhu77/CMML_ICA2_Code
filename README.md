# CMML_ICA2_Code

/content/drive/My Drive/CMML_ICA2/  (Main Project Directory)
│
├── Data_Preprocessing_Outputs/  (Changed "Filter/" to be more descriptive)
│   │
│   ├── figures_pbmc10k_mtx/  (QC figures from preprocessing)
│   │   ├── qc_metrics_before_filtering_mtx.png  # QC metrics distribution BEFORE filtering
│   │   └── qc_metrics_after_filtering_mtx.png   # QC metrics distribution AFTER filtering
│   │
│   └── data_pbmc10k_mtx/  (Data related to the PBMC10k MTX-based preprocessing)
│       │
│       ├── raw/  (Original downloaded data)
│       │   ├── truth_10X10k.csv  # Ground truth cell type labels for PBMC10k
│       │   ├── pbmc_10k_protein_v3_filtered_feature_bc_matrix.tar.gz  # Downloaded 10x archive
│       │   └── pbmc_10k_protein_v3_filtered_feature_bc_matrix/  # Extracted 10x MTX folder
│       │       ├── matrix.mtx.gz      # RNA + ADT counts (sparse matrix)
│       │       ├── features.tsv.gz    # Feature names (genes and proteins) and types
│       │       └── barcodes.tsv.gz    # Cell barcodes
│       │
│       └── processed/  (Data after your unified Python preprocessing script)
│           ├── pbmc10k_cite_seq_processed_for_totalVI.h5ad  # Main processed AnnData/MuData for totalVI input (contains RNA raw counts, protein raw counts, HVG info, ground truth labels, etc.)
│           └── pbmc10k_for_SeuratPCA_preprocessed/  (Data specifically formatted for Seurat and PCA)
│               ├── protein_counts_raw.csv          # Raw protein counts (cells x proteins)
│               ├── cell_metadata_filtered.csv      # Cell metadata including ground truth and QC stats
│               └── rna_hvg_counts_mtx/             # Raw RNA counts for HVGs in MTX format
│                   ├── matrix.mtx.gz
│                   ├── features.tsv.gz             # HVG gene names
│                   └── barcodes.tsv.gz
│
├── Model_Outputs/  (Directory for outputs from each integration method)
│   │
│   ├── totalVI_pbmc_model/  (Results from totalVI.ipynb)
│   │   ├── totalVI_leiden_labels.csv          # Cell barcodes and totalVI Leiden cluster labels
│   │   ├── totalVI_denoised_rna_hvg_from_layer.csv  # Denoised/normalized RNA expression (HVGs) by totalVI
│   │   ├── totalVI_denoised_protein_from_layer.csv # Denoised/corrected Protein expression by totalVI
│   │   └── pbmc10k_mdata_totalVI_benchmark_ready.h5mu # The FINAL MuData object from totalVI run, containing latent space, UMAP, Leiden, denoised layers, and corrected protein var_names. THIS IS KEY.
│   │   # Optional: other intermediate .h5mu files or model.pt could be here too.
│   │   # You are right, pbmc10k_mdata_totalVI_benchmark_ready.h5mu should be the most comprehensive.
│   │
│   ├── Seurat_pbmc_model/  (Results from Seurat.Rmd, exported as CSVs)
│   │   ├── seurat_wnn_umap_coordinates.csv    # UMAP coordinates from Seurat WNN (cells x UMAP_dims)
│   │   ├── seurat_wnn_leiden_labels.csv     # Cell barcodes and Seurat WNN Leiden cluster labels
│   │   ├── seurat_processed_rna_hvg_sct_data.csv # Processed RNA expression (e.g., SCTransform data for HVGs)
│   │   ├── seurat_processed_adt_clr_data.csv  # Processed ADT expression (e.g., CLR normalized)
│   │   └── seurat_cell_metadata_for_benchmark.csv # Relevant cell metadata from Seurat object (includes GT, Leiden labels from Seurat, WNN weights etc.)
│   │   # Optional: pbmc10k_seurat_wnn_processed_final.rds (the full Seurat object)
│   │
│   └── pca_pbmc_model/  (Results from PCA.ipynb)
│       ├── pca_scaled_rna_hvg.csv             # Scaled RNA expression (HVGs) used for PCA input concatenation
│       ├── pca_scaled_protein.csv           # Scaled Protein expression used for PCA input concatenation
│       ├── pca_leiden_labels.csv            # Cell barcodes and PCA-based Leiden cluster labels
│       ├── pca_latent_space.csv             # PCA latent space (cell x PCs)
│       └── pbmc10k_pca_results.h5ad         # AnnData object containing PCA, UMAP, and Leiden results from PCA workflow
│
├── Benchmark_Analysis/  (Directory for the final benchmark notebook and its outputs)
│   └── (This is where your Benchmark.ipynb's figures and summary tables will be saved)
│
└── Notebooks_and_Scripts/ (Directory for your code)
    ├── Data_Preprocessing.ipynb  # Your Python script for initial data download and preprocessing
    ├── totalVI.ipynb             # Python script for running totalVI
    ├── PCA_Integration.ipynb       # Python script for running PCA-based integration
    ├── Seurat_WNN.Rmd            # R Markdown script for running Seurat WNN
    └── Benchmark_Analysis.ipynb  # Python script for final benchmark comparisons and visualizations
