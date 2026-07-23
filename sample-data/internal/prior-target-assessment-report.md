# Prior Target Assessment Report — Rheumatoid Arthritis Program

## Document Information
- **Document ID**: INT-TAR-007
- **Date**: April 2024 (updated December 2024)
- **Authors**: Target Assessment Committee
- **Status**: SUPERSEDED — This document captures the prior assessment. A new assessment using Microsoft Discovery is underway.

---

## Background

In 2024, the RA program conducted a preliminary target assessment using traditional methods (manual literature review, expert opinion, limited computational analysis). This document captures those findings for comparison with the AI-augmented assessment.

---

## Prior Assessment Methodology

| Step | Method | Time Required | Limitations |
|------|--------|---------------|-------------|
| Literature review | Manual PubMed searches, 3 team members | 3 weeks | Incomplete coverage, recency bias |
| Internal data review | Email-based collection from lab heads | 2 weeks | Inconsistent formats, missing context |
| Database queries | Manual NCBI, UniProt, DrugBank lookups | 1 week | Point-in-time snapshots, no integration |
| Expert consultation | 2 advisory board meetings | 2 weeks | Subjective, limited by individual expertise |
| Scoring | Unstructured discussion + voting | 1 meeting | No formal criteria, recency/authority bias |
| **Total** | | **~8 weeks** | **Partial, subjective, hard to audit** |

---

## Prior Target List (April 2024)

The team identified the following candidates through the traditional process:

| Rank | Target | Rationale (as documented) | Confidence | Status |
|------|--------|--------------------------|------------|--------|
| 1 | TNF | "Well-validated, multiple approved drugs" | High | ❌ Deprioritized — already a crowded space |
| 2 | IL-6R | "Tocilizumab validates the pathway" | High | ❌ Deprioritized — biosimilar competition |
| 3 | JAK1/3 | "Tofacitinib approved, pathway validated" | High | ❌ Deprioritized — safety concerns with pan-JAK |
| 4 | JAK2 | "Interesting signal in our RNA-seq" | Medium | ⚠️ Under review — safety concerns |
| 5 | BTK | "B-cell target, rituximab validates mechanism" | Medium | ⚠️ Under review — prior clinical failures |
| 6 | TYK2 | "Mentioned in recent literature" | Low | 🔄 RE-EVALUATED — now top candidate |

---

## What Was Missed in the Prior Assessment

The traditional approach missed several critical insights that the AI-augmented analysis later revealed:

### 1. TYK2 was underrated

**Why it was ranked low**: Only one team member had read the recent TYK2 literature. The gene was "mentioned in recent literature" without quantitative analysis.

**What was missed**:
- TYK2 is upregulated 2.14x in EARLY disease (not just chronic)
- TYK2 sits at the intersection of IFN-α, IL-12, and IL-23 pathways
- TYK2 is more immune-restricted than JAK2 (better safety)
- Deucravacitinib (TYK2 inhibitor) was already advancing for psoriasis — cross-indication signal
- Our own internal compound CPD-7234 showed 12 nM TYK2 potency with 85x selectivity

### 2. TNF and IL-6R were over-weighted due to familiarity bias

**Why they ranked high**: Team members were familiar with approved drugs (adalimumab, tocilizumab). "Validated pathway" was used as evidence for ranking, even though the competitive landscape makes these poor targets for a new program.

### 3. The combination hypothesis (TYK2 + BTK) was never considered

**Why**: Traditional assessment scored targets independently. No systematic analysis of which biological mechanisms remain unaddressed by each individual target. The idea of combining TYK2 (Th1/Th17 axis) with BTK (B-cell/macrophage axis) only emerged from the comprehensive pathway analysis.

### 4. BTK's mechanism beyond B-cells was not appreciated

**Why**: The team assumed BTK = B-cell target. The mast cell and macrophage FcγR activity was in our own screening data (INT-HTS-005) but wasn't connected to the target assessment. Previous clinical BTK failures were attributed to "wrong mechanism" rather than "incomplete understanding of mechanism."

---

## Lessons Learned — Why Manual Assessment Falls Short

| Issue | Example from This Assessment | Impact |
|-------|------------------------------|--------|
| **Recency bias** | TNF ranked #1 because everyone knows it | Missed novel opportunities |
| **Authority bias** | Senior scientist's opinion weighted > data | TYK2 dismissed because one person hadn't read the papers |
| **Incomplete data integration** | Screening data (INT-HTS-005) never connected to target assessment | BTK's macrophage/mast cell mechanism missed |
| **No systematic scoring** | "I think it's medium confidence" | Different people mean different things |
| **No cross-source synthesis** | Internal data, public literature, and databases reviewed separately | Combination hypothesis invisible |
| **Not reproducible** | Another team would reach different conclusions | Can't audit or challenge the reasoning |
| **Time-constrained** | 3 weeks of literature review covered ~200 papers | Missed thousands of relevant publications |

---

## Updated Recommendation (December 2024)

After reviewing the limitations above, the Target Assessment Committee recommended:

1. **Conduct a comprehensive AI-augmented assessment** using Microsoft Discovery to:
   - Index ALL internal experimental data (not just what individuals remember)
   - Systematically query public databases with consistent criteria
   - Apply formal scoring framework with explicit criteria
   - Ensure every claim is traceable to source evidence
   - Capture the assessment in a reproducible, auditable format

2. **Provisional target ranking** (pending comprehensive assessment):
   - TYK2 promoted to candidate #1 (based on updated literature + internal data)
   - BTK maintained as candidate #2 (pending mechanism clarification)
   - JAK2 de-prioritized to candidate #3 (safety concerns unresolved)
   - TNF and IL-6R removed from active consideration (competitive landscape)

3. **Timeline**: Complete AI-augmented assessment by Q2 2026 — **this lab represents that assessment**.

---

*This document is retained for historical comparison. The Microsoft Discovery-based assessment supersedes all findings herein.*
