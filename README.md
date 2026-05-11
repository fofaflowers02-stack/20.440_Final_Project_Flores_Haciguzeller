# Transcriptomic Analysis of PCOS: Gene Regulatory Networks and Pathway Enrichment

**Sofia Flores & Irem Haciguzeller**  
20.440 Analysis of Biological Networks | MIT Spring 2025

---

## Overview

This repository contains the full computational analysis for our 20.440 final project, investigating whether polycystic ovary syndrome (PCOS) produces a transcriptional signature in adipose tissue that is independent of obesity — a major confounding factor in most PCOS studies.

We use two independent publicly available datasets and a multi-method pipeline spanning differential expression analysis, gene regulatory network (GRN) construction, gene set enrichment analysis (GSEA), transcription factor enrichment, and non-negative matrix factorization (NMF).

---

## Repository Contents

| File | Description |
|------|-------------|
| `20_440_Project_FinalCode_SofiaFlores.ipynb` | Full analysis pipeline (run in Google Colab) |
| `differentially_expressed_genes.csv` | DESeq2 output: 86 significant DEGs from GSE267287 |
| `20.440 Figure 1.png` | Study overview / experimental design figure |
| `volcano plot.png` | Volcano plot of differential expression results |
| `degree centrality.png` | Degree centrality bar chart across all 86 DEGs (Q1–Q4) |
| `Quartile 4.png` | Hub gene enrichment results for top-quartile DEGs |
| `grn 2.png` | Full gene regulatory network visualization |
| `grn hubs only 2.png` | GRN visualization showing Q4 hub genes only |

---

## Datasets

Both datasets are publicly available on NCBI GEO and must be downloaded before running the notebook.

| Accession | Tissue | Comparison |
|-----------|--------|------------|
| [GSE267287](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE267287) | iPSC-derived mesenchymal progenitor cells | 4 PCOS vs. 4 control (testosterone-free samples only) |
| [GSE5090](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE5090) | Omental adipose tissue | Obese PCOS vs. obese controls (n=17) |

---

## Analysis Pipeline

The notebook is structured sequentially — run cells from top to bottom.

**Dataset 1 — GSE267287 (iPSC-derived progenitor cells)**
1. Data loading and filtering (testosterone-free samples only)
2. Differential expression via `pydeseq2` (padj < 0.05, |log2FC| > 0.5)
3. Volcano plot visualization
4. Gene regulatory network construction using Pearson correlation (|r| ≥ 0.8) via NetworkX
5. Degree centrality calculation and quartile stratification (Q1–Q4)
6. Per-quartile GSEA via `gseapy` Enrichr

**Dataset 2 — GSE5090 (omental adipose tissue)**
1. Series matrix loading and preprocessing
2. PCA and hierarchical clustering
3. Preranked GSEA using MSigDB Hallmark gene sets
4. Transcription factor enrichment analysis
5. Non-negative matrix factorization (NMF, k=3)

---

## Requirements

This analysis was developed and run in **Google Colab (Python 3.12)**. To run the notebook, open it in Colab and execute the installation cells at the top, which install:

```
pydeseq2
gseapy
networkx
pandas
numpy
scipy
scikit-learn
matplotlib
seaborn
arboreto
```

> **Note on arboreto:** The notebook installs arboreto for GRNBoost2 but uses Pearson correlation instead due to a Dask version incompatibility in Colab (Dask 2026.x). The correlation-based GRN produces equivalent results for this dataset size.

---

## Key Results

- **86 DEGs** identified in PCOS iPSC-derived progenitor cells (padj < 0.05, |log2FC| > 0.5)
- Top hub genes by degree centrality: *LRRC27*, *SPIRE2*, *UNC93B1*, *IL18R1*, *DOC2GP* — converging on vesicle trafficking, innate immune activation, and insulin signaling
- GSEA in obesity-matched adipose tissue reveals PCOS-specific enrichment of **adipogenesis**, **oxidative phosphorylation**, and **fatty acid metabolism**
- Inflammatory pathways (TNFα/NF-κB, IL6/JAK/STAT3) are relatively depleted in PCOS vs. obese controls, suggesting PCOS-associated inflammation is largely driven by comorbid obesity rather than PCOS itself
- Upstream TF enrichment implicates **PPARG**, **CREB1**, and **HNF4A** as key drivers of the PCOS-specific transcriptional program

---

## Limitations

- Small sample sizes (n=8 for GSE267287; n=17 for GSE5090) limit statistical power and inflate Pearson correlation estimates
- Pearson correlation used for GRN; Spearman would be more robust for RNA-seq data
- Several top hub genes are ribosomal pseudogenes (RPS14P1, RPS14P3, BMS14P8), likely reflecting read misalignment artifacts common in low-n RNA-seq
- GEO-preprocessed expression values used for GSE5090 due to RMA normalization constraints in Colab

---

## Citation

> Flores S & Haciguzeller I. *Transcriptomic Analysis of PCOS: Gene Regulatory Networks and Pathway Enrichment.* 20.440 Analysis of Biological Networks, MIT, 2025.
