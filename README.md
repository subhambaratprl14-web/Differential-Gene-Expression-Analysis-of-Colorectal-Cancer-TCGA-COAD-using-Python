# Differential-Gene-Expression-Analysis-of-Colorectal-Cancer-TCGA-COAD-using-Python
# TCGA-COAD Differential Gene Expression Analysis

An independent computational analysis of colorectal cancer RNA-seq data from TCGA, comparing gene expression in **tumour vs. normal** colon tissue using Python.

> **Note:** This is an exploratory analysis of public data. It is not a publication, and the results require independent biological validation.

---

## Project Question

Which genes are differentially expressed between colorectal tumour tissue and normal tissue, and which biological processes do they point to?

**Why colorectal cancer?** It connects directly to my microbiology background through gut microbiome research and DNA mismatch repair biology.

---

## Data

| Item | Details |
|---|---|
| Cohort | TCGA-COAD (colon adenocarcinoma) |
| Source | UCSC Xena Browser, GDC TCGA Colon Cancer (COAD) |
| File | `TCGA-COAD.star_counts.tsv.gz` (GDC data release 40.0) |
| Samples | 514 (473 tumour, 41 normal) |
| Genes tested | 54,199 (after filtering low-count genes) |

The Xena counts are stored as log2(count + 1), so they were converted back to raw integer counts before analysis, as DESeq2-style tools require raw counts.

---

## Methods

1. **Data loading:** loaded the STAR counts file and the phenotype file with pandas.
2. **Sample labelling:** classified samples as tumour or normal from the TCGA barcode sample-type code.
3. **Filtering:** removed genes with very low total counts.
4. **Differential expression:** ran `pydeseq2` (a Python implementation of DESeq2) for tumour vs. normal.
5. **Significance thresholds:** adjusted p-value (FDR) < 0.05 and |log2 fold change| > 1.
6. **Visualisation:** volcano plot and heatmap of the top significant genes (`matplotlib`, `seaborn`).
7. **Gene ID conversion and enrichment:** converted Ensembl IDs to gene symbols with `mygene`, then ran GO enrichment with `gseapy`.

**Tools:** Python, pandas, NumPy, pydeseq2, matplotlib, seaborn, gseapy, mygene

---

## Key Results

- **514** samples analysed: **473** tumour and **41** normal
- **54,199** genes tested
- **14,072** genes significantly differentially expressed (FDR < 0.05, |log2FC| > 1)
- GO enrichment analysis completed

### Volcano plot
Red points are upregulated and blue points are downregulated in tumour tissue.


### Heatmap of top significant genes
Samples cluster clearly by tumour vs. normal status.


The full report with the top-ranked genes and run summary is in [`report/TCGA-COAD_analysis_report.pdf`](report/TCGA-COAD_analysis_report.pdf).

---

## Limitations

- **Sample imbalance:** only 41 normal samples versus 473 tumour samples (8% vs. 92%).
- **Possible batch effects:** not explicitly corrected for.
- **Exploratory analysis:** findings have not been validated experimentally.

---

## Repository Structure

```
TCGA-COAD-differential-expression/
├── README.md
├── analysis.py (or notebook)
├── requirements.txt
├── figures/
│   ├── volcano_plot.png
│   └── heatmap_top50.png
└── report/
    └── TCGA-COAD_analysis_report.pdf
```

## How to Run

```bash
python3 -m venv tcga-env
source tcga-env/bin/activate        # Windows: tcga-env\Scripts\activate
pip install pandas numpy scipy matplotlib seaborn requests
pip install pydeseq2 gseapy mygene scikit-learn jupyter
```

Download the TCGA-COAD STAR counts and phenotype files from the [UCSC Xena Browser](https://xenabrowser.net), place them in a `data/` folder, then run the analysis script.

---

## Author

**Subham Barat**
M.Sc. Microbiology, Amity University
Email: subhambaratprl14@gmail.com
