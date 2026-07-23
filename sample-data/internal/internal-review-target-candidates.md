# Internal Review: Candidate Therapeutic Targets for Rheumatoid Arthritis

## Document Information
- **Document ID**: INT-REV-004
- **Date**: November 2025
- **Authors**: Dr. Sarah Chen (Genomics), Dr. Marcus Williams (Immunology), Dr. Priya Patel (Computational Biology), Dr. James Morrison (Medicinal Chemistry)
- **Classification**: Internal — Confidential
- **Purpose**: Consolidate internal experimental evidence for target candidate review meeting

---

## Executive Summary

Based on three internal studies (INT-RNA-001, INT-RNA-002, INT-GEP-003), we have identified a shortlist of candidate therapeutic targets for Rheumatoid Arthritis. This document summarizes the internal evidence for each candidate and provides preliminary recommendations.

**Top 3 candidates based on internal data:**
1. **TYK2** — Best overall profile (selectivity + efficacy + safety)
2. **BTK** — Most specific (B-cell selective) with strong blood biomarker potential
3. **JAK2** — Highest magnitude signal but safety concerns (hematological)

---

## Candidate Assessment Summary

### 1. TYK2 (Tyrosine Kinase 2)

**Internal evidence strength: STRONG**

| Dimension | Evidence | Source |
|-----------|----------|--------|
| Expression in disease | Upregulated 1.82x (Study 001), 2.14x early RA (Study 002) | INT-RNA-001, INT-RNA-002 |
| Early disease signal | ✅ Elevated before chronic remodeling | INT-RNA-002 |
| Blood detectable | ✅ log2FC +0.98 in PBMCs | INT-RNA-002 |
| Cell-type specificity | Dendritic cells (38%), NK cells (25%) | INT-RNA-002 (CIBERSORTx) |
| Tissue expression | Relatively immune-restricted | INT-GEP-003 |
| Network position | Hub score 0.71 in interferon module | INT-GEP-003 |
| Safety profile | Low off-target risk (immune-restricted) | INT-GEP-003 |

**Strengths**: Excellent therapeutic window. Immune-restricted expression means targeting TYK2 should not cause the hematological toxicities seen with pan-JAK inhibitors. The interferon module role is disease-relevant but more specific than broad JAK inhibition.

**Weaknesses**: Lower fold-change than JAK2 (2.14x vs. 3.42x). Interferon signaling has important antiviral functions — herpes zoster risk. Network hub score is moderate, suggesting it may not control as much of the disease network as JAK2.

**Internal recommendation**: ADVANCE — Strong candidate for further validation.

---

### 2. BTK (Bruton Tyrosine Kinase)

**Internal evidence strength: STRONG**

| Dimension | Evidence | Source |
|-----------|----------|--------|
| Expression in disease | Upregulated 1.76x (Study 001), 1.88x early RA (Study 002) | INT-RNA-001, INT-RNA-002 |
| Early disease signal | ✅ Elevated before chronic remodeling | INT-RNA-002 |
| Blood detectable | ✅ log2FC +1.52 in PBMCs (strongest blood signal) | INT-RNA-002 |
| Cell-type specificity | B cells (72%), Plasmablasts (18%) | INT-RNA-002 (CIBERSORTx) |
| Tissue expression | Immune-cell-specific (B cells, macrophages, mast cells) | INT-GEP-003 |
| Network position | Hub score 0.76 in B-cell module | INT-GEP-003 |
| Safety profile | Low off-target risk (B-cell-specific) | INT-GEP-003 |

**Strengths**: Highest selectivity of all candidates. B-cell-specific expression provides the best therapeutic window. Strongest blood-based biomarker signal enables PD monitoring. B-cell depletion is a validated mechanism in RA (rituximab precedent).

**Weaknesses**: B-cell-centric mechanism may not adequately address macrophage/Th17-driven inflammation. Multiple BTK inhibitors have failed or shown modest efficacy in RA clinical trials — suggests B-cell inhibition alone may be insufficient. May need combination approach.

**Internal recommendation**: ADVANCE WITH CAUTION — Consider combination strategies. Explore whether our BTK signal reflects B-cell pathology that previous trials didn't adequately target.

---

### 3. JAK2 (Janus Kinase 2)

**Internal evidence strength: VERY STRONG (expression) / MODERATE (safety)**

| Dimension | Evidence | Source |
|-----------|----------|--------|
| Expression in disease | Upregulated 3.42x (Study 001), 2.89x early RA (Study 002) | INT-RNA-001, INT-RNA-002 |
| Early disease signal | ✅ Highest magnitude in early disease | INT-RNA-002 |
| Blood detectable | ✅ log2FC +1.24 in PBMCs | INT-RNA-002 |
| Cell-type specificity | Macrophages M1 (45%), Th17 cells (28%) | INT-RNA-002 (CIBERSORTx) |
| Tissue expression | Moderate specificity — also in bone marrow, liver | INT-GEP-003 |
| Network position | Hub score 0.87 — second highest connectivity | INT-GEP-003 |
| Safety profile | ⚠️ Hematological toxicity risk (bone marrow expression) | INT-GEP-003 |

**Strengths**: Strongest expression signal of any candidate. Highest hub score among kinases. Addresses macrophage and Th17 pathology (key disease drivers). Tofacitinib (JAK1/3) already approved for RA — validates JAK pathway.

**Weaknesses**: JAK2 is essential for erythropoiesis and thrombopoiesis. JAK2 inhibitors (ruxolitinib) cause anemia and thrombocytopenia. Selective JAK2 inhibition in RA would need careful dose optimization. Expression in bone marrow and liver creates off-target risk.

**Internal recommendation**: DEPRIORITIZE as monotherapy target due to safety. Consider as combination partner at sub-maximal doses, or pursue allosteric/degrader approaches that achieve tissue-selective JAK2 modulation.

---

### 4. IRAK4 (Interleukin-1 Receptor Associated Kinase 4)

**Internal evidence strength: MODERATE**

| Dimension | Evidence | Source |
|-----------|----------|--------|
| Expression in disease | Upregulated 1.98x (Study 001), 1.38x early RA (Study 002) | INT-RNA-001, INT-RNA-002 |
| Early disease signal | ⚠️ Weaker in early disease vs. established | INT-RNA-002 |
| Blood detectable | ❌ Not significant in PBMCs | INT-RNA-002 |
| Tissue expression | Broadly expressed | INT-GEP-003 |
| Network position | Hub score 0.54 in TLR module | INT-GEP-003 |
| Safety profile | ⚠️ Moderate immune suppression risk | INT-GEP-003 |

**Internal recommendation**: LOWER PRIORITY — Signal is weaker in early disease and not blood-detectable. TLR pathway is relevant but IRAK4's position in the network is less central than JAK2 or TYK2. Multiple companies pursuing this target with mixed results.

---

### 5. PTPN22 (Protein Tyrosine Phosphatase Non-Receptor Type 22)

**Internal evidence strength: MODERATE**

| Dimension | Evidence | Source |
|-----------|----------|--------|
| Expression in disease | Upregulated 2.34x (Study 001), 1.21x early RA (Study 002) | INT-RNA-001, INT-RNA-002 |
| Genetic association | Known GWAS hit for RA (R620W variant) | Literature |
| Tissue expression | T-cells, B-cells (immune-restricted) | INT-GEP-003 |
| Druggability | ⚠️ Phosphatase — historically difficult to drug | Chemistry assessment |

**Internal recommendation**: WATCH — Strong genetic validation but phosphatases are notoriously difficult drug targets. May become tractable with new modalities (PROTACs, allosteric modulators).

---

## Overall Target Ranking (Internal Data Only)

| Rank | Target | Internal Evidence | Safety Profile | Druggability | Overall |
|------|--------|-------------------|----------------|--------------|---------|
| 1 | **TYK2** | Strong | Excellent | Good (kinase) | ⭐⭐⭐⭐⭐ |
| 2 | **BTK** | Strong | Excellent | Good (kinase) | ⭐⭐⭐⭐ |
| 3 | **JAK2** | Very Strong | Concerning | Good (kinase) | ⭐⭐⭐ |
| 4 | **IRAK4** | Moderate | Moderate | Good (kinase) | ⭐⭐⭐ |
| 5 | **PTPN22** | Moderate | Good | Poor (phosphatase) | ⭐⭐ |

---

## Next Steps

1. **External validation**: Cross-reference candidates against public datasets (GEO, Expression Atlas) and published literature
2. **Clinical landscape**: Check ClinicalTrials.gov for competitive activity
3. **Druggability deep dive**: Structural assessment for TYK2 and BTK
4. **Combination rationale**: Explore TYK2 + BTK combination hypothesis
5. **Biomarker strategy**: Design blood-based PD assays for TYK2 and BTK

---

*This document is for internal discussion purposes only. Not for external distribution.*
