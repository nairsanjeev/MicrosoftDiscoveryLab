# Chapter 9: Capturing Findings — Notebooks & Reporting

> **Goal**: Document your target identification and validation workflow in Discovery Notebooks. Create a publishable target assessment report that captures all evidence, rankings, and recommendations with full traceability.

---

## 9.1 What You Will Learn

- How to create and use the three Notebook formats (Jupyter, Wiki, Brief)
- How to capture findings from Bookshelf queries and engine outputs
- How to build a structured target assessment report
- How to publish notebooks as shareable documents
- How to version-control your entire Discovery workflow

---

## 9.2 Choosing the Right Notebook Format

| Format | Best For | Structure |
|--------|----------|-----------|
| **Wiki** | Team-shared target assessment knowledge | Folder of themed .md files (`findings.md`, `decisions.md`, `notes.md`) |
| **Jupyter** | Personal lab journal with computational analysis | Single notebook file with typed cells |
| **Brief** | Executive summary for leadership | Auto-updating `brief.md` document |

For this lab, we'll create all three:
1. **Wiki**: Complete target assessment documentation
2. **Brief**: Executive summary for R&D leadership
3. **Jupyter**: Computational analysis log

---

## 9.3 Create the Target Assessment Wiki

### Step 1: Create the Wiki Notebook

Open Copilot Chat and run:

```
Create a wiki notebook called "Target Assessment — [DISEASE]"
```

Or via the sidebar: Notebooks panel → click **+** → choose **Wiki** → name it.

### Step 2: Populate the Wiki Pages

The Wiki creates a folder of Markdown files. Structure it for the target assessment:

**Task 9.1** — Create the findings page:
```
Save the following to my Target Assessment wiki as a findings document:

# Target Assessment Findings — [DISEASE]

## Identified Candidate Targets
[Insert the candidate target list from Chapter 5]

## Prioritization Scores
[Insert the ranked scoring table from Chapter 6]

## Validation Results
[Insert the validated recommendations from Chapter 7]

## Evidence Gaps
[List all identified gaps that need further investigation]
```

**Task 9.2** — Create the decisions page:
```
Save the following as a decisions document in my Target Assessment wiki:

# Target Selection Decisions

## Decision 1: Top Target Selection
- **Decision**: [GENE_A] is recommended as the primary target
- **Rationale**: Highest combined score (X/30), confirmed by 
  independent expression data, active clinical trials in related 
  indication
- **Evidence**: [Citations from Chapters 5-7]
- **Risks**: [Counter-evidence from Chapter 7]
- **Next steps**: [Experimental validation plan]

## Decision 2: Secondary Target
- **Decision**: [GENE_B] recommended for parallel investigation
- **Rationale**: Strong biological rationale, good druggability
- **Evidence**: [Citations]
```

**Task 9.3** — Create the methods page:
```
Save the following as a methods document in my Target Assessment wiki:

# Methodology

## Data Sources
- Internal Bookshelf: [X] documents indexed (RNA-seq results, 
  screening data, internal reports)
- Public Literature Bookshelf: [Y] curated publications indexed
- Live APIs: PubMed, ClinicalTrials.gov, NCBI Entrez, UniProt

## Scoring Framework
[Insert the 6-dimension scoring framework from Chapter 6]

## Validation Approach
[Insert the 5-step validation methodology from Chapter 7]

## Tools Used
- Microsoft Discovery App v[version]
- Bookshelves: InternalResearchData, PublicLiterature
- MCP Tools: BioMCP, NCBI Entrez, UniProt
- Discovery Engine: target-prioritization (Supervised mode)
```

---

## 9.4 Create the Executive Brief

The Brief format auto-updates as work progresses — ideal for leadership summaries.

**Task 9.4** — Create the executive brief:
```
Create a brief notebook called "Target Assessment Executive Summary"

The brief should contain:

# Target Assessment Executive Summary — [DISEASE]
**Date**: [Today's date]
**Team**: [Team name]
**Status**: Target identification and validation complete

## Key Findings
- Evaluated [N] candidate therapeutic targets
- Analyzed [X] internal datasets + [Y] public data sources
- Top recommended target: [GENE_A] (score: X/30)

## Recommended Targets (Go / No-Go)
| Target | Score | Recommendation | Key Risk |
|--------|-------|---------------|----------|
| GENE_A | 25/30 | **GO** | Limited clinical data |
| GENE_B | 22/30 | **GO** | Needs internal validation |
| GENE_C | 20/30 | **CONDITIONAL** | Counter-evidence exists |

## Evidence Strength
- Internal data: Strong (RNA-seq + screening validated)
- Public literature: Strong (N publications supporting)
- Clinical precedent: Moderate (related trials exist)
- Independent validation: Confirmed for top 2 targets

## Next Steps
1. Experimental validation of GENE_A in [disease model]
2. Structural biology study for druggability confirmation
3. Begin lead compound identification
4. [Other next steps]

## Methodology
This assessment used Microsoft Discovery to aggregate and 
synthesize evidence from [N] internal documents and [M] public 
databases. All recommendations are traceable to specific evidence 
with citations. Full methodology in the Target Assessment Wiki.
```

---

## 9.5 Create the Analysis Jupyter Notebook

For computational analysis logging:

**Task 9.5** — Create the Jupyter notebook:
```
Create a Jupyter notebook called "Target Analysis Log"
```

In the Jupyter notebook, create cells for each analysis step. Use the typed cell types:

| Cell Type | Purpose |
|-----------|---------|
| **Finding** | Key results from Bookshelf queries |
| **Decision** | Target selection decisions and rationale |
| **Hypothesis** | Hypotheses for further testing |
| **Note** | General observations and context |

---

## 9.6 Pin Results from Bookshelf Searches

As you work through the analysis, you can pin Bookshelf search results directly to your notebooks:

**Task 9.6** — Pin key evidence:
```
Search my InternalResearchData bookshelf for the top evidence 
supporting [GENE_A] as a target. Pin the top 3 results to my 
Target Analysis Log notebook.
```

This creates a traceable chain: **Source Document → Bookshelf Search → Notebook Entry → Report**.

---

## 9.7 Publish Your Findings

Notebooks can be rendered to shareable formats:

**Task 9.7** — Publish the wiki:
```
Render my "Target Assessment — [DISEASE]" notebook as LaTeX.
```

Supported output formats:
- **Wiki pages** — shareable Markdown
- **LaTeX** — for academic-style reports
- **PowerPoint outlines** — for leadership presentations

---

## 9.8 Version Control Your Work

Everything in `.discovery/` is plain files on disk. Version-control the entire workflow:

```powershell
cd C:\MicrosoftDiscoveryLab\workspace
git init
git add .discovery/
git commit -m "Target assessment for [DISEASE] - initial analysis complete"
```

This captures:
- All Bookshelf configurations and indices
- Task graphs with dependencies and results
- Notebook content and findings
- Engine configurations
- Model and tool settings

---

## 9.9 The Complete Documentation Stack

After this chapter, you have:

```
📓 Notebooks
├── Wiki: "Target Assessment — [DISEASE]"
│   ├── findings.md          ← All candidate targets with evidence
│   ├── decisions.md         ← Target selection decisions with rationale
│   ├── methods.md           ← Complete methodology documentation
│   └── notes.md             ← Additional observations
├── Brief: "Executive Summary"
│   └── brief.md             ← Leadership-ready summary
└── Jupyter: "Target Analysis Log"
    └── analysis.ipynb        ← Computational log with typed cells
```

---

## 9.10 Checkpoint

Before proceeding to Chapter 10, confirm:

- [ ] A Wiki notebook documents all findings, decisions, and methods
- [ ] An Executive Brief summarizes recommendations for leadership
- [ ] A Jupyter notebook logs the computational analysis
- [ ] Key Bookshelf results are pinned to notebooks
- [ ] The `.discovery/` folder is under version control (or ready to be)
- [ ] The documentation is self-contained and traceable — any finding can be traced back to source data

---

**Previous**: [← Chapter 8 — HPC & Analysis](chapter-08-hpc-and-analysis.md)
**Next**: [Chapter 10 — Recap & Next Steps →](chapter-10-recap-next-steps.md)
