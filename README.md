# Network and Pathway-Level Transcriptomic Analysis of Adipose Dysfunction in Polycystic Ovary Syndrome

**MIT 20.440: Analysis of Biological Networks | Spring 2025**

**Authors:** Sofia Flores & H. Zeynep Haciguzeller

---

## Overview

Polycystic ovary syndrome (PCOS) is the most common reproductive-endocrine disorder in women of reproductive age, strongly associated with obesity and metabolic dysfunction. A key challenge in studying PCOS is disentangling disease-specific transcriptional effects from those driven by obesity itself.

This project takes an integrative, systems-level approach to characterize adipose-related gene expression changes in PCOS across two complementary datasets. Rather than focusing solely on individual differentially expressed genes, we combine differential expression analysis, gene correlation network construction, gene set enrichment analysis (GSEA), transcription factor enrichment, and unsupervised module detection (NMF) to capture transcriptional reprogramming across multiple biological scales.

**Central Hypotheses:** PCOS-associated adipose dysfunction is not driven by large, statistically significant changes in individual genes, but by subtle, coordinated transcriptional reprogramming across pathways and networks — characterized by shifts in metabolic and adipogenic programs alongside altered inflammatory signaling, rather than a simple amplification of obesity-associated inflammation. Gene regulatory network hub genes are candidate regulators connecting inflammatory, metabolic, and ECM-related processes.


---

## Datasets

### Dataset 1 — GSE267287 (RNA-seq, iPSC-derived Mesenchymal Progenitor Cells)

| Field | Detail |
|---|---|
| GEO Accession | [GSE267287](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE267287) |
| Data type | RNA-seq (bulk) |
| Platform | Illumina NovaSeq 6000 |
| Cell type | iPSC-derived mesenchymal progenitor cells (MPCs), adipocyte precursors |
| Samples | 8 total: 4 PCOS, 4 healthy controls |
| Comparison | PCOS vs. control (progenitor cell system) |
| Source paper | Transcriptome Analysis of Mesenchymal Progenitor Cells Revealed Molecular Insights into Metabolic Dysfunction and Inflammation in Polycystic Ovary Syndrome (MDPI, 2024) |

### Dataset 2 — GSE5090 (Microarray, Omental Adipose Tissue)

| Field | Detail |
|---|---|
| GEO Accession | [GSE5090](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE5090) |
| Data type | Affymetrix HG-U133A microarray (GPL96), 22,283 probes |
| Tissue | Omental adipose tissue (collected during bariatric surgery) |
| Samples | 17 total: 9 obese with PCOS, 8 obese without PCOS (controls) |
| Comparison | Obese PCOS vs. obese controls — **obesity-controlled design** |
| Source paper | Cortón et al., Differential gene expression profile in omental adipose tissue in women with polycystic ovary syndrome. *J. Clin. Endocrinol. Metab.* 92, 328–337 (2007) |

---

## Repository Structure

\```
20.440_Final_Project_Flores_Haciguzeller/
│
├── 20_440_Project_FinalCode_SofiaFlores.ipynb                    # GSE267287 analysis (DEG + GRN)
├── GSE5090_data_analysis_ipynb_05_11.ipynb                       # GSE5090 analysis (microarray pipeline)
│
├── 20.440 Figure 1.png                                            # GSE267287: volcano plot, GRN, degree centrality, GSEA by quartile
├── 20.440 Figure 2.png                                            # GSE5090: multi-panel summary figure
├── 20.440 Figure 3.png                                            # GSE5090: sample correlation, top variable genes, NMF modules
│
│── PCA.png                                                        # GSE5090: PCA of omental adipose gene expression
├── differential_gene_expression_analysis_volcano_plot.png         # GSE5090: volcano plot
├── differential_gene_expression_analysis_clustered_heatmap.png    # GSE5090: top DEG clustered heatmap
├── gene_set_enrichment_analysis_positively_enriched_pathways.png  # GSE5090: GSEA positive NES barplot
├── gene_set_enrichment_analysis_negatively_enriched_pathways.png  # GSE5090: GSEA negative NES barplot
├── transcription_factor_enrichment_for_upregulated_genes.png      # GSE5090: TF enrichment (upregulated)
├── transcription_factor_enrichment_for_downregulated_genes.png    # GSE5090: TF enrichment (downregulated)
├── sample-to-sample_pearson_correlation_heatmap.png               # GSE5090: sample correlation heatmap
├── clustered_heatmap_of_the_top_variable_genes.png                # GSE5090: top variable genes heatmap
│
├── volcano plot.png                                               # Standalone volcano plot (GSE267287)
├── degree centrality.png                                          # Degree centrality distribution (GRN)
├── grn 2.png                                                      # Full gene correlation network
├── grn hubs only 2.png                                            # GRN hub genes only (Q4)
└── Quartile 4.png                                                 # Top-quartile hub gene subnetwork
\```

---

## Methods

### Dataset 1: GSE267287 — RNA-seq Analysis

#### 1. Differential Expression Analysis
- Raw count matrices analyzed using **pydeseq2** (Python adaptation of DESeq2).
- Pipeline: count normalization → dispersion estimation → log2 fold-change → Wald test (PCOS vs. control).
- Significance thresholds: adjusted p-value (padj) < 0.05 and |log2FC| > 0.5.
- **86 genes** identified as significantly differentially expressed.
- Key DEGs include immune genes (*IL18R1*, *CD47*, *UNC93B1*), ECM remodeling genes (*TNC*, *ADAM23*), and metabolic regulators (*PLA2G1B*, *NADK2*).
- Results visualized as a volcano plot; gene symbols retrieved via the myGene library from Ensembl IDs.

#### 2. Gene Correlation Network (GRN) Construction
- Expression matrix filtered to the 86 DEGs.
- Pairwise **Pearson correlation** computed across all gene pairs (86×86 matrix).
- Edges retained for gene pairs with |r| ≥ 0.9 (strong co-expression threshold).
- Positive correlations colored blue; negative correlations colored red.

#### 3. Hub Gene Identification via Degree Centrality
- **Degree centrality** calculated for each gene as the fraction of other DEGs connected above the |r| ≥ 0.9 threshold.
- Genes ranked and divided into quartiles (Q1–Q4).
- **Q4 (top quartile)** = hub genes, candidates for central regulatory roles in PCOS transcriptional dysregulation.

#### 4. GSEA by Network Quartile
- Gene set enrichment analysis (via **Enrichr/gseapy**) performed separately on each quartile to identify biological processes enriched in highly vs. lowly connected DEGs.
- Key Q4 hub genes: *LRRC27* (top hub; ERBB-pathway-related), *SPIRE2* (oocyte biology), *IL18R1* & *UNC93B1* (innate immune), *DOC2GP* (insulin/GLUT4 vesicle trafficking).

---

### Dataset 2: GSE5090 — Microarray Analysis

#### 1. Data Preprocessing
- GEO Series Matrix parsed into a probe-by-sample expression matrix.
- Probe IDs mapped to gene symbols via **GPL96 platform annotation** (downloaded using GEOparse).
- Probes collapsed to gene-level by retaining the most significant probe per gene.
- Sample metadata used to assign PCOS and obese control labels.

#### 2. Principal Component Analysis (PCA)

![PCA of Omental Adipose Gene Expression](PCA.png)

- Expression matrix standardized (StandardScaler) and decomposed into 2 PCs.
- PC1 (22.7%) + PC2 (13.5%) = 36% variance explained.
- No clear group separation; consistent with subtle PCOS-specific effects in an obesity-controlled comparison.

#### 3. Differential Expression Analysis

![Volcano Plot](differential_gene_expression_analysis_volcano_plot.png)

![Clustered Heatmap of Top DEGs](differential_gene_expression_analysis_clustered_heatmap.png)

- **Welch's t-test** applied probe-by-probe; log2FC computed from mean expression.
- **Benjamini–Hochberg FDR correction** applied across all probes.
- No genes met combined thresholds (FDR < 0.05 and |log2FC| > 0.5) after multiple testing correction.
- Results visualized as a volcano plot; top-ranked genes shown in a hierarchically clustered heatmap (row-wise z-scored).

#### 4. Gene Set Enrichment Analysis (GSEA)

![GSEA Positively Enriched Pathways](gene_set_enrichment_analysis_positively_enriched_pathways.png)

![GSEA Negatively Enriched Pathways](gene_set_enrichment_analysis_negatively_enriched_pathways.png)

- Genes ranked by composite score: logFC × (−log10 p-value).
- Preranked GSEA via **gseapy** against **MSigDB Hallmark 2020** gene sets.
- **Top positively enriched pathways** (upregulated in PCOS): Adipogenesis (NES ≈ 2.17), Oxidative Phosphorylation, Protein Secretion, Fatty Acid Metabolism, Reactive Oxygen Species Pathway, Bile Acid Metabolism, Peroxisome, TGF-beta Signaling, Notch Signaling.
- **Top negatively enriched pathways** (downregulated in PCOS): Inflammatory Response, Epithelial–Mesenchymal Transition (EMT), TNF-alpha Signaling via NF-κB, IL-6/JAK/STAT3 Signaling, Allograft Rejection, KRAS Signaling Dn, mTORC1 Signaling, Unfolded Protein Response.

#### 5. Transcription Factor (TF) Enrichment Analysis

![TF Enrichment — Upregulated Genes](transcription_factor_enrichment_for_upregulated_genes.png)

![TF Enrichment — Downregulated Genes](transcription_factor_enrichment_for_downregulated_genes.png)

- Top 300 upregulated and top 300 downregulated genes analyzed via **Enrichr** (ChEA_2016 gene set).
- Upregulated TF enrichments: **HNF4A**, **NUCKS1**, **CREB1**, **PPARG**, **WT1**, **KDM5B**, **ZFX**, **DMRT1**, **RUNX1**.
- Downregulated TF enrichments: **BACH1**, **NUCKS1**, **KDM5B**, **RELA**, **SMAD2**, **SMAD3**, **HNF4A**, **DCP1A**, **EKLF**.

#### 6. Sample Similarity & Top Variable Genes

![Sample-to-Sample Pearson Correlation Heatmap](sample-to-sample_pearson_correlation_heatmap.png)

![Clustered Heatmap of Top Variable Genes](clustered_heatmap_of_the_top_variable_genes.png)

- **Sample-to-sample Pearson correlation heatmap** generated (r range: ~0.86–1.00); no outliers detected.
- **Top 50 most variable genes** visualized as a clustered heatmap (row z-scored), revealing structured co-expression patterns: ECM cluster (*COL1A1*, *COL1A2*, *COL3A1*), inflammatory cluster (*IL6*, *CXCL8*, *JUN*, *FOS*), metabolic cluster (*FABP4*, *GPX3*, *PLIN1*).

#### 7. Non-Negative Matrix Factorization (NMF)
- NMF (sklearn, n_components=3) applied to decompose the expression matrix into 3 co-expression modules.
- **Module 1 — Inflammatory/stress-response:** *IL6*, *FOS*, *JUN*, *DUSP1*, *S100A8* — slightly lower in PCOS (Welch's t-test p = 0.379, ns).
- **Module 2 — Structural/ECM & ribosomal:** *COL1A1*, *COL1A2*, *COL3A1*, *RPL* family — slightly higher in PCOS (p = 0.547, ns).
- **Module 3 — Metabolic/oxidative stress:** *FABP4*, *GPX3*, *ALDH2*, *FTL*, *PLIN1* — slightly higher in PCOS (p = 0.137, ns).
- Directional trends consistent with GSEA findings despite not reaching statistical significance.

---

## Key Results Summary

| Analysis | Dataset | Key Finding |
|---|---|---|
| Differential expression (DESeq2) | GSE267287 | 86 significant DEGs; immune, ECM, and metabolic genes dysregulated |
| Gene correlation network (GRN) | GSE267287 | Network built from 86 DEGs at \|r\| ≥ 0.9; hub genes identified |
| Hub genes (Q4) | GSE267287 | *LRRC27*, *SPIRE2*, *IL18R1*, *UNC93B1*, *DOC2GP* as top hubs |
| PCA | GSE5090 | 36% variance explained; no clear obese PCOS vs. obese control group separation |
| Differential expression (Welch's + FDR) | GSE5090 | No genes significant after FDR correction but subtle coordinated shifts in clustered heatmap|
| GSEA (Hallmark 2020) | GSE5090 | Adipogenesis & oxidative metabolism up; Inflammatory Response down in PCOS |
| TF enrichment (ChEA_2016) | GSE5090 | Upregulated: PPARG, CREB1, HNF4A; Downregulated: RELA, SMAD2/3 |
| NMF (3 modules) | GSE5090 | Subtle non-significant activity shifts consistent with GSEA; metabolic modules slightly higher in PCOS |

---

## Getting Started

### Prerequisites

\```bash
pip install pandas numpy scipy matplotlib seaborn scikit-learn statsmodels gseapy GEOparse pydeseq2 mygene networkx jupyter
\```

### Running the Analyses

1. Clone this repository:
   \```bash
   git clone https://github.com/fofaflowers02-stack/20.440_Final_Project_Flores_Haciguzeller.git
   cd 20.440_Final_Project_Flores_Haciguzeller
   \```

2. Launch Jupyter:
   \```bash
   jupyter notebook
   \```

3. **For GSE267287 analysis** (DEA + GRN + hub gene identification):
   Open and run `20_440_Project_FinalCode_SofiaFlores.ipynb`.

4. **For GSE5090 analysis** (microarray pipeline + GSEA + NMF):
   Open and run `GSE5090_data_analysis_ipynb_05_11.ipynb`.
   - The notebook downloads the GEO Series Matrix and GPL96 annotation automatically from NCBI.
   - Outputs are saved to a local `results/` directory (with subdirectories for plots and tables).

> **Note:** The GSE5090 notebook was developed in Google Colab. If running locally, remove or replace the `drive.mount()` cell and update the `WORKDIR` path to a local directory.

---

## Output Files (GSE5090 Pipeline)

| File | Description |
|---|---|
| `processed_expression_matrix.csv` | Probe-level expression matrix (all samples) |
| `processed_expression_annotated.csv` | Expression matrix with gene symbol annotations |
| `probe_level_differential_expression.csv` | Probe-level DEA results (logFC, p-value, FDR) |
| `gene_level_differential_expression.csv` | Gene-level DEA (best probe per gene) |
| `phenotype_table.csv` | Sample metadata with PCOS/control labels |
| `leading_edge_genes_positive_pathways.csv` | Leading-edge genes for positively enriched GSEA pathways |
| `leading_edge_genes_negative_pathways.csv` | Leading-edge genes for negatively enriched GSEA pathways |
| `tf_enrichment_upregulated.csv` | TF enrichment results for upregulated genes |
| `tf_enrichment_downregulated.csv` | TF enrichment results for downregulated genes |
| `nmf_gene_weights.csv` | NMF W matrix (genes × modules) |
| `nmf_sample_loadings.csv` | NMF H matrix (modules × samples) |
| `nmf_module_activity_by_sample.csv` | Per-sample module activity with group labels |
| `nmf_module_stats.csv` | Module activity group comparison statistics (Welch's t-test) |

---

## Authors

- **Sofia Flores** — analysis, code, visualization
- **H. Zeynep Haciguzeller** — analysis, code, visualization

MIT Department of Biological Engineering | Spring 2025

---

## Course

**20.440 — Analysis of Biological Networks**
Massachusetts Institute of Technology

This course covers computational and mathematical approaches to modeling, analyzing, and interpreting biological network data, including gene regulatory networks, protein interaction networks, and metabolic networks.

---

## References

1. Bril, F. *et al.* Adipose Tissue Dysfunction in Polycystic Ovary Syndrome. *J. Clin. Endocrinol. Metab.* **109**, 10–24 (2024).
2. Lemaitre, M., Christin-Maitre, S. & Kerlan, V. Polycystic ovary syndrome and adipose tissue. *Ann. Endocrinol.* **84**, 308–315 (2023).
3. Chen, H. *et al.* RNA editing landscape of adipose tissue in polycystic ovary syndrome provides insight into the obesity-related immune responses. *Front. Endocrinol.* **15** (2024).
4. Hellberg, A., Salamon, D., Ujvari, D., Rydén, M. & Hirschberg, A. L. Weight Changes Are Linked to Adipose Tissue Genes in Overweight Women with Polycystic Ovary Syndrome. *Int. J. Mol. Sci.* **25** (2024).
5. Barber, T. M. Why are women with polycystic ovary syndrome obese? *Br. Med. Bull.* **143**, 4–15 (2022).
6. Joharatnam, J. *et al.* Determinants of dyslipidaemia in probands with polycystic ovary syndrome and their sisters. *Clin. Endocrinol. (Oxf.)* **74**, 714–719 (2011).
7. Barber, T. M. & Franks, S. Obesity and polycystic ovary syndrome. *Clin. Endocrinol. (Oxf.)* **95**, 531–541 (2021).
8. Scicchitano, P. *et al.* Cardiovascular Risk in Women With PCOS. *Int. J. Endocrinol. Metab.* **10**, 611–618 (2012).
9. Transcriptome Analysis of Mesenchymal Progenitor Cells Revealed Molecular Insights into Metabolic Dysfunction and Inflammation in Polycystic Ovary Syndrome. *Int. J. Mol. Sci.* (2024). https://www.mdpi.com/1422-0067/25/14/7948
10. Ramalingam, L. *et al.* Doc2b is a key effector of insulin secretion and skeletal muscle insulin sensitivity. *Diabetes* **61**, 2424–2432 (2012).
11. Fukui, R. *et al.* Unc93B1 restricts systemic lethal inflammation by orchestrating Toll-like receptor 7 and 9 trafficking. *Immunity* **35**, 69–81 (2011).
12. Pfender, S., Kuznetsov, V., Pleiser, S., Kerkhoff, E. & Schuh, M. Spire-type actin nucleators cooperate with Formin-2 to drive asymmetric oocyte division. *Curr. Biol. CB* **21**, 955–960 (2011).
13. Day, F. R. *et al.* Causal mechanisms and balancing selection inferred from genetic associations with polycystic ovary syndrome. *Nat. Commun.* **6**, 8464 (2015).
14. Cortón, M. *et al.* Differential gene expression profile in omental adipose tissue in women with polycystic ovary syndrome. *J. Clin. Endocrinol. Metab.* **92**, 328–337 (2007).
