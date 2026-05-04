# Amplicon Analysis & Functional Profiling Suite

This directory contains two specialized Shiny applications for microbiome analysis, separated by analytical focus.

## Directory Structure

```
Amplicon_viz/
├── amplicon_explorer/          # Standalone exploratory phyloseq-based app
│   ├── app.R                   # 16S/ITS diversity and composition analysis
│   └── README.md
├── functional_profiler/        # Standalone functional analysis app (DESeq2 + PICRUSt2)
│   ├── app.R                   # Differential abundance & metabolic pathways
│   └── README.md
├── data/                       # All input datasets
│   ├── otu_table.csv           # OTU/ASV count table
│   ├── taxonomy_table.csv      # Taxonomy assignments
│   ├── sample_metadata.csv     # Sample metadata
│   ├── centrifuge_reports.biom # BIOM-format centrifuge output
│   └── qiimeandpicrust_oyester/# Oyster microbiome example data (QIIME2/PICRUSt2)
├── ggpicrust2_cache/           # Cached KO reference data (ko_reference.rds)
├── Amplicon_viz.Rproj          # RStudio project file
├── index.html                  # Project landing page
└── README.md                   # This file
```

## Quick Start

### Launch Root App (all-in-one)
```r
# From the Amplicon_viz/ project root:
shiny::runApp()
```

### Launch Amplicon Explorer (standalone)
```r
shiny::runApp("amplicon_explorer")
```

### Launch Functional Profiler (standalone)
```r
shiny::runApp("functional_profiler")
```

## What Each App Does

### 🔬 Amplicon Explorer
**Focus**: Taxonomic diversity and community composition analysis

**Analyses Included**:
- Data Summary (sample counts, read depth, taxonomy overview)
- Rarefaction curves (sequencing depth assessment)
- Alpha diversity (Shannon, Simpson, Observed richness)
- Beta ordination (NMDS, PCoA with Bray-Curtis/Jaccard)
- Taxa composition (stacked bar charts)
- Hierarchical clustering (UPGMA dendrograms)
- PERMANOVA (community composition testing)
- Core microbiome (prevalence & abundance filtering)

**Input Data**:
- OTU/ASV count table (CSV)
- Taxonomy table (CSV)
- Sample metadata (CSV)
- Or: BIOM format file
- Or: QIIME2 feature table

**Output**: Tables and plots for download

---

### 🧬 Functional Profiler  
**Focus**: Differential functional analysis and metabolic pathways

**Analyses Included**:
- Data Summary (phyloseq object overview)
- Differential Abundance (DESeq2-based, with volcano plots)
- Functional Pathway Analysis (ggpicrust2-based):
  - KO (KEGG Ortholog) pathways
  - MetaCyc pathways
  - EC (Enzyme Commission) numbers
  - KO-to-KEGG conversion
  - Multiple DAA methods (LinDA, ALDEx2, DESeq2, edgeR)
  - Errorbar plots, PCA, heatmaps for results

**Input Data**:
- OTU/ASV count table (CSV) - for DESeq2
- Sample metadata (CSV)
- PICRUSt2 functional abundance table (TSV/TXT) - for ggpicrust2
  - Example: `pred_metagenome_unstrat_descrip.tsv`

**Output**: Statistical tables, annotated results, pathway plots

---

## Key Features

✅ **Modular Design** - Each app focused on a specific analytical question
✅ **Data Flexibility** - Multiple input formats (CSV, BIOM, QIIME2)
✅ **Publication-Ready Plots** - High-resolution downloads
✅ **Detailed Help Blocks** - Guidance within each tab
✅ **Robust Error Handling** - Clear messages if data is missing/malformed

## Data Requirements

### For Amplicon Explorer
- Sample metadata MUST have ≥2 samples and ≥1 grouping variable
- OTU/taxonomy tables MUST have overlapping sample/feature IDs
- Minimum 2 taxa and 2 samples after filtering

### For Functional Profiler
- Phyloseq requirements (above)
- For DESeq2: grouping variable with ≥2 levels, ideally with replication
- For ggpicrust2: PICRUSt2 output table with sample IDs matching metadata
  - Row names: functional IDs (KO, MetaCyc pathway, EC number)
  - Column names: sample IDs matching loaded metadata

## Recommended Workflow

1. **Start with Amplicon Explorer** to understand:
   - Sample/taxa counts and quality
   - Which groups differ in community composition
   - Which taxa are dominant

2. **Then use Functional Profiler** to ask:
   - Which specific taxa or pathways differ?
   - What metabolic functions are enriched/depleted?
   - How do predicted metabolic capabilities differ?

## Shared Resources

All apps read data from the `data/` directory relative to the project root:
- `data/otu_table.csv`, `data/taxonomy_table.csv`, `data/sample_metadata.csv`, `data/centrifuge_reports.biom`
- `ggpicrust2_cache/ko_reference.rds` — cached KO reference data (speeds up pathway lookups)

## Troubleshooting

**"Package X not installed"** → Install with:
```r
install.packages("package_name")      # CRAN packages
BiocManager::install("package_name")  # Bioconductor packages
```

**"Fewer than 2 overlapping samples"** → Check that sample IDs in OTU table match metadata exactly (case-sensitive)

**"No variance in grouping variable"** → Ensure your grouping variable has ≥2 different values

For more details, see app-specific README files in each directory.

---

**Created**: April 2026  
**Framework**: R Shiny  
**Primary Packages**: phyloseq, DESeq2, ggpicrust2, tidyverse
