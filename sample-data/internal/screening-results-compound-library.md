# High-Throughput Screening Results — Kinase Inhibitor Compound Library

## Study Information
- **Study ID**: INT-HTS-005
- **Date**: June 2025
- **Lead**: Dr. James Morrison (Medicinal Chemistry), Dr. Aisha Rahman (Screening)
- **Objective**: Screen internal kinase-focused compound library against RA-relevant targets
- **Library**: 12,400 compounds (internal kinase inhibitor collection)
- **Assay format**: Biochemical kinase activity (IC50 determination for hits)
- **Primary screen**: Single-point at 10 µM, >50% inhibition = hit
- **Confirmation**: Dose-response (8-point, 3-fold dilution)

---

## Primary Screen Results — Hit Rates

| Target | Compounds Screened | Hits (>50% inh.) | Hit Rate | Confirmed Hits (IC50 < 1 µM) |
|--------|-------------------|-------------------|----------|-------------------------------|
| **TYK2** | 12,400 | 287 | 2.3% | 43 |
| **BTK** | 12,400 | 412 | 3.3% | 78 |
| **JAK2** | 12,400 | 534 | 4.3% | 124 |
| **IRAK4** | 12,400 | 198 | 1.6% | 31 |

---

## Top Confirmed Hits — TYK2

| Compound ID | IC50 (TYK2) | Selectivity vs JAK1 | Selectivity vs JAK2 | Selectivity vs JAK3 | Drug-like Properties |
|-------------|-------------|---------------------|---------------------|---------------------|---------------------|
| **CPD-7234** | 12 nM | 85x | 120x | 92x | ✅ Good |
| CPD-7891 | 28 nM | 42x | 67x | 55x | ✅ Good |
| CPD-8102 | 45 nM | 23x | 34x | 28x | ⚠️ Moderate (high MW) |
| CPD-6554 | 67 nM | 31x | 45x | 38x | ✅ Good |
| CPD-9012 | 89 nM | 15x | 22x | 18x | ⚠️ Low solubility |

**Lead compound**: CPD-7234 shows excellent TYK2 potency (12 nM) with >85x selectivity over all other JAK family members. This is a strong starting point for medicinal chemistry optimization.

---

## Top Confirmed Hits — BTK

| Compound ID | IC50 (BTK) | Selectivity vs ITK | Selectivity vs BMX | Cell Activity (Ramos B-cell) | Drug-like Properties |
|-------------|------------|--------------------|--------------------|------------------------------|---------------------|
| **CPD-4521** | 3.2 nM | 145x | 89x | IC50 = 18 nM | ✅ Excellent |
| CPD-4890 | 8.7 nM | 67x | 42x | IC50 = 45 nM | ✅ Good |
| CPD-5102 | 15 nM | 34x | 28x | IC50 = 78 nM | ✅ Good |
| CPD-3876 | 22 nM | 23x | 18x | IC50 = 112 nM | ⚠️ Moderate |
| CPD-5567 | 31 nM | 56x | 45x | IC50 = 67 nM | ✅ Good |

**Lead compound**: CPD-4521 shows excellent BTK potency (3.2 nM) with strong selectivity and good translation to cell-based activity (18 nM in Ramos B-cells).

---

## Selectivity Profiling — Lead Compounds

### CPD-7234 (TYK2 lead) — Kinase panel (468 kinases at 1 µM)

- Kinases inhibited >50%: **4 out of 468** (TYK2, DYRK1A at 62%, CDK7 at 54%, FLT3 at 51%)
- **Selectivity score (S10)**: 0.009 — Highly selective
- **JAK family selectivity confirmed**: No significant inhibition of JAK1, JAK2, or JAK3 at 1 µM

### CPD-4521 (BTK lead) — Kinase panel (468 kinases at 1 µM)

- Kinases inhibited >50%: **3 out of 468** (BTK, TEC at 67%, BLK at 55%)
- **Selectivity score (S10)**: 0.006 — Extremely selective
- **ITK sparing confirmed**: <10% inhibition at 1 µM (important for T-cell safety)

---

## Cellular Activity — Disease-Relevant Assays

### TYK2 Lead (CPD-7234)

| Assay | Cell Type | Readout | IC50 | Interpretation |
|-------|-----------|---------|------|----------------|
| IFN-α signaling | PBMCs | pSTAT1 | 34 nM | Blocks type I interferon response |
| IL-12 signaling | T cells | pSTAT4 | 56 nM | Blocks Th1 polarization |
| IL-23 signaling | T cells | pSTAT3 | 78 nM | Blocks Th17 differentiation |
| TNF production | Macrophages | TNF ELISA | >10 µM | No direct TNF effect (expected) |
| B-cell proliferation | Ramos cells | CellTiter | >10 µM | No B-cell effect (expected) |

**Interpretation**: CPD-7234 selectively blocks TYK2-dependent signaling (IFN-α, IL-12, IL-23) without affecting TNF or B-cell biology. This is a clean TYK2 pharmacological profile.

### BTK Lead (CPD-4521)

| Assay | Cell Type | Readout | IC50 | Interpretation |
|-------|-----------|---------|------|----------------|
| BCR signaling | Ramos B cells | pBTK/pPLCγ2 | 18 nM | Blocks B-cell receptor signaling |
| B-cell proliferation | Primary B cells | CellTiter | 42 nM | Inhibits B-cell expansion |
| Antibody secretion | Plasmablasts | IgG ELISA | 89 nM | Reduces antibody production |
| FcγR signaling | Macrophages | TNF release | 234 nM | Moderate macrophage effect |
| T-cell activation | Jurkat T cells | IL-2 | >10 µM | No T-cell effect (ITK-sparing) |

**Interpretation**: CPD-4521 potently inhibits B-cell activation and function while sparing T cells. The moderate macrophage effect via FcγR is a bonus for RA.

---

## ADME Properties — Lead Compounds

| Property | CPD-7234 (TYK2) | CPD-4521 (BTK) | Ideal Range |
|----------|------------------|----------------|-------------|
| MW | 423 | 487 | <500 |
| cLogP | 2.8 | 3.4 | 1-4 |
| Solubility (pH 7.4) | 45 µM | 28 µM | >10 µM |
| Permeability (Caco-2) | 18 × 10⁻⁶ cm/s | 12 × 10⁻⁶ cm/s | >5 × 10⁻⁶ |
| Microsomal stability (human) | t½ = 67 min | t½ = 43 min | >30 min |
| hERG IC50 | >30 µM | >30 µM | >10 µM |
| CYP inhibition | Clean | CYP3A4 (IC50 = 8 µM) | >10 µM |

---

## Conclusions

1. **Both TYK2 and BTK have druggable screening hits** with sub-100 nM potency and good selectivity.
2. **CPD-7234 (TYK2)** is a strong lead with excellent JAK-family selectivity and clean cellular pharmacology.
3. **CPD-4521 (BTK)** is a potent, selective lead with good translation from biochemical to cellular assays.
4. **Both leads have acceptable drug-like properties** suitable for oral dosing optimization.
5. **Recommendation**: Advance both leads into lead optimization. The TYK2 lead (CPD-7234) has a cleaner overall profile; the BTK lead may need CYP3A4 liability addressed.

---

## Data Location

Screening data: `//screening-core/HTS/INT-HTS-005/`
Dose-response curves: `//screening-core/HTS/INT-HTS-005/dose-response/`
ADME data: `//dmpk/ADME/INT-HTS-005-leads/`
