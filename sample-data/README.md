# Sample Data for Microsoft Discovery Lab

This folder contains **synthetic research documents** created specifically for this lab. All data is fictional and designed to simulate realistic pharmaceutical R&D documents for a drug target identification workflow in **Rheumatoid Arthritis (RA)**.

> **These files are NOT real research data.** They are structured to demonstrate how Microsoft Discovery Bookshelves work with scientific content. The genes, targets, and findings are based on publicly known biology but the specific experimental results, compound IDs, and study details are fabricated.

---

## Folder Structure

```
sample-data/
├── internal/                              ← Simulates proprietary company data
│   ├── rna-seq-results-study-001.md       ← RNA-seq differential expression (established RA)
│   ├── rna-seq-results-study-002.md       ← Follow-up RNA-seq (early vs. late RA + blood)
│   ├── gene-expression-profile-disease-X.md  ← Cross-tissue expression profiling
│   ├── internal-review-target-candidates.md  ← Internal target review committee document
│   ├── screening-results-compound-library.md ← High-throughput compound screening results
│   ├── immunology-pathway-analysis.md     ← Internal immunology experiments & pathway mapping
│   └── prior-target-assessment-report.md  ← Previous (manual) target assessment for comparison
│
└── public-literature/                     ← Simulates curated public publications
    ├── review-ra-therapeutic-targets-2024.md         ← Review article: RA target landscape
    ├── tyk2-therapeutic-target-evidence.md           ← TYK2 evidence review (genetics + clinical)
    ├── btk-inhibitors-autoimmune-clinical-review.md  ← BTK clinical trial outcomes review
    ├── rna-seq-meta-analysis-ra-synovium.md          ← Public RNA-seq meta-analysis (8 datasets)
    ├── clinical-trials-ra-targets-summary.md         ← ClinicalTrials.gov compilation
    └── immunology-pathways-ra-review.md              ← Immunology review (pathways + cells)
```

---

## Disease Focus: Rheumatoid Arthritis (RA)

The lab uses RA as the disease model because it has:
- Well-characterized immunology with multiple druggable pathways
- Both approved therapies (TNF, IL-6R, JAK) and emerging targets (TYK2, BTK)
- Rich public datasets for cross-referencing
- A clear "before/after" story: prior manual assessment missed key insights that the AI-augmented approach reveals

---

## Target Candidates in the Data

The synthetic data follows a coherent narrative around these target candidates:

| Target | Gene | Story Arc |
|--------|------|-----------|
| **TYK2** | Tyrosine Kinase 2 | Emerges as #1 candidate — best balance of efficacy + safety |
| **BTK** | Bruton Tyrosine Kinase | Strong #2 — B-cell specific, best biomarker potential |
| **JAK2** | Janus Kinase 2 | Strongest signal but deprioritized on safety (anemia risk) |
| **IRAK4** | IL-1R Associated Kinase 4 | Lower priority — weaker signal, less specific |
| **PTPN22** | Protein Tyrosine Phosphatase 22 | Watch — genetically validated but hard to drug |

---

## How to Use These Files

### In the Discovery App (Chapters 3-4):

1. **Ingest the `internal/` folder** into a Bookshelf called `InternalResearchData`
2. **Ingest the `public-literature/` folder** into a Bookshelf called `PublicLiterature`
3. Both Bookshelves will be indexed using GraphRAG, creating searchable knowledge graphs

### What You'll Be Able to Ask:

After ingestion, try questions like:
- "What genes are most consistently upregulated across our RNA-seq studies?"
- "What is the safety profile of targeting JAK2 vs. TYK2?"
- "Where have we seen BTK in our internal immunology work?"
- "What does the public literature say about TYK2 in autoimmune disease?"
- "Compare our internal findings with the published meta-analysis"
- "What combination strategies are supported by the evidence?"

---

## File Format Note

Files are in **Markdown (.md)** format, which is one of the supported formats for Microsoft Discovery Bookshelf ingestion. In a real scenario, you would typically ingest PDFs, DOCX, and other document formats. Markdown is used here because:
- It renders nicely on GitHub for browsing
- It's lightweight and portable
- It's fully supported by the Discovery Bookshelf indexer
- It doesn't require specialized software to view/edit
