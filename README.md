# Microsoft Discovery Lab — Drug Target Identification & Validation

---

## What is Microsoft Discovery?

**Microsoft Discovery** is an AI-powered scientific research platform that brings together **agentic orchestration**, **advanced reasoning**, a **graph-based knowledge foundation**, and **high-performance computing (HPC)** to accelerate R&D across pharmaceuticals, materials science, chemicals, semiconductors, energy, and advanced manufacturing.

At its core, Microsoft Discovery mimics the scientific method: specialized AI agents reason over large amounts of knowledge, generate hypotheses, validate them across a vast search space, and store results in a persistent knowledge graph — all autonomously and with full explainability.

### How It Works — The Architecture in Detail

Microsoft Discovery is **not** a simple chatbot or RAG tool. It is a **multi-layered cognitive research platform** with a unique architecture:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE LAYER                                 │
│                                                                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌────────────────────────────┐  │
│  │  Discovery App   │  │ Discovery Studio │  │  dx CLI (terminal)         │  │
│  │  (VS Code ext.)  │  │ (Web portal)     │  │  (scripting/automation)    │  │
│  └────────┬─────────┘  └────────┬─────────┘  └────────────┬──────────────┘  │
│           │                     │                          │                 │
├───────────┼─────────────────────┼──────────────────────────┼─────────────────┤
│           └─────────────────────┼──────────────────────────┘                 │
│                                 ▼                                            │
│                    ┌──────────────────────┐                                  │
│                    │   DISCOVERY ENGINE    │ ← Cognitive orchestrator         │
│                    │   (Cognition Loop)    │                                  │
│                    │                       │                                  │
│                    │  Plan → Delegate →    │                                  │
│                    │  Execute → Validate → │                                  │
│                    │  Adapt → Store        │                                  │
│                    └──────────┬───────────┘                                  │
│                               │                                              │
├───────────────────────────────┼──────────────────────────────────────────────┤
│                               ▼                                              │
│  ┌──────────────┐  ┌──────────────────┐  ┌──────────────────────────────┐   │
│  │   AGENTS     │  │   BOOKSHELF      │  │   TOOLS                      │   │
│  │              │  │   (Knowledge)     │  │                              │   │
│  │ • Prompt     │  │                   │  │ • PubMed (BioMCP)            │   │
│  │   agents     │  │ • GraphRAG index  │  │ • ClinicalTrials.gov         │   │
│  │ • Custom     │  │ • Knowledge graph │  │ • NCBI Entrez                │   │
│  │   agents     │  │ • Vector DB       │  │ • UniProt                    │   │
│  │ • Discovery  │  │ • Citations       │  │ • RCSB PDB                   │   │
│  │   agent      │  │                   │  │ • Code Interpreter           │   │
│  └──────────────┘  └──────────────────┘  └──────────────────────────────┘   │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│                          COMPUTE LAYER                                        │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │   SUPERCOMPUTER (HPC Node Pools)                                     │    │
│  │   • Memory-optimized VMs (E-series: 20-96 vCPU, 160-768 GB RAM)     │    │
│  │   • GPU workloads for simulation and modeling                        │    │
│  │   • Containerized tool execution                                     │    │
│  │   • Auto-scaling based on workload                                   │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│                          DATA LAYER                                           │
│                                                                              │
│  ┌────────────────────┐  ┌────────────────────┐  ┌─────────────────────┐    │
│  │ Storage Containers  │  │ Azure Blob Storage  │  │ Azure SQL / AI      │    │
│  │ (discovery:// URIs) │  │ (your data stays    │  │ Search (KB index)   │    │
│  │                     │  │  in your account)   │  │                     │    │
│  └────────────────────┘  └────────────────────┘  └─────────────────────┘    │
└──────────────────────────────────────────────────────────────────────────────┘
```

### The Two Experiences — Discovery App vs. Discovery Services

Microsoft Discovery is offered in **two complementary experiences** that share the same core concepts and APIs:

| Dimension | Discovery App (Local) | Microsoft Discovery Services (Cloud) |
|-----------|----------------------|--------------------------------------|
| **What it is** | Self-contained Windows application (VS Code extension) | Enterprise Azure cloud platform |
| **Where it runs** | Your laptop — no Azure subscription needed | Azure infrastructure in your subscription |
| **Interface** | VS Code sidebar + GitHub Copilot Chat + `dx` CLI | Discovery Studio (web) + VS Code + `dx` CLI |
| **Who it's for** | Individual researchers, rapid prototyping | Teams, enterprise-scale production workflows |
| **Agents** | Custom agents via GitHub Copilot extensibility model | Prompt agents via Discovery Studio + Foundry Agent Service |
| **Knowledge** | Local Bookshelf (bundled ONNX embedding model) | Enterprise Bookshelf (Azure AI Search + Azure SQL + GraphRAG) |
| **Compute** | Limited by laptop CPU/RAM | Supercomputer node pools (memory-optimized VMs, GPUs) |
| **Orchestration** | Discovery Engine (local) + skill-based | Discovery Engine (cloud) with autonomous cognition |
| **Collaboration** | Single-user | Multi-user, role-based access, team sharing |
| **Models** | Flexible (supports non-Microsoft endpoints) | Workspace-level managed deployments (GPT-5.4, etc.) |
| **Cost** | $0 infrastructure | Azure consumption-based pricing |
| **Data governance** | Files on your machine in `.discovery/` | Enterprise-grade encryption, RBAC, private networking |
| **Graduation path** | Start here → validate → promote to cloud | Scale validated workflows to production |

> **Key insight**: The Discovery App is ideal for individual exploration and validation. Once you've proven a workflow, you promote it to Microsoft Discovery Services for team collaboration, HPC compute, and enterprise governance. **Your bookshelves, agents, and workflows carry forward without starting over.**

### How the Interface Works

**The Discovery App** installs as a VS Code extension. When you open VS Code, you see the Microsoft Discovery icon in the Activity Bar. Clicking it reveals a sidebar with panels for Workspace, Notebooks, Bookshelf, Tool Catalog, Tasks, and Engines. You interact with the platform in three ways:

1. **GitHub Copilot Chat** — Ask questions in natural language. Copilot uses your Bookshelves and Tools as grounding sources. Type `@AgentName` to route messages to specific agents.
2. **VS Code Sidebar** — Visual management of your knowledge bases, task graphs, engines, and notebooks. Toggle checkboxes to expose/hide capabilities from Copilot.
3. **`dx` CLI** — The same SDK exposed as a command-line tool for scripting, automation, and CI/CD pipelines.

**Microsoft Discovery Studio** (cloud) is a web-based research environment at `https://studio.discovery.microsoft.com/` where teams interact with agents through shared sessions, manage projects, create Bookshelves, and run the Discovery Engine for autonomous multi-day investigations.

### What Makes It Different from General-Purpose AI

| General-Purpose AI (ChatGPT, Copilot) | Microsoft Discovery |
|----------------------------------------|---------------------|
| Question → Answer → Done | Objective → Multi-step autonomous investigation → Validated results |
| Stateless conversations | Persistent knowledge graph that grows over time |
| Single model, no tools | Specialized agents + scientific tools + HPC compute |
| No data integration | Indexes YOUR data (proprietary + public) into a queryable graph |
| No auditability | Every claim traced to source with citations |
| Immediate interaction only | Discovery Engine runs autonomously for hours/days |
| Generic knowledge | Domain-specific scientific reasoning grounded in your corpus |

---

## Value Proposition — Why This Lab Matters

### The Problem This Lab Solves

In pharmaceutical R&D, **target identification and validation** is one of the most critical and time-consuming steps. A biologist needs to:

1. **Understand a gene's role in biology** — which takes weeks of literature review across PubMed, patents, and internal reports
2. **Aggregate internal datasets** — proprietary RNA-seq, high-throughput screening, experimental results scattered across lab notebooks and file servers
3. **Combine with external knowledge** — expensive datasets like ClinicalTrials.gov, public gene expression databases (GEO, Expression Atlas/XPR), NCBI, UniProt
4. **Reason across all sources** — connecting immunology pathways, expression patterns, clinical evidence, and druggability assessments
5. **Prioritize targets** — answering "what are the best targets to go after for this disease?" with explainable, defensible evidence
6. **Validate independently** — cross-referencing against independent datasets to confirm findings

**All of this happens manually today.** A single target assessment takes 4-6 weeks. Evidence is scattered. Reasoning is implicit and hard to audit. Decisions are presented in PowerPoints where the supporting logic is invisible.

### Business Outcomes from This Lab

| Outcome | Metric |
|---------|--------|
| **Time to target recommendation** | 4-6 weeks → 1-2 days |
| **Literature coverage** | Partial (what one person can read) → Comprehensive (entire corpus indexed) |
| **Reproducibility** | Low (implicit reasoning) → High (every step recorded with citations) |
| **Auditability** | Difficult (buried in emails/meetings) → Complete (knowledge graph + notebook) |
| **Evidence aggregation** | Manual copy-paste across websites → Automated multi-source synthesis |
| **Explainability for leadership** | "I think this is a good target" → "Here are 15 supporting citations across 4 data sources" |
| **Knowledge preservation** | Lost when people leave → Persistent in organizational knowledge graph |
| **Cost of scaling** | Linear (more people = more cost) → Sublinear (engine handles volume) |

### What You Will Learn

By completing this lab, you will be able to:

1. **Set up Microsoft Discovery** — both the local app (free, no cloud) and the enterprise Azure platform
2. **Build AI-searchable knowledge bases** from proprietary research data using GraphRAG
3. **Connect live scientific APIs** (PubMed, ClinicalTrials.gov, NCBI, UniProt) as tools
4. **Structure complex research** as directed-acyclic task graphs with dependencies and validation criteria
5. **Create and configure agents** — both locally and in Discovery Studio — tailored to target assessment
6. **Run the Discovery Engine** for autonomous multi-step target prioritization with explainable results
7. **Aggregate and synthesize** internal experimental data with external public knowledge
8. **Validate targets** against independent evidence sources for scientific rigor
9. **Understand the enterprise architecture** — supercomputers, node pools, workspaces, projects, and storage
10. **Document and publish** findings in structured notebooks with full traceability

---

## Lab Scenario

> A biologist on a pharmaceutical R&D team needs to identify, prioritize, and validate therapeutic targets for a specific disease. The workflow involves aggregating internal experimental datasets (RNA-seq, proprietary screening results) with external public knowledge (ClinicalTrials.gov, PubMed, gene expression databases) to understand the biology of candidate genes, rank them by evidence strength, and build an explainable case for which targets to pursue.

**Specific activities**:
- Target identification and target validation
- Aggregating internal datasets + external knowledge (including costly datasets like ClinicalTrials.gov)
- Understanding a gene's role in biology (immunology pathways, expression patterns)
- Experimenting and validating targets against independent evidence
- Target prioritization — "What are the best targets to go after for this disease?"
- Connecting public data (PubMed, GEO, XPR/Expression Atlas, RNA-seq databases) with internal datasets
- Leveraging HPC as a game-changer for high-dimensional data
- Ensuring explainability — every recommendation traceable to evidence

---

## Lab Overview

### Track A: Discovery App (Local — No Azure Required)

| Chapter | Title | Focus |
|---------|-------|-------|
| 1 | [Environment Setup & Installation](chapters/chapter-01-installation.md) | Install Microsoft Discovery app, create workspace, verify dependencies |
| 2 | [Navigating the Discovery Interface](chapters/chapter-02-interface-tour.md) | Sidebar tour, understanding Bookshelves, Tools, Tasks, Engines, Notebooks |
| 3 | [Building Your First Bookshelf — Internal Data](chapters/chapter-03-bookshelf-internal-data.md) | Ingest proprietary RNA-seq data, experimental reports, internal publications |
| 4 | [Connecting External Public Data Sources](chapters/chapter-04-external-data-sources.md) | Enable PubMed, ClinicalTrials.gov, NCBI, UniProt MCP tools; query public knowledge |
| 5 | [Target Identification — Aggregating Public + Internal Knowledge](chapters/chapter-05-target-identification.md) | Combine Bookshelves with external tools to identify candidate genes |
| 6 | [Target Prioritization — Ranking & Explainability](chapters/chapter-06-target-prioritization.md) | Use Discovery Engine to score, rank, and explain target choices |
| 7 | [Target Validation — Cross-Referencing & Evidence Synthesis](chapters/chapter-07-target-validation.md) | Validate top targets against clinical evidence, immunology data, internal experiments |

### Track B: Enterprise Microsoft Discovery (Azure Infrastructure)

| Chapter | Title | Focus |
|---------|-------|-------|
| 8 | [Enterprise Infrastructure Deployment](chapters/chapter-08-enterprise-infrastructure.md) | Deploy workspace, supercomputer, node pools, projects, and agents on Azure |
| 9 | [High-Dimensional Data Analysis & HPC Integration](chapters/chapter-09-hpc-and-analysis.md) | Leverage supercomputer node pools for large-scale analysis |
| 10 | [Capturing Findings — Notebooks & Reporting](chapters/chapter-10-notebooks-reporting.md) | Document findings in Discovery Notebooks, publish reports |
| 11 | [End-to-End Recap & Next Steps](chapters/chapter-11-recap-next-steps.md) | Review the complete workflow, cost considerations, scaling to enterprise |

---

## Understanding Projects and Agents

### Projects — The Organizational Unit

A **Project** is the organizational unit within a Microsoft Discovery workspace where you bring together agents, tools, knowledge bases, storage containers, and shared sessions into a single, access-controlled boundary. Every research activity happens within the context of a project.

```
Subscription
└── Resource Group
    ├── Supercomputer
    │   └── Node Pools
    ├── Bookshelf
    │   └── Knowledge Bases
    └── Workspace
        ├── Chat Model Deployments (shared across projects)
        └── Project
            ├── Agents (prompt agents)
            │   ├── Knowledge Bases (attached for grounding)
            │   └── Tools (attached for capabilities)
            ├── Storage Containers
            └── Shared Sessions
                └── Conversations with agents
```

**Key concepts**:
- **Workspace** provides the shared infrastructure (networking, supercomputers, managed identities, chat model deployments)
- **Projects** consume workspace resources while maintaining isolated agents, sessions, and data
- **One project per research initiative** — keeps agents, knowledge, and data isolated and manageable

### Agents — The Fundamental Building Blocks

**Discovery Agents** are AI assistants that execute scientific research tasks on your behalf. They build on Foundry Agent Service with scientific discovery extensions.

| Aspect | Detail |
|--------|--------|
| **What they are** | AI-powered systems that perform specific scientific tasks using LLMs, tools, and knowledge bases |
| **How you create them** | Discovery Studio UI (cloud) or GitHub Copilot skills (local app) |
| **How you invoke them** | Type `@AgentName` in a conversation to route messages to specific agents |
| **What they can use** | Chat models, Bookshelf knowledge bases, scientific tools (PubMed, UniProt, etc.), code interpreter |
| **Orchestration** | Discovery Engine coordinates multiple agents for multi-step investigations |
| **Versioning** | Every save creates a new immutable version with full history |

**Agent types in this lab**:
- **Default Discovery Agent** — comes with every project, general-purpose scientific reasoning
- **Custom Prompt Agents** — you'll create specialized agents (e.g., "Target Prioritization Agent") with domain-specific instructions, attached knowledge bases, and tools

---

## Prerequisites

### Track A (Chapters 1-7) — Local Discovery App
- **Operating System**: Windows 11
- **GitHub Copilot**: Active subscription
- **Sample Data**: Internal research documents (PDFs, CSVs) — sample files provided in `sample-data/`
- **Cost**: $0 — everything runs on your laptop

### Track B (Chapters 8-11) — Enterprise Discovery on Azure
- **Azure subscription** with Microsoft Discovery access enabled
- **Permissions**: Owner or Contributor on a resource group
- **Region**: East US, Sweden Central, or UK South (supported production regions)
- **Quotas**: Azure OpenAI, VM SKUs (E-series), and Microsoft Foundry quota in your region
- **Cost**: Azure consumption-based (VMs, storage, Azure OpenAI TPM)

---

## Reference Links

| Resource | URL |
|----------|-----|
| What is Microsoft Discovery? | https://learn.microsoft.com/en-us/azure/microsoft-discovery/overview-what-is-microsoft-discovery |
| Key Scenarios | https://learn.microsoft.com/en-us/azure/microsoft-discovery/overview-key-scenarios |
| Infrastructure Quickstart (Azure Portal) | https://learn.microsoft.com/en-us/azure/microsoft-discovery/quickstart-infrastructure-portal |
| Projects & Shared Sessions | https://learn.microsoft.com/en-us/azure/microsoft-discovery/concept-projects-investigations |
| Discovery Agents | https://learn.microsoft.com/en-us/azure/microsoft-discovery/concept-discovery-agent |
| Discovery App Quick Start (GitHub) | https://github.com/microsoft/discovery/blob/main/docs/discovery-app/quickstart.md |
| Discovery GitHub Repository | https://github.com/microsoft/discovery |
| Bookshelf & Knowledge Bases | https://learn.microsoft.com/en-us/azure/microsoft-discovery/concept-bookshelf-knowledge-bases |
| Discovery Engine | https://learn.microsoft.com/en-us/azure/microsoft-discovery/concept-discovery-engine |
| Create Agents | https://learn.microsoft.com/en-us/azure/microsoft-discovery/how-to-agent-creation |
| Discovery Studio | https://studio.discovery.microsoft.com/ |
