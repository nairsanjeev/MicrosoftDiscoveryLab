# Chapter 6: Target Prioritization — Ranking & Explainability

> **Goal**: Configure a Discovery Engine to autonomously prioritize candidate targets using a structured scoring framework. Produce an explainable, auditable ranking that biologists can trust and present to leadership.

---

## 6.1 What You Will Learn

- How to configure a Discovery Engine for autonomous target prioritization
- How to define scoring criteria and validation requirements
- How the Discovery Engine's cognition loop works
- How to ensure explainability — every recommendation traced to evidence
- How to review and redirect the engine's work

---

## 6.2 Why Explainability Is Key

In drug target selection, the question is not just "what is the best target?" but "**why** is it the best target?" Leadership, regulatory teams, and project decision-makers need:

| Requirement | What It Means |
|-------------|---------------|
| **Evidence traceability** | Every claim links back to a specific document, experiment, or database entry |
| **Scoring transparency** | The ranking criteria are explicit and auditable |
| **Comparative analysis** | Why Target A ranks above Target B |
| **Gap identification** | What evidence is missing for each candidate |
| **Reproducibility** | Another scientist could follow the same process and reach the same conclusions |

Microsoft Discovery's outputs are **explainable by design** — results are stored in a knowledge graph with citations, and the Discovery Engine's reasoning is auditable.

---

## 6.3 Define the Prioritization Scoring Framework

Before starting the engine, establish explicit scoring criteria. Ask Copilot to help:

**Task 6.1** — Define scoring criteria:
```
Create a target prioritization scoring framework for Rheumatoid Arthritis. 
Define the following dimensions, each scored 1-5:

1. **Genetic Evidence** (1-5): Strength of genetic association with disease
   - 5 = GWAS hit + functional validation
   - 3 = Differential expression in multiple studies
   - 1 = Mentioned in one publication only

2. **Biological Rationale** (1-5): Mechanistic understanding
   - 5 = Well-characterized mechanism directly linked to disease
   - 3 = Known pathway involvement, mechanism partially understood
   - 1 = Association only, no mechanistic evidence

3. **Druggability** (1-5): Feasibility of therapeutic intervention
   - 5 = Known drug target with existing small molecules/antibodies
   - 3 = Predicted druggable, structural data available
   - 1 = No known binding sites or modulation strategies

4. **Clinical Precedent** (1-5): Existing clinical evidence
   - 5 = Active Phase II/III trials for this indication
   - 3 = Clinical trials in related indications
   - 1 = No clinical data

5. **Internal Data Support** (1-5): Our proprietary evidence
   - 5 = Strong internal experimental validation
   - 3 = Suggestive internal data
   - 1 = No internal data

6. **Safety/Selectivity** (1-5): Expected therapeutic window
   - 5 = Tissue-restricted expression, low off-target risk
   - 3 = Moderate expression breadth, manageable risks
   - 1 = Ubiquitous expression, high off-target concern

Store this framework as a task called "Prioritization Framework."
```

---

## 6.4 Configure the Discovery Engine

The Discovery Engine is configured in `.discovery/config.json`. Create a dedicated engine for target prioritization:

### Step 1: Edit the Configuration

Open `.discovery/config.json` in your workspace and add an engine definition:

```json
{
  "cognition": {
    "engines": [
      {
        "definitionId": "target-prioritization",
        "displayName": "Target Prioritization Engine",
        "adapterKind": "copilot-cli",
        "systemPrompt": "You are a drug target prioritization agent. Score and rank candidate therapeutic targets using the defined scoring framework. For each target: (1) search all Bookshelves for internal and external evidence, (2) query PubMed, UniProt, NCBI, and ClinicalTrials.gov for public evidence, (3) score each dimension 1-5 with explicit justification and citations, (4) calculate a weighted total score, (5) identify evidence gaps. Produce an explainable ranked list where every score is justified with specific citations.",
        "policy": {
          "level": "Supervised"
        }
      }
    ]
  }
}
```

### Step 2: Understand the Configuration

| Field | Purpose |
|-------|---------|
| `definitionId` | Unique identifier for this engine |
| `displayName` | Human-readable name shown in the UI |
| `adapterKind` | Runtime adapter — `copilot-cli` drives GitHub Copilot CLI |
| `systemPrompt` | Instructions that define the engine's behavior and methodology |
| `policy.level` | `Supervised` = engine proposes actions, you approve/deny |

> **Important**: Start in **Supervised** mode. This lets you see exactly what the engine wants to do before it does it, which builds trust and helps you understand the reasoning.

---

## 6.5 Create the Prioritization Task Graph

**Task 6.2** — Create prioritization tasks:
```
Use the tasks tool to create a target prioritization workflow. 
Parent task: "Target Prioritization for Rheumatoid Arthritis"

Sub-tasks:
1. "Score Genetic Evidence" - For each candidate target, assess genetic 
   evidence strength (1-5) using internal RNA-seq data + public GWAS data
   Validation: Each score must cite at least 2 supporting sources

2. "Score Biological Rationale" - Assess mechanistic understanding of each 
   target using pathway analysis and literature
   Validation: Each score must reference specific pathway and mechanism

3. "Score Druggability" - Assess therapeutic feasibility using UniProt 
   structure data and known ligand information
   Validation: Each score must reference structural or pharmacological data

4. "Score Clinical Precedent" - Check ClinicalTrials.gov and PubMed for 
   clinical evidence
   Validation: Must list specific trial IDs or state "no trials found"

5. "Score Internal Evidence" - Rate our proprietary data support for 
   each candidate
   Validation: Must cite specific internal experiments/documents

6. "Score Safety/Selectivity" - Assess expression breadth and off-target 
   risks using NCBI and UniProt tissue expression data
   Validation: Must reference tissue expression data source

7. "Compile Ranked Target List" - Calculate weighted total scores, rank 
   targets, identify top 3-5 candidates, flag evidence gaps
   Depends on: Tasks 1-6
   Validation: Final table with all scores, citations, and gap analysis
```

---

## 6.6 Start the Discovery Engine

### Option A: Via Command Palette

1. Press `Ctrl+Shift+P`
2. Type: `Microsoft Discovery: Start Engine`
3. Select `target-prioritization`

### Option B: Via Copilot Chat

```
Start the target-prioritization engine and begin scoring all candidate 
targets identified in our previous analysis. Use the prioritization 
scoring framework and work through each sub-task.
```

### Option C: Via dx CLI

```powershell
dx engine list-definitions --workspace C:\MicrosoftDiscoveryLab\workspace
# Note the engine definition ID, then start it
```

### Supervised Mode Interaction

In **Supervised** mode, the engine will propose actions one at a time:

```
Engine proposes: Search InternalResearchData bookshelf for RNA-seq data 
on gene BRCA1 to score genetic evidence
→ [Approve] [Deny]
```

**Task 6.3**: Approve the first 5-10 actions to observe how the engine reasons. Notice:
- It selects the right tool for each question
- It builds evidence incrementally
- It cites sources for every claim

---

## 6.7 Review Engine Output

After the engine completes (or after you've run through enough actions), review the results:

**Task 6.4** — Review the prioritized target list:
```
Show me the complete ranked target list with all scoring dimensions. 
For each target, show:
1. Gene name
2. Score for each of the 6 dimensions (1-5)
3. Weighted total score
4. Top 3 supporting citations
5. Key evidence gaps
6. Overall recommendation

Format as a table, sorted by total score descending.
```

### Expected Output Format

| Rank | Gene | Genetic (5) | Mechanism (5) | Druggable (5) | Clinical (5) | Internal (5) | Safety (5) | Total (/30) | Recommendation |
|------|------|-------------|---------------|----------------|--------------|-------------|------------|-------------|----------------|
| 1 | TYK2 | 5 | 4 | 4 | 3 | 5 | 4 | 25 | **Strong** — pursue |
| 2 | BTK | 4 | 5 | 3 | 2 | 4 | 4 | 22 | **Strong** — pursue |
| 3 | JAK1 | 3 | 3 | 5 | 4 | 2 | 3 | 20 | **Moderate** — needs internal validation |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

---

## 6.8 Explainability Deep Dive

For each top candidate, get a detailed evidence report:

**Task 6.5** — Deep dive on top target:
```
For our top-ranked target TYK2, provide a detailed evidence report:

1. **Genetic Evidence**: List every piece of genetic evidence, with 
   specific citations from our internal data and public literature
2. **Biological Mechanism**: Describe the mechanistic rationale step by 
   step, citing specific pathway analyses
3. **Druggability**: What structural/pharmacological data supports 
   therapeutic intervention? Any known compounds?
4. **Clinical Context**: What clinical trials exist? What indications? 
   Phase? Results?
5. **Our Internal Data**: Exactly which experiments support this target? 
   What were the key measurements?
6. **Risk Assessment**: What could go wrong? What evidence is missing?

This report must be presentable to R&D leadership as justification for 
target selection.
```

> **This is explainability**: Every recommendation can be traced back to specific evidence. A biologist can audit every claim. Leadership can understand the reasoning.

---

## 6.9 Handling Disagreements Between Sources

Often, internal data and public literature may disagree:

**Task 6.6** — Identify disagreements:
```
Are there any cases where our internal experimental data disagrees with 
the published literature on any candidate target? 

For each disagreement:
1. What does our internal data say?
2. What does the public literature say?
3. What might explain the difference?
4. How should this affect the target's priority?
```

---

## 6.10 Checkpoint

Before proceeding to Chapter 7, confirm:

- [ ] A prioritization scoring framework with 6 dimensions is defined
- [ ] A Discovery Engine (`target-prioritization`) is configured in `config.json`
- [ ] The engine ran in Supervised mode and you approved its actions
- [ ] A ranked target list exists with scores and citations
- [ ] Top candidates have detailed evidence reports
- [ ] Every score is traced back to specific evidence (explainability)
- [ ] Any disagreements between internal and public data are identified

---

**Previous**: [← Chapter 5 — Target Identification](chapter-05-target-identification.md)
**Next**: [Chapter 7 — Target Validation →](chapter-07-target-validation.md)
