# Microsoft Discovery Lab — Drug Target Identification & Validation

> **Scenario**: A biologist on a pharmaceutical R&D team needs to identify, prioritize, and validate therapeutic targets for a specific disease. The workflow involves aggregating internal experimental datasets (RNA-seq, proprietary screening results) with external public knowledge (ClinicalTrials.gov, PubMed, gene expression databases) to understand the biology of candidate genes, rank them by evidence strength, and build an explainable case for which targets to pursue.

---

## Lab Overview

| Chapter | Title | Focus |
|---------|-------|-------|
| 1 | [Environment Setup & Installation](chapters/chapter-01-installation.md) | Install Microsoft Discovery app, create workspace, verify dependencies |
| 2 | [Navigating the Discovery Interface](chapters/chapter-02-interface-tour.md) | Sidebar tour, understanding Bookshelves, Tools, Tasks, Engines, Notebooks |
| 3 | [Building Your First Bookshelf — Internal Data](chapters/chapter-03-bookshelf-internal-data.md) | Ingest proprietary RNA-seq data, experimental reports, internal publications |
| 4 | [Connecting External Public Data Sources](chapters/chapter-04-external-data-sources.md) | Enable PubMed, ClinicalTrials.gov, NCBI, UniProt MCP tools; query public knowledge |
| 5 | [Target Identification — Aggregating Public + Internal Knowledge](chapters/chapter-05-target-identification.md) | Combine Bookshelves with external tools to identify candidate genes |
| 6 | [Target Prioritization — Ranking & Explainability](chapters/chapter-06-target-prioritization.md) | Use Discovery Engine to score, rank, and explain target choices |
| 7 | [Target Validation — Cross-Referencing & Evidence Synthesis](chapters/chapter-07-target-validation.md) | Validate top targets against clinical evidence, immunology data, internal experiments |
| 8 | [High-Dimensional Data Analysis & HPC Integration](chapters/chapter-08-hpc-and-analysis.md) | Leverage supercomputer node pools for large-scale analysis |
| 9 | [Capturing Findings — Notebooks & Reporting](chapters/chapter-09-notebooks-reporting.md) | Document findings in Discovery Notebooks, publish reports |
| 10 | [End-to-End Recap & Next Steps](chapters/chapter-10-recap-next-steps.md) | Review the complete workflow, cost considerations, scaling to enterprise |

---

## Prerequisites

- **Operating System**: Windows 11
- **GitHub Copilot**: Active subscription
- **Sample Data**: A set of internal research documents (PDFs, CSVs) representing RNA-seq results, gene expression profiles, and internal experimental reports. Sample files are provided in the `sample-data/` folder.
- **Azure subscription** (optional): Required for Chapters 8 and 10 (enterprise-scale features). The first 7 chapters work entirely on a local laptop with no cloud setup.

## Reference Links

| Resource | URL |
|----------|-----|
| What is Microsoft Discovery? | https://learn.microsoft.com/en-us/azure/microsoft-discovery/overview-what-is-microsoft-discovery |
| Key Scenarios | https://learn.microsoft.com/en-us/azure/microsoft-discovery/overview-key-scenarios |
| Discovery App Quick Start (GitHub) | https://github.com/microsoft/discovery/blob/main/docs/discovery-app/quickstart.md |
| Discovery GitHub Repository | https://github.com/microsoft/discovery |
| Bookshelf & Knowledge Bases | https://learn.microsoft.com/en-us/azure/microsoft-discovery/concept-bookshelf-knowledge-bases |
| Discovery Engine | https://learn.microsoft.com/en-us/azure/microsoft-discovery/concept-discovery-engine |
| Create Agents | https://learn.microsoft.com/en-us/azure/microsoft-discovery/how-to-agent-creation |
| Discovery Studio | https://studio.discovery.microsoft.com/ |
