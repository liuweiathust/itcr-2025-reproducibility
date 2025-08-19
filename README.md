# Acute Myeloid Leukemia RNA-seq Analysis

This repository contains a reproducible bioinformatics analysis of acute myeloid leukemia (AML) RNA-sequencing data, demonstrating clustering and heatmap visualization techniques. The analysis is adapted from the [refine.bio-examples](https://alexslemonade.github.io/refinebio-examples/03-rnaseq/clustering_rnaseq_01_heatmap.html) repository and focuses on gene expression patterns in AML mouse models with different genetic mutations and treatment conditions.

## Overview

This analysis examines RNA-seq data from 19 AML model mice samples obtained from [refine.bio](https://www.refine.bio/) (dataset [SRP070849](https://www.refine.bio/experiments/SRP070849)). The dataset includes samples from mice with different genetic backgrounds:

- **Wild-type (WT)** controls
- **IDH2 R140Q + FLT3-ITD** mutant mice treated with vehicle or AG-221 (IDH2 inhibitor)
- **TET2-/- + FLT3-ITD** mutant mice treated with vehicle or 5-Azacytidine (hypomethylating agent)

The analysis generates clustered heatmaps to visualize gene expression patterns and identify how different mutations and treatments affect transcriptional profiles in AML.

## Dataset Information

- **Source**: [Shih et al., 2017](https://pubmed.ncbi.nlm.nih.gov/28193779/) - "Combination Targeted Therapy to Disrupt Aberrant Oncogenic Signaling and Reverse Epigenetic Dysfunction in IDH2- and TET2-Mutant Acute Myeloid Leukemia"
- **Technology**: RNA-sequencing (Illumina HiSeq 2000)
- **Organism**: Mus musculus (mouse)
- **Cell type**: Hematopoietic stem cells (LSK population: lin-sca+ckit+)
- **Sample count**: 19 samples
- **Processing**: Quantile normalized, processed by refine.bio using Tximport

## Repository Structure

```
├── 00-download-data.py          # Python script to download data from refine.bio
├── 01-heatmap.Rmd              # R Markdown analysis notebook
├── 01-heatmap.nb.html          # Rendered HTML output of analysis
├── run_analysis.sh             # Shell script to run complete analysis pipeline
├── data/                       # Data directory
│   ├── aggregated_metadata.json   # Dataset metadata from refine.bio
│   ├── dataset.zip              # Downloaded dataset archive
│   ├── LICENSE.TXT              # Data license information
│   └── SRP070849/              # Extracted dataset
│       ├── metadata_SRP070849.tsv  # Sample metadata
│       └── SRP070849.tsv           # Gene expression matrix
├── docker/
│   └── Dockerfile              # Docker container configuration
├── plots/
│   └── aml_heatmap.png         # Generated heatmap visualization
├── results/
│   └── top_90_var_genes.tsv    # High-variance genes used for clustering
├── renv/                       # R environment management
├── renv.lock                   # R package dependencies lockfile
└── README.md                   # This file
```

## Requirements

### R Dependencies
- R >= 4.0.2
- pheatmap (clustering and heatmap visualization)
- magrittr (pipe operators)
- readr (data import)
- dplyr (data manipulation)
- tibble (data frames)

### Python Dependencies
- Python 3
- pyrefinebio (data download from refine.bio)

## Usage

### Option 1: Run Complete Pipeline
Execute the entire analysis pipeline:
```bash
bash run_analysis.sh
```

This will:
1. Download the dataset using Python
2. Run the R Markdown analysis
3. Generate all outputs (heatmap, results files, HTML report)

### Option 2: Step-by-Step Execution

1. **Download data**:
   ```bash
   python3 00-download-data.py
   ```

2. **Run analysis**:
   ```bash
   Rscript -e "rmarkdown::render('01-heatmap.Rmd', clean = TRUE)"
   ```

### Option 3: Docker Environment
Build and run using Docker for a fully reproducible environment:

```bash
# Build the Docker image
docker build -t aml-analysis docker/

# Run the container
docker run -it --rm -v $(pwd):/home/rstudio aml-analysis
```

## Analysis Details

The analysis performs the following steps:

1. **Data Import**: Loads gene expression data and sample metadata
2. **Quality Control**: Ensures data integrity and sample order consistency
3. **Gene Filtering**: Selects genes with high variance (upper quartile) for clustering
4. **Annotation Preparation**: Creates sample annotations based on mutation type and treatment
5. **Heatmap Generation**: Creates clustered heatmaps with hierarchical clustering of both genes and samples
6. **Visualization**: Generates publication-ready heatmap with color-coded sample annotations

### Key Outputs

- **`plots/aml_heatmap.png`**: Annotated clustered heatmap showing gene expression patterns
- **`results/top_90_var_genes.tsv`**: List of high-variance genes used for analysis
- **`01-heatmap.nb.html`**: Complete analysis report with code, results, and interpretations

## Scientific Context

This analysis demonstrates how epigenetic mutations (TET2, IDH2) combined with signaling mutations (FLT3-ITD) create distinct transcriptional signatures in AML, and how targeted therapies can restore more normal gene expression patterns. The clustering analysis reveals:

- Distinct expression profiles between different genetic backgrounds
- Treatment-specific transcriptional responses
- Potential biomarkers for therapeutic efficacy

## Reproducibility

This repository implements several reproducibility best practices:

- **Environment Management**: `renv` lockfile ensures consistent R package versions
- **Containerization**: Docker configuration for platform-independent execution
- **Automated Pipeline**: Single-command execution via shell script
- **Version Control**: All code, configurations, and documentation under version control
- **Clear Documentation**: Comprehensive README and inline code comments

## References

1. Shih AH, Jiang Y, Meydan C, et al. Combination targeted therapy to disrupt aberrant oncogenic signaling and reverse epigenetic dysfunction in IDH2- and TET2-mutant acute myeloid leukemia. *Cancer Discov*. 2017;7(5):494-505. [PubMed: 28193779](https://pubmed.ncbi.nlm.nih.gov/28193779/)

2. Dataset: [SRP070849 on refine.bio](https://www.refine.bio/experiments/SRP070849)

3. Analysis adapted from: [refine.bio RNA-seq examples](https://alexslemonade.github.io/refinebio-examples/03-rnaseq/clustering_rnaseq_01_heatmap.html)

## License

The dataset is provided under the terms specified in `data/LICENSE.TXT`. The analysis code in this repository is provided for educational and research purposes.
