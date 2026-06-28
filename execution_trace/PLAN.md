# Plan: Comprehensive Word Report — GSE181294 Prostate Cancer TME scRNA-seq Analysis

## Summary
Generate a high-quality, publication-style `.docx` report covering all completed analysis modules for GSE181294 (prostate cancer TME, Mei et al. 2023). The document will be written from the perspective of an expert computational biologist, with full biological interpretation of every result. It will be structured as a scientific manuscript with Abstract, Introduction, Methods, Results, and Discussion.

---

## Document Structure

### Title
**Single-Cell Transcriptomic Dissection of the Prostate Cancer Tumor Microenvironment Across Disease Progression: Cell Type Composition, Trajectory Inference, and Cell-Cell Communication Networks**

### Authors / Affiliation block
Placeholder line (Angela fal et al.)

### Abstract (~350 words)
- Background: prostate cancer TME heterogeneity, need for single-cell resolution
- Methods summary: GSE181294, 149,772 cells, 5 disease stages, Seurat clustering, PAGA/DPT, CellChat v2
- Key findings: 16 cell types, progressive increase in intercellular communication (964→1,716 interactions), 32 tumor-gained pathways, CCL20-CCR6 myeloid-Treg axis, HLA-E/NKG2A immune evasion in tumor_HG
- Conclusion sentence

### 1. Introduction (~600 words)
- Prostate cancer epidemiology and clinical staging (Gleason grade, low vs high grade)
- Limitations of bulk RNA-seq; rationale for scRNA-seq
- The TME as a therapeutic target: immune evasion, stromal remodeling, angiogenesis
- GSE181294 dataset context (Mei et al. 2023, PMID 36750562)
- Study objectives: (1) characterize TME cell types, (2) infer lineage trajectories, (3) map intercellular communication networks across 5 disease stages

### 2. Materials and Methods
**2.1 Dataset and Preprocessing**
- GEO accession GSE181294; 37 samples, 5 conditions (normal, adjacent_LG, adjacent_HG, tumor_LG, tumor_HG), 22 patients
- QC thresholds: nFeature_RNA 200–6000, nCount_RNA 500–30000, percent.mt < 20
- Cells retained: 149,772 / 157,819 (94.9%)

**2.2 Normalization, Dimensionality Reduction, and Clustering**
- LogNormalize (scale.factor=10,000), FindVariableFeatures (nfeatures=3,000)
- ScaleData (all genes), PCA (50 PCs), Harmony batch correction (by sample_id)
- UMAP (dims=1:30), Leiden clustering (resolution=0.6) → 19 clusters
- Cell type annotation: majority-vote mapping to published labels (Mei et al. 2023)

**2.3 Trajectory Inference (PAGA + DPT)**
- AnnData assembled from HVG-subset (3,000 genes × 149,772 cells)
- Scanpy: neighbors (n_neighbors=15, n_pcs=30), PAGA connectivity
- Diffusion pseudotime (DPT) rooted in normal epithelial cells (877 cells)
- Pseudotime exported for all 149,772 cells

**2.4 Cell-Cell Communication Analysis (CellChat v2)**
- CellChat v2.2.0; CellChatDB.human (3,233 L-R pairs, 290 pathways)
- 5 condition-specific objects; triMean communication probability; population.size=TRUE; min.cells=10
- Tumor cells (153, only in tumor_LG) merged into Epithelial cells prior to analysis
- liftCellChat to shared 15-cell-type label space; mergeCellChat for comparative analysis
- netAnalysis_computeCentrality for sender/receiver role quantification

### 3. Results

**3.1 Quality Control and Cell Composition**
- Table 1: QC summary per sample (37 samples, cells before/after, median nFeature, nCount, %MT)
- Figure 1: QC violin plots (nFeature, nCount, %MT by condition)
- Figure 2: Cells per sample bar chart
- Key finding: high-quality dataset, 94.9% retention, low mitochondrial contamination

**3.2 Single-Cell Transcriptomic Landscape of the Prostate TME**
- Figure 3: UMAP colored by cluster (19 clusters)
- Figure 4: UMAP colored by condition (5 disease stages)
- Figure 5: UMAP colored by annotated cell type (16 types)
- Figure 6: Cell proportion by condition (stacked bar)
- Table 2: Cell type counts and proportions per condition
- Biological interpretation: immune cell dominance (CD8 CTL 24.8%, CD4 Th 21.0%), epithelial/tumor compartment, stromal cells; shifts in immune composition across stages

**3.3 Lineage Trajectory and Pseudotime Analysis**
- Figure 7: PAGA graph colored by cell type
- Figure 8: PAGA connectivity heatmap (top connections)
- Figure 9: UMAP colored by DPT pseudotime
- Figure 10: Pseudotime distribution by condition (violin)
- Figure 11: Pseudotime distribution by cell type (violin)
- Table 3: Top PAGA connectivity values (cell type pairs)
- Biological interpretation: epithelial lineage axis (Epithelial→Basal→Tumor, pseudotime 0.09→0.27→0.43); T cell co-stimulatory axis (CD4↔Treg, 0.28); myeloid axis (Monocyte↔Myeloid, 0.26)

**3.4 Cell-Cell Communication Networks Across Disease Progression**

*3.4.1 Global Communication Landscape*
- Figure 12: Interaction overview (3-panel: counts, strength, pathways per condition)
- Table 4: Interaction summary (n_interactions, total_strength, n_pathways per condition)
- Biological interpretation: progressive increase normal→tumor_LG (+78% interactions, +108% pathways); slight decrease tumor_HG (immune exhaustion hypothesis)

*3.4.2 Pathway Activity Across Disease Stages*
- Figure 13: Pathway activity heatmap (60 pathways × 5 conditions)
- Figure 14: Pathway rankNet (stacked information flow comparison)
- Table 5: Top 10 pathways per condition (by communication probability)
- Biological interpretation: constitutive pathways (MHC-I, CLEC, MIF, CD99, CypA, COLLAGEN); tumor-gained pathways (MHC-II, VEGF, CCL, ANGPT, TNF, COMPLEMENT, Prostaglandin)

*3.4.3 Tumor-Gained Signaling Pathways*
- Figure 15: Gained pathways bar chart (32 pathways, ranked by max probability)
- Table 6: Differential pathway table (60 pathways, presence/absence per condition, gained/lost annotation)
- Biological interpretation: 32 pathways gained in tumor, 0 lost — TME becomes progressively more complex; key gained pathways grouped by function (immune evasion, angiogenesis, immunosuppression, stromal remodeling, metabolic)

*3.4.4 Differential Interaction Networks*
- Figure 16: Differential network tumor_HG vs normal (count + strength)
- Figure 17: Differential network tumor_LG vs normal (count + strength)
- Biological interpretation: which cell type pairs gain/lose interactions; myeloid and stromal cells as major drivers of gained interactions

*3.4.5 Sender and Receiver Cell Type Roles*
- Figure 18: Sender/receiver scatter plots (5 conditions, 2×3 panel)
- Figure 19: Signaling role heatmaps (normal vs tumor_HG comparison)
- Biological interpretation: Myeloid/Mac/DC and Fibroblasts as dominant senders; T cells as dominant receivers; role shifts across disease stages

*3.4.6 Key Signaling Axes*
- Figure 20: CCL20-CCR6 bubble plot across conditions
- Figure 21: CXCL pathway bubble (all conditions)
- Figure 22: MHC-II pathway bubble (all conditions)
- Figure 23: VEGF pathway bubble (all conditions)
- Table 7: CCL20-CCR6 interactions (source, target, prob, condition)
- Biological interpretation:
  - CCL20-CCR6: myeloid-driven Treg recruitment in adjacent/early tumor stages; absent from normal and tumor_HG
  - CXCL: chemokine-mediated immune cell trafficking, progressive activation
  - MHC-II: antigen presentation axis emerging in adjacent_HG and tumors; Myeloid/DC → CD4 T cell
  - VEGF: angiogenic signaling gained in tumor stages; Endothelial/Fibroblast → Pericyte axis
  - HLA-E/NKG2A (tumor_HG-specific): non-classical MHC-I inhibitory checkpoint suppressing CTL/NK killing

### 4. Discussion (~800 words)
- Summary of findings in context of prostate cancer biology
- Progressive TME rewiring: from immune surveillance (normal) to immune evasion (tumor_HG)
- CCL20-CCR6 axis: validation of Mei et al. 2023 published finding; biological significance of myeloid-Treg crosstalk
- HLA-E/NKG2A emergence in tumor_HG: novel immune checkpoint axis; therapeutic implications (anti-NKG2A antibodies, monalizumab)
- MHC-II upregulation: paradox of increased antigen presentation alongside immune evasion
- VEGF/ANGPT angiogenic switch: tumor vascularization and immune exclusion
- Prostaglandin/PGE2 immunosuppression: T cell exhaustion mechanism
- Trajectory findings: epithelial-to-tumor lineage axis; Treg-CD4 co-stimulatory hub
- Limitations: cross-sectional design (no longitudinal tracking), single-sample pseudotime root, CellChat probability estimates are statistical (not functional validation)
- Future directions: spatial transcriptomics validation, functional assays for CCL20-CCR6 and HLA-E/NKG2A axes, integration with clinical outcomes

### 5. Supplementary Tables (embedded in document)
- Supp Table 1: All 7,095 significant L-R interactions (summary statistics per condition)
- Supp Table 2: Full 60-pathway differential table
- Supp Table 3: PAGA connectivity matrix (16×16)

---

## Formatting Specifications
- Font: Times New Roman 12pt body, 14pt section headers, 11pt figure captions
- Margins: 2.5 cm all sides
- Line spacing: 1.5
- Figure captions: bold figure number + descriptive caption below each figure
- Tables: formatted with header row shading (light blue), alternating row shading, borders
- All figures embedded inline at ~14 cm width
- Page numbers: bottom center
- References: numbered inline [1], [2]... with full reference list at end (Vancouver style)
- Key references: Mei et al. 2023 (PMID 36750562), Jin et al. 2021 CellChat v2, Wolf et al. 2018 Scanpy, Hao et al. 2021 Seurat v4, Korsunsky et al. 2019 Harmony

---

## Implementation Approach
- Use `python-pptx`-equivalent: **`python-docx`** library in Python
- Machine: recreate `main-worker` (2 CPU, 8 GB sufficient for docx generation — no heavy compute)
- Figures: embed all PNGs from `/mnt/results/` inline using `docx.add_picture()`
- Tables: build from CSV data read with pandas
- Output: `/workspace/report_GSE181294_TME_analysis.docx` → copy to `/mnt/results/`
- Estimated runtime: ~5 minutes

---

## Explicit Assumptions
1. All figures are already generated and available in `/mnt/results/`
2. Biological interpretation is written by the agent as domain expert (not extracted from external sources)
3. References are cited from known literature (Mei et al. 2023, CellChat, Seurat, Scanpy, Harmony papers)
4. The document is written in English, publication-ready style
5. Figure numbering follows order of appearance in Results section
6. No statistical re-analysis is performed — all numbers come from completed pipeline outputs
