# Chapter 8: High-Dimensional Data Analysis & HPC Integration

> **Goal**: Understand how Microsoft Discovery connects to High-Performance Computing (HPC) for large-scale analyses, how to handle high-dimensional datasets (multi-omics, large RNA-seq panels), and the cost/architecture implications. This chapter bridges the local Discovery app to the enterprise cloud platform.

---

## 8.1 What You Will Learn

- Why HPC integration is a game-changer for target assessment
- How the enterprise Discovery platform connects to supercomputer node pools
- How storage containers and storage assets provide the data layer
- How to structure large-scale analysis workflows
- Cost considerations for local vs. cloud analysis
- When to scale from the Discovery app to Microsoft Discovery services

---

## 8.2 Why HPC Matters for Target Discovery

High-dimensional biological data is growing exponentially:

| Data Type | Size Challenge | Analysis Need |
|-----------|---------------|---------------|
| Bulk RNA-seq | Dozens of samples, ~20K genes per sample | Differential expression, pathway enrichment |
| Single-cell RNA-seq | Millions of cells × thousands of genes | Cell-type-specific expression, trajectory analysis |
| Proteomics | Thousands of proteins across conditions | Protein-level validation of transcriptomic hits |
| Spatial transcriptomics | Tissue-level spatial gene expression | Target localization in disease tissue |
| Multi-omics integration | All of the above, combined | Cross-platform target validation |
| Large compound libraries | Millions of compound-target interactions | Virtual screening for druggability |

**A laptop can't handle this.** The Discovery app running locally is ideal for literature review, knowledge aggregation, and target identification. But when you need to:
- Run large-scale simulations
- Process multi-omics datasets
- Perform high-throughput virtual screening
- Execute computationally intensive analyses

...you need the enterprise Microsoft Discovery platform with HPC.

---

## 8.3 Architecture: Local App vs. Enterprise Platform

```
┌─────────────────────────────────────────────────────────┐
│  Discovery App (Chapters 1-7)                           │
│  ✅ Runs on your laptop                                 │
│  ✅ No Azure subscription needed                        │
│  ✅ Bookshelf, Tools, Tasks, Engine, Notebooks          │
│  ⚠️ Limited by local compute power                      │
│  ⚠️ Single-user                                         │
├─────────────────────────────────────────────────────────┤
│  Microsoft Discovery Services (This chapter+)           │
│  ☁️ Runs on Azure                                       │
│  ☁️ Supercomputer node pools (memory-optimized VMs)     │
│  ☁️ Enterprise Bookshelf with Azure AI Search           │
│  ☁️ Team collaboration                                  │
│  ☁️ Discovery Studio web interface                      │
│  ☁️ Full Discovery Engine with autonomous cognition     │
└─────────────────────────────────────────────────────────┘
```

> **Key point**: Your Bookshelves, tools, and workflows carry forward from the app to the enterprise platform without starting over.

---

## 8.4 Enterprise Infrastructure Overview

> **Prerequisite**: This section requires an Azure subscription. If you don't have one, read through this chapter as reference material.

### The Enterprise Stack

When deployed on Azure, Microsoft Discovery includes:

| Component | Purpose | Azure Service |
|-----------|---------|---------------|
| **Workspace** | Top-level container for projects | Microsoft.Discovery/workspaces |
| **Project** | Logical grouping of agents, tools, data | Resource within workspace |
| **Supercomputer Node Pool** | HPC compute for tools and indexing | Memory-optimized VMs (E-series) |
| **Bookshelf** | Enterprise knowledge base with GraphRAG | Azure AI Search + Azure SQL |
| **Storage Containers** | Data layer — references to blob storage | Azure Blob Storage |
| **Discovery Studio** | Web UI for research | https://studio.discovery.microsoft.com |
| **Discovery Engine** | Autonomous cognition | Agent orchestration layer |

### Storage Architecture

Microsoft Discovery uses a two-level storage hierarchy:

```
Resource Group
├── Storage Container (references Azure Blob Storage account)
│   ├── Storage Asset (specific blob path — input data)
│   ├── Storage Asset (output from agent analysis)
│   └── Storage Asset (results from HPC tool runs)
└── Storage Container (another storage account)
    └── Storage Asset (different dataset)
```

**Key design principle**: Creating a storage container **registers** an existing Azure Blob Storage account — it doesn't move or copy data. Your data stays in your storage account under your control.

---

## 8.5 Connecting Internal Data at Enterprise Scale

In the enterprise platform, instead of ingesting local files, you:

### Step 1: Upload Data to Azure Blob Storage

```
Azure Blob Storage Account
└── Container: target-research-data
    ├── rna-seq/
    │   ├── study-001-differential-expression.csv
    │   ├── study-002-differential-expression.csv
    │   └── single-cell-atlas.h5ad
    ├── proteomics/
    │   └── mass-spec-results.csv
    ├── literature/
    │   ├── curated-papers/
    │   └── internal-reports/
    └── screening/
        └── compound-library-results.csv
```

### Step 2: Create Discovery Storage Container

Register the blob storage with your Discovery workspace via the Azure portal or REST API.

### Step 3: Create Storage Assets

Point to specific data paths within the storage account. Each path becomes addressable as:
```
discovery://resources/{storageContainerName}/paths/{blobPath}
```

### Step 4: Agents and Tools Access the Data

Agents use built-in tools like `GetResourceContext` and `PreviewResource` to discover and read files through `discovery://` URIs.

---

## 8.6 Enterprise Bookshelf: Scaling Knowledge Bases

The enterprise Bookshelf supports different index sizes:

| Size | Data Volume | Compute (Indexing) | Compute (Search) |
|------|-------------|-------------------|-------------------|
| **Small** | ~200 MB text | Standard_E20s_v6 (20 vCPU, 160 GB) | E4 Container App |
| **Medium** | ~500 MB text | Standard_E64s_v6 (64 vCPU, 512 GB) | E8 Container App |
| **Large** | ~1 GB text | Standard_E96s_v6 (96 vCPU, 768 GB) | E16 Container App |

### Enterprise Bookshelf Creation Steps (Reference)

1. Create a Bookshelf resource in Azure Portal
2. Configure networking (private endpoints)
3. Configure encryption (Microsoft-managed or customer-managed keys)
4. Configure managed identity for blob storage access
5. Create storage container and storage asset
6. Create a Knowledgebase in Discovery Studio
7. Start indexing (runs on supercomputer node pool)
8. Track indexing progress

---

## 8.7 HPC Tool Runs for Large-Scale Analysis

In the enterprise platform, computational tools run on supercomputer node pools:

**Example**: Running a large-scale gene expression meta-analysis:

```
Task: "Perform differential expression meta-analysis across all 
internal RNA-seq datasets for [DISEASE]"

Engine orchestrates:
1. Agent reads input data via storage asset
2. Tool runs on supercomputer (memory-optimized VM)
3. Analysis executes in containerized environment
4. Results written to output storage asset
5. Agent reads and summarizes results
6. Discovery Engine validates against task criteria
```

---

## 8.8 Cost Considerations

| Dimension | Discovery App (Local) | Discovery Services (Cloud) |
|-----------|----------------------|---------------------------|
| **Infrastructure** | $0 — runs on your laptop | Azure VM costs for node pools |
| **Bookshelf Indexing** | Local compute, limited by RAM | Cloud compute, scalable |
| **Model costs** | Bundled local embedding model (free) | Azure OpenAI deployment costs (TPM-based) |
| **Data storage** | Local disk | Azure Blob Storage costs |
| **Collaboration** | Single user | Multi-user, team access |
| **Best for** | Individual researcher, initial exploration | Team workflows, large datasets, production use |

### Cost Optimization Tips

1. **Start local**: Use the Discovery app for initial target identification (Chapters 1-7) — $0 infrastructure cost
2. **Scale selectively**: Only move to enterprise when you need HPC or team collaboration
3. **Right-size node pools**: Use Small Bookshelf (~200 MB) unless you truly need larger
4. **Manage model quota**: Increase Azure OpenAI TPM during indexing, reduce after
5. **Use storage tiers**: Archive completed analysis results to cool/archive storage

---

## 8.9 Lab Exercise: Planning Your HPC Workflow

Even without an Azure subscription, you can plan the workflow:

**Task 8.1** — Design the HPC analysis plan:
```
Using the tasks tool, create a plan for scaling our target validation 
to enterprise Microsoft Discovery with HPC:

1. "Data Migration Plan" - List all datasets that need to move to 
   Azure Blob Storage, estimate total size
2. "Enterprise Bookshelf Design" - Determine optimal Bookshelf size 
   (Small/Medium/Large) based on our document corpus
3. "Compute Requirements" - Estimate supercomputer node pool 
   requirements for our analyses
4. "Team Access Plan" - Define who needs access and with what roles
5. "Cost Estimate" - Estimate monthly Azure costs for our workflow
```

**Task 8.2** — Evaluate the HPC value:
```
Compare what we achieved locally in Chapters 1-7 with what the 
enterprise platform would additionally enable:

1. What analyses couldn't we run locally?
2. What dataset sizes would require HPC?
3. What collaboration workflows would benefit from the cloud?
4. What is the estimated ROI of moving to enterprise?
```

---

## 8.10 Checkpoint

Before proceeding to Chapter 9, confirm:

- [ ] You understand the difference between the local Discovery app and enterprise Discovery services
- [ ] You understand the storage container/asset architecture for enterprise data
- [ ] You know how enterprise Bookshelf sizing works (Small/Medium/Large)
- [ ] You understand how HPC node pools enable large-scale analysis
- [ ] You have a cost model for local vs. enterprise deployment
- [ ] You have a plan for which parts of your workflow would benefit from HPC

---

**Previous**: [← Chapter 7 — Target Validation](chapter-07-target-validation.md)
**Next**: [Chapter 9 — Notebooks & Reporting →](chapter-09-notebooks-reporting.md)
