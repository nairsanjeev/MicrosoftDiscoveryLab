# Chapter 5: Target Identification — Aggregating Public + Internal Knowledge

> **Goal**: Use the Task graph to structure a target-identification workflow that aggregates internal experimental data with external public knowledge. Ask Discovery to identify candidate therapeutic targets by reasoning across all data sources.

---

## 5.1 What You Will Learn

- How to build a task graph (DAG) for target identification
- How to combine internal and external knowledge in a structured workflow
- How to use Copilot to run multi-source target identification queries
- How to capture candidate gene lists with supporting evidence

---

## 5.2 The Target Identification Challenge

In a traditional pharma workflow, target identification looks like this:

```
Biologist has a disease of interest
        ↓
Manual literature review (weeks)              ← PubMed, Google Scholar
        ↓
Query internal databases (days)               ← RNA-seq, screening data
        ↓
Cross-reference with public databases (days)  ← NCBI, UniProt, GEO
        ↓
Synthesize findings manually (days)           ← Spreadsheets, presentations
        ↓
Propose candidate targets (meetings)          ← Subjective, hard to audit
```

With Microsoft Discovery, this becomes:

```
Define objective in Discovery
        ↓
Task graph structures the workflow            ← Automated planning
        ↓
Copilot + Bookshelf + MCP Tools              ← Minutes, not weeks
        ↓
Discovery Engine synthesizes autonomously     ← Explainable, cited
        ↓
Ranked, documented target list                ← Auditable, reproducible
```

---

## 5.3 Build the Target Identification Task Graph

### Step 1: Define the High-Level Objective

Open Copilot Chat (`Ctrl+Alt+I`) and create a structured task graph:

**Task 5.1** — Create the task graph:
```
Use the tasks tool to plan a target identification workflow for [DISEASE].
Create a parent task called "Target Identification for [DISEASE]" and create
the following dependent sub-tasks:

1. "Literature Review" - Review published literature on known targets and 
   pathways for [DISEASE]
2. "Internal Data Analysis" - Analyze our internal RNA-seq and screening 
   data for differentially expressed genes
3. "Public Database Cross-Reference" - Cross-reference candidate genes 
   against NCBI, UniProt, and ClinicalTrials.gov
4. "Immunology Pathway Mapping" - Map candidates to immunology pathways 
   and check for prior evidence
5. "Candidate Target List" - Compile ranked list of candidate targets 
   with evidence summaries (depends on tasks 1-4)
```

### Step 2: Verify the Task Graph

Open the **Tasks** panel in the sidebar to see your task graph. You should see:

```
📋 Target Identification for [DISEASE]
 ├── 📋 Literature Review                    [new]
 ├── 📋 Internal Data Analysis               [new]
 ├── 📋 Public Database Cross-Reference      [new]
 ├── 📋 Immunology Pathway Mapping           [new]
 └── 📋 Candidate Target List                [new] ← depends on all above
```

Check which tasks are ready:

```
What tasks are ready to work on right now?
```

Tasks 1-4 should be ready (no dependencies blocking them). Task 5 is blocked until tasks 1-4 complete.

---

## 5.4 Execute Task 1: Literature Review

This task leverages both your Bookshelves and live PubMed queries.

**Task 5.2** — Comprehensive literature review:
```
For [DISEASE], conduct a literature review using both my Bookshelves and PubMed:

1. Search my InternalResearchData bookshelf for any prior work on targets 
   for this disease
2. Search PubMed for the most cited review articles on [DISEASE] targets 
   from the last 5 years
3. Identify the top 10-15 genes most frequently mentioned as therapeutic 
   targets
4. For each gene, note: what evidence supports it, which publications 
   mention it, and any known limitations

Present results in a table with columns: Gene | Evidence Type | Key Publications | Limitations
```

**Task 5.3** — Update task status:
```
Mark the "Literature Review" task as executionDone and add the results 
as the task output.
```

---

## 5.5 Execute Task 2: Internal Data Analysis

This task focuses exclusively on your proprietary data.

**Task 5.4** — Internal data synthesis:
```
Search my InternalResearchData bookshelf exclusively:

1. What genes showed statistically significant differential expression 
   in our RNA-seq studies?
2. Which genes were identified as hits in our high-throughput screening?
3. Are there any genes that appear in multiple internal studies?
4. What does our internal immunology pathway analysis say about these genes?

Create a table: Gene | Internal Evidence Source | Expression Change | 
Screening Result | Internal Confidence Level
```

**Task 5.5** — Mark task:
```
Mark "Internal Data Analysis" as executionDone.
```

---

## 5.6 Execute Task 3: Public Database Cross-Reference

This task uses live MCP tools to validate candidates.

**Task 5.6** — Cross-reference each candidate:
```
For each of the top gene candidates identified in Tasks 1 and 2, 
cross-reference against public databases:

1. NCBI Entrez: Confirm gene identity, function, pathway membership
2. UniProt: Check protein druggability, known interaction partners, 
   disease associations
3. ClinicalTrials.gov: Are there active trials targeting this gene 
   or its pathway?

For each gene, create an entry:
Gene | NCBI Function | UniProt Druggability | Active Trials | 
Public Evidence Strength (Low/Medium/High)
```

**Task 5.7** — Mark task:
```
Mark "Public Database Cross-Reference" as executionDone.
```

---

## 5.7 Execute Task 4: Immunology Pathway Mapping

For targets in immunology-relevant diseases, pathway context is critical.

**Task 5.8** — Pathway analysis:
```
For the candidate genes from our analysis:

1. Map each gene to its known immunology pathways (e.g., NF-κB, JAK-STAT, 
   TNF signaling, interferon response)
2. Check our internal Bookshelf: where have we seen these genes in our 
   immunology work internally?
3. Search PubMed for pathway-level evidence: are entire pathways 
   validated, or just individual genes?
4. Identify any genes that sit at pathway "hubs" (interact with many 
   other targets)

Create a pathway map summary: Gene | Pathway(s) | Hub Score | 
Internal Evidence | Public Evidence
```

**Task 5.9** — Mark task:
```
Mark "Immunology Pathway Mapping" as executionDone.
```

---

## 5.8 Execute Task 5: Compile the Candidate Target List

Now that tasks 1-4 are complete, ask Copilot to synthesize:

**Task 5.10** — Synthesize all evidence:
```
All prerequisite tasks for "Candidate Target List" are now complete. 
Synthesize the results from:
- Literature Review (public knowledge)
- Internal Data Analysis (our proprietary data)
- Public Database Cross-Reference (NCBI, UniProt, ClinicalTrials.gov)
- Immunology Pathway Mapping (pathway context)

Create a consolidated candidate target list ranked by overall evidence 
strength. For each candidate:
1. Gene name and full name
2. Evidence from our internal data (with citations)
3. Evidence from public literature (with citations)
4. Druggability assessment
5. Clinical trial status
6. Pathway context and hub score
7. Overall recommendation: Strong / Moderate / Weak candidate

Rank from strongest to weakest candidate.

Mark "Candidate Target List" as complete.
```

---

## 5.9 Review the Complete Picture

**Task 5.11** — Check the task graph:
```
Show me the current state of all tasks. Which are complete, which are 
in progress, and which are still blocked?
```

You should see all five tasks as `complete` or `executionDone`, and the parent task ready for final review.

---

## 5.10 Checkpoint

Before proceeding to Chapter 6, confirm:

- [ ] A task graph for target identification has been created with dependencies
- [ ] Literature review combined internal Bookshelf + PubMed results
- [ ] Internal data analysis surfaced proprietary experimental findings
- [ ] Public database cross-referencing validated candidates via NCBI, UniProt, ClinicalTrials.gov
- [ ] Immunology pathway mapping provided pathway context
- [ ] A consolidated ranked candidate target list exists with multi-source evidence
- [ ] Every finding includes citations back to source data

---

**Previous**: [← Chapter 4 — External Data Sources](chapter-04-external-data-sources.md)
**Next**: [Chapter 6 — Target Prioritization →](chapter-06-target-prioritization.md)
