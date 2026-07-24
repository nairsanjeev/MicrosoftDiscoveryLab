# Chapter 3: Building Your First Bookshelf — Internal Data

> **Goal**: Create a Bookshelf from your organization's proprietary research data — RNA-sequencing results, internal experimental reports, gene expression profiles — and query it with Copilot to get cited answers grounded in your own data.

---

## 3.1 What You Will Learn

- How to create a Project (required before you can ingest documents)
- How to choose between Chat-based setup and the Welcome screen
- How to use the project setup orchestrator agent to configure your environment
- How to create a Bookshelf from internal research documents
- How GraphRAG indexing works (knowledge graph + vector embeddings)
- How to query your Bookshelf with Copilot and get cited answers
- How to validate that ingestion succeeded

---

## 3.2 Prepare Your Internal Data

For this lab, sample data files are provided in the `sample-data/internal/` folder. These are synthetic research documents that simulate realistic pharmaceutical R&D outputs for Rheumatoid Arthritis target identification:

```
C:\MicrosoftDiscoveryLab\sample-data\internal\
├── rna-seq-results-study-001.md           ← RNA-seq differential expression (established RA vs. controls)
├── rna-seq-results-study-002.md           ← Follow-up RNA-seq (early vs. late RA + blood PBMCs)
├── gene-expression-profile-disease-X.md   ← Cross-tissue expression profiling + network analysis
├── internal-review-target-candidates.md   ← Internal target review committee document
├── screening-results-compound-library.md  ← High-throughput kinase inhibitor screening results
├── immunology-pathway-analysis.md         ← Internal immunology experiments & pathway mapping
└── prior-target-assessment-report.md      ← Previous manual target assessment (for comparison)
```

> **Note**: In a real scenario, these would be your organization's proprietary research papers (PDFs, DOCX), lab reports, and experimental data summaries. The sample files use Markdown format for portability, but the Bookshelf supports `.pdf`, `.docx`, `.pptx`, `.xlsx`, `.txt`, `.html`, and `.md`.

---

## 3.3 Create a Project

Before you can create Bookshelves or ingest documents, you need a **Project**. A Project is the organizational unit that scopes your agents, Bookshelves, tools, tasks, and notebooks into a single research initiative.

### Create the Project

1. Click the **Microsoft Discovery** icon in the Activity Bar.
2. In the **PROJECT** section at the top of the sidebar, click **Create New Project**.
3. Choose a name — for this lab use: `MicrosoftDiscoveryProject`
4. Select the workspace folder: `C:\MicrosoftDiscoveryLab\workspace`
5. The project appears in the sidebar with a **Files** node underneath it.

Once the project is created, Discovery opens the **"Set Up Your Project"** screen with two setup paths.

---

## 3.4 Set Up Your Project — Chat-Based Setup vs. Welcome Screen

The Discovery App offers two ways to configure your newly created project:

| | Option 1: Chat-Based Setup | Option 2: Go to Welcome |
|---|---|---|
| **Button** | **Start Chat** | **Open Welcome Screen** |
| **What it does** | Opens a chat session with the **project setup orchestrator agent** — an AI agent that walks you through configuration conversationally | Returns to the Discovery Welcome screen where you manually configure each component through the sidebar panels |
| **How you interact** | Natural language — describe your research goals and the orchestrator configures Bookshelves, tools, engines, and agents for you | Point-and-click — you create each component individually via the sidebar |
| **Best for** | First-time users, complex setups, or when you want AI-guided recommendations | Users who already know exactly what they want, or who prefer direct control over every setting |
| **Speed** | Faster — the orchestrator handles multiple configuration steps in a single conversation | More manual — each component (Bookshelf, tools, engine) is configured separately |
| **Discoverability** | The orchestrator suggests components you might not know about | You need to explore each sidebar panel yourself |
| **Result** | Same end state — a fully configured project | Same end state — a fully configured project |

> **Key point**: Both paths produce the same result. The difference is the journey: one is conversational and guided, the other is manual and self-directed.

---

## 3.5 Recommended Path — Using the Chat-Based Setup Orchestrator

For this lab, use **Option 1: Chat-based setup** to experience how Discovery agents work. The project setup orchestrator is itself a Discovery agent.

### Step 1: Start the Chat

1. On the "Set Up Your Project" screen, click the **Start Chat** button.
2. A Copilot Chat session opens on the right side of VS Code, pre-connected to the **project setup orchestrator agent**.

### Step 2: Describe Your Research Goal

Type a natural-language description of your project. For this lab, enter:

```
I'm setting up a drug target identification project for Rheumatoid Arthritis.
I have internal research data (RNA-seq results, screening data, immunology
reports) in a local folder that I want to index into a Bookshelf. I also
need access to public databases like PubMed, ClinicalTrials.gov, NCBI, and
UniProt. My goal is to identify, prioritize, and validate therapeutic targets.
```

### Step 3: Follow the Orchestrator's Guidance

The orchestrator agent will:

1. **Acknowledge your goal** and confirm the research domain (drug discovery / immunology).
2. **Ask about your data** — where your files are located, what format they're in.
3. **Propose a Bookshelf** — suggest creating `InternalResearchData` pointed at your data folder.
4. **Recommend tools** — suggest enabling biomedical MCP tools (PubMed, ClinicalTrials.gov, NCBI, UniProt).
5. **Offer to configure** — upon your confirmation, it creates the Bookshelf and begins ingestion.

Example exchange:

> **Orchestrator**: "I'll create a Bookshelf called InternalResearchData using GraphRAG indexing. Where are your internal research files located?"
>
> **You**: "They're in C:\MicrosoftDiscoveryLab\sample-data\internal\"
>
> **Orchestrator**: "Got it. I'll index those 7 documents. I'll also enable the BioMCP tools for PubMed, NCBI, and UniProt. Shall I proceed?"
>
> **You**: "Yes, go ahead."

### Step 4: Verify the Setup

After the orchestrator finishes, check the sidebar:
- **BOOKSHELF** section shows `InternalResearchData` with "7 docs · graphrag-zero" and a green health indicator
- **TOOLS** section shows your enabled MCP tools
- **Documents (7)** is expandable to see each ingested file
- **Provider** shows `graphrag-zero · healthy · 7 docs`

> **If you used Option 2 (Welcome Screen) instead**: You'll configure these same components manually in the steps below. Skip ahead to Section 3.7 to create the Bookshelf manually.

---

## 3.6 Alternative Path — Welcome Screen (Manual Setup)

If you clicked **Open Welcome Screen** instead of Start Chat, you configure each component individually through the Discovery sidebar:

1. **Bookshelf**: Click **+** in the BOOKSHELF panel → name it → select provider
2. **Tools**: Expand the TOOLS panel → enable desired MCP plugins
3. **Engines**: Configure any engine settings in the ENGINES panel
4. **Agents**: Optionally create custom agents in the AGENTS panel

This gives you direct control but requires you to know what each component is. The Chat-based path is recommended for first-time users because the orchestrator explains what it's setting up and why.

---

## 3.7 Create the "Internal Research" Bookshelf (Manual Path)

> **Skip this section if the orchestrator already created your Bookshelf in Step 3.5.**

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

## 3.8 Ingest Your Documents

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

## 3.9 Verify Ingestion

When ingestion finishes:
1. The shelf shows a **document count** and a **green health dot** ✅
2. Confirm via CLI:
   ```powershell
   dx bookshelf search <shelf-id> "gene expression" --workspace C:\MicrosoftDiscoveryLab\workspace
   ```

You should see search results with snippets from your ingested documents.

---

## 3.10 Query Your Bookshelf with Copilot

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
target identification for Rheumatoid Arthritis.
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

## 3.11 Understanding Global vs. Local Queries

The Bookshelf is optimized for **global queries** — questions that require reasoning across the entire corpus:

| Query Type | Example | Bookshelf Strength |
|------------|---------|-------------------|
| **Global** (synthesis) | "What are the main themes in our research?" | ⭐ Excellent |
| **Global** (implications) | "What are the implications for target X?" | ⭐ Excellent |
| **Global** (relationships) | "Describe the relationships between genes A, B, C" | ⭐ Excellent |
| **Local** (lookup) | "What was the exact p-value for gene X in study 001?" | ⚠️ Use with standard search |

---

## 3.12 CLI-Based Querying with Sources

For programmatic access or to get source citations:

```powershell
dx bookshelf ask "What are the most promising target candidates based on our experimental data?" --shelf <shelf-id> --sources --workspace C:\MicrosoftDiscoveryLab\workspace
```

The `--sources` flag returns the specific document chunks that grounded the answer.

---

## 3.13 Checkpoint

Before proceeding to Chapter 4, confirm:

- [ ] You created a Project (`MicrosoftDiscoveryProject`)
- [ ] You chose a setup path (Chat-based orchestrator or Welcome screen)
- [ ] You created a Bookshelf named `InternalResearchData`
- [ ] You successfully ingested 7 internal documents (sidebar shows "7 docs · graphrag-zero" with green health dot)
- [ ] You can query the Bookshelf from Copilot Chat and get cited answers
- [ ] You understand the difference between global and local queries
- [ ] You see how GraphRAG provides relationship-aware answers beyond simple keyword search

---

**Previous**: [← Chapter 2 — Interface Tour](chapter-02-interface-tour.md)
**Next**: [Chapter 4 — Connecting External Public Data Sources →](chapter-04-external-data-sources.md)
