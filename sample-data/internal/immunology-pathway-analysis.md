# Immunology Pathway Analysis — Rheumatoid Arthritis Target Candidates

## Study Information
- **Study ID**: INT-IMM-006
- **Date**: August 2025
- **Lead**: Dr. Marcus Williams (Immunology), Dr. Elena Vasquez (Translational)
- **Objective**: Map target candidates to immunological pathways, determine which immune cell populations and signaling cascades they control, and assess immunological rationale for therapeutic intervention

---

## Pathway-Level Analysis

### JAK-STAT Signaling in RA

```
Cytokines → Receptor → JAK activation → STAT phosphorylation → Gene transcription
                          │
                          ├── JAK1/JAK2 → IL-6, IFN-γ, GM-CSF
                          ├── JAK1/TYK2 → IFN-α/β, IL-12, IL-23
                          ├── JAK1/JAK3 → IL-2, IL-4, IL-7, IL-15, IL-21
                          └── JAK2/JAK2 → EPO, TPO (hematopoiesis)
```

**Key insight for selectivity**:
- **JAK2 inhibition** → blocks IL-6 AND EPO/TPO signaling → anti-inflammatory BUT anemia/thrombocytopenia
- **TYK2 inhibition** → blocks IFN-α, IL-12, IL-23 WITHOUT affecting EPO/TPO → anti-inflammatory WITHOUT hematological toxicity
- **JAK1/3 inhibition** (tofacitinib) → blocks IL-2, IL-4, IL-7, IL-15 → broad lymphocyte suppression

### Where Our Targets Sit in the Immune Response

```
INNATE IMMUNITY                          ADAPTIVE IMMUNITY
─────────────────                        ──────────────────

Dendritic Cell activation               T-cell differentiation
    │                                       │
    ├── TYK2 (IFN-α/β response)           ├── Th1 (TYK2: IL-12 → IFN-γ)
    │                                       ├── Th17 (TYK2: IL-23 → IL-17)
    └── IRAK4 (TLR signaling)             └── Treg (FOXP3 suppression)
                                                    
Macrophage polarization                  B-cell activation
    │                                       │
    ├── JAK2 (M1 inflammatory)             ├── BTK (BCR signaling)
    ├── TNF production                      ├── Antibody production
    └── IL-6 production (JAK2/STAT3)       └── Autoantibody (RF, anti-CCP)
```

---

## Internal Immunology Experiments — Historical Evidence

### Our immunology team has previously studied these targets in the following contexts:

#### TYK2 in Autoimmunity (Internal experiments 2023-2024)

| Experiment | Model | Result | Reference |
|------------|-------|--------|-----------|
| TYK2 knockout BMDMs + LPS | Bone marrow-derived macrophages | 70% reduction in IFN-β production | INT-IMM-2023-014 |
| TYK2 inhibitor (tool cpd) in CIA mice | Collagen-induced arthritis | 45% reduction in clinical score | INT-IMM-2023-027 |
| TYK2 expression in RA vs OA synovium | Human tissue bank samples | 2.1x higher in RA (n=8 paired) | INT-IMM-2024-003 |
| IL-23 neutralization in CIA mice | CIA model | 62% efficacy (benchmark) | INT-IMM-2024-008 |

**Summary**: Internal immunology data consistently supports TYK2 as a driver of IFN and IL-23-mediated inflammation in RA. The CIA mouse model showed meaningful efficacy with a tool compound.

#### BTK in B-cell Autoimmunity (Internal experiments 2022-2024)

| Experiment | Model | Result | Reference |
|------------|-------|--------|-----------|
| BTK inhibitor in CAIA model | Anti-collagen antibody model | 55% reduction in paw swelling | INT-IMM-2022-041 |
| B-cell depletion (anti-CD20) in CIA | CIA model | 38% efficacy (lower than expected) | INT-IMM-2023-005 |
| BTK KO mice + K/BxN serum transfer | Serum transfer arthritis | 65% reduction in arthritis score | INT-IMM-2023-019 |
| BTK in FLS co-culture | Synovial fibroblast + B-cell | Reduced FLS activation via B-cell products | INT-IMM-2024-012 |
| Mast cell degranulation | Human mast cells | BTK inhibitor reduces histamine 80% | INT-IMM-2024-015 |

**Summary**: BTK shows efficacy in antibody-driven RA models. The B-cell depletion (anti-CD20) comparator underperformed, suggesting BTK's additional effects on macrophages and mast cells may contribute to its activity beyond simple B-cell depletion.

#### JAK2 in Inflammation (Internal experiments 2021-2023)

| Experiment | Model | Result | Reference |
|------------|-------|--------|-----------|
| JAK2 selective inhibitor in CIA | CIA model | 72% efficacy (but 40% weight loss) | INT-IMM-2021-033 |
| JAK2 inhibitor hematology | Rat 14-day tox | Dose-dependent anemia (Hgb -25% at efficacious dose) | INT-TOX-2022-008 |
| JAK2 in M1 macrophage polarization | Human monocyte-derived macrophages | JAK2 inhibition blocks M1 by 80% | INT-IMM-2022-055 |
| Sub-maximal JAK2 + anti-TNF combination | CIA model | 65% efficacy with acceptable hematology | INT-IMM-2023-042 |

**Summary**: JAK2 is highly efficacious but causes predictable hematological toxicity at therapeutic doses. A sub-maximal dosing strategy in combination may be viable but adds complexity.

---

## Immune Cell Population Analysis — Which Cells Are Affected?

### Expected immunological effects of inhibiting each target:

| Target | Primary Immune Effect | Cell Populations Affected | Expected Clinical Benefit | Expected Side Effects |
|--------|----------------------|---------------------------|---------------------------|----------------------|
| **TYK2** | Block IFN-α, IL-12, IL-23 signaling | DCs, NK cells, Th1, Th17 | Reduce Th17 inflammation + IFN signature | Herpes zoster risk (IFN suppression) |
| **BTK** | Block BCR signaling, reduce Ab production | B cells, plasmablasts, macrophages, mast cells | Reduce autoantibodies + mast cell inflammation | Increased infection risk (reduced Ab) |
| **JAK2** | Block IL-6, GM-CSF, IFN-γ signaling | Macrophages (M1), Th17, myeloid progenitors | Broadly anti-inflammatory | Anemia, thrombocytopenia |
| **IRAK4** | Block TLR/IL-1 signaling | Macrophages, DCs | Reduce innate inflammation | Moderate immunosuppression |

---

## Immunological Rationale — Final Assessment

### TYK2: The Interferon-IL23 Axis

**Why this pathway matters in RA:**
- Type I interferons (IFN-α/β) are elevated in early RA and drive DC maturation and T-cell activation
- IL-12 drives Th1 cells (IFN-γ producers) — key in erosive disease
- IL-23 drives Th17 cells (IL-17 producers) — the dominant pathogenic T-cell subset in RA
- TYK2 is the ONLY kinase that sits at the intersection of all three pathways

**Internal evidence summary**: Three lines of internal evidence support TYK2:
1. Transcriptomic (elevated in disease, immune-restricted)
2. Functional (tool compound efficacy in CIA model)
3. Mechanistic (controls IFN + IL-23 in vitro)

### BTK: The B-cell-Macrophage-Mast Cell Axis

**Why this pathway matters in RA:**
- Autoantibodies (RF, anti-CCP) are pathognomonic for seropositive RA
- Immune complexes activate macrophages via FcγR (BTK-dependent)
- Mast cells in synovium release histamine and proteases (BTK-dependent)
- B cells present antigen to T cells (perpetuating adaptive immunity)

**Internal evidence summary**: Strong functional data in antibody-mediated models + unexpected macrophage/mast cell effects that differentiate from simple B-cell depletion.

---

## Combination Hypothesis: TYK2 + BTK

**Rationale**: TYK2 addresses the Th1/Th17/IFN axis (innate → adaptive bridge) while BTK addresses the B-cell/macrophage/mast cell axis. Together they would cover the major pathogenic immune mechanisms in RA without the hematological toxicity of JAK2 inhibition.

**Preliminary combination data**: Not yet tested internally. Recommend as a priority experiment for Q1 2026.

---

## Data Location

Immunology experiments: `//immunology-lab/projects/RA-targets/`
CIA model data: `//in-vivo/studies/RA/CIA-series/`
Flow cytometry: `//flow-core/data/RA-immune-phenotyping/`
