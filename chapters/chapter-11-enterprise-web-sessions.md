# Chapter 11: Microsoft Discovery Enterprise — Web Sessions Interface

> **Goal**: Explore the Discovery Enterprise web interface — a cloud-hosted, session-based research environment for collaborative scientific investigation. Understand how it complements the VS Code extension and when to use each.

---

## 11.1 What You Will Learn

- What the Discovery Enterprise web interface is and how it differs from the VS Code extension
- How to create and manage research sessions
- How Collections organize knowledge across projects
- How to use Agents and Knowledge Bases from the web UI
- How to leverage quick-action workflows (Research, Plan, Explore)
- When to use the web interface vs. the VS Code extension

---

## 11.2 Accessing the Discovery Enterprise Interface

The Discovery Enterprise web interface is available to organizations with the Enterprise tier. It provides a browser-based research experience backed by the same AI capabilities as the VS Code extension, but designed for session-based collaborative workflows.

### How to Access

1. Navigate to your organization's Discovery Enterprise URL (provided by your admin after the Chapter 8 Azure deployment).
2. Sign in with your Microsoft Entra ID credentials.
3. You'll land on the **Discovery Home** screen.

### What You See

The interface has three main areas:

| Area | Location | Purpose |
|------|----------|---------|
| **Sidebar** | Left panel | Navigation: Home, Project Data, AI Capabilities, Jobs, Collections |
| **Session Panel** | Center | Interactive research workspace — where you describe goals and get results |
| **Session Artifacts** | Bottom | Outputs generated during your session (documents, figures, tables) |

---

## 11.3 The Home Screen

When you first open Discovery Enterprise, you see:

```
┌─────────────────────────────────────────────────────────────────┐
│  Microsoft Discovery                                             │
│  What would you like to discover today?                          │
│                                                                  │
│  ● Enterprise    Learn more                                      │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ 🔗 Add inputs and context...                                ││
│  │ Describe what you want to achieve...                        ││
│  │                                           ▶ [Model: Claude] ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                  │
│  [Help me get started] [Research] [Explore agents] [Plan] [...]  │
│                                                                  │
│  ▷ RECENT                                                        │
└─────────────────────────────────────────────────────────────────┘
```

### Key Elements

- **Enterprise Badge**: Confirms you're on the enterprise tier with full capabilities.
- **Prompt Box**: Natural-language input — describe your research goal, and Discovery orchestrates agents, knowledge bases, and tools to deliver results.
- **Add Inputs and Context**: Attach files, datasets, or prior session outputs as context.
- **Model Selector**: Shows the active LLM (e.g., Claude Opus 4.6). Enterprise deployments can configure model availability.
- **Quick Actions**: Pre-built workflow starters (see Section 11.5).

---

## 11.4 Sidebar Navigation

### HOME

Your landing page with the session interface and recent activity.

### Project Data

Access and manage data assets associated with your project:
- Upload datasets directly
- Browse existing project files
- Connect to Azure storage accounts or data lakes

### AI Capabilities

| Component | Purpose |
|-----------|---------|
| **Agents** | Specialized AI agents configured for your research domain (e.g., TargetAssessmentAgent, LiteratureReviewAgent) |
| **Knowledge Bases** | Enterprise-managed collections of indexed documents — the cloud equivalent of local Bookshelves |
| **All** | Browse all available AI capabilities in one view |

### Jobs

Long-running tasks that execute asynchronously:
- Large-scale document ingestion
- Batch analysis across multiple targets
- Scheduled research sweeps
- HPC-connected analyses

### Collections

Collections are curated groups of knowledge bases organized by research theme:

**Example Collections** (from a materials science project):
- Computational design
- Direct evolution
- Polymer chemistry
- Industrial biocatalysis
- PET Decomposition

For our Rheumatoid Arthritis lab, you might create:
- Immunology pathways
- JAK-STAT signaling
- Clinical evidence
- Target validation data
- Internal experimental results

**To create a Collection**:
1. Click **New collection** in the COLLECTIONS section.
2. Name it (e.g., "Rheumatoid Arthritis — TYK2 Evidence").
3. Add Knowledge Bases to the collection.

---

## 11.5 Quick-Action Workflows

The Enterprise interface offers four quick-action buttons that launch pre-configured workflows:

### 1. Help Me Get Started

Opens a guided onboarding experience:
- Introduces available agents and knowledge bases
- Suggests a research workflow based on your project type
- Recommends collections to explore

**Try it**:
```
Click [Help me get started]
```

The agent will ask about your research goals and walk you through the available capabilities.

### 2. Research

Launches a focused research session:
- Searches across all connected Knowledge Bases
- Queries live external sources (PubMed, NCBI, UniProt)
- Synthesizes findings with citations
- Produces a structured research summary

**Task 11.1** — Start a Research session:
```
Click [Research], then enter:

What is the current state of TYK2 inhibitor development for 
autoimmune diseases? Include clinical trial status, approved 
drugs, and emerging compounds in the pipeline.
```

### 3. Explore Agents & Capabilities

Browse all available AI agents and tools:
- See which agents are configured for your project
- Understand each agent's expertise and tools
- Invoke specific agents for targeted tasks

**Task 11.2** — Explore available agents:
```
Click [Explore agents & capabilities]
```

You'll see agents like:
- **Literature Review Agent** — searches and synthesizes publications
- **Target Assessment Agent** — scores gene targets on multiple dimensions
- **Clinical Evidence Agent** — focuses on trial data and outcomes
- **Data Analysis Agent** — runs computational analyses

### 4. Plan

Creates a structured research plan with milestones:
- Breaks complex goals into phases
- Assigns tasks to appropriate agents
- Tracks progress through each phase
- Produces a timeline with dependencies

**Task 11.3** — Create a research plan:
```
Click [Plan], then enter:

Plan a 4-week target validation study for TYK2 in Rheumatoid 
Arthritis. Include literature review, independent expression 
validation, clinical evidence assessment, and a final 
go/no-go recommendation report.
```

---

## 11.6 Creating and Managing Sessions

### New Session

Each research question or investigation gets its own **Session**. A Session:
- Captures the full conversation history
- Tracks all artifacts generated (documents, tables, figures)
- Can be resumed later
- Can be shared with team members

**To create a session**:
1. Click **New Session** in the sidebar.
2. Describe your objective in the prompt box.
3. The session begins, and Discovery orchestrates the work.

### Session Artifacts

As you work, Discovery generates artifacts that appear in the **SESSION ARTIFACTS** panel at the bottom:
- Research summaries (Markdown)
- Evidence tables (structured data)
- Comparison matrices
- Citation lists
- Figures and visualizations

These artifacts persist with the session and can be:
- Downloaded individually
- Exported as a complete research package
- Shared with collaborators
- Referenced in new sessions

---

## 11.7 Knowledge Bases vs. Local Bookshelves

| Feature | Local Bookshelf (VS Code) | Enterprise Knowledge Base (Web) |
|---------|---------------------------|----------------------------------|
| **Storage** | On your machine | Azure cloud (managed) |
| **Access** | Single user | Team-wide, RBAC-controlled |
| **Indexing** | Local GraphRAG | Enterprise GraphRAG with GPU acceleration |
| **Scale** | Hundreds of documents | Millions of documents |
| **Freshness** | Manual re-index | Automated scheduled ingestion |
| **Use case** | Personal exploration, prototyping | Production team workflows |

> **Key insight**: Start with local Bookshelves for exploration (Chapters 3-4), then promote validated knowledge to enterprise Knowledge Bases for team-wide access.

---

## 11.8 Enterprise Web vs. VS Code Extension — When to Use Each

| Scenario | Use VS Code Extension | Use Enterprise Web |
|----------|----------------------|-------------------|
| Writing code alongside research | ✅ | |
| Quick personal exploration | ✅ | |
| Team collaboration on findings | | ✅ |
| Presenting to stakeholders | | ✅ |
| Long-running batch analysis | | ✅ |
| Offline/air-gapped research | ✅ | |
| Managing large Knowledge Bases | | ✅ |
| Prototyping new Bookshelves | ✅ | |
| Scheduled automated sweeps | | ✅ |
| Notebook development | ✅ | |

Both interfaces share the same underlying AI capabilities — agents, knowledge bases, and tools. The difference is the **interaction model**: VS Code is developer-centric and local-first; the web interface is session-based and collaboration-first.

---

## 11.9 Demo Walkthrough — PET Plastic Decomposition Example

The screenshot shows a real Discovery Enterprise deployment for a materials science team working on **PET plastic decomposition** (enzymatic degradation of polyethylene terephthalate). Here's how they've organized their workspace:

### Their Collections

| Collection | Purpose |
|-----------|---------|
| Computational design | Enzyme engineering computational methods |
| Direct evolution | Directed evolution experimental results |
| Polymer chemistry | PET polymer structure and degradation chemistry |
| Industrial biocatalysis | Scale-up and industrial enzyme application |
| PET Decomposition | Core project — all PET degradation evidence |

### How This Maps to Our RA Lab

You can replicate the same organizational pattern for Rheumatoid Arthritis:

| Their Collection | Our Equivalent |
|-----------------|----------------|
| Computational design | Target modeling & structure prediction |
| Direct evolution | Compound screening & optimization |
| Polymer chemistry | Immunology pathway chemistry |
| Industrial biocatalysis | Drug delivery & formulation |
| PET Decomposition | RA Target Identification (core) |

### Creating Your Collections

**Task 11.4** — Set up RA collections in Enterprise:
```
In the Discovery Enterprise web interface:

1. Click "New collection" → name it "RA - JAK-STAT Signaling"
2. Click "New collection" → name it "RA - Clinical Evidence" 
3. Click "New collection" → name it "RA - Internal Experiments"
4. Click "New collection" → name it "RA - Target Validation"
5. Click "New collection" → name it "RA - Druggability Assessment"

Then add relevant Knowledge Bases to each collection.
```

---

## 11.10 Running a Complete Research Session

Here's a full walkthrough of an Enterprise web session for our RA project:

### Step 1: Start a New Session

Click **New Session** and enter:

```
I want to assess whether TYK2 is a viable therapeutic target for 
Rheumatoid Arthritis. Search across all our knowledge bases and 
public sources to build a comprehensive evidence dossier. Score it 
on genetic evidence, biological rationale, druggability, clinical 
precedent, safety, and internal data support.
```

### Step 2: Discovery Orchestrates

The system will:
1. Query your RA Knowledge Bases for internal evidence
2. Search PubMed for published TYK2/RA literature
3. Check ClinicalTrials.gov for active TYK2 trials
4. Pull UniProt data on TYK2 protein structure
5. Cross-reference with your internal RNA-seq and screening data
6. Score each dimension with citations

### Step 3: Review Session Artifacts

After a few minutes, the SESSION ARTIFACTS panel shows:
- `TYK2_evidence_dossier.md` — Full evidence report
- `TYK2_scoring_table.md` — 6-dimension scoring with justifications
- `TYK2_clinical_trials.md` — Active trial summary
- `TYK2_literature_review.md` — Top publications synthesized

### Step 4: Iterate

Ask follow-up questions in the same session:

```
What are the main risks or counter-evidence against TYK2 as a target?
Are there any failed trials or safety signals I should know about?
```

```
Compare TYK2 against BTK as alternative targets. Which has stronger 
overall evidence for RA?
```

---

## 11.11 Checkpoint

Before proceeding to Chapter 12, confirm:

- [ ] You understand the difference between the VS Code extension and the Enterprise web interface
- [ ] You know when to use each interface
- [ ] You can navigate the sidebar (Home, Project Data, AI Capabilities, Jobs, Collections)
- [ ] You understand what Collections are and how to create them
- [ ] You've tried the quick-action buttons (Research, Plan, Explore agents)
- [ ] You know how Sessions work and how artifacts are generated
- [ ] You understand the relationship between local Bookshelves and enterprise Knowledge Bases

---

**Previous**: [← Chapter 10 — Notebooks & Reporting](chapter-10-notebooks-reporting.md)
**Next**: [Chapter 12 — End-to-End Recap & Next Steps →](chapter-12-recap-next-steps.md)
