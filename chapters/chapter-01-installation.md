# Chapter 1: Environment Setup & Installation

> **Goal**: Install the Microsoft Discovery app on your Windows machine, create your first workspace, and verify all dependencies are healthy.

---

## 1.1 What You Will Learn

- What Microsoft Discovery is and how it fits into drug-target R&D
- How to download and install the Discovery app
- How to create a workspace and verify dependencies
- How to use the `dx` CLI for health checks

---

## 1.2 Background: Why Microsoft Discovery for Target Assessment?

Traditional drug target identification is a manual, time-intensive process:

| Traditional Step | Pain Point | Discovery Solution |
|-----------------|------------|-------------------|
| Literature review across PubMed, patents, internal reports | Takes weeks, easy to miss relevant papers | AI-assisted literature review via Bookshelf (GraphRAG) — minutes instead of weeks |
| Aggregating internal experimental data (RNA-seq, screening) | Siloed in lab notebooks, spreadsheets, file servers | Bookshelf indexes your internal documents into a searchable knowledge graph |
| Querying public databases (ClinicalTrials.gov, NCBI, UniProt) | Manual searches across multiple websites | Curated MCP tool plugins query these APIs directly from your AI assistant |
| Synthesizing findings across sources | Requires deep domain expertise, days of effort | Discovery Engine autonomously reasons across all your data sources |
| Explaining and justifying target selection | Implicit reasoning, hard to audit | Explainable outputs with citations back to source documents |

---

## 1.3 Download & Install Microsoft Discovery

### Step 1: Download the Installer

1. Navigate to the Discovery releases page:
   👉 **https://github.com/microsoft/discovery/releases/latest**
2. Download the `DiscoveryExpressSetup-x.y.z.exe` asset (or `Discovery-app-*-win-x64.exe` for x64, `Discovery-app-*-win-arm64.exe` for ARM64).

### Step 2: Run the Installer

1. Double-click the downloaded `.exe` file.
2. Follow the installer prompts — no Azure subscription or cloud credentials are needed.
3. The installer is self-contained; it includes all required runtimes.

### Step 3: Verify Installation

1. Open **VS Code**.
2. Look for the **Microsoft Discovery icon** in the Activity Bar (the vertical strip on the left side of the VS Code window).
3. Click it. The sidebar should open showing the Discovery tree view.

> **Troubleshooting**: If you don't see the icon, restart VS Code. If it's still missing after a restart, re-run the installer.

---

## 1.4 Create Your First Workspace

A *workspace* is any folder on disk where Microsoft Discovery stores its state in a `.discovery/` subdirectory.

### Using the VS Code Sidebar

1. Click the **Microsoft Discovery icon** in the Activity Bar.
2. In the **Workspace** panel, click **Create a new Workspace**.
3. Select a folder — for this lab, create or choose:
   ```
   C:\MicrosoftDiscoveryLab\workspace
   ```
4. Wait for all dependencies to finish installing. Each dependency shows a progress bar.

### Using the dx CLI (Alternative)

Open a terminal (PowerShell) and run:

```powershell
dx init --workspace C:\MicrosoftDiscoveryLab\workspace
```

### Verify the Workspace Structure

After creation, your folder should look like this:

```
C:\MicrosoftDiscoveryLab\workspace\
└── .discovery/
    ├── config.json       ← engines, models, providers
    ├── bookshelves/      ← indexed knowledge (empty for now)
    ├── tasks/            ← task graph (empty for now)
    ├── notebooks/        ← findings and write-ups
    └── engines/          ← Discovery Engine state
```

---

## 1.5 Run a Health Check

Run the diagnostic command to verify everything is ready:

```powershell
dx doctor --workspace C:\MicrosoftDiscoveryLab\workspace
```

This command walks through every dependency, model route, and provider and reports what's healthy and what needs attention.

**Expected output**: All core dependencies should show green. LLM-backed features may show a warning if no model route is configured — that's fine for now. The bundled local embedding model (`all-MiniLM-L6-v2` ONNX) provides semantic search out of the box.

---

## 1.6 (Optional) Configure an Azure OpenAI Model

If you have an Azure OpenAI endpoint, you can configure it for higher-quality responses:

```powershell
dx workspace config llm show --workspace C:\MicrosoftDiscoveryLab\workspace
```

To set your Azure OpenAI endpoint:

```powershell
dx workspace config llm set-azure-openai --endpoint <your-endpoint> --deployment <your-deployment> --workspace C:\MicrosoftDiscoveryLab\workspace
```

> **Note**: This is optional. All core functionality (Bookshelf indexing, semantic search, task management) works without an API key using the bundled local models.

---

## 1.7 Checkpoint

Before proceeding to Chapter 2, confirm:

- [ ] Microsoft Discovery app is installed and the icon appears in the VS Code Activity Bar
- [ ] A workspace has been created at `C:\MicrosoftDiscoveryLab\workspace`
- [ ] `dx doctor` reports healthy status for core dependencies
- [ ] The `.discovery/` folder exists in your workspace

---

**Next**: [Chapter 2 — Navigating the Discovery Interface →](chapter-02-interface-tour.md)
