# RNA-Seq Differential Expression Analysis — Study 001

## Study Information
- **Study ID**: INT-RNA-001
- **Date**: March 2025
- **Principal Investigator**: Dr. Sarah Chen
- **Disease Model**: Rheumatoid Arthritis (RA)
- **Tissue**: Synovial tissue biopsies
- **Comparison**: RA patients (n=24) vs. healthy controls (n=18)
- **Platform**: Illumina NovaSeq 6000, paired-end 150bp
- **Alignment**: STAR v2.7.10 → hg38
- **Quantification**: featureCounts → DESeq2

---

## Top Differentially Expressed Genes (FDR < 0.01, |log2FC| > 1.5)

| Rank | Gene Symbol | Gene Name | log2FC | p-value | FDR | Direction |
|------|-------------|-----------|--------|---------|-----|-----------|
| 1 | **JAK2** | Janus Kinase 2 | +3.42 | 1.2e-12 | 8.4e-10 | Up |
| 2 | **STAT3** | Signal Transducer and Activator of Transcription 3 | +2.87 | 3.1e-11 | 1.1e-08 | Up |
| 3 | **TNF** | Tumor Necrosis Factor | +2.65 | 8.7e-11 | 2.0e-08 | Up |
| 4 | **IL6** | Interleukin 6 | +2.51 | 1.4e-10 | 2.5e-08 | Up |
| 5 | **PTPN22** | Protein Tyrosine Phosphatase Non-Receptor Type 22 | +2.34 | 5.6e-10 | 8.1e-08 | Up |
| 6 | **CCL2** | C-C Motif Chemokine Ligand 2 | +2.28 | 9.2e-10 | 1.1e-07 | Up |
| 7 | **MMP9** | Matrix Metallopeptidase 9 | +2.15 | 2.1e-09 | 2.1e-07 | Up |
| 8 | **IRAK4** | Interleukin 1 Receptor Associated Kinase 4 | +1.98 | 5.8e-09 | 4.9e-07 | Up |
| 9 | **FOXP3** | Forkhead Box P3 | -2.31 | 3.2e-09 | 2.9e-07 | Down |
| 10 | **IL10** | Interleukin 10 | -1.87 | 1.8e-08 | 1.3e-06 | Down |
| 11 | **TYK2** | Tyrosine Kinase 2 | +1.82 | 4.1e-08 | 2.7e-06 | Up |
| 12 | **BTK** | Bruton Tyrosine Kinase | +1.76 | 7.3e-08 | 4.4e-06 | Up |
| 13 | **RIPK2** | Receptor Interacting Serine/Threonine Kinase 2 | +1.71 | 1.2e-07 | 6.6e-06 | Up |
| 14 | **CD40** | CD40 Molecule | +1.68 | 2.0e-07 | 1.0e-05 | Up |
| 15 | **CTLA4** | Cytotoxic T-Lymphocyte Associated Protein 4 | -1.64 | 3.5e-07 | 1.6e-05 | Down |

---

## Pathway Enrichment Analysis (GSEA, FDR < 0.05)

| Pathway | NES | FDR | Leading Edge Genes |
|---------|-----|-----|--------------------|
| JAK-STAT signaling | +2.84 | 0.001 | JAK2, STAT3, TYK2, STAT1, JAK1 |
| TNF signaling | +2.61 | 0.001 | TNF, TNFRSF1A, RIPK1, TRAF2, NFKB1 |
| NF-κB signaling | +2.45 | 0.002 | NFKB1, RELA, IKBKB, IRAK4, TRAF6 |
| IL-6/JAK/STAT3 | +2.38 | 0.003 | IL6, IL6R, JAK2, STAT3, SOCS3 |
| B-cell receptor signaling | +2.12 | 0.008 | BTK, SYK, CD79A, BLNK, PIK3CD |
| T-cell suppression | -2.05 | 0.012 | FOXP3, CTLA4, IL10, TGFB1, PDCD1 |

---

## Key Findings

1. **JAK-STAT pathway is the dominant upregulated signature** in RA synovial tissue, with JAK2 showing the highest fold change (3.42x) among all differentially expressed genes.

2. **JAK2 and STAT3 are co-upregulated**, suggesting constitutive activation of the JAK2-STAT3 axis in diseased tissue.

3. **TNF and IL-6 inflammatory cascades are strongly activated**, consistent with known RA pathology but here quantified at the transcriptomic level with high confidence.

4. **Regulatory T-cell markers are suppressed** (FOXP3 down 2.31x, IL10 down 1.87x), indicating impaired immune tolerance.

5. **BTK (Bruton Tyrosine Kinase) shows significant upregulation** (1.76x), suggesting B-cell activation contributes to the inflammatory milieu.

6. **TYK2 is independently upregulated** (1.82x), providing a second JAK-family target beyond JAK2.

---

## Conclusions and Recommendations

- JAK2 emerges as the strongest single-gene target based on expression magnitude and pathway centrality
- TYK2 represents an alternative JAK-family target with potentially better selectivity (more tissue-restricted)
- BTK warrants investigation as a B-cell-lineage-specific target
- Combined JAK2 + TNF targeting may address both innate and adaptive immune dysregulation
- Recommended: Follow-up study with single-cell resolution to determine cell-type-specific expression patterns

---

## Data Availability

Raw FASTQ files: Internal server `//genomics-share/projects/RA-001/`
Processed counts: `//bioinformatics/results/INT-RNA-001/`
DESeq2 results: `//bioinformatics/results/INT-RNA-001/deseq2_output/`
