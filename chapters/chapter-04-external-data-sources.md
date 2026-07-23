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

## 4.3 Enable the Agent Plugin Marketplace

The Agent Plugin Marketplace is a Git repository of plugin manifests for curated scientific MCP servers.

### Step 1: Add the Marketplace URL

Open VS Code settings (`Ctrl+,`) or edit `settings.json` directly and add:

```json
{
  "chat.plugins.marketplaces": [
    // Add the Agent Plugin Marketplace URL as documented in the Discovery quickstart
  ]
}
```

> **Note**: Refer to the latest Discovery documentation or the `quickstart.md` in the GitHub repo for the exact marketplace URL.

### Step 2: Reload VS Code

After adding the marketplace URL, reload VS Code (`Ctrl+Shift+P` → `Developer: Reload Window`).

---

## 4.4 Enable Biomedical MCP Tools

Open the **Tool Catalog** panel in the Discovery sidebar. You should now see available tools from the marketplace.

### Enable the following tools for our target-identification workflow:

#### 1. BioMCP (PubMed + Clinical Trials) 🧬

**What it provides**: Semantic search over PubMed articles and ClinicalTrials.gov entries.

1. Find **BioMCP** in the Tool Catalog panel.
2. Toggle it **on**.
3. Wait for the green health dot to confirm the plugin is connected.

**Task 4.1** — Test PubMed access:
```
Using PubMed, find recent publications on [your target gene] and its role 
in [your disease]. Summarize the top 5 findings.
```

**Task 4.2** — Test ClinicalTrials.gov access:
```
Search ClinicalTrials.gov for any active clinical trials targeting 
[your gene/pathway] for [your disease]. What compounds are being tested?
```

#### 2. NCBI Entrez (Gene / Protein / Nucleotide) 🧬

**What it provides**: Direct access to NCBI gene records, protein entries, and nucleotide sequences.

1. Find **NCBI Entrez** in the Tool Catalog.
2. Toggle it **on**.

**Task 4.3** — Gene lookup:
```
Look up gene [your gene name] in NCBI. What is its full name, chromosomal 
location, and known function? What pathways is it involved in?
```

#### 3. UniProt (Protein Sequences & Annotations) 🧬

**What it provides**: Protein sequence data, functional annotations, interaction partners, and disease associations.

1. Find **UniProt** in the Tool Catalog.
2. Toggle it **on**.

**Task 4.4** — Protein function:
```
Using UniProt, describe the protein encoded by [your gene]. What are its 
known interaction partners and disease associations? Is it considered 
druggable?
```

#### 4. arXiv & bioRxiv (Preprints) 📄

**What it provides**: Latest preprints that may not yet be indexed in PubMed.

1. Find **arXiv** and **bioRxiv/medRxiv** in the Tool Catalog.
2. Toggle them **on**.

**Task 4.5** — Latest research:
```
Find recent bioRxiv preprints on [your gene/pathway] in [your disease] 
from the last 6 months. Are there any new findings not yet in PubMed?
```

---

## 4.5 Querying Multiple External Sources Simultaneously

Now that your tools are enabled, you can ask questions that span multiple public databases in a single prompt:

**Task 4.6** — Multi-source query:
```
For gene [GENE_NAME]:
1. What does PubMed say about its role in [DISEASE]?
2. Are there any clinical trials targeting it?
3. What is the protein's function according to UniProt?
4. Are there any recent preprints with new findings?

Synthesize the information and assess whether this gene is a viable 
therapeutic target.
```

> **This is the power of Discovery**: A question that would take a biologist hours of manual searching across multiple websites is answered in seconds, with citations.

---

## 4.6 (Optional) Create a "Public Literature" Bookshelf

For deeper analysis, you can curate a collection of key public papers and index them in a second Bookshelf. This gives you GraphRAG-powered reasoning over public literature, not just live API lookups.

### Step 1: Curate Public Papers

Create a folder with downloaded PDFs of key publications:

```
C:\MicrosoftDiscoveryLab\sample-data\public-literature\
├── review-disease-X-targets-2024.pdf
├── rna-seq-meta-analysis-disease-X.pdf
├── immunology-pathway-review.pdf
├── gene-XYZ-functional-study.pdf
└── clinical-evidence-target-candidates.pdf
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
Compare what our internal experimental data says about [GENE] with what 
the published literature reports. Are our findings consistent with the 
public evidence? Where do they diverge?
```

---

## 4.7 Understanding Data Source Tiers

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

## 4.8 Checkpoint

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
