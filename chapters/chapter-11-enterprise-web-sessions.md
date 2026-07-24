# Chapter 11: Target Assessment in Microsoft Discovery Studio

> **Goal**: Repeat the complete target identification, prioritization, and validation workflow (Chapters 4-7) — but this time entirely in **Microsoft Discovery Studio**, the browser-based collaborative research environment. Experience how the same scientific workflow translates to the enterprise web platform with shared sessions, team agents, and cloud-backed Knowledge Bases.

---

## 11.1 What You Will Learn

- How to sign in to Discovery Studio and navigate the interface
- How to create Knowledge Bases (the cloud equivalent of local Bookshelves)
- How to connect external tools (PubMed, NCBI, UniProt) in the enterprise environment
- How to run a complete target identification workflow using shared sessions
- How to score and prioritize targets using a custom agent
- How to validate targets with cross-referencing across Knowledge Bases and live APIs
- How shared sessions enable team collaboration on the same research

---

## 11.2 Prerequisites

You need the infrastructure deployed in Chapter 8:

- [ ] A Discovery Workspace accessible at https://studio.discovery.microsoft.com
- [ ] A Project created within the workspace (e.g., `prj-ra-targetlab`)
- [ ] A chat model deployment active (e.g., `gpt-5-2`)
- [ ] The `TargetAssessmentAgent` created in Chapter 8 (or the default Discovery agent)
- [ ] Storage container linked to the project
- [ ] **Scientist** or **Platform Administrator** role assigned

---

## 11.3 Sign In and Navigate to Your Project

1. Open your browser and navigate to **https://studio.discovery.microsoft.com**
2. Sign in with your Microsoft Entra ID credentials.
3. From the sidebar, click **Projects**.
4. Select your RA project (e.g., `prj-ra-targetlab`) — it opens in the VS Code for the Web environment.

You should see:
- **Discovery tab** (left sidebar) — lists shared sessions and quick actions
- **Resources tab** (left sidebar) — lists agents, tools, knowledge bases, and storage
- **Welcome page** (center) — with a chat prompt box and quick-action buttons

---

## 11.4 Set Up Knowledge Bases (Cloud Bookshelves)

In the local workflow (Chapters 3-4), you created local Bookshelves. In Studio, the equivalent is **Knowledge Bases** — cloud-hosted, GraphRAG-indexed document collections that are shared across all project members.

### Create the Internal Research Knowledge Base

1. In the **Resources** tab, find the **KNOWLEDGE** section.
2. Click **+** to create a new Knowledge Base.
3. Configure:
   - **Name**: `InternalResearchData`
   - **Source**: Select your storage container → navigate to the internal research documents
   - **Index type**: GraphRAG (default)
4. Click **Create** and wait for indexing to complete.

The Knowledge Base indexes the same documents you used locally:
- RNA-seq differential expression results
- Gene expression profiles
- Internal target review documents
- Screening results
- Immunology pathway analysis
- Prior target assessment report

### Create the Public Literature Knowledge Base

1. Click **+** again in the KNOWLEDGE section.
2. Configure:
   - **Name**: `PublicLiterature`
   - **Source**: Storage container → public literature documents
3. Create and wait for indexing.

### Verify Knowledge Bases

Both Knowledge Bases should show:
- Document count (7 internal, 6 public literature)
- Health status: **Healthy**
- Index status: **Completed**

> **Cloud advantage**: These Knowledge Bases are now accessible to every team member with project access — no need to distribute files or re-index locally.

---

## 11.5 Connect External Tools

In the local workflow (Chapter 4), you enabled MCP tools individually. In Studio, tools are managed at the project level and shared across all agents.

### Verify Available Tools

1. In the **Resources** tab, expand the **TOOLS** section.
2. You should see built-in Discovery tools already available.
3. If biomedical tools (PubMed, NCBI, UniProt) are configured by your platform admin, they appear here.

> **Note**: In the enterprise deployment, tool availability is managed by the Platform Administrator. The default Discovery agent has access to search Knowledge Bases. Additional external tools may need to be provisioned via the workspace configuration.

### Test Tool Access

Start a quick shared session to verify:

```
Search PubMed for recent publications on TYK2 and its role in 
Rheumatoid Arthritis. Return the top 3 findings.
```

If the agent responds with PubMed citations, your tools are connected.

---

## 11.6 Target Identification — Shared Session 1

Now run the same target identification workflow from Chapter 5, but in a shared session.

### Start the Session

1. On the Welcome page, type your research goal in the chat box:

```
I need to identify candidate therapeutic targets for Rheumatoid 
Arthritis. Use both the InternalResearchData and PublicLiterature 
knowledge bases, plus any available external tools (PubMed, NCBI).

Conduct a systematic target identification:

1. Search the InternalResearchData knowledge base for genes with 
   significant differential expression in our RNA-seq studies
2. Search PublicLiterature for the most-cited RA therapeutic targets 
   from the last 5 years
3. Cross-reference candidates against NCBI for gene function and 
   pathway involvement
4. Identify the top 10 candidate genes that appear in both our 
   internal data AND published literature
5. Present results in a table: Gene | Internal Evidence | Public 
   Evidence | Pathways | Strength of Association

Name this investigation "RA Target Identification — Round 1"
```

2. Click **Send** — a new shared session is created automatically.

### Review the Results

The Discovery agent will:
- Query both Knowledge Bases using GraphRAG
- Search external tools for public evidence
- Synthesize findings into a ranked table
- Cite specific documents from your Knowledge Bases

**Expected output**: A table of 10-15 candidate genes with evidence from both internal and public sources. Based on our sample data, top candidates should include:

| Gene | Internal Evidence | Public Evidence | Key Pathway |
|------|------------------|-----------------|-------------|
| TYK2 | High DE in RNA-seq study-001, study-002 | 50+ publications, validated target | JAK-STAT signaling |
| BTK | DE in study-002, screening hit | Active clinical trials (fenebrutinib) | B-cell receptor signaling |
| JAK1 | Strong DE across all studies | Tofacitinib approved (JAK inhibitor) | JAK-STAT signaling |
| IRAK4 | Moderate DE, pathway analysis hit | Emerging target, Phase II trials | TLR/IL-1R signaling |
| SYK | Screening hit, immunology pathway | Fostamatinib (approved for ITP) | B-cell/myeloid signaling |

### Follow-Up Queries

Continue in the same session:

```
For the top 5 candidates (TYK2, BTK, JAK1, IRAK4, SYK), provide more 
detail on each:
1. What specific evidence do we have from our internal RNA-seq studies?
2. What is the magnitude of differential expression?
3. In which cell types / tissues is this gene most relevant?
4. Are there any known safety concerns?
```

---

## 11.7 Target Prioritization — Shared Session 2

Now use your custom `TargetAssessmentAgent` to score and rank targets, replicating the Chapter 6 workflow.

### Start a New Session for Prioritization

1. Click **New shared session** in the Discovery tab.
2. Select your `TargetAssessmentAgent` from the agent dropdown (or type `@TargetAssessmentAgent`).
3. Enter:

```
@TargetAssessmentAgent Score and rank the following candidate 
therapeutic targets for Rheumatoid Arthritis: TYK2, BTK, JAK1, IRAK4, SYK

Use this 6-dimension scoring framework (each dimension scored 1-5):

1. Genetic Evidence - Strength of genetic association with RA
   (5 = GWAS hit + functional validation, 1 = single mention)
2. Biological Rationale - Mechanistic understanding
   (5 = well-characterized disease mechanism, 1 = association only)
3. Druggability - Feasibility of therapeutic intervention
   (5 = existing drugs/small molecules, 1 = no known binding sites)
4. Clinical Precedent - Existing clinical evidence
   (5 = active Phase II/III for RA, 1 = no clinical data)
5. Internal Data Support - Our proprietary evidence strength
   (5 = strong experimental validation, 1 = no internal data)
6. Safety/Selectivity - Expected therapeutic window
   (5 = tissue-restricted, low off-target, 1 = ubiquitous expression)

For each gene and each dimension:
- Search the InternalResearchData knowledge base for internal evidence
- Search PublicLiterature for published evidence
- Query external tools for additional public data
- Provide a score with explicit justification and citations

Present the final ranking as a table sorted by total score.
```

### Review the Prioritization Results

The agent should produce a structured scoring table:

| Rank | Gene | Genetic | Mechanism | Druggable | Clinical | Internal | Safety | Total (/30) | Recommendation |
|------|------|---------|-----------|-----------|----------|----------|--------|-------------|----------------|
| 1 | TYK2 | 5 | 4 | 4 | 4 | 5 | 4 | 26 | **Strong** — pursue |
| 2 | BTK | 4 | 5 | 4 | 3 | 4 | 3 | 23 | **Strong** — pursue |
| 3 | JAK1 | 4 | 4 | 5 | 5 | 3 | 2 | 23 | **Strong** — but safety concern |
| 4 | IRAK4 | 3 | 3 | 3 | 2 | 3 | 4 | 18 | **Moderate** — emerging |
| 5 | SYK | 3 | 3 | 4 | 2 | 3 | 3 | 18 | **Moderate** — needs validation |

### Deep Dive on Top Target

```
@TargetAssessmentAgent For TYK2 (our top-ranked target), provide a 
detailed evidence dossier:

1. All genetic evidence with specific citations from our knowledge bases
2. The biological mechanism linking TYK2 to RA pathogenesis
3. Known TYK2 inhibitors and their clinical status
4. Our internal experimental data supporting TYK2
5. Any counter-evidence or risks
6. Comparison with deucravacitinib (approved TYK2 inhibitor for psoriasis)
7. Gap analysis — what additional evidence would strengthen the case?
```

---

## 11.8 Target Validation — Shared Session 3

Create a third shared session focused on validation, replicating the Chapter 7 workflow.

### Start the Validation Session

1. Click **New shared session**.
2. Enter:

```
Conduct independent validation of our top 5 RA target candidates 
(TYK2, BTK, JAK1, IRAK4, SYK). For each target, perform:

VALIDATION 1: Independent Expression Data
- Search for public RNA-seq or microarray studies (NOT from our 
  internal data) that show differential expression in RA
- Check if direction and magnitude are consistent with our findings
- Report: how many independent datasets confirm each target?

VALIDATION 2: Clinical Evidence
- Search ClinicalTrials.gov for active trials targeting each gene in RA
- For targets with trials: what phase? what compound? any results?
- For targets without RA trials: any trials in related autoimmune diseases?

VALIDATION 3: Functional Evidence
- Search for CRISPR, siRNA, or genetic model studies validating each 
  gene's role in RA or inflammation
- Any loss-of-function variants that inform human biology?

VALIDATION 4: Counter-Evidence
- Actively search for evidence AGAINST each target
- Failed trials, negative results, safety signals, contradictory findings

VALIDATION 5: Immunology Cross-Reference
- Search our InternalResearchData knowledge base for immunology-specific 
  evidence on each gene
- Which immune cell types express these genes?
- Any connection to T-cell, B-cell, or macrophage biology?

Present a validation summary table:
Gene | Independent Datasets | Clinical Trials | Functional Evidence | 
Counter-Evidence | Immunology Support | Validation Confidence
```

### Review Validation Results

The agent synthesizes across all evidence layers:

| Gene | Independent Datasets | Clinical Trials | Functional | Counter-Evidence | Immunology | Confidence |
|------|---------------------|-----------------|------------|-----------------|------------|------------|
| TYK2 | 5+ datasets confirm | Deucravacitinib (Phase III, psoriasis); Phase II RA | CRISPR validated | Selective enough? | JAK-STAT in T/NK cells | **High** |
| BTK | 3 datasets confirm | Fenebrutinib (Phase III RA), evobrutinib | KO mice protected | Infection risk? | B-cell signaling | **High** |
| JAK1 | 8+ datasets confirm | Tofacitinib (approved RA) | Well-validated | VTE risk, infections | Broad immune signaling | **High** (but safety) |
| IRAK4 | 2 datasets confirm | Phase II (PF-06650833) | Partial KO data | Limited human data | TLR/IL-1 in macrophages | **Moderate** |
| SYK | 2 datasets confirm | Fostamatinib (ITP only) | KO mice data | Not pursued in RA | Myeloid + B-cell | **Moderate** |

### Final Recommendation

```
Based on all validation evidence, provide a final go/no-go 
recommendation for each target:

For each gene:
1. Overall recommendation: GO / CONDITIONAL / NO-GO
2. Confidence level: High / Medium / Low
3. Key strength (one sentence)
4. Key risk (one sentence)  
5. Recommended next experiment to address the biggest evidence gap
```

---

## 11.9 Collaboration — Sharing Sessions with Team Members

One of the key advantages of Discovery Studio over the local app is **shared sessions**. Everything you did in Sections 11.6-11.8 is automatically visible to team members.

### How Sharing Works

- All shared sessions are visible to any project member
- Team members can:
  - Read the full conversation history
  - Continue the conversation with follow-up questions
  - Switch to a different agent for a second opinion
  - Open multiple sessions in split-view for comparison

### Exercise: Parallel Investigation

In a team setting, you would assign different sessions to different scientists:

| Team Member | Session | Focus |
|-------------|---------|-------|
| Scientist A | RA Target Identification | Literature + internal data synthesis |
| Scientist B | TYK2 Deep Dive | Detailed evidence dossier for lead target |
| Scientist C | Counter-Evidence Search | Actively challenge all candidates |
| Scientist D | Clinical Landscape | Competitive intelligence from trials |

All work in the same project, all sessions are shared, and findings cross-pollinate.

---

## 11.10 Comparing the Local vs. Studio Workflows

You've now completed the same scientific workflow in both environments:

| Step | Local (Chapters 4-7) | Studio (This Chapter) |
|------|---------------------|----------------------|
| **Data setup** | Local Bookshelf + ingestion | Cloud Knowledge Base + storage upload |
| **External tools** | MCP plugins in VS Code | Tools attached at project level |
| **Target ID** | Copilot Chat + Bookshelf | Shared session + Knowledge Base |
| **Prioritization** | Discovery Engine (Supervised) | Custom agent in shared session |
| **Validation** | Multi-step prompts in Copilot | Multi-faceted single-session validation |
| **Collaboration** | Git + share `.discovery/` folder | Built-in shared sessions |
| **Persistence** | Files on disk | Cloud-backed, auto-saved |
| **Access** | Your machine only | Any browser, any device |

### When to Use Each

| Scenario | Recommended Environment |
|----------|------------------------|
| First-time exploration of a new disease area | Local (fast, free, no infrastructure) |
| Solo deep-dive with your own data | Local |
| Team target assessment with multiple scientists | **Studio** |
| Presenting findings to leadership | **Studio** (shareable sessions) |
| Running large-scale analyses (HPC) | **Studio** (supercomputer node pools) |
| Offline research (airplane, restricted network) | Local |
| Reproducible pipeline for new projects | **Studio** (templates, shared agents) |

---

## 11.11 Studio-Specific Features Not Available Locally

| Feature | What It Enables |
|---------|----------------|
| **Agent logs** | Detailed view of prompts, responses, and tool call logs (including raw output) |
| **Multiple concurrent sessions** | Open in split tabs for side-by-side comparison |
| **Agent selector dropdown** | Quickly switch between Discovery, TargetAssessment, or any custom agent |
| **Session summaries** | Auto-generated summaries as the session progresses |
| **Preferences** | Customize agentic behavior (response style, citation format, verbosity) |
| **Project storage** | Browse input/output files directly in the sidebar |

---

## 11.12 Checkpoint

Before proceeding to Chapter 12, confirm:

- [ ] You signed in to Discovery Studio and opened your project
- [ ] You created (or verified) Knowledge Bases for internal and public data
- [ ] You ran a target identification shared session and got a candidate list
- [ ] You used the TargetAssessmentAgent to score and rank targets
- [ ] You ran a validation session with multi-dimensional evidence review
- [ ] You understand how shared sessions enable team collaboration
- [ ] You can compare the local vs. Studio workflows and articulate when to use each
- [ ] Your top target recommendation (TYK2) is consistent across both environments

---

**Previous**: [← Chapter 10 — Notebooks & Reporting](chapter-10-notebooks-reporting.md)
**Next**: [Chapter 12 — End-to-End Recap & Next Steps →](chapter-12-recap-next-steps.md)
