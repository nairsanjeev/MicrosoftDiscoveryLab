# RNA-Seq Meta-Analysis: Gene Expression Signatures in Rheumatoid Arthritis Synovium

## Publication Information
- **Journal**: Arthritis & Rheumatology (simulated)
- **Date**: January 2024
- **Authors**: Park JH, Andersen BK, Liu X, et al.
- **DOI**: 10.xxxx/simulated-rna-meta-analysis
- **Type**: Systematic meta-analysis of public RNA-seq datasets

---

## Abstract

We performed a systematic meta-analysis of publicly available RNA-seq datasets from RA synovial tissue to identify robust, reproducible gene expression signatures. By integrating data from 8 independent studies (total n=342 RA patients, n=156 healthy controls), we identified consensus differentially expressed genes that replicate across cohorts.

---

## Datasets Included

| Dataset | Source | n (RA) | n (Control) | Platform | GEO Accession |
|---------|--------|--------|-------------|----------|---------------|
| Study A (2019) | Johns Hopkins | 45 | 20 | RNA-seq | GSE89408 |
| Study B (2020) | Karolinska | 68 | 30 | RNA-seq | GSE144570 |
| Study C (2021) | Beijing PUMCH | 52 | 22 | RNA-seq | GSE178292 |
| Study D (2021) | UCL London | 38 | 18 | RNA-seq | GSE181307 |
| Study E (2022) | Stanford AIMI | 41 | 20 | RNA-seq | GSE202456 |
| Study F (2022) | Leiden LUMC | 35 | 16 | RNA-seq | GSE215891 |
| Study G (2023) | Tokyo Keio | 33 | 15 | RNA-seq | GSE229104 |
| Study H (2023) | Mayo Clinic | 30 | 15 | RNA-seq | GSE241785 |

---

## Consensus Differentially Expressed Genes

Genes significant (FDR < 0.05, |log2FC| > 1.0) in ≥6 out of 8 studies:

| Gene | # Studies Significant | Mean log2FC | Meta p-value | Direction |
|------|----------------------|-------------|--------------|-----------|
| **TNF** | 8/8 | +2.41 | 1.2e-28 | Up |
| **IL6** | 8/8 | +2.18 | 3.4e-25 | Up |
| **MMP9** | 8/8 | +2.05 | 8.7e-23 | Up |
| **JAK2** | 7/8 | +1.89 | 4.1e-19 | Up |
| **STAT3** | 7/8 | +1.76 | 2.3e-17 | Up |
| **CCL2** | 7/8 | +1.72 | 5.6e-16 | Up |
| **TYK2** | 7/8 | +1.54 | 1.8e-14 | Up |
| **BTK** | 6/8 | +1.48 | 4.2e-12 | Up |
| **IRAK4** | 6/8 | +1.35 | 2.1e-10 | Up |
| **PTPN22** | 6/8 | +1.28 | 7.8e-09 | Up |
| **FOXP3** | 7/8 | -1.62 | 3.9e-15 | Down |
| **IL10** | 6/8 | -1.21 | 5.4e-08 | Down |
| **CD274 (PD-L1)** | 6/8 | +1.45 | 8.9e-11 | Up |
| **CXCL13** | 7/8 | +2.34 | 6.7e-22 | Up |

---

## Pathway-Level Meta-Analysis

| Pathway (Reactome) | Mean NES | # Studies Enriched | Consensus FDR |
|--------------------|---------|--------------------|---------------|
| Cytokine signaling in immune system | +3.12 | 8/8 | <0.001 |
| JAK-STAT signaling pathway | +2.87 | 8/8 | <0.001 |
| Interferon alpha/beta signaling | +2.64 | 7/8 | <0.001 |
| Toll-like receptor cascades | +2.41 | 7/8 | 0.001 |
| B-cell receptor signaling | +2.18 | 6/8 | 0.003 |
| IL-23 signaling | +2.05 | 6/8 | 0.005 |
| Complement cascade | +1.92 | 6/8 | 0.008 |
| Extracellular matrix degradation | +2.56 | 8/8 | <0.001 |

---

## Reproducibility Assessment

### How Often Are Key Targets Replicated Across Studies?

| Target | Replicated in independent studies? | Robustness Score |
|--------|-----------------------------------|--------------------|
| **TNF** | 8/8 studies | ⭐⭐⭐⭐⭐ (gold standard) |
| **IL6** | 8/8 studies | ⭐⭐⭐⭐⭐ |
| **JAK2** | 7/8 studies | ⭐⭐⭐⭐ |
| **TYK2** | 7/8 studies | ⭐⭐⭐⭐ |
| **BTK** | 6/8 studies | ⭐⭐⭐⭐ |
| **STAT3** | 7/8 studies | ⭐⭐⭐⭐ |
| **IRAK4** | 6/8 studies | ⭐⭐⭐ |
| **PTPN22** | 6/8 studies | ⭐⭐⭐ |

---

## Subgroup Analyses

### Seropositive (RF+/CCP+) vs. Seronegative RA

| Gene | Seropositive log2FC | Seronegative log2FC | Difference |
|------|---------------------|--------------------:|------------|
| **BTK** | +1.87 | +0.92 | Higher in seropositive |
| **CXCL13** | +2.89 | +1.45 | Higher in seropositive |
| **TYK2** | +1.61 | +1.48 | Similar |
| **JAK2** | +1.95 | +1.82 | Similar |
| **TNF** | +2.38 | +2.44 | Similar |

**Key finding**: BTK is significantly more upregulated in seropositive RA (where B-cell pathology dominates), while TYK2 and JAK2 are equally elevated in both serotypes. This supports BTK patient stratification by serostatus.

### Early RA (< 1 year) vs. Established RA (> 5 years)

| Gene | Early RA log2FC | Established RA log2FC | Interpretation |
|------|-----------------|----------------------|----------------|
| **TYK2** | +1.68 | +1.52 | Elevated early — upstream driver |
| **JAK2** | +1.54 | +2.12 | Increases with chronicity |
| **BTK** | +1.62 | +1.41 | Elevated early — upstream driver |
| **MMP9** | +0.89 | +2.45 | Late-stage tissue destruction |
| **TNF** | +2.12 | +2.58 | Elevated throughout |

**Key finding**: TYK2 and BTK are already elevated in early RA, supporting their role as upstream disease drivers rather than consequences of chronic inflammation.

---

## Expression Atlas (XPR) Cross-Reference

We compared our meta-analysis results with the Expression Atlas public database for independent validation:

| Gene | Our Meta-Analysis (synovium) | Expression Atlas (synovial biopsy experiments) | Concordance |
|------|------------------------------|------------------------------------------------|-------------|
| JAK2 | Up (log2FC 1.89) | Up (differential expression score: 8.2/10) | ✅ Concordant |
| TYK2 | Up (log2FC 1.54) | Up (differential expression score: 6.8/10) | ✅ Concordant |
| BTK | Up (log2FC 1.48) | Up (differential expression score: 7.1/10) | ✅ Concordant |
| IRAK4 | Up (log2FC 1.35) | Up (differential expression score: 5.4/10) | ✅ Concordant |
| FOXP3 | Down (log2FC -1.62) | Down (differential expression score: -6.9/10) | ✅ Concordant |

All key targets from our meta-analysis are independently validated in the Expression Atlas.

---

## Conclusions

1. **JAK2, TYK2, and BTK are robustly and reproducibly upregulated** in RA synovium across 6-8 independent public datasets worldwide.
2. **TYK2 and BTK elevation begins in early disease**, supporting their role as upstream drivers suitable for early intervention.
3. **BTK upregulation is stronger in seropositive RA**, suggesting biomarker-guided patient selection.
4. **The IFN signature (TYK2-driven) and B-cell signature (BTK-driven)** are consistently among the top enriched pathways.
5. **All findings validated** against the Expression Atlas (XPR) independently.

---

## Data Availability

All source datasets are publicly available from GEO (accession numbers listed above).
Meta-analysis code: https://github.com/simulated/ra-meta-analysis (simulated)
