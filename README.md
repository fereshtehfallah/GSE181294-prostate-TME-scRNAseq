# Single-Cell Transcriptomic Dissection of the Prostate Cancer Tumor Microenvironment

> **Single-Cell Transcriptomic Dissection of the Prostate Cancer Tumor Microenvironment Across Disease Progression: Cell Type Composition, Trajectory Inference, and Cell-Cell Communication Networks**

## Overview

This repository contains the full computational analysis pipeline, results, and manuscript for a comprehensive single-cell RNA-sequencing (scRNA-seq) study of the prostate cancer tumor microenvironment (TME). Using the publicly available dataset [GSE181294](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE181294) (Mei et al. 2023), we profiled **149,772 high-quality cells** from **37 samples** across **5 disease stages** to characterize TME cellular composition, lineage trajectories, and intercellular communication networks.

---

## Key Findings

| Finding | Detail |
|---|---|
| Dataset | GSE181294, 22 patients, 37 samples, 5 disease stages |
| Cells profiled | 149,772 (94.9% QC retention from 157,819) |
| Cell types identified | 16 distinct cell types across 19 Leiden clusters |
| Interaction growth | 964 (normal) → 1,716 (tumor\_LG) intercellular interactions |
| Gained pathways | 32 signaling pathways gained in tumor vs. normal |
| Key axes | CCL20–CCR6 myeloid–Treg recruitment; HLA-E/NKG2A immune evasion in high-grade tumor |
| Angiogenesis | VEGF and ANGPT pathways activated in tumor stages |

---

## Dataset

- **GEO accession:** [GSE181294](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE181294)
- **Reference:** Mei et al. 2023 (*PMID: 36750562*)
- **Conditions:** normal prostate (n=5 patients), adjacent tissue low-grade (adjacent\_LG, n=9), adjacent tissue high-grade (adjacent\_HG, n=6), tumor low-grade (tumor\_LG, n=12), tumor high-grade (tumor\_HG, n=9)
- **Technology:** 10x Genomics Chromium scRNA-seq

Raw data (`.rds`, `.h5ad`) are not tracked in this repository due to file size. Download instructions are provided below.

---

## Repository Structure

```
.
├── 01_qc/                        # Quality control results
│   ├── qc_summary_table.csv      # Per-sample QC metrics (37 samples)
│   ├── qc_violin_by_condition.png
│   └── cells_per_sample.png
│
├── 02_seurat/                    # Seurat clustering & annotation
│   ├── cell_metadata.csv         # Full cell metadata (149,772 × 16 columns)
│   ├── umap_clusters.png
│   ├── umap_conditions.png
│   ├── umap_annotated_celltypes.png
│   ├── umap_published_celltypes.png
│   ├── cell_proportion_by_condition.png
│   └── elbow_plot.png
│
├── 03_trajectory/                # PAGA + Diffusion Pseudotime
│   ├── pseudotime_assignments.csv
│   ├── paga_connectivity.csv
│   ├── paga_celltypes.png
│   ├── paga_conditions.png
│   ├── paga_heatmap.png
│   ├── pseudotime_umap.png
│   ├── pseudotime_by_celltype.png
│   └── pseudotime_by_condition.png
│
├── 06_cellchat/                  # CellChat v2 cell-cell communication
│   ├── per_condition/            # Condition-specific CellChat objects & plots
│   │   ├── normal/
│   │   ├── adjacent_LG/
│   │   ├── adjacent_HG/
│   │   ├── tumor_LG/
│   │   └── tumor_HG/
│   ├── comparative/              # Cross-condition comparative analysis
│   │   ├── interaction_summary.csv
│   │   ├── differential_pathways.csv
│   │   ├── pathway_summary_comparative.csv
│   │   └── *.png (heatmaps, bubble plots, network plots)
│   └── significant_interactions_*.csv
│
├── execution_trace/              # Analysis notebooks & plan
│   ├── main-worker.ipynb         # Main scRNA-seq pipeline (QC → Seurat → Trajectory)
│   ├── doc-worker.ipynb          # Manuscript generation notebook
│   └── PLAN.md                   # Detailed analysis plan
│
├── report_GSE181294_TME_analysis.docx   # Drafted manuscript
├── environment.yml               # Conda environment (Python dependencies)
├── requirements_r.md             # R package requirements
└── CITATION.md                   # References and citation
```

---

## Analysis Pipeline

### 1. Quality Control (`01_qc/`)

Cells were filtered using the following thresholds:

| Metric | Threshold |
|---|---|
| nFeature\_RNA | 200 – 6,000 genes |
| nCount\_RNA | 500 – 30,000 UMIs |
| percent.mt | < 20% |

**Result:** 149,772 / 157,819 cells retained (94.9%)

### 2. Dimensionality Reduction & Clustering (`02_seurat/`)

- **Normalization:** LogNormalize (scale factor = 10,000)
- **HVGs:** 3,000 variable features
- **Dimensionality reduction:** PCA (50 PCs), Harmony batch correction (by `sample_id`)
- **Clustering:** UMAP (dims 1:30) + Leiden clustering (resolution = 0.6) → 19 clusters
- **Annotation:** Majority-vote mapping to published cell labels (Mei et al. 2023)

**16 identified cell types:**

| Cell Type | Cells | % |
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

### 3. Trajectory Inference (`03_trajectory/`)

- **Method:** PAGA + Diffusion Pseudotime (DPT) via Scanpy
- **Input:** 3,000 HVG subset × 149,772 cells (AnnData format)
- **Parameters:** `n_neighbors=15`, `n_pcs=30`; DPT rooted in normal epithelial cells (n=877)
- **Key trajectory axes:**
  - Epithelial lineage: Epithelial → Basal → Tumor (pseudotime 0.09 → 0.27 → 0.43)
  - T cell co-stimulatory hub: CD4 T cells ↔ Tregs (PAGA connectivity = 0.28)
  - Myeloid axis: Monocytes ↔ Myeloid Mac/DC (connectivity = 0.26)

### 4. Cell-Cell Communication (`06_cellchat/`)

- **Tool:** CellChat v2.2.0
- **Database:** CellChatDB.human (3,233 L-R pairs, 290 pathways)
- **Method:** triMean communication probability; `population.size=TRUE`; `min.cells=10`
- **Comparison:** 5 condition-specific CellChat objects merged for comparative analysis

**Interaction landscape across disease progression:**

| Condition | Interactions | Strength | Pathways |
|---|---|---|---|
| Normal | 964 | 0.229 | 25 |
| Adjacent\_LG | 1,263 | 0.481 | 35 |
| Adjacent\_HG | 1,522 | 0.471 | 52 |
| Tumor\_LG | 1,716 | 0.426 | 52 |
| Tumor\_HG | 1,630 | 0.368 | 46 |

**Key signaling axes:**
- **CCL20–CCR6:** Myeloid-driven Treg recruitment; present in adjacent/early tumor, absent in normal and tumor\_HG
- **CXCL:** Progressive chemokine-mediated immune trafficking across all tumor stages
- **MHC-II:** Antigen presentation axis emerging in adjacent\_HG and tumors (Myeloid/DC → CD4 T)
- **VEGF:** Angiogenic signaling gained in tumor stages (Endothelial/Fibroblast → Pericyte)
- **HLA-E/NKG2A:** Non-classical MHC-I inhibitory checkpoint — tumor\_HG-specific immune evasion

---

## Software Requirements

### Python (Trajectory & Report Generation)

See [environment.yml](environment.yml) for the full Conda environment.

Key packages:
- `scanpy >= 1.9`
- `anndata >= 0.9`
- `python-docx`
- `pandas`, `numpy`, `matplotlib`, `seaborn`

### R (Seurat & CellChat)

See [requirements_r.md](requirements_r.md) for package versions.

Key packages:
- `Seurat >= 4.3`
- `harmony >= 0.1`
- `CellChat >= 2.2.0`
- `leiden` (via `leidenAlg`)

---

## Reproducing the Analysis

### 1. Download raw data

```bash
# Download from GEO (requires sra-tools or manual download)
# Accession: GSE181294
wget "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE181nnn/GSE181294/suppl/GSE181294_RAW.tar"
```

### 2. Set up environment

```bash
conda env create -f environment.yml
conda activate prostate-tme
```

### 3. Run the pipeline

The analysis is implemented as Jupyter notebooks in `execution_trace/`:

```bash
jupyter notebook execution_trace/main-worker.ipynb   # QC, Seurat, Trajectory
jupyter notebook execution_trace/doc-worker.ipynb    # Report generation
```

---

## Manuscript

The drafted manuscript is available as [report_GSE181294_TME_analysis.docx](report_GSE181294_TME_analysis.docx).

**Title:** Single-Cell Transcriptomic Dissection of the Prostate Cancer Tumor Microenvironment Across Disease Progression: Cell Type Composition, Trajectory Inference, and Cell-Cell Communication Networks

---

## Citation

If you use this analysis or data in your work, please cite:

**Primary dataset:**
> Mei S, et al. (2023). Immunotherapy of Prostate Cancer Using Single-Cell Transcriptome Analysis. *PMID: 36750562*

**This analysis:**
> [Manuscript in preparation — see CITATION.md for full reference list]

See [CITATION.md](CITATION.md) for all software references.

---

## License

This repository contains analysis code and results derived from publicly available data (GSE181294). Code is provided for academic and research use. The manuscript (`.docx`) is the intellectual property of the authors.
