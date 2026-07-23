# Chapter 3: Building Your First Bookshelf — Internal Data

> **Goal**: Create a Bookshelf from your organization's proprietary research data — RNA-sequencing results, internal experimental reports, gene expression profiles — and query it with Copilot to get cited answers grounded in your own data.

---

## 3.1 What You Will Learn

- How to create a Bookshelf from internal research documents
- How GraphRAG indexing works (knowledge graph + vector embeddings)
- How to query your Bookshelf with Copilot and get cited answers
- How to validate that ingestion succeeded

---

## 3.2 Prepare Your Internal Data

For this lab, gather the following types of documents into a single folder. If you don't have real data, create placeholder files:

```
C:\MicrosoftDiscoveryLab\sample-data\internal\
├── rna-seq-results-study-001.pdf          ← RNA sequencing differential expression results
├── rna-seq-results-study-002.pdf          ← Follow-up RNA-seq experiment
├── gene-expression-profile-disease-X.pdf  ← Gene expression profiling for your disease
├── internal-review-target-candidates.docx ← Internal review of candidate gene targets
├── screening-results-compound-library.pdf ← High-throughput screening results
├── immunology-pathway-analysis.pdf        ← Immunology pathway analysis report
└── prior-target-assessment-report.docx    ← Previous target assessment documentation
```

> **Tip**: In a real scenario, these would be your organization's proprietary research papers, lab reports, and experimental data summaries. Even 5-10 documents are enough to see the value.

**Supported file formats**: `.pdf`, `.docx`, `.pptx`, `.xlsx`, `.txt`, `.html`, `.md`

---

## 3.3 Create the "Internal Research" Bookshelf

### Option A: Via VS Code Sidebar

1. Open the **Bookshelf** panel in the Discovery sidebar.
2. Click the **+** button.
3. Name it: `InternalResearchData`
4. Choose the default provider when prompted (uses the bundled `all-MiniLM-L6-v2` ONNX embedding model — works offline, no API key required).

### Option B: Via dx CLI

```powershell
dx bookshelf create InternalResearchData --workspace C:\MicrosoftDiscoveryLab\workspace
```

---

## 3.4 Ingest Your Documents

### Option A: Via VS Code Sidebar

1. Right-click the newly created `InternalResearchData` shelf.
2. Select **Ingest Documents**.
3. Choose the folder: `C:\MicrosoftDiscoveryLab\sample-data\internal\`
4. Wait for indexing to complete. The shelf shows progress as it processes each document.

### Option B: Via dx CLI

```powershell
dx bookshelf ingest <shelf-id> C:\MicrosoftDiscoveryLab\sample-data\internal\ --recursive --workspace C:\MicrosoftDiscoveryLab\workspace
```

### What Happens During Ingestion

Microsoft Discovery uses **GraphRAG** (Graph Retrieval-Augmented Generation) — an advanced technique developed by Microsoft Research:

```
Your Documents → Text Extraction → Chunking → Embedding → Knowledge Graph + Vector DB
                                                              ↓
                                                    Queryable Bookshelf
```

Unlike traditional RAG (which only creates vector embeddings), GraphRAG also builds a **knowledge graph** that captures entity relationships:
- Gene → associated with → Disease
- Protein → interacts with → Protein
- Compound → targets → Gene
- Experiment → measured → Expression Level

This means the Bookshelf can answer **global questions** that require synthesizing across your entire corpus, such as:
- "What are the main themes across all our experiments?"
- "What relationships exist between the genes studied and the disease?"
- "What context is missing to determine the best target?"

---

## 3.5 Verify Ingestion

When ingestion finishes:
1. The shelf shows a **document count** and a **green health dot** ✅
2. Confirm via CLI:
   ```powershell
   dx bookshelf search <shelf-id> "gene expression" --workspace C:\MicrosoftDiscoveryLab\workspace
   ```

You should see search results with snippets from your ingested documents.

---

## 3.6 Query Your Bookshelf with Copilot

Now make Copilot use your internal data:

### Step 1: Expose the Bookshelf to Copilot

1. In the Bookshelf panel, **tick the checkbox** ☑️ next to `InternalResearchData`.
2. This exposes the Bookshelf to GitHub Copilot as a tool.

### Step 2: Open Copilot Chat

Press `Ctrl+Alt+I` (or click the Copilot Chat icon).

### Step 3: Ask Domain-Specific Questions

Try these prompts that only your internal data can answer:

**Task 3.1** — Basic retrieval:
```
What genes showed the highest differential expression in our RNA-seq studies?
```

**Task 3.2** — Cross-document synthesis:
```
Summarize the key findings across all our internal experiments related to 
target identification for [your disease].
```

**Task 3.3** — Gap analysis:
```
Based on our internal data, what additional experiments or information 
would be needed to confidently validate our top gene targets?
```

**Task 3.4** — Relationship discovery:
```
What relationships exist between the genes identified in our screening 
results and the immunology pathway analysis?
```

> **Key point**: Every response includes **citations** that link back to your source documents. This is the explainability that biologists need — you can trace every claim back to the evidence.

---

## 3.7 Understanding Global vs. Local Queries

The Bookshelf is optimized for **global queries** — questions that require reasoning across the entire corpus:

| Query Type | Example | Bookshelf Strength |
|------------|---------|-------------------|
| **Global** (synthesis) | "What are the main themes in our research?" | ⭐ Excellent |
| **Global** (implications) | "What are the implications for target X?" | ⭐ Excellent |
| **Global** (relationships) | "Describe the relationships between genes A, B, C" | ⭐ Excellent |
| **Local** (lookup) | "What was the exact p-value for gene X in study 001?" | ⚠️ Use with standard search |

---

## 3.8 CLI-Based Querying with Sources

For programmatic access or to get source citations:

```powershell
dx bookshelf ask "What are the most promising target candidates based on our experimental data?" --shelf <shelf-id> --sources --workspace C:\MicrosoftDiscoveryLab\workspace
```

The `--sources` flag returns the specific document chunks that grounded the answer.

---

## 3.9 Checkpoint

Before proceeding to Chapter 4, confirm:

- [ ] You created a Bookshelf named `InternalResearchData`
- [ ] You successfully ingested your internal documents (green health dot)
- [ ] You can query the Bookshelf from Copilot Chat and get cited answers
- [ ] You understand the difference between global and local queries
- [ ] You see how GraphRAG provides relationship-aware answers beyond simple keyword search

---

**Previous**: [← Chapter 2 — Interface Tour](chapter-02-interface-tour.md)
**Next**: [Chapter 4 — Connecting External Public Data Sources →](chapter-04-external-data-sources.md)
