# Module 02 — Seurat Clustering & Cell Type Annotation

## Purpose

Dimensionality reduction, unsupervised clustering, and cell type annotation of the 149,772 QC-passed cells using Seurat v4 with Harmony batch correction.

## Pipeline Steps

1. **Normalization:** LogNormalize (scale factor = 10,000)
2. **Variable features:** 3,000 HVGs via `FindVariableFeatures`
3. **Scaling:** `ScaleData` on all genes
4. **PCA:** 50 principal components
5. **Batch correction:** Harmony on `sample_id`
6. **UMAP:** dims 1:30
7. **Clustering:** Leiden algorithm (resolution = 0.6) → 19 clusters
8. **Annotation:** Majority-vote mapping to published labels (Mei et al. 2023)

## Cell Type Composition

| Cell Type | Cells | % of Total |
|---|---|---|
| CD8 T cells (CTL) | 37,100 | 24.8% |
| CD4 T cells (Th) | 31,434 | 21.0% |
| TNK cells | 10,170 | 6.8% |
| Endothelial cells | 9,306 | 6.2% |
| B cells | 9,268 | 6.2% |
| Myeloid (Mac/DC) | 8,701 | 5.8% |
| Tregs | 7,919 | 5.3% |
| Epithelial cells | 7,590 | 5.1% |
| Pericytes | 6,968 | 4.7% |
| NK cells | 5,957 | 4.0% |
| Mast cells | 5,626 | 3.8% |
| Monocytes | 3,544 | 2.4% |
| Basal epithelial | 3,473 | 2.3% |
| Fibroblasts | 1,797 | 1.2% |
| PDC/Plasma cells | 766 | 0.5% |
| Tumor cells | 153 | 0.1% |
| **Total** | **149,772** | **100%** |

## Files

| File | Description |
|---|---|
| `cell_metadata.csv` | Full cell metadata: cell barcode, sample, condition, cluster, cell type, UMAP coordinates |
| `umap_clusters.png` | UMAP colored by Leiden cluster (19 clusters) |
| `umap_conditions.png` | UMAP colored by disease condition (5 stages) |
| `umap_annotated_celltypes.png` | UMAP colored by annotated cell type (16 types) |
| `umap_published_celltypes.png` | UMAP colored by published labels from Mei et al. 2023 |
| `cell_proportion_by_condition.png` | Stacked bar chart of cell type proportions per condition |
| `elbow_plot.png` | PCA elbow plot used to select number of dimensions (30) |

## Key Observations

- T lymphocytes dominate the TME (CD8 CTL 24.8% + CD4 Th 21.0% + TNK 6.8% + Tregs 5.3% = ~58%)
- Immune cell proportions shift across disease stages: Treg enrichment in tumor, NK/CTL changes
- Epithelial/tumor compartment (Epithelial + Basal + Tumor = 7.5%) is a minor fraction by cell count
- Stromal cells (Endothelial + Pericytes + Fibroblasts = 11.9%) form a substantial niche
