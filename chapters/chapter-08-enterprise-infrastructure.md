# Chapter 8: Enterprise Infrastructure Deployment

> **Goal**: Deploy the full Microsoft Discovery enterprise platform on Azure — workspace, supercomputer, node pools, storage, projects, agents, and shared sessions. This is the cloud-scale version of what you did locally in Chapters 1-7, enabling team collaboration, HPC compute, and autonomous multi-day investigations.

---

## 8.1 What You Will Learn

- How to deploy the complete Microsoft Discovery infrastructure on Azure
- The difference between the Discovery App (local) and Discovery Services (cloud)
- How to create a supercomputer with node pools for HPC workloads
- How to create workspaces and projects for team-based research
- How to create agents and shared sessions in Discovery Studio
- How to connect storage containers for your data
- Enterprise security: private networking, managed identities, RBAC

---

## 8.2 Why Move to Enterprise? — Local vs. Cloud Comparison

You completed Chapters 1-7 entirely on your laptop. Here's what the enterprise platform adds:

| Capability | Discovery App (Ch. 1-7) | Enterprise Discovery (This Chapter) |
|------------|------------------------|--------------------------------------|
| **Bookshelf scale** | Limited by laptop RAM | Up to 1 GB indexed text (Azure AI Search + GraphRAG) |
| **Compute** | Local CPU only | Supercomputer node pools (20-96 vCPU, 160-768 GB RAM, GPUs) |
| **Models** | Bundled ONNX (offline) or bring-your-own | Managed GPT-5.4 deployments with enterprise SLA |
| **Collaboration** | Single user, files on disk | Multi-user projects, shared sessions, RBAC |
| **Discovery Engine** | Local cognition loop | Full autonomous engine running for hours/days |
| **Agents** | GitHub Copilot skills (local) | Prompt agents in Discovery Studio + Foundry Agent Service |
| **Interface** | VS Code + dx CLI | Discovery Studio (web) + VS Code + dx CLI |
| **Data governance** | Files in `.discovery/` | Azure Blob Storage + managed identities + encryption |
| **Networking** | Open | Private endpoints, VNet isolation, NSG |
| **Indexing throughput** | Minutes for small corpora | Parallel indexing on memory-optimized VMs |

**When to use enterprise**:
- Your Bookshelf corpus exceeds what fits in laptop memory
- You need GPUs for simulations or large-scale virtual screening
- Multiple team members need shared access to agents and knowledge bases
- You need enterprise governance (audit trails, RBAC, encryption)
- You want the Discovery Engine to run autonomously for extended periods
- You need to index more than a few hundred documents

---

## 8.3 Architecture Overview

```
Azure Subscription
└── Resource Group (e.g., rg-discovery-lab-eastus)
    │
    ├── Virtual Network (10.0.0.0/16)
    │   ├── supercomputerNodepoolSubnet (10.0.1.0/24)
    │   ├── aksSubnet (10.0.2.0/24)
    │   ├── workspaceSubnet (10.0.3.0/24) — delegated: Microsoft.App/environments
    │   ├── privateEndpointSubnet (10.0.4.0/24)
    │   ├── agentSubnet (10.0.5.0/24) — delegated: Microsoft.App/environments
    │   └── searchSubnet (10.0.6.0/24) — delegated: Microsoft.App/environments
    │
    ├── User Assigned Managed Identity (UAMI)
    │   └── Roles: Platform Contributor, Storage Blob Data Contributor, ACRPull
    │
    ├── Storage Account (Azure Blob Storage)
    │   └── Container: discoveryoutputs
    │
    ├── Supercomputer (Microsoft.Discovery/supercomputers)
    │   └── Node Pool (VM SKU: Standard_D4s_v6 or E-series for HPC)
    │
    └── Workspace (Microsoft.Discovery/workspaces)
        ├── Chat Model Deployment (gpt-5-4 → model: gpt-5.4)
        ├── Managed Resource Group (auto-provisioned)
        │   └── Foundry Agent Service resources
        └── Project
            ├── Default Discovery Agent
            ├── Custom Prompt Agents (your target assessment agents)
            ├── Storage Container → linked to blob storage
            └── Shared Sessions → your research conversations
```

---

## 8.4 Prerequisites

Before starting, ensure you have:

- [ ] An active Azure subscription enabled for Microsoft Discovery
- [ ] Owner or Contributor role on the subscription/resource group
- [ ] Resource provider `Microsoft.Discovery` registered on your subscription
- [ ] Additional resource providers registered: `Microsoft.Network`, `Microsoft.Compute`, `Microsoft.Storage`, `Microsoft.ManagedIdentity`, `Microsoft.CognitiveServices`, `Microsoft.ContainerService`, `Microsoft.KeyVault`, `Microsoft.Search`, `Microsoft.Sql`, `Microsoft.App`
- [ ] Available quota in one of the supported regions: **East US**, **Sweden Central**, or **UK South**
- [ ] Azure OpenAI quota and VM SKU quota available

### Register the Microsoft.Discovery Resource Provider

1. Sign in to the [Azure portal](https://portal.azure.com/)
2. Navigate to **Subscriptions** → select your subscription
3. In the left menu, select **Resource Providers**
4. Search for `Microsoft.Discovery`
5. Select the provider and click **Register**

---

## 8.5 Step 1: Set Up Networking

Create a Virtual Network with the required subnets:

1. In the Azure portal, search for **Virtual networks** → **Create**
2. Configure:
   - **Subscription**: Your subscription
   - **Resource group**: Create or select (e.g., `rg-discovery-lab-eastus`)
   - **Name**: `vnet-discovery-lab`
   - **Region**: East US (or your chosen supported region)
3. Configure IP addresses:
   - **IPv4 address space**: `10.0.0.0/16`
   - Add subnets:

| Subnet Name | CIDR | Delegation | Service Endpoints |
|-------------|------|------------|-------------------|
| `supercomputerNodepoolSubnet` | `10.0.1.0/24` | None | `Microsoft.Storage` |
| `aksSubnet` | `10.0.2.0/24` | None | `Microsoft.Storage` |
| `workspaceSubnet` | `10.0.3.0/24` | `Microsoft.App/environments` | `Microsoft.Storage` |
| `privateEndpointSubnet` | `10.0.4.0/24` | None | None |
| `agentSubnet` | `10.0.5.0/24` | `Microsoft.App/environments` | `Microsoft.Storage` |
| `searchSubnet` | `10.0.6.0/24` | `Microsoft.App/environments` | None |

4. Review and create the virtual network.

---

## 8.6 Step 2: Create Managed Identity and Assign Roles

### Create the UAMI

1. Search for **Managed Identities** → **Create**
2. Configure: same subscription, resource group, region, and a name (e.g., `uami-discovery-lab`)
3. Create

### Assign RBAC Roles to the UAMI

At the **resource group** level, assign these roles to the UAMI:

- **Microsoft Discovery Platform Contributor (Preview)**
- **Storage Blob Data Contributor**
- **ACRPull**

### Assign Roles to Yourself (Administrator)

At the subscription or resource group level, assign these roles to your user account:

- Microsoft Discovery Platform Administrator (Preview)
- Managed Identity Contributor
- Managed Identity Operator
- Storage Account Contributor
- Storage Blob Data Contributor
- Network Contributor
- ACRPush
- Foundry User
- Microsoft Discovery Bookshelf Index Data Reader (Preview)

> **Tip**: Use the `Set-DiscoveryRoleAssignments.ps1` PowerShell script for a single, idempotent assignment of all persona roles. See [Assign persona roles](https://learn.microsoft.com/en-us/azure/microsoft-discovery/how-to-assign-persona-roles).

---

## 8.7 Step 3: Create Storage Account

1. Search for **Storage accounts** → **Create**
2. Configure:
   - **Name**: globally unique (e.g., `stdiscoverylabeastus`)
   - **Region**: Same as your VNet
   - **Primary service**: Azure Blob Storage
3. On the **Networking** tab:
   - Public access: **Enable from selected virtual networks and IP addresses**
   - Add your VNet and all subnets
   - Add your client IP address
4. Create the storage account

### Create the Output Container

1. Navigate to the storage account → **Containers**
2. Click **+ Container** → name it `discoveryoutputs` → Create

### Configure CORS

1. In the storage account → **Settings** → **Resource sharing (CORS)**
2. Under **Blob service**, add:
   - Allowed origins: `https://studio.discovery.microsoft.com`, `https://vscode.dev`, `https://*.vscode-cdn.net`
   - Allowed methods: GET, HEAD, DELETE, OPTIONS, PUT
   - Allowed headers: `*`
   - Exposed headers: `*`
   - Max age: `200`
3. Save

---

## 8.8 Step 4: Create the Supercomputer

The supercomputer provides HPC compute for tool execution, Bookshelf indexing, and scientific simulations.

1. Search for **Microsoft Discovery Supercomputers** → **Create**
2. **Basics**: Subscription, Resource Group, Location (same region), Name (e.g., `sc-discovery-lab`)
3. **Networking**: Select your VNet and `aksSubnet`
4. **System SKU**: Select `Standard_D4s_v6`
5. **Identities**: Add your UAMI for cluster identity, kubelet identity, and workload identity
6. **Encryption**: Use Microsoft-managed keys (uncheck CMK)
7. Review and Create

### Create a Node Pool

After the supercomputer is provisioned:

1. Open the supercomputer resource → **Settings** → **Node pool** → **Create**
2. **Name**: `targetanalysis` (lowercase, max 12 chars)
3. **Networking**: Same VNet, select `supercomputerNodepoolSubnet`
4. **VM SKU**: Choose based on your workload:

| Workload | Recommended SKU | vCPU | Memory |
|----------|----------------|------|--------|
| General tool execution | `Standard_D4s_v6` | 4 | 16 GB |
| Bookshelf indexing (small) | `Standard_E20s_v6` | 20 | 160 GB |
| Bookshelf indexing (medium) | `Standard_E64s_v6` | 64 | 512 GB |
| Large-scale analysis | `Standard_E96s_v6` | 96 | 768 GB |

5. **Scaling**: Max node count (e.g., 5)
6. Create

---

## 8.9 Step 5: Create the Workspace

A workspace is the collaborative environment that brings together supercomputers, agents, tools, and Bookshelves.

1. Search for **Microsoft Discovery Workspaces** → **Create**
2. **Basics**:
   - Name: globally unique, lowercase (e.g., `ws-targetlab`)
   - Region: same as other resources
3. **Networking**:
   - Public network access: Enable (for this lab)
   - Private Endpoint subnet: `privateEndpointSubnet`
   - Agent subnet: `agentSubnet`
   - Workspace subnet: `workspaceSubnet`
4. **Encryption**: Microsoft-Managed Keys (uncheck CMK)
5. **Supercomputer**: Add the supercomputer created in Step 4
6. **Workspace Identity**: Add your UAMI
7. **Tags** (important for features):
   - `discovery.workbench.enableGhcpAiFeatures` = `true` (enables GitHub Copilot)
   - `discovery.workbench.enableExtensions` = `true` (enables VS Code extensions)
   - `NetworkIsolation` = `false` (enables public access for this lab)
8. Review and Create

### Assign Foundry User Role on Managed Resource Group

After the workspace is created:

1. Find the **Managed Resource Group** name on the workspace overview page
2. Navigate to that managed resource group → **Access control (IAM)**
3. Add role assignment → **Foundry User** → assign to all users who need agent access

---

## 8.10 Step 6: Create Chat Model Deployment

The Discovery Engine requires a chat model to function.

1. Open your workspace in the Azure portal
2. **Settings** → **Chat Model Deployments** → **+ Create**
3. Configure:
   - **Name**: `gpt-5-4` (this exact name is required for Discovery Engine)
   - **Model Format**: OpenAI
   - **Model Name**: `gpt-5.4`
4. Review + Create

> **Important**: The name `gpt-5-4` with model `gpt-5.4` is required for Discovery Engine and tasks to work within sessions. You can create additional deployments for agents.

---

## 8.11 Step 7: Sign In to Discovery Studio & Create Project

### Sign In

1. Navigate to **https://studio.discovery.microsoft.com/**
2. Sign in with your Entra ID (work/school account)
3. Verify you land on the Discovery tab

### Create Storage Container

1. In Studio, select **Data** tab → **Storage Containers** → **Create Container**
2. Enter name, subscription, resource group, location
3. Select the storage account from Step 3
4. Wait for provisioning to succeed

### Create Project

A project defines the functional boundary for your agents, tools, and data:

1. Select **Projects** → **Create Project**
2. Enter:
   - **Name**: lowercase, max 12 chars (e.g., `targetlab`)
   - **Workspace**: select your workspace
   - Uncheck "Create storage container for me"
   - Select the storage container you just created
3. Create and wait for provisioning

---

## 8.12 Step 8: Create Your Target Assessment Agent

### Create a Prompt Agent in Discovery Studio

1. Open your project in Discovery Studio
2. In the **Resources** pane, click **+** next to **Agents** → **Create new agent**
3. Configure:
   - **Name**: `TargetAssessmentAgent`
   - **Description**: `Specialized agent for drug target identification, prioritization, and validation. Scores candidate therapeutic targets using a multi-dimensional evidence framework.`
   - **Chat model**: Select `gpt-5-4`
   - **Instructions**:
     ```
     You are a drug target assessment specialist. Your role is to:
     1. Identify candidate therapeutic targets by reviewing scientific literature, 
        internal experimental data, and public databases
     2. Score each target on 6 dimensions: genetic evidence, biological rationale, 
        druggability, clinical precedent, internal data support, safety/selectivity
     3. Provide explainable rankings where every score cites specific evidence
     4. Cross-reference findings against independent datasets for validation
     5. Identify evidence gaps and recommend next experiments
     
     Always cite your sources. When using a Bookshelf, include document references.
     When using external tools, include database identifiers (PMID, NCT numbers, 
     UniProt IDs). Present results in structured tables when possible.
     ```
4. **Tools** (optional): Expand the Tools section and attach available tools (Microsoft Discovery tool, code interpreter)
5. **Knowledge Bases** (optional): If you've created enterprise Bookshelves, attach them here
6. Click **Create agent**

### Test the Agent

In your shared session, type:
```
@TargetAssessmentAgent What are the key considerations when selecting 
a therapeutic target for an autoimmune disease?
```

---

## 8.13 Step 9: Create a Shared Session and Start Researching

### Create a Shared Session

1. In Discovery Studio, navigate to your project
2. Type a prompt in the chat box (a new shared session is created automatically)
3. Or select **New shared session** from the Discovery tab

### Run Your First Enterprise Investigation

**Task 8.1** — Test the default Discovery agent:
```
I need to identify therapeutic targets for Rheumatoid Arthritis. Help me plan 
a systematic approach using available knowledge bases and tools.
```

**Task 8.2** — Use your custom agent:
```
@TargetAssessmentAgent Analyze gene TYK2 as a potential 
therapeutic target for Rheumatoid Arthritis. Score it on all 6 dimensions of 
the target assessment framework.
```

**Task 8.3** — Multi-agent collaboration:
```
Start a comprehensive target identification investigation for Rheumatoid Arthritis. 
Break this into sub-tasks: literature review, expression analysis, 
druggability assessment, and clinical evidence review. Coordinate 
across available knowledge bases and tools.
```

---

## 8.14 Enterprise vs. Local — Side-by-Side Workflow Comparison

| Workflow Step | Discovery App (Local) | Discovery Services (Enterprise) |
|-------------|----------------------|----------------------------------|
| Create workspace | `dx init --workspace .` | Azure Portal → Create Workspace |
| Build Bookshelf | Sidebar → ingest local PDFs | Studio → Create Bookshelf → Azure Blob → Index on supercomputer |
| Create agent | `.agent.md` file + Copilot skills | Discovery Studio UI → prompt agent form |
| Run investigation | Copilot Chat + local engine | Shared session → Discovery Engine (autonomous, multi-day) |
| Query knowledge | Copilot Chat with bookshelf tool | `@AgentName` in shared session with enterprise KB |
| Use tools | MCP plugins in VS Code | Tools attached to agents in Studio |
| Scale compute | Laptop CPU/RAM | Supercomputer node pool (up to 96 vCPU, 768 GB RAM) |
| Collaborate | Git + share `.discovery/` folder | RBAC + shared projects + team sessions |
| Store results | `.discovery/notebooks/` on disk | Azure Blob Storage + Storage Assets |

---

## 8.15 Checkpoint

Before proceeding to Chapter 9, confirm:

- [ ] Virtual network with 6 subnets is created
- [ ] UAMI is created with correct RBAC roles
- [ ] Storage account with `discoveryoutputs` container exists
- [ ] Supercomputer is provisioned with at least one node pool
- [ ] Workspace is created and linked to the supercomputer
- [ ] Chat model deployment `gpt-5-4` is active
- [ ] Discovery Studio is accessible and you're signed in
- [ ] A project is created with a storage container
- [ ] A custom `TargetAssessmentAgent` is created and tested
- [ ] A shared session is active and you can chat with agents

---

**Previous**: [← Chapter 7 — Target Validation](chapter-07-target-validation.md)
**Next**: [Chapter 9 — HPC & High-Dimensional Analysis →](chapter-09-hpc-and-analysis.md)
