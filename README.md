# Plasma Exosomal miRNA Differential Expression Analysis in First-episode Schizophrenia (FES)

## Overview

This project investigates **differential expression of plasma-derived exosomal microRNAs (miRNAs)** in schizophrenia using small RNA sequencing data.

The workflow integrates preprocessing of sequencing data, miRNA quantification, construction of a miRNA count matrix, and downstream differential expression analysis using **DESeq2**.

The goal of this analysis is to identify **dysregulated exosomal miRNAs associated with schizophrenia**, which may serve as potential biomarkers.

---
# Data acquisition

Small RNA sequencing datasets were obtained from the **NCBI Gene Expression Omnibus (GEO)**.

Datasets used:

- https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE292347  
- https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE228681  

The original GEO studies contain multiple sequencing datasets.  
For this analysis, only **small RNA sequencing samples relevant to miRNA profiling** were selected.

Biological source:

- **Plasma-derived exosomes**
  
After preprocessing and quality control, **38 samples were retained for downstream analysis** consisting of:

- 20 control samples
- 18 schizophrenia samples

---

# Analysis Workflow

The analysis pipeline includes preprocessing on the Galaxy platform, miRNA quantification using miRDeep2, construction of a count matrix using Python, and differential expression analysis using DESeq2.

```
GEO datasets
        ↓
Preprocessing on Galaxy
        ↓
Quality control (fastp)
        ↓
miRNA read mapping (miRDeep2 mapper)
        ↓
miRNA quantification (miRDeep2 quantifier)
        ↓
Count matrix construction (Python / Google Colab)
        ↓
Differential expression analysis (DESeq2)
```

---

# Preprocessing

All preprocessing steps were performed using the **Galaxy bioinformatics platform**.

### Quality Control

Tool used: **fastp**

Purpose:

- Adapter trimming  
- Removal of low-quality reads  
- Filtering short reads  

---

### Read Mapping

Tool used: **miRDeep2 Mapper**

Purpose:

- Collapse identical reads  
- Map reads to the reference genome  
- Prepare reads for miRNA quantification  

---

### miRNA Quantification

Tool used: **miRDeep2 Quantifier**

Purpose:

- Quantify known miRNAs  
- Assign read counts to mature miRNAs  
- Generate per-sample miRNA expression tables  

Each sample produces a `.tabular` file containing miRNA counts.

Samples with insufficient mapped miRNA reads after preprocessing and quantification were excluded from downstream analysis.

---

# Count Matrix Construction

miRDeep2 quantifier output files were processed using a **Python notebook executed in Google Colab**.

Processing steps:

1. Upload quantifier `.tabular` files
2. Extract miRNA identifiers and read counts
3. Rename columns using sample SRR identifiers
4. Merge all samples into a single count matrix
5. Replace missing values with zero
6. Merge duplicate miRNAs by summing counts
7. Convert counts to integer values

Output file:

```
miRNA_count_matrix.csv
```

Matrix structure: Rows represent **miRNAs**, and columns represent **samples**.

---

# Differential Expression Analysis

Differential expression analysis will be performed using **DESeq2 in R** to identify miRNAs that are differentially expressed between schizophrenia patients and healthy controls.

Downstream analysis includes:

- normalization of count data  
- estimation of log2 fold changes  
- statistical testing  
- identification of significantly dysregulated miRNAs  

---

# Planned Visualizations

The following visualizations will be generated:

- PCA plots (sample clustering)
- Volcano plots
- Heatmaps of differentially expressed miRNAs

These analyses help identify global expression patterns and disease-associated miRNAs.

---

# Tools Used

- Galaxy
- fastp
- miRDeep2
- Python (pandas)
- Google Colab
- R
- DESeq2

---

# Author

Isra Aman Aziz  
Bioinformatics Project – miRNA Differential Expression Analysis
