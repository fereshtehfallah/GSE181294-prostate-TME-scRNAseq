# R Package Requirements

Tested on R 4.3.x. Install packages in the order listed to avoid dependency conflicts.

## Core Packages

```r
# Bioconductor base
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install(version = "3.17")

# Seurat ecosystem
install.packages("Seurat")           # >= 4.3.0
install.packages("SeuratObject")     # >= 4.1.3
install.packages("SeuratDisk")

# Batch correction
install.packages("harmony")          # >= 0.1.1

# Leiden clustering
install.packages("leidenAlg")        # >= 1.1.0

# CellChat
devtools::install_github("jinworks/CellChat")   # v2.2.0

# Data wrangling & visualization
install.packages(c("tidyverse", "ggplot2", "patchwork", "viridis", "RColorBrewer"))
install.packages(c("data.table", "Matrix"))

# Sparse matrix support
BiocManager::install("BiocNeighbors")
BiocManager::install("SingleCellExperiment")
```

## Verified Versions

| Package | Version |
|---|---|
| R | 4.3.x |
| Seurat | 4.3.0 |
| harmony | 0.1.1 |
| CellChat | 2.2.0 |
| leidenAlg | 1.1.0 |
| ggplot2 | 3.4.x |

## Notes

- CellChat v2 requires `NMF >= 0.26`, `circlize >= 0.4.15`, and `ComplexHeatmap >= 2.13`
- Seurat requires `igraph >= 1.5` for Leiden clustering via `leidenAlg`
- For Python interoperability (PAGA pseudotime import), install `reticulate` and point to the `prostate-tme` conda environment
