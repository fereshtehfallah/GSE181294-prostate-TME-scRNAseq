# Module 06 — Cell-Cell Communication Analysis (CellChat v2)

## Purpose

Map intercellular signaling networks across 5 disease stages using CellChat v2 to identify key ligand-receptor interactions, signaling pathways gained or lost during prostate cancer progression, and sender/receiver roles of each cell type.

## Methods

- **Tool:** CellChat v2.2.0 (Jin et al. 2021)
- **Database:** CellChatDB.human — 3,233 L-R pairs across 290 signaling pathways
- **Communication probability:** triMean method; `population.size=TRUE`; `min.cells=10`
- **Cell type merging:** Tumor cells (n=153, tumor\_LG only) merged into Epithelial cells prior to per-condition analysis to achieve a shared 15-cell-type label space
- **Comparative analysis:** `liftCellChat` to shared label space; `mergeCellChat` for cross-condition comparison
- **Centrality:** `netAnalysis_computeCentrality` for sender/receiver/mediator/influencer quantification

## Interaction Summary

| Condition | Interactions | Total Strength | Pathways |
|---|---|---|---|
| Normal | 964 | 0.229 | 25 |
| Adjacent\_LG | 1,263 | 0.481 | 35 |
| Adjacent\_HG | 1,522 | 0.471 | 52 |
| Tumor\_LG | 1,716 | 0.426 | 52 |
| Tumor\_HG | 1,630 | 0.368 | 46 |

Progressive increase: **+78% interactions** and **+108% pathways** from normal to tumor\_LG.
Slight decrease in tumor\_HG consistent with immune exhaustion.

## Key Signaling Axes

| Pathway | Pattern | Significance |
|---|---|---|
| CCL20–CCR6 | Myeloid → Treg; present in adjacent/tumor\_LG, absent normal & tumor\_HG | Myeloid-driven Treg recruitment in early tumor stages |
| CXCL | Progressive activation across all stages | Chemokine-mediated immune trafficking |
| MHC-II | Emerges in adjacent\_HG and tumors (Myeloid/DC → CD4 T) | Antigen presentation axis upregulation |
| VEGF | Gained in tumor stages (Endothelial/Fibroblast → Pericyte) | Tumor angiogenic switch |
| HLA-E/NKG2A | Tumor\_HG specific | Non-classical MHC-I inhibitory checkpoint; CTL/NK suppression |
| ANGPT | Gained in tumor\_LG | Angiopoietin angiogenesis and vascular remodeling |

## Tumor-Gained Pathways

32 pathways gained in tumor vs. normal (0 lost), reflecting progressive TME complexity:

**Immune evasion:** HLA-E/NKG2A, CD160, SELPLG
**Immunosuppression:** Prostaglandin, GALECTIN, COMPLEMENT, BAFF, VISFATIN
**Angiogenesis:** VEGF, ANGPT, CDH5
**Stromal remodeling:** COLLAGEN (enhanced), LAMININ, CDH, FN1 (enhanced), FLRT
**Antigen presentation:** MHC-II, CD40
**Inflammation:** TNF, CCL, NOTCH, IFN-II, OSM (enhanced)
**Metabolic:** Cholesterol, Testosterone, RA (retinoic acid)

## Directory Structure

```
06_cellchat/
├── per_condition/                    # Condition-specific analysis
│   ├── normal/
│   ├── adjacent_LG/
│   ├── adjacent_HG/
│   ├── tumor_LG/
│   └── tumor_HG/
│       ├── interaction_count_matrix.csv
│       ├── interaction_strength_matrix.csv
│       ├── interaction_count_network.png
│       ├── interaction_strength_network.png
│       ├── bubble_ligand_receptor.png
│       ├── signaling_roles.csv
│       ├── signaling_incoming_heatmap.png
│       ├── signaling_outgoing_heatmap.png
│       └── pathway_*.png
│
├── comparative/                       # Cross-condition comparison
│   ├── interaction_summary.csv        # Aggregate counts per condition
│   ├── interaction_overview_summary.png
│   ├── interaction_count_comparison.png
│   ├── interaction_strength_comparison.png
│   ├── pathway_activity_heatmap.png
│   ├── differential_pathways.csv      # 62 pathways × presence/absence matrix
│   ├── pathway_summary_comparative.csv
│   ├── gained_pathways_tumor.png
│   ├── diff_network_tumorHG_vs_normal.png
│   ├── diff_network_tumorLG_vs_normal.png
│   ├── role_heatmap_*.png             # Sender/receiver heatmaps per condition
│   ├── sender_receiver_*.png          # Scatter plots per condition
│   ├── CCL20_CCR6_bubble.png
│   ├── CCL20_CCR6_interactions.csv
│   ├── bubble_CXCL_all_conditions.png
│   ├── bubble_MHC-II_all_conditions.png
│   └── bubble_VEGF_all_conditions.png
│
└── significant_interactions_*.csv     # Per-condition significant L-R pairs
```

## Significant Interaction Files

| File | Description |
|---|---|
| `significant_interactions_normal.csv` | Significant L-R pairs in normal tissue |
| `significant_interactions_adjacent_LG.csv` | Significant L-R pairs in adjacent low-grade |
| `significant_interactions_adjacent_HG.csv` | Significant L-R pairs in adjacent high-grade |
| `significant_interactions_tumor_LG.csv` | Significant L-R pairs in tumor low-grade |
| `significant_interactions_tumor_HG.csv` | Significant L-R pairs in tumor high-grade |
