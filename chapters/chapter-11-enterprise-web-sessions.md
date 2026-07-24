# Chapter 11: Microsoft Discovery Studio — The Web Research Environment

> **Goal**: Learn to use **Microsoft Discovery Studio** — the browser-based, collaborative research environment for Microsoft Discovery. Studio is built on Visual Studio Code for the Web and provides shared sessions, agents, knowledge bases, tools, and data management without any local installation.

---

## 11.1 What You Will Learn

- What Microsoft Discovery Studio is and how it differs from the local VS Code extension
- How to sign in and navigate the Studio interface (Home, Workspaces, Projects, Resources)
- How to create a project and configure agents
- How to run shared sessions — collaborative research conversations with AI agents
- How to manage Knowledge Bases, Tools, and Data from the web
- When to use Studio vs. the local Discovery App

---

## 11.2 What Is Microsoft Discovery Studio?

Microsoft Discovery Studio is the **web-based, unified research environment** for Microsoft Discovery. It runs entirely in the browser at:

> **https://studio.discovery.microsoft.com**

Key characteristics:

| Feature | Detail |
|---------|--------|
| **Built on** | Visual Studio Code for the Web |
| **Installation** | None — browser only (any modern browser) |
| **Authentication** | Microsoft Entra ID (SSO) |
| **Collaboration** | Shared sessions between team members on the same project |
| **Customization** | Full VS Code theming, layout, split editors |
| **Infrastructure** | Requires an Azure-deployed Discovery Workspace (Chapter 8) |

> **Key distinction**: The local Discovery App (Chapters 1-10) runs on your machine with local Bookshelves and is single-user. Discovery Studio runs in the cloud, backed by enterprise infrastructure, with shared sessions and team-wide Knowledge Bases.

---

## 11.3 Prerequisites

Before using Discovery Studio, you need:

1. **A deployed Discovery Workspace** — set up via Azure portal or Bicep (see Chapter 8)
2. **A Project** created within that workspace
3. **A persona role assignment** — either **Scientist** or **Platform Administrator**
4. **A chat model deployment** — an LLM deployed in your workspace (e.g., Claude Opus 4.6, GPT-4)

If you completed Chapter 8, you already have all of this in place.

---

## 11.4 Sign In to Discovery Studio

1. Open your browser and navigate to **https://studio.discovery.microsoft.com**
2. Sign in with your **Microsoft Entra ID** credentials (work or school account).
3. If you have access to multiple Entra tenants, click your profile icon (top-right) to select the correct tenant.
4. You land on the **Home** page.

> **Tip**: You can also find the Studio URL on your Workspace's overview page in the Azure portal.

---

## 11.5 The Home Page

After signing in, the Home page serves as your landing page:

```
┌─────────────────────────────────────────────────────────────────┐
│  Microsoft Discovery Studio                                      │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ Getting      │  │ Learn More   │  │ What's New   │          │
│  │ Started      │  │              │  │              │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
│                                                                  │
│  RECENT ACTIVITY                                                 │
│  • Project: RA-Target-Assessment (last accessed 2h ago)          │
│  • Project: PET-Decomposition (last accessed yesterday)          │
│  • Workspace: mdqlabs-dev-uksouth (3 projects)                   │
└─────────────────────────────────────────────────────────────────┘
```

| Section | Purpose |
|---------|---------|
| **Getting Started** | Launch the project creation flow |
| **Learn More** | Access documentation and introductory guides |
| **What's New** | Latest platform announcements and features |
| **Recent Activity** | Quick access to recently used projects and workspaces |

---

## 11.6 Navigation Sidebar

The sidebar is always visible on the left and provides access to all major areas:

### Top-Level Navigation

| Item | Purpose |
|------|---------|
| **Home** | Return to the landing page with getting-started cards and recent activity |
| **Workspaces** | View and manage all workspaces you have access to |
| **Projects** | Browse, manage, and open projects across all workspaces |

### Resources

| Item | Purpose |
|------|---------|
| **Tools** | Browse computational tools available to agents |
| **Knowledge** | Manage Bookshelves and Knowledge Bases (GraphRAG-indexed documents that provide agents with domain-specific context) |
| **Data** | Access and manage storage containers linked to your projects for input/output data |

---

## 11.7 Workspaces View

Select **Workspaces** from the sidebar to see all workspaces you have access to:

| Column | Description |
|--------|-------------|
| **Name** | Workspace name (click to open) |
| **Region** | Azure region where the workspace is deployed |
| **Resource Group** | The Azure resource group containing workspace resources |
| **Provisioning State** | Deployment state (Succeeded, Failed, Deleting) |
| **Created By** | Identity that created the workspace |
| **Created At** | Timestamp of creation |

Use the **Refresh** and **Filter** controls above the table to update or narrow results.

For our lab, you should see the workspace deployed in Chapter 8 (e.g., `ra-discovery-workspace` in your chosen region).

---

## 11.8 Opening a Project

When you open a project, Discovery Studio transitions into a **full Visual Studio Code for the Web environment**. This is where you conduct research.

The project view includes:

| Component | Location | Purpose |
|-----------|----------|---------|
| **Discovery tab** | Left sidebar | Lists quick actions and all shared sessions in the project |
| **Resources tab** | Left sidebar | Lists agents, tools, knowledge bases, and project storage |
| **Chat interface** | Main editor area | Natural-language conversation with agents |
| **Agent selector** | Chat input area | Dropdown or `@` mention to route messages to specific agents |
| **Preferences** | Settings | Customize agentic behavior to your style |
| **Agent logs** | Panel | Detailed view of prompts, responses, and tool call logs |
| **Breadcrumb bar** | Top of working area | Shows current location for quick navigation |

### Customizing Your Environment

Because Studio is built on VS Code for the Web, you get full customization:

- **Themes**: `File > Preferences > Color Theme` or `Ctrl+K Ctrl+T`
- **Layout**: Drag and drop panels, resize sidebar, split editors
- **Multiple sessions**: Open multiple shared sessions in separate tabs to compare results

---

## 11.9 Agents in Discovery Studio

### The Default Discovery Agent

Every project comes with a built-in **Discovery** agent that you can use immediately — no configuration needed. This is the same agent that powers the "What would you like to discover today?" prompt you saw in the screenshot.

### Creating a Custom Agent

To create a domain-specific agent (e.g., a TargetAssessmentAgent for RA):

1. Sign in to Discovery Studio and open your project.
2. In the **Resources** tab (left sidebar), find the **AGENTS (FOUNDRY)** section.
3. Click the **+** button next to AGENTS.
4. Fill in the agent details:
   - **Name**: `TargetAssessmentAgent`
   - **Description**: `Evaluates gene targets for Rheumatoid Arthritis by scoring genetic evidence, druggability, clinical precedent, and safety across internal and public data sources.`
5. Under **Chat model**, select your deployed model (e.g., Claude Opus 4.6).
6. Enter agent **Instructions**:
   ```
   You are a drug target assessment expert specializing in Rheumatoid 
   Arthritis. When asked to evaluate a gene target, score it on 6 
   dimensions (1-5 each): Genetic Evidence, Biological Rationale, 
   Druggability, Clinical Precedent, Internal Data Support, and 
   Safety/Selectivity. Always cite specific sources for each score.
   Use available Knowledge Bases and Tools to gather evidence.
   ```
7. Click **Create agent**.

The agent now appears in your Resources pane and can be selected in any shared session.

> **Note**: You can create multiple agents. To add more, click the **+** button next to Agents and select **Create new agent**.

---

## 11.10 Shared Sessions — Collaborative Research

Shared sessions are the primary research interface in Discovery Studio. They are:

- **Conversational** — chat with one or more agents using natural language
- **Collaborative** — shared between all users with access to the same project
- **Persistent** — sessions are saved and can be resumed later
- **Rich** — agents can generate HTML reports, data tables, calculations, and analyses

### Creating a Shared Session

There are two ways:

**Option A — Type in the Welcome page chat box:**
1. On the project Welcome page, type a prompt in the chat box.
2. Click **Send**.
3. A new shared session is automatically created and the agent responds.

**Option B — From the Discovery tab:**
1. In the Discovery tab (left sidebar), select **New shared session**.
2. A blank session opens in the editor area.

### Chatting in a Shared Session

1. Select an agent using the **agent selector dropdown** in the chat input area, or type `@AgentName` to route a message to a specific agent.
2. Enter your prompt and click **Send**.
3. The agent responds with text, citations, and potentially generated outputs (reports, tables).

### Session Contents

Each shared session contains:
- A conversational thread with one or more agents
- Agent-generated outputs (HTML reports, calculations, data analyses)
- A **summary section** that updates as the session progresses

---

## 11.11 Hands-On: Running a Research Session for RA

Let's walk through a complete research session in Discovery Studio for our Rheumatoid Arthritis project.

### Step 1: Open Your Project

1. From the Home page, click your RA project in Recent Activity (or navigate via Workspaces → your workspace → your project).
2. The project opens in the VS Code for the Web environment.

### Step 2: Start a Shared Session

On the Welcome page, type:

```
I want to evaluate TYK2 as a therapeutic target for Rheumatoid Arthritis.
Search available knowledge bases and public literature. Score it on genetic 
evidence, biological rationale, druggability, clinical precedent, internal 
data support, and safety/selectivity. Provide citations for each dimension.
```

Click **Send**. A new shared session is created.

### Step 3: The Discovery Agent Responds

The default Discovery agent will:
1. Search connected Knowledge Bases for TYK2 evidence
2. Query available tools (PubMed, NCBI, UniProt) for public data
3. Synthesize findings into a structured assessment
4. Score each dimension with explicit citations

### Step 4: Route to a Custom Agent

If you created the `TargetAssessmentAgent`, switch to it for a more focused analysis:

```
@TargetAssessmentAgent Compare TYK2 and BTK as therapeutic targets for 
Rheumatoid Arthritis. Which has stronger overall evidence? Present a 
side-by-side scoring table.
```

### Step 5: Ask Follow-Up Questions

Continue the conversation:

```
What are the main risks or counter-evidence against TYK2? Are there 
any failed clinical trials I should be aware of?
```

```
Search ClinicalTrials.gov for active Phase II/III trials targeting TYK2 
in autoimmune diseases. Summarize their status and preliminary results.
```

### Step 6: Review Generated Outputs

As the agent works, it may generate:
- Structured evidence tables (viewable inline)
- HTML reports (openable in a new tab)
- Citation lists with links to sources
- Scoring matrices

These outputs persist with the shared session and are visible to all team members with project access.

---

## 11.12 Managing Knowledge Bases in Studio

From the **Resources** tab → **Knowledge** section, you can:

| Action | How |
|--------|-----|
| View existing Knowledge Bases | Listed in the Knowledge section |
| Create a new Knowledge Base | Click **+** next to Knowledge |
| Upload documents | Select a Knowledge Base → add documents |
| Check indexing status | Status indicator next to each KB |

Knowledge Bases in Studio are the enterprise equivalent of local Bookshelves. They use the same GraphRAG indexing but run on cloud infrastructure with:
- GPU-accelerated indexing
- Team-wide access (RBAC-controlled)
- Larger document capacity
- Automated scheduled ingestion

### Knowledge Bases for Our RA Project

Create Knowledge Bases that mirror our local Bookshelves:

| Knowledge Base | Contents | Source |
|---------------|----------|--------|
| `InternalResearchData` | RNA-seq, screening, immunology reports | Upload from `sample-data/internal/` |
| `PublicLiterature` | Curated RA publications | Upload from `sample-data/public-literature/` |
| `ClinicalEvidence` | Trial summaries, outcomes data | Curated clinical documents |

---

## 11.13 Managing Tools in Studio

From the **Resources** tab → **Tools** section, browse computational tools available to agents:

- Tools are shared across all agents in the project
- Enable/disable tools to control what agents can access
- Tools include MCP-based connectors (PubMed, NCBI, UniProt) and computational tools

---

## 11.14 Managing Data in Studio

The **Data** section provides access to storage containers linked to your project:

- **Input data**: Upload datasets for analysis
- **Output data**: Access agent-generated results
- **Storage assets**: Browse all files in project storage containers

This connects to the Azure storage deployed in Chapter 8.

---

## 11.15 Discovery Studio vs. Local Discovery App

| Aspect | Local Discovery App (VS Code Extension) | Microsoft Discovery Studio (Web) |
|--------|----------------------------------------|----------------------------------|
| **Access** | VS Code on your machine | Any modern browser — no install |
| **Infrastructure** | Local — no Azure required | Requires Azure-deployed Workspace |
| **Collaboration** | Single user | Shared sessions across team |
| **Knowledge** | Local Bookshelves (on disk) | Cloud Knowledge Bases (RBAC, scalable) |
| **Agents** | Local agents | Foundry-backed agents with model selection |
| **Customization** | Full VS Code desktop | Full VS Code for the Web |
| **Data** | Local files | Azure storage containers |
| **Offline** | ✅ Works offline | ❌ Requires internet |
| **Best for** | Personal exploration, prototyping, development | Team research, collaboration, production workflows |

### Recommended Workflow

1. **Explore locally** (Chapters 1-7): Use the local Discovery App to prototype Bookshelves, test queries, and develop your research approach.
2. **Deploy infrastructure** (Chapter 8): Set up Azure Workspace, model deployments, and storage.
3. **Scale to Studio** (This chapter): Move validated knowledge to enterprise Knowledge Bases, create shared sessions for team collaboration, and use custom agents for repeatable workflows.

---

## 11.16 Quick Actions in Studio

The project Welcome page provides quick-action buttons for common workflows:

| Button | What It Does |
|--------|-------------|
| **Help me get started** | Guided onboarding — introduces available agents and suggests workflows |
| **Research** | Launches a focused research session across Knowledge Bases and tools |
| **Explore agents & capabilities** | Browse all available agents and their capabilities |
| **Plan** | Creates a structured research plan with milestones and tasks |

**Task 11.1** — Try each quick action:

```
1. Click [Help me get started] — follow the guided tour
2. Click [Research] → enter: "What is known about TYK2 inhibitors 
   for autoimmune diseases?"
3. Click [Explore agents & capabilities] — review available agents
4. Click [Plan] → enter: "Plan a 4-week target validation study 
   for TYK2 in Rheumatoid Arthritis"
```

---

## 11.17 Checkpoint

Before proceeding to Chapter 12, confirm:

- [ ] You can sign in to Discovery Studio at https://studio.discovery.microsoft.com
- [ ] You can navigate the sidebar (Home, Workspaces, Projects, Resources)
- [ ] You understand that Studio is built on VS Code for the Web
- [ ] You've opened a project and seen the Discovery tab and Resources tab
- [ ] You've created (or understand how to create) a custom agent
- [ ] You've started a shared session and chatted with an agent
- [ ] You understand the difference between local Bookshelves and cloud Knowledge Bases
- [ ] You know when to use Studio (collaboration, team workflows) vs. the local app (prototyping, offline)

---

**Previous**: [← Chapter 10 — Notebooks & Reporting](chapter-10-notebooks-reporting.md)
**Next**: [Chapter 12 — End-to-End Recap & Next Steps →](chapter-12-recap-next-steps.md)
