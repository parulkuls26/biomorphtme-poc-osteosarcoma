### What is BioMorph-TME?

BioMorph-TME is a proposed multimodal framework that aims to learn tumour ecosystem states by integrating morphology-derived embeddings with spatial transcriptomics. It combines foundation model representations from Cell Painting and digital pathology with spatial measurements of cell states, signalling networks, and tissue architecture. Through contrastive learning, BioMorph-TME seeks to align morphology with underlying tumour biology and cell–cell communication programs. The framework incorporates active learning and experimental validation in a lab-in-the-loop setting. Ultimately, BioMorph-TME aims to create a scalable morphology-based representation space for studying tumour ecosystems, therapeutic response, and disease evolution.

### Why it may be relevant to morphology foundation models

* **Extends morphology from single-cell phenotypes to ecosystem biology** by incorporating immune, stromal, and tumour interactions that are largely absent from current Cell Painting resources.

* **Provides biological supervision for morphology embeddings** through spatial transcriptomics, ligand–receptor communication networks, and pathway activity programs, improving interpretability of learned representations.

* **Enables scalable tumour microenvironment profiling** by learning morphology signatures associated with spatially defined ecosystem states, potentially reducing reliance on expensive spatial transcriptomic assays.


## Dataset

All data used in this notebook are publicly available at
[osteosarc.com](https://osteosarc.com), hosted on Backblaze B2 at
[b2.osteosarc.com](https://b2.osteosarc.com).

| Modality | Description |
|----------|-------------|
| scRNA-seq + VDJ-T | 10x Genomics Cell Ranger multi, T1/T2/T3 tumour + 12 PBMC draws |
| Xenium spatial | 428-gene panel, 122,202 cells, May 2026 run |
| WGS | Sarek pipeline, T0/T1/T2, matched normal |
| ctDNA | Personalis (tumour fraction) + Signatera (MTM/mL) longitudinal |
| Flow cytometry | 18 longitudinal blood draws, CD4/CD8/activation markers |
| Clinical | Treatment timeline, vaccine doses V1–V4, MRD measurements |

---

## Analyses

### 1. Clinical Context
- bGIS trajectory (Xu et al. 2024, Cancer Cell 42, 1598–1613)
- bTMB, bITH, bCIN metrics across T0/T1/T2
- Personalis ctDNA, Signatera MRD, somatic VAF trajectories

### 2. TCR Clonotype Persistence
**Tool:** Scirpy v0.24  
**Method:** Exact CDR3β amino acid identity, receptor_arms='VDJ'  
**Key findings:**
- 44,959 cells across tumour (T1/T2/T3) and blood (Jan/Apr/Aug 2025)
- 23,516 unique clonotypes defined
- **40 clonotypes persistent across all 3 tumour resections**
- **36/40 (90%) detectable in peripheral blood**
- **26/40 (65%) present across all 3 blood timepoints**
- Dominant clone CASSIPGTGSVTGELFF shows tumour→blood redistribution under vaccine pressure

### 3. NicheNet/LIANA Signalling
**Tool:** LIANA v1.7.3, rank_aggregate  
**Methods combined:** CellPhoneDB, NATMI, Connectome, log2FC, SingleCellSignalR  
**Input:** 19,547 cells, T1+T2 tumour scRNA-seq, 20 cell types  
**Receiver:** CD8+ Cytotoxic T cells  
**Key findings:**
- 4,939 ligand-receptor pairs evaluated
- Top interaction: **MMP9→CD44 (Osteoclast→CD8 T)** — highest combined magnitude and specificity
- HLA-DR→LAG3 cluster from Monocytes — immunosuppressive checkpoint axis
- **Cancer-associated Fibroblasts ranked as strongest overall sender** to CD8 T cells

### 4. Xenium Spatial Context
**Data:** 122,202 cells, 428-gene panel  
**Key populations:**
- Cluster 1 TCR sequences match clonotypes persistent across all 3 tumour resections
- SPP1+ TAMs (top NicheNet sender) spatially adjacent to tumour and T cell clusters
- 13 annotated cell populations including osteosarcoma-specific ASPM mutant tumour cells

---

## Key Figures

### TCR Clonotype Persistence
![TCR figure](figures/biomorphtme_poc_tcr_figure.png)

### NicheNet/LIANA Signalling
![NicheNet figure](figures/biomorphtme_poc_nichenet_figure.png)

### Xenium Spatial Context
![Xenium highlight](figures/xenium_highlight_immune.png)

---

## Limitations

- Single patient dataset — findings are hypothesis-generating not generalisable
- No Cell Painting data — morphological alignment not yet demonstrated
- LIANA run on pooled T1+T2 — temporal directionality of signalling not resolved
- TCR clonotypes defined by CDR3β only — paired alpha-beta would increase specificity
- Xenium provides spatial context only — formal spatial niche modelling not performed
- Cell type annotations inherited from Kamil's pre-annotated pipeline

---

## Connection to BioMorph-TME Proposal

This notebook establishes biological feasibility for three components that would
serve as supervisory signals in the full BioMorph-TME framework:

| PoC Component | BioMorph-TME Role |
|---------------|-------------------|
| TCR clonotype persistence | Functional immune outcome variable for validation |
| LIANA ligand-receptor interactions | Supervisory signal for contrastive alignment |
| Xenium spatial context | Spatial substrate for morphology-transcriptomics pairing |

The full BioMorph-TME pipeline — cross-modal contrastive learning aligning
Cell Painting morphology embeddings with NicheNet-derived signalling vectors —
is the next experimental step and is not performed in this notebook.

---

## Environment

```bash
# Core dependencies
python>=3.10
scanpy>=1.9
scirpy==0.24.0
liana==1.7.3
anndata
pandas
numpy
matplotlib
awkward
```

---

## Citation

If you use this notebook or the BioMorph-TME framework please cite:

- Xu et al. 2024, Cancer Cell 42, 1598–1613 (bGIS framework)
- Sijbrandij osteosarcoma dataset: [osteosarc.com](https://osteosarc.com)
- LIANA: Dimitrov et al. 2022, Nature Cell Biology
- Scirpy: Sturm et al. 2020, Bioinformatics

---

## Contact

Dr. Parul Kulshreshtha  
GitHub: [parulkuls26](https://github.com/parulkuls26)  
