# Gene Expression Profiling — Disease X (Rheumatoid Arthritis)

## Study Overview
- **Study ID**: INT-GEP-003
- **Date**: January 2025
- **Lead**: Dr. Priya Patel, Computational Biology
- **Objective**: Characterize the gene expression landscape across multiple RA-affected tissues to identify tissue-specific and shared target candidates
- **Tissues profiled**: Synovial membrane, articular cartilage, bone marrow, peripheral blood
- **Patients**: 12 RA patients undergoing joint replacement surgery
- **Platform**: Affymetrix Human Genome U133 Plus 2.0 + Illumina RNA-seq (matched)

---

## Cross-Tissue Expression Matrix — Target Candidates

Expression level: TPM (Transcripts Per Million), mean across patients

| Gene | Synovium | Cartilage | Bone Marrow | Blood (PBMCs) | Tissue Specificity Index |
|------|----------|-----------|-------------|---------------|--------------------------|
| **JAK2** | 187.4 | 42.1 | 89.3 | 62.8 | 0.45 (moderate) |
| **TYK2** | 94.2 | 18.7 | 31.5 | 48.6 | 0.38 (moderate) |
| **BTK** | 156.8 | 8.2 | 198.4 | 124.5 | 0.31 (low — broadly expressed) |
| **TNF** | 234.1 | 67.3 | 112.7 | 89.4 | 0.42 (moderate) |
| **IL6** | 312.5 | 156.8 | 45.2 | 12.3 | 0.68 (high — tissue enriched) |
| **STAT3** | 245.6 | 189.3 | 167.4 | 134.2 | 0.18 (very low — ubiquitous) |
| **IRAK4** | 78.4 | 34.5 | 56.2 | 41.8 | 0.22 (low) |
| **PTPN22** | 45.6 | 5.2 | 67.8 | 89.4 | 0.42 (moderate) |
| **MMP9** | 198.7 | 287.4 | 34.5 | 23.1 | 0.62 (high — joint enriched) |
| **FOXP3** | 12.3 | 2.1 | 45.6 | 67.8 | 0.52 (blood/marrow enriched) |

---

## Normal Tissue Expression (Human Protein Atlas Reference)

Critical for assessing safety — ubiquitously expressed genes are higher risk.

| Gene | Tissues with high expression | Selectivity for immune/joint tissue | Safety Concern |
|------|------------------------------|-------------------------------------|----------------|
| **JAK2** | Bone marrow, spleen, liver | Moderate — also in hematopoiesis | ⚠️ Anemia risk |
| **TYK2** | Immune cells, limited elsewhere | **High** — relatively immune-restricted | ✅ Low off-target risk |
| **BTK** | B cells, macrophages, mast cells | **High** — immune-cell-specific | ✅ Low off-target risk |
| **TNF** | Macrophages, many immune cells | Moderate | ⚠️ Broad immune suppression |
| **IL6** | Synovium, liver, adipose | Moderate | ⚠️ Liver/metabolic effects |
| **STAT3** | **Ubiquitous** (all tissues) | **Very low** | ❌ High off-target risk |
| **IRAK4** | Broadly expressed | Low | ⚠️ Immune suppression |
| **PTPN22** | T cells, B cells | High — immune-restricted | ✅ Low off-target risk |

---

## Expression Correlation Analysis

Genes that co-vary in expression may be in the same regulatory network:

| Gene Pair | Pearson r | p-value | Interpretation |
|-----------|-----------|---------|----------------|
| JAK2 — STAT3 | 0.91 | 2.3e-05 | Same pathway (JAK-STAT) |
| JAK2 — TYK2 | 0.72 | 0.003 | Both JAK-family, partially co-regulated |
| BTK — CD79A | 0.94 | 4.1e-06 | B-cell receptor co-expression |
| TNF — IL6 | 0.83 | 4.5e-04 | Co-regulated inflammatory cytokines |
| TYK2 — IFNAR1 | 0.88 | 1.2e-04 | Interferon signaling co-expression |
| FOXP3 — CTLA4 | 0.89 | 8.7e-05 | T-reg co-expression signature |

---

## Network Analysis — Hub Scores

Hub score reflects how connected a gene is in the co-expression network (0-1 scale):

| Gene | Hub Score | Network Module | Module Function |
|------|-----------|----------------|-----------------|
| **STAT3** | 0.94 | Blue (inflammation) | Master transcription factor, connects multiple pathways |
| **JAK2** | 0.87 | Blue (inflammation) | Central kinase in cytokine signaling |
| **TNF** | 0.82 | Blue (inflammation) | Pro-inflammatory cytokine hub |
| **BTK** | 0.76 | Green (B-cell) | B-cell signaling hub |
| **TYK2** | 0.71 | Yellow (interferon) | Interferon/innate immunity hub |
| **IL6** | 0.68 | Blue (inflammation) | Cytokine, downstream effector |
| **IRAK4** | 0.54 | Red (TLR signaling) | TLR/IL-1 pathway |
| **PTPN22** | 0.48 | Purple (T-cell) | T-cell regulation |

---

## Key Conclusions for Target Selection

1. **STAT3 is the most connected hub BUT is ubiquitously expressed** — high efficacy potential but unacceptable safety profile as a direct target.

2. **JAK2 is a high-value target** (hub score 0.87, elevated in disease) but broad expression in bone marrow raises hematological toxicity concerns.

3. **TYK2 offers the best balance**: moderate hub score (0.71), restricted expression (immune cells), elevated in early disease, blood-detectable. Inhibiting TYK2 would disrupt the interferon module without broadly suppressing all JAK signaling.

4. **BTK is the most selective target**: B-cell-specific expression, high hub score within the B-cell module, early elevation. Risk: may not address macrophage/Th17-driven inflammation.

5. **Recommended target ranking based on expression profiling alone**:
   1. TYK2 (best balance of selectivity + relevance + safety)
   2. BTK (highest selectivity, but more lineage-restricted efficacy)
   3. JAK2 (highest efficacy potential, but safety concerns)
   4. IRAK4 (moderate, but in a different signaling axis — TLR)

---

## Data Location

Expression matrices: `//bioinformatics/results/INT-GEP-003/`
Network analysis: `//bioinformatics/results/INT-GEP-003/WGCNA/`
HPA reference data: `//databases/human-protein-atlas/v23/`
