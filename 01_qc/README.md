# Module 01 — Quality Control

## Purpose

Filter low-quality cells and doublets from the 37-sample GSE181294 dataset before downstream analysis.

## Thresholds Applied

| Metric | Min | Max |
|---|---|---|
| nFeature\_RNA (genes detected) | 200 | 6,000 |
| nCount\_RNA (UMIs per cell) | 500 | 30,000 |
| percent.mt (mitochondrial %) | 0 | 20 |

## Results

- **Cells before QC:** 157,819
- **Cells after QC:** 149,772
- **Retention rate:** 94.9%

## Files

| File | Description |
|---|---|
| `qc_summary_table.csv` | Per-sample QC metrics: cells before/after, median nFeature, nCount, %MT |
| `qc_violin_by_condition.png` | Violin plots of QC metrics split by condition (5 disease stages) |
| `cells_per_sample.png` | Bar chart of cell counts per sample before and after filtering |

## Sample Overview

37 samples from 22 patients across 5 disease stages:
- **Normal:** 5 samples (HP0–HP4)
- **Adjacent\_LG:** 9 samples
- **Adjacent\_HG:** 6 samples
- **Tumor\_LG:** 12 samples
- **Tumor\_HG:** 9 samples
