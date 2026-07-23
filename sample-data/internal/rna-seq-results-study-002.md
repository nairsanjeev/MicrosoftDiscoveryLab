# RNA-Seq Differential Expression Analysis — Study 002 (Follow-up)

## Study Information
- **Study ID**: INT-RNA-002
- **Date**: September 2025
- **Principal Investigator**: Dr. Sarah Chen, Dr. Marcus Williams
- **Disease Model**: Rheumatoid Arthritis (RA) — Early vs. Established disease
- **Tissue**: Synovial tissue biopsies + peripheral blood mononuclear cells (PBMCs)
- **Cohort A**: Early RA (< 6 months from diagnosis, n=16)
- **Cohort B**: Established RA (> 2 years, treatment-resistant, n=20)
- **Cohort C**: Healthy controls (n=15)
- **Platform**: Illumina NovaSeq 6000, paired-end 150bp
- **Analysis**: DESeq2 with multi-factor design (disease stage + tissue)

---

## Purpose

Follow-up to Study INT-RNA-001 to determine whether target candidates identified in the initial screen are differentially expressed in EARLY disease (before chronic remodeling) and whether they are detectable in blood (enabling potential biomarker use).

---

## Key Results — Synovial Tissue

### Early RA vs. Controls (Cohort A vs. C)

| Gene | log2FC | FDR | Also significant in Study 001? |
|------|--------|-----|-------------------------------|
| **JAK2** | +2.89 | 2.1e-08 | ✅ Yes (3.42 in established) |
| **TYK2** | +2.14 | 5.5e-07 | ✅ Yes (1.82) |
| **TNF** | +1.95 | 8.3e-07 | ✅ Yes (2.65) |
| **BTK** | +1.88 | 1.4e-06 | ✅ Yes (1.76) |
| **STAT3** | +1.72 | 4.2e-06 | ✅ Yes (2.87) |
| **IL6** | +1.45 | 2.8e-05 | ✅ Yes (2.51) |
| **IRAK4** | +1.38 | 5.1e-05 | ✅ Yes (1.98) |
| **PTPN22** | +1.21 | 1.8e-04 | ✅ Yes (2.34) |

### Established RA vs. Early RA (Cohort B vs. A)

| Gene | log2FC | FDR | Interpretation |
|------|--------|-----|----------------|
| **JAK2** | +0.53 | 0.018 | Further increased in established disease |
| **STAT3** | +1.15 | 3.2e-04 | Markedly increased with chronicity |
| **MMP9** | +1.42 | 8.7e-05 | Tissue remodeling in late disease |
| **TNF** | +0.70 | 0.005 | Moderately increased |
| **TYK2** | +0.32 | 0.12 | Not significantly different between stages |
| **BTK** | +0.28 | 0.15 | Not significantly different between stages |

**Key insight**: JAK2 and TYK2 are elevated EARLY in disease, before chronic tissue remodeling. This suggests they are upstream drivers, not just consequences of chronic inflammation. BTK is also elevated early.

---

## Key Results — Peripheral Blood (PBMCs)

### Can targets be detected in blood? (RA patients vs. Controls)

| Gene | Detectable in PBMCs? | log2FC (blood) | FDR | Blood-tissue concordance |
|------|---------------------|----------------|-----|--------------------------|
| **JAK2** | ✅ Yes | +1.24 | 3.8e-04 | Concordant (up in both) |
| **TYK2** | ✅ Yes | +0.98 | 0.003 | Concordant |
| **BTK** | ✅ Yes | +1.52 | 1.2e-05 | Concordant, stronger in blood |
| **TNF** | ✅ Yes | +0.87 | 0.008 | Concordant |
| **STAT3** | ⚠️ Marginal | +0.62 | 0.045 | Weaker in blood |
| **IL6** | ❌ Not significant | +0.31 | 0.22 | Tissue-restricted signal |
| **IRAK4** | ❌ Not significant | +0.18 | 0.41 | Tissue-restricted signal |

**Key insight**: JAK2, TYK2, and BTK are detectable in peripheral blood, making them potential blood-based biomarkers for treatment monitoring. IL6 and IRAK4 signals appear tissue-restricted.

---

## Single-Cell Deconvolution (CIBERSORTx)

Estimated cell-type contributions to the JAK2/TYK2/BTK signals:

| Gene | Primary Cell Type | % Signal | Secondary Cell Type | % Signal |
|------|-------------------|----------|--------------------|---------:|
| **JAK2** | Macrophages (M1) | 45% | T-helper cells (Th17) | 28% |
| **TYK2** | Dendritic cells | 38% | NK cells | 25% |
| **BTK** | B cells (activated) | 72% | Plasmablasts | 18% |
| **TNF** | Macrophages (M1) | 58% | Neutrophils | 22% |
| **STAT3** | Fibroblast-like synoviocytes | 41% | Macrophages | 31% |

**Key insight**: 
- **JAK2** signal comes primarily from macrophages and Th17 cells — both key drivers of RA
- **TYK2** is enriched in dendritic cells and NK cells — suggests role in innate immune activation
- **BTK** is overwhelmingly from B cells — confirms B-cell-specific targeting potential
- **STAT3** in fibroblasts suggests tissue-autonomous activation beyond immune cells

---

## Conclusions

1. **JAK2, TYK2, and BTK are validated across two independent studies** — consistent upregulation in both Study 001 and Study 002.

2. **All three are elevated in EARLY disease** — they are upstream drivers, not late-stage consequences. This makes them attractive intervention points.

3. **Blood-detectable targets** (JAK2, TYK2, BTK) enable pharmacodynamic biomarker strategies.

4. **Cell-type specificity matters for selectivity**:
   - JAK2 inhibition would affect macrophages + Th17 (broad anti-inflammatory)
   - TYK2 inhibition would affect DCs + NK cells (innate immunity)
   - BTK inhibition would be B-cell-selective (most specific)

5. **Recommendation**: Prioritize TYK2 and BTK for further validation based on:
   - TYK2: Early elevation + blood-detectable + more tissue-restricted than JAK2
   - BTK: Early elevation + strong blood signal + B-cell-specific (best therapeutic window)

---

## Data Availability

Raw data: `//genomics-share/projects/RA-002/`
CIBERSORTx results: `//bioinformatics/results/INT-RNA-002/deconvolution/`
