# Chapter 2: Navigating the Discovery Interface

> **Goal**: Understand the Discovery sidebar, learn what each panel does, and how Bookshelves, Tools, Tasks, Engines, and Notebooks work together for target discovery.

---

## 2.1 What You Will Learn

- The layout of the Discovery sidebar in VS Code
- The purpose of each panel: Workspace, Notebooks, Bookshelf, Tool Catalog, Tasks, Engines
- How checkboxes expose capabilities to GitHub Copilot
- How to read health indicators

---

## 2.2 The Sidebar Tree View

Click the **Microsoft Discovery icon** in the Activity Bar. The sidebar displays:

```
◆ MICROSOFT DISCOVERY
 ├── 📁 Workspace        ← files in your project, with Discovery-aware actions
 ├── 📓 Notebooks         ← Jupyter, Wiki, and Brief notebooks
 ├── 📚 Bookshelf         ← your indexed knowledge bases
 ├── 🔧 Tool Catalog      ← MCP servers and agent plugins
 ├── 📋 Tasks             ← your task graph (DAG)
 └── 🤖 Engines           ← configured Discovery Engines
```

---

## 2.3 Panel-by-Panel Walkthrough

### 📁 Workspace Panel

Your lab folder with Discovery-aware context menus. Right-click any file to see Discovery-specific actions (e.g., "Ingest to Bookshelf").

**Task 2.1**: Right-click a file in the Workspace panel and explore the available context menu options.

### 📚 Bookshelf Panel

This is where your indexed knowledge lives. Each Bookshelf is a local, multi-strategy index (vector, keyword, graph) of documents you've ingested.

Key concepts:
- **Bookshelf**: A searchable index of your documents
- **Provider**: The backend algorithm for indexing and search (default works out of the box)
- **Checkbox**: Ticking the checkbox next to a Bookshelf exposes it to GitHub Copilot as a tool

**Task 2.2**: Look at the Bookshelf panel. It should be empty. We will populate it in Chapter 3.

### 🔧 Tool Catalog Panel

Curated MCP (Model Context Protocol) servers that connect Discovery to external scientific APIs:

| Category | Available Tools |
|----------|----------------|
| Biomedical & Life Sciences 🧬 | BioMCP (PubMed + clinical trials), RCSB PDB, UniProt, NCBI Entrez |
| Physical Sciences & Engineering ⚛️ | NASA PDS, OPTIMADE |
| Scientific Literature & Search 📄 | arXiv, bioRxiv / medRxiv |

**Task 2.3**: Click on the Tool Catalog panel. Note the available tools. We will enable specific ones in Chapter 4.

### 📋 Tasks Panel

Tasks in Discovery are **not a flat checklist** — they are a **directed-acyclic graph (DAG)** with:
- Explicit dependencies between tasks
- A real status state machine (`new` → `executing` → `executionDone` → `complete`)
- First-class queries like "what's ready?" and "what's blocked?"

| Status | Meaning |
|--------|---------|
| `new` | Created, not started |
| `executing` | Actively in progress |
| `executionDone` | Work finished, awaiting verification |
| `complete` | Verified done |
| `onHold` | Paused intentionally |
| `failed` | Tried, didn't work |
| `flaggedHuman` | Needs attention from a person |
| `flaggedAi` | Needs attention from an AI |

**Task 2.4**: Open the Tasks panel and confirm it's empty. In Chapters 5-7, we will build task graphs for target identification and prioritization.

### 🤖 Engines Panel

Discovery Engines are long-running autonomous agents that use your Bookshelves, tools, and agents to make multi-step progress. They run in the background like a tireless research assistant.

Three autonomy levels:

| Level | Behavior |
|-------|----------|
| **Supervised** | Engine proposes each tool call and waits for your approval (recommended for first use) |
| **Full** | Engine uses any allowed tool without asking |
| **Locked** | Engine can only use a strict whitelist of tools |

**Task 2.5**: Open the Engines panel. No engines are configured yet — we will set one up in Chapter 6.

### 📓 Notebooks Panel

Where research content lives — findings, decisions, hypotheses, and write-ups:

| Format | Best For |
|--------|----------|
| **Jupyter** | Personal lab journal with executable cells |
| **Wiki** | Team-shared project knowledge with themed pages |
| **Brief** | Executive summary that auto-updates as work progresses |

---

## 2.4 Understanding the Three Surfaces

Microsoft Discovery provides three equivalent surfaces — all powered by the same SDK:

| Surface | How | Best For |
|---------|-----|----------|
| **VS Code Extension** | Click through the sidebar | Visual interaction, browsing results |
| **dx CLI** | Type commands in a terminal | Scripting, automation, batch operations |
| **AI Assistant (Copilot Chat)** | Natural language in Copilot Chat | Conversational research, querying |

Anything you do in one surface can be done in the others.

**Task 2.6**: Open a terminal and run the following to confirm the CLI works:
```powershell
dx bookshelf list --workspace C:\MicrosoftDiscoveryLab\workspace
```
Expected: Empty list (no Bookshelves yet).

---

## 2.5 Mapping Discovery Concepts to Our Use Case

Here's how each Discovery concept maps to the target identification workflow:

| Discovery Concept | Role in Our Lab |
|-------------------|-----------------|
| **Bookshelf — "Internal Data"** | Indexes our proprietary RNA-seq results, gene expression data, internal experimental reports, and publications |
| **Bookshelf — "Public Literature"** | Indexes curated public papers from PubMed, review articles on our disease area |
| **Tool Catalog — BioMCP** | Live queries to PubMed and ClinicalTrials.gov during conversations |
| **Tool Catalog — NCBI Entrez** | Gene/protein/nucleotide database lookups |
| **Tool Catalog — UniProt** | Protein sequence and function information |
| **Tasks** | Structured DAG of target-identification steps with dependencies |
| **Discovery Engine** | Autonomous multi-step target prioritization across all sources |
| **Notebook** | Documented findings, ranked target list, evidence summaries |

---

## 2.6 Checkpoint

Before proceeding to Chapter 3, confirm:

- [ ] You can navigate all six panels in the Discovery sidebar
- [ ] You understand the checkbox mechanism for exposing Bookshelves to Copilot
- [ ] You can run `dx` CLI commands from a terminal
- [ ] You understand the mapping between Discovery concepts and the target identification workflow

---

**Previous**: [← Chapter 1 — Installation](chapter-01-installation.md)
**Next**: [Chapter 3 — Building Your First Bookshelf →](chapter-03-bookshelf-internal-data.md)
