# Module 03 — Trajectory Inference (PAGA + Diffusion Pseudotime)

## Purpose

Reconstruct lineage relationships and developmental ordering of cells across the prostate TME using graph abstraction (PAGA) and diffusion pseudotime (DPT) via Scanpy.

## Methods

- **Tool:** Scanpy (Wolf et al. 2018)
- **Input:** AnnData assembled from Seurat HVG subset (3,000 genes × 149,772 cells)
- **Graph construction:** `sc.pp.neighbors(n_neighbors=15, n_pcs=30)`
- **PAGA:** `sc.tl.paga()` — partition-based graph abstraction on cell type labels
- **DPT root:** Normal epithelial cells (n=877 cells; the most undifferentiated compartment)
- **Output:** Pseudotime value for all 149,772 cells + 16×16 PAGA connectivity matrix

## Key Trajectory Axes

| Axis | Cell Types | PAGA Connectivity | Pseudotime Range |
|---|---|---|---|
| Epithelial lineage | Epithelial → Basal → Tumor | — | 0.09 → 0.27 → 0.43 |
| T cell hub | CD4 T cells ↔ Tregs | 0.28 | — |
| Myeloid axis | Monocytes ↔ Myeloid Mac/DC | 0.26 | — |

**Interpretation:** The epithelial-to-tumor axis captures malignant progression. The Treg–CD4 hub reflects immunosuppressive co-stimulatory crosstalk. The myeloid axis indicates differentiation from circulating monocytes to tissue-resident macrophages/dendritic cells.

## Files

| File | Description |
|---|---|
| `pseudotime_assignments.csv` | DPT pseudotime value for each of the 149,772 cells |
| `paga_connectivity.csv` | 16×16 PAGA connectivity matrix (cell type pairs) |
| `paga_celltypes.png` | PAGA graph with nodes colored by cell type |
| `paga_conditions.png` | PAGA graph with node proportions by disease condition |
| `paga_heatmap.png` | Heatmap of top PAGA connectivity values |
| `pseudotime_umap.png` | UMAP colored by DPT pseudotime (continuous scale) |
| `pseudotime_by_celltype.png` | Violin plot of pseudotime distribution per cell type |
| `pseudotime_by_condition.png` | Violin plot of pseudotime distribution per disease stage |

## Interpretation Notes

- Higher pseudotime values indicate more differentiated / advanced transcriptional states
- Normal epithelial cells anchor the root (pseudotime ≈ 0)
- Tumor cells reach the highest pseudotime (≈ 0.43), consistent with malignant transformation
- T cell and myeloid axes reflect immune remodeling rather than classical lineage differentiation
