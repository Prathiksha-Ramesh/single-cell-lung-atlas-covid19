# Single-cell Lung Atlas of COVID-19

<p align="left">
  <img src="https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white" alt="Jupyter">
  <img src="https://img.shields.io/badge/Scanpy-1.9-3F8F00?logo=scanpy&logoColor=white" alt="Scanpy">
  <img src="https://img.shields.io/badge/scVI--tools-Deep%20Generative%20Modeling-6A0DAD" alt="scVI-tools">
  <img src="https://img.shields.io/badge/AnnData-Single--cell%20Data-FF6F00" alt="AnnData">
  <img src="https://img.shields.io/badge/pandas-Data%20Analysis-150458?logo=pandas&logoColor=white" alt="pandas">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="MIT License">
  <img src="https://img.shields.io/github/stars/Prathiksha-Ramesh/single-cell-lung-atlas-covid19?style=social" alt="Stars">
  <img src="https://img.shields.io/github/last-commit/Prathiksha-Ramesh/single-cell-lung-atlas-covid19" alt="Last Commit">
</p>

An end-to-end single-cell RNA-seq analysis pipeline that builds a lung cell atlas from COVID-19 and control samples covering quality control, doublet removal, batch integration, cell type annotation, and differential expression to characterize how the lung's cellular landscape shifts in COVID-19.

---

## Table of Contents

- [Overview](#overview)
- [Background](#background)
- [Pipeline Stages](#pipeline-stages)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Results Summary](#results-summary)
- [License](#license)
- [Citation](#citation)

---

## Overview

This repository contains a complete scRNA-seq analysis workflow applied to lung tissue samples from COVID-19 and control donors. Raw count matrices are processed through doublet detection, quality control, normalization, dimensionality reduction, batch-aware integration, and cell type annotation, followed by downstream analyses comparing cell composition and gene expression between COVID-19 and control conditions.

The pipeline is organized as a sequence of notebooks — each one covering a discrete stage — so individual steps can be run, inspected, or re-run independently.

## Background

Lung tissue from COVID-19 patients shows substantial shifts in immune and epithelial cell populations compared to healthy controls. Capturing this at single-cell resolution requires careful handling of technical artifacts (doublets, ambient RNA, batch effects across samples) before any biological comparison is meaningful. This project applies a standard but rigorous scRNA-seq analysis pipeline — built on `scanpy` and `scVI` — to integrate multiple samples, annotate cell types by canonical markers, and quantify compositional and transcriptional differences associated with COVID-19.

## Pipeline Stages

| Stage | Description |
|---|---|
| **1. Doublet Removal** | Per-sample doublet detection using scVI + SOLO |
| **2. Preprocessing & QC** | Mitochondrial/ribosomal content filtering, gene/cell filtering, outlier removal |
| **3. Normalization** | Total-count normalization and log1p transformation |
| **4. Clustering (single-sample)** | HVG selection, PCA, neighbors graph, UMAP, Leiden clustering |
| **5. Integration** | Multi-sample concatenation and batch-aware integration with scVI |
| **6. Marker Identification & Annotation** | `rank_genes_groups` + scVI differential expression to assign cell type labels per cluster |
| **7. Downstream Analysis** | Cell type composition by condition, differential expression (diffxpy / scVI), GO enrichment, gene-signature scoring |

## Repository Structure

```
.
├── data/                                  # Raw count matrices (per-sample CSVs)
├── 01_doublet_removal_qc.ipynb            # Doublet detection (scVI + SOLO), QC filtering
├── 02_preprocessing_normalization.ipynb   # Filtering, normalization, log transform
├── 03_clustering.ipynb                    # HVG, PCA, neighbors, UMAP, Leiden (single sample)
├── 04_integration.ipynb                   # Batch processing across samples, scVI integration
├── 05_cell_type_annotation.ipynb          # Marker detection and cell type labeling
├── 06_downstream_analysis.ipynb           # Composition, DE, GO enrichment, gene scoring
├── combined.h5ad                          # Concatenated raw data (generated)
├── integrated.h5ad                        # Integrated, annotated dataset (generated)
├── model.model/                           # Saved scVI model
├── datp_sig.txt                           # Gene signature list used for scoring
├── requirements.txt                       # Python dependencies
├── LICENSE                                # MIT License
└── README.md
```

> Notebook filenames above reflect the intended stage-by-stage split of the analysis. Adjust to match the actual filenames once the notebooks are committed.

## Installation

```bash
# Clone the repository
git clone https://github.com/Prathiksha-Ramesh/single-cell-lung-atlas-covid19.git
cd single-cell-lung-atlas-covid19

# (Recommended) create a virtual environment
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

Core dependencies include `scanpy`, `scvi-tools`, `anndata`, `pandas`, `numpy`, `seaborn`, `diffxpy`, and `gseapy`.

## Usage

Run the notebooks in order to reproduce the full atlas:

1. **`01_doublet_removal_qc.ipynb`** — train an scVI model per sample, run SOLO to flag doublets, and remove them
2. **`02_preprocessing_normalization.ipynb`** — annotate mitochondrial/ribosomal genes, filter low-quality cells, normalize and log-transform counts
3. **`03_clustering.ipynb`** — select highly variable genes, run PCA, build the neighbor graph, compute UMAP, and cluster with Leiden
4. **`04_integration.ipynb`** — repeat QC across all samples, concatenate into a single object, and integrate with scVI using sample as a batch covariate
5. **`05_cell_type_annotation.ipynb`** — identify cluster markers (`rank_genes_groups` and scVI differential expression) and assign cell type labels
6. **`06_downstream_analysis.ipynb`** — compare cell type frequencies between COVID-19 and control, run differential expression between cell types/conditions, perform GO enrichment, and score custom gene signatures

## Results Summary

- Per-sample doublet removal (scVI + SOLO) and QC filtering (mitochondrial/ribosomal content, gene count outliers) produced a clean, multi-sample dataset suitable for integration.
- scVI-based batch integration controlled for inter-sample technical variation while preserving biological structure across COVID-19 and control samples.
- Leiden clustering combined with canonical marker genes (e.g. `EPCAM`, `MUC1`) resolved major lung cell populations, including AT1/AT2 epithelial cells, macrophages, fibroblasts, endothelial cells, and T/B/NK lymphocytes.
- Differential expression (both `diffxpy` and scVI's built-in DE) identified condition- and cell-type-specific gene signatures, further characterized through GO/KEGG enrichment and custom gene-signature scoring.

## License

This project is licensed under the [MIT License](LICENSE).

## Citation

If you use this pipeline in your research, please cite this repository:

```bibtex
@misc{ramesh2025lungatlas,
  author = {Ramesh, Prathiksha},
  title  = {Single-cell Lung Atlas of COVID-19},
  year   = {2025},
  url    = {https://github.com/Prathiksha-Ramesh/single-cell-lung-atlas-covid19}
}
```




   


