# 20.440 Final Project — Flores & Haciguzeller

## Overview

This repository contains the code, data, and figures for the final project for **20.440 (Analysis of Biological Networks)** at MIT, completed by **Sofia Flores** and **H. Zeynep Haciguzeller**. The project investigates differential gene expression and gene regulatory network (GRN) structure, identifying hub genes and key regulators through network topology analysis.

---

## Repository Structure

```
20.440_Final_Project_Flores_Haciguzeller/
│
├── 20_440_Project_FinalCode_SofiaFlores.ipynb   # Main analysis notebook
│
├── differentially_expressed_genes.csv            # Dataset of DEGs
│
├── 20.440 Figure 1.png                           # Figure 1
├── volcano plot.png                              # Volcano plot of DEGs
├── degree centrality.png                         # Degree centrality distribution
├── grn 2.png                                     # Full gene regulatory network
├── grn hubs only 2.png                           # GRN filtered to hub genes only
└── Quartile 4.png                                # Top quartile hub gene analysis
```

---

## Methods

- **Differential Expression Analysis**: Identification of significantly differentially expressed genes, visualized via volcano plot
- **Gene Regulatory Network (GRN) Construction**: Network built from DEG interactions to model regulatory relationships
- **Network Topology Analysis**: Degree centrality computed to identify hub genes; top-quartile hubs isolated for focused analysis

---

## Figures

| Figure | Description |
|--------|-------------|
| `volcano plot.png` | Volcano plot showing log2 fold change vs. significance for DEGs |
| `degree centrality.png` | Distribution of degree centrality across all network nodes |
| `grn 2.png` | Full gene regulatory network |
| `grn hubs only 2.png` | Subnetwork showing only high-degree hub genes |
| `Quartile 4.png` | Analysis of genes in the top quartile of degree centrality |
| `20.440 Figure 1.png` | Additional project figure |

---

## Data

- **`differentially_expressed_genes.csv`**: Contains the list of differentially expressed genes used to construct the GRN, including expression values and statistical metrics.

---

## Requirements

The analysis was performed in a Jupyter Notebook. Key Python libraries used include:

- `pandas` — data manipulation
- `numpy` — numerical computation
- `matplotlib` / `seaborn` — visualization
- `networkx` — network construction and analysis

To install dependencies:
```bash
pip install pandas numpy matplotlib seaborn networkx
```

---

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/fofaflowers02-stack/20.440_Final_Project_Flores_Haciguzeller.git
   ```
2. Open the notebook:
   ```bash
   jupyter notebook 20_440_Project_FinalCode_SofiaFlores.ipynb
   ```
3. Run all cells to reproduce the analysis and figures.

---

## Authors

- **Sofia Flores** — MIT
- **H. Zeynep Haciguzeller** — MIT

*20.440 Analysis of Biological Networks, Spring 2025*
