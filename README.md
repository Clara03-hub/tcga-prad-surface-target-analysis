# RLT Target Discovery in Prostate Cancer

Transcriptomic analysis of TCGA-PRAD data to identify new potential surface targets for Radioligand Therapy (RLT) in prostate cancer.

## Overview

This project uses RNA-seq data from TCGA-PRAD (493 tumours, 52 normal adjacent tissues) to identify genes that are overexpressed in prostate tumour tissue and encode surface proteins accessible to a circulating radioligand. The pipeline filters candidates through multiple databases (Ensembl, UniProt, GTEx, Human Protein Atlas) and uses FOLH1/PSMA as positive control throughout.

Two candidates were identified:  OR51E2 and TRPM8, both exceeding the FOLH1 benchmark in terms of prostate-to-vital-tissue expression ratio.

## Project structure

```
├── data/
│   └── raw/                    # input data files (not tracked, see below)
├── notebooks/
│   └── RLT_target_discovery_TCGA_PRAD.ipynb
├── requirements.txt
└── README.md
```

## Data

The following files need to be downloaded manually and placed in `data/raw/` before running the notebook:

**TCGA-PRAD** (from cBioPortal, Prostate Adenocarcinoma TCGA PanCancer Atlas):
- `data_mrna_seq_v2_rsem.txt` - tumour RNA-seq expression
- `data_mrna_seq_v2_rsem_normal_samples.txt` (in a `normals/` subfolder) - normal adjacent tissue expression
- `data_clinical_patient.txt` - clinical data

Download from: https://www.cbioportal.org/study/summary?id=prad_tcga_pan_can_atlas_2018

**GTEx**:
- `GTEx_Analysis_2025-08-22_v11_RNASeQCv2.4.3_gene_median_tpm.gct`

Download from: https://gtexportal.org/home/downloads/adult-gtex/bulk_tissue_expression

**Human Protein Atlas**:
- `proteinatlas.tsv` (version 25.0)

Download from: https://www.proteinatlas.org/about/download

Note: Ensembl and UniProt data are queried via their REST APIs during notebook execution so no manual download needed for those.

## Setup

```bash
pip install -r requirements.txt
```

Tested with python 3.13.2 on macOS.

## Running

Open and run the notebook sequentially. The API calls to Ensembl and UniProt take a few minutes because of the rate limiting (sleep between requests). Total execution time is around 10-15 min depending on network.

## Key results

- 311 differentially expressed genes identified (|log2FC| > 2, BH adj. p < 0.05)
- 95 overexpressed in tumour
- 17 final candidates after surface protein filtering and HPA validation
- Sensitivity analysis (Wilcoxon signed-rank on 52 paired samples) confirms robustness of key candidates
- OR51E2 (GTEx ratio 3.04) and TRPM8 (GTEx ratio 1.89) identified as top candidates, both above the FOLH1/PSMA benchmark (0.80)

## Limitations

This is a first discovery step based on transcriptomic data only. Key limitations include: mRNA expression does not confirm protein surface accessibility, no correction for cell-type composition was applied, and no independent replication cohort was used. A paired sensitivity analysis was added to address the partially paired data structure. See Section 11 of the notebook for full discussion.

## Author

Clara Rodriguez - April 2026
