# Chapter 4: Connecting External Public Data Sources

> **Goal**: Enable scientific MCP tools (PubMed, ClinicalTrials.gov, NCBI, UniProt) to bring live public knowledge into your Discovery workflow alongside your internal data. Optionally create a second Bookshelf from curated public literature.

---

## 4.1 What You Will Learn

- What public datasets are available and why they matter for target identification
- How to enable MCP tool plugins from the Agent Plugin Marketplace
- How to query live public APIs directly from Copilot Chat
- How to build a second Bookshelf from curated public literature
- The cost implications of using public vs. proprietary datasets

---

## 4.2 The Public Data Landscape for Drug Target Discovery

Understanding a gene in biology requires pulling together information from many public sources. Here's the landscape:

| Data Source | What It Contains | Why It Matters for Target ID |
|-------------|-----------------|------------------------------|
| **PubMed** | 36M+ biomedical publications | Literature review — what's known about a gene/disease |
| **ClinicalTrials.gov** | 500K+ clinical studies | Is anyone already pursuing this target? What's the clinical evidence? |
| **NCBI Entrez** | Gene, protein, nucleotide databases | Gene function, genomic context, cross-species conservation |
| **UniProt** | 250M+ protein sequences + annotations | Protein function, structure, interactions, druggability |
| **RCSB PDB** | 220K+ protein structures | 3D structure for binding-site analysis |
| **arXiv / bioRxiv** | Preprints (not yet peer-reviewed) | Latest research not yet in PubMed |
| **GEO (Gene Expression Omnibus)** | Public gene expression datasets | Independent validation of your expression findings |
| **XPR (Expression Atlas)** | Curated gene expression across tissues/conditions | Baseline expression, disease-specific differential expression |

### Cost Considerations

| Aspect | Manual Approach | With Microsoft Discovery |
|--------|----------------|--------------------------|
| Literature review | Weeks of manual PubMed searching | Minutes — Bookshelf + BioMCP gives synthesized, cited answers |
| Cross-referencing databases | Hours per gene, copy-pasting between websites | Seconds — single Copilot prompt queries multiple sources |
| Keeping up with new publications | Easily missed, requires constant monitoring | Engine can run periodic sweeps |
| Integrating internal + external | Entirely manual, error-prone | Bookshelf + Tools unified in one workspace |

---

## 4.3 Connect External Sources via Chat (Recommended)

The fastest way to connect external data sources is through the same chat-based setup you used in Chapter 3. The project setup orchestrator can enable all your MCP tools in a single conversation.

### Step 1: Open Copilot Chat

Press `Ctrl+Alt+I` or click the Copilot Chat icon.

### Step 2: Ask the Orchestrator to Connect Public Databases

Enter the following prompt:

```
I need to connect my Discovery project to external biomedical databases 
for drug target research on Rheumatoid Arthritis. Please enable:
1. PubMed and ClinicalTrials.gov (via BioMCP)
2. NCBI Entrez (gene, protein, nucleotide lookups)
3. UniProt (protein annotations and druggability)
4. bioRxiv/arXiv (preprints)

Configure all of them and confirm each is connected with a green health dot.
```

### Step 3: Confirm Connection

The orchestrator will enable each tool and report status. You should see:

```
✅ BioMCP (PubMed + ClinicalTrials.gov) — connected
✅ NCBI Entrez — connected  
✅ UniProt — connected
✅ bioRxiv/arXiv — connected
```

Check the **TOOLS** section in the Discovery sidebar — each tool should show a green health indicator.

> **Why chat-based?** The orchestrator handles marketplace registration, plugin installation, and health verification in one step. No manual settings.json editing required.

---

## 4.4 Connect External Sources Manually (Alternative)

If you prefer manual setup, or if the orchestrator approach didn't work:

### Step 1: Add the Agent Plugin Marketplace

Open VS Code settings (`Ctrl+,`) and search for `chat.plugins.marketplaces`. Add the marketplace URL:

```json
{
  "chat.plugins.marketplaces": [
    "https://aka.ms/discovery-plugin-marketplace"
  ]
}
```

### Step 2: Reload VS Code

Press `Ctrl+Shift+P` → `Developer: Reload Window`.

### Step 3: Enable Tools from the Sidebar

1. Open the **TOOLS** section in the Discovery sidebar.
2. Click the **+** button to browse available tools.
3. Enable each tool individually (see Section 4.5 for details on each).

---

## 4.5 Enable Biomedical MCP Tools

Whether you used the chat or manual approach, here's what each tool provides and how to verify it's working:

### Enable the following tools for our target-identification workflow:

#### 1. BioMCP (PubMed + Clinical Trials) 🧬

**What it provides**: Semantic search over PubMed articles and ClinicalTrials.gov entries.

**Manual setup**:
1. Find **BioMCP** in the TOOLS panel.
2. Toggle it **on**.
3. Wait for the green health dot to confirm the plugin is connected.

**Task 4.1** — Test PubMed access:
```
Using PubMed, find recent publications on TYK2 and its role 
in Rheumatoid Arthritis. Summarize the top 5 findings.
```

**Task 4.2** — Test ClinicalTrials.gov access:
```
Search ClinicalTrials.gov for any active clinical trials targeting 
TYK2 or the JAK-STAT pathway for Rheumatoid Arthritis. What compounds are being tested?
```

#### 2. NCBI Entrez (Gene / Protein / Nucleotide) 🧬

**What it provides**: Direct access to NCBI gene records, protein entries, and nucleotide sequences.

**Manual setup**:
1. Find **NCBI Entrez** in the TOOLS panel.
2. Toggle it **on**.

**Task 4.3** — Gene lookup:
```
Look up gene TYK2 in NCBI. What is its full name, chromosomal 
location, and known function? What pathways is it involved in?
```

#### 3. UniProt (Protein Sequences & Annotations) 🧬

**What it provides**: Protein sequence data, functional annotations, interaction partners, and disease associations.

**Manual setup**:
1. Find **UniProt** in the TOOLS panel.
2. Toggle it **on**.

**Task 4.4** — Protein function:
```
Using UniProt, describe the protein encoded by TYK2. What are its 
known interaction partners and disease associations? Is it considered 
druggable?
```

#### 4. arXiv & bioRxiv (Preprints) 📄

**What it provides**: Latest preprints that may not yet be indexed in PubMed.

**Manual setup**:
1. Find **arXiv** and **bioRxiv/medRxiv** in the TOOLS panel.
2. Toggle them **on**.

**Task 4.5** — Latest research:
```
Find recent bioRxiv preprints on TYK2 or JAK-STAT signaling in Rheumatoid Arthritis 
from the last 6 months. Are there any new findings not yet in PubMed?
```

---

## 4.6 Querying Multiple External Sources Simultaneously

Now that your tools are enabled, you can ask questions that span multiple public databases in a single prompt:

**Task 4.6** — Multi-source query:
```
For gene TYK2:
1. What does PubMed say about its role in Rheumatoid Arthritis?
2. Are there any clinical trials targeting it?
3. What is the protein's function according to UniProt?
4. Are there any recent preprints with new findings?

Synthesize the information and assess whether this gene is a viable 
therapeutic target.
```

> **This is the power of Discovery**: A question that would take a biologist hours of manual searching across multiple websites is answered in seconds, with citations.

---

## 4.7 (Optional) Create a "Public Literature" Bookshelf

For deeper analysis, you can curate a collection of key public papers and index them in a second Bookshelf. This gives you GraphRAG-powered reasoning over public literature, not just live API lookups.

### Step 1: Curate Public Papers

Sample public literature files are provided in `sample-data/public-literature/`:

```
C:\MicrosoftDiscoveryLab\sample-data\public-literature\
├── review-ra-therapeutic-targets-2024.md         ← Review: RA target landscape
├── tyk2-therapeutic-target-evidence.md           ← TYK2 evidence (genetics + clinical)
├── btk-inhibitors-autoimmune-clinical-review.md  ← BTK clinical outcomes review
├── rna-seq-meta-analysis-ra-synovium.md          ← Public RNA-seq meta-analysis (8 datasets)
├── clinical-trials-ra-targets-summary.md         ← ClinicalTrials.gov compilation
└── immunology-pathways-ra-review.md              ← Immunology pathways & cell types
```

### Step 2: Create and Ingest

```powershell
dx bookshelf create PublicLiterature --workspace C:\MicrosoftDiscoveryLab\workspace
dx bookshelf ingest <shelf-id> C:\MicrosoftDiscoveryLab\sample-data\public-literature\ --recursive --workspace C:\MicrosoftDiscoveryLab\workspace
```

### Step 3: Expose to Copilot

Tick the checkbox ☑️ next to `PublicLiterature` in the Bookshelf panel.

### Step 4: Query Across Both Bookshelves

Now Copilot has access to both your internal data AND curated public literature:

**Task 4.7** — Cross-Bookshelf reasoning:
```
Compare what our internal experimental data says about TYK2 with what 
the published literature reports. Are our findings consistent with the 
public evidence? Where do they diverge?
```

---

## 4.8 Understanding Data Source Tiers

Think of your data sources in tiers:

```
┌─────────────────────────────────────────────────────┐
│  Tier 1: Internal Bookshelf                         │
│  Your proprietary data — RNA-seq, experiments,      │
│  screening results, internal publications           │
│  → Deep GraphRAG reasoning, fully private           │
├─────────────────────────────────────────────────────┤
│  Tier 2: Curated Public Bookshelf                   │
│  Key public papers you've selected and indexed      │
│  → GraphRAG reasoning over your curated subset      │
├─────────────────────────────────────────────────────┤
│  Tier 3: Live Public APIs (MCP Tools)               │
│  PubMed, ClinicalTrials.gov, NCBI, UniProt          │
│  → Real-time lookups, always current, broad reach   │
└─────────────────────────────────────────────────────┘
```

The Discovery Engine can orchestrate across all three tiers simultaneously.

---

## 4.9 Checkpoint

Before proceeding to Chapter 5, confirm:

- [ ] Agent Plugin Marketplace is configured in VS Code settings
- [ ] BioMCP (PubMed + Clinical Trials) is enabled and working (green dot)
- [ ] NCBI Entrez is enabled and working
- [ ] UniProt is enabled and working
- [ ] You can query multiple external sources in a single Copilot prompt
- [ ] (Optional) A `PublicLiterature` Bookshelf is created and indexed
- [ ] You understand the three-tier data model (Internal Bookshelf → Public Bookshelf → Live APIs)

---

**Previous**: [← Chapter 3 — Internal Data Bookshelf](chapter-03-bookshelf-internal-data.md)
**Next**: [Chapter 5 — Target Identification →](chapter-05-target-identification.md)
