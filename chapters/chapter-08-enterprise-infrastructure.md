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

## 8.14 (Optional) Deploy Everything with GitHub Copilot Agent Mode

Instead of manually executing Steps 1-9 above, you can use **GitHub Copilot Agent Mode** in VS Code to deploy the entire Microsoft Discovery infrastructure in a single conversational interaction. This uses the official Bicep template from [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.discovery/discovery-infra-deployment).

### Prerequisites for Agent Mode Deployment

- [ ] Azure CLI installed and authenticated (`az login`)
- [ ] GitHub Copilot with Agent Mode enabled in VS Code
- [ ] Azure subscription allow-listed for Microsoft Discovery
- [ ] `Microsoft.Discovery` resource provider registered on your subscription
- [ ] Discovery NSP Perimeter Joiner custom role created and assigned (see [Configure NSP](https://learn.microsoft.com/en-us/azure/microsoft-discovery/how-to-configure-network-security))
- [ ] Sufficient quota reservations in your target region

### Step 1: Open Copilot Agent Mode

1. Open VS Code in the `C:\MicrosoftDiscoveryLab\workspace` folder.
2. Press `Ctrl+Shift+I` to open **Copilot Chat in Agent Mode** (or click the Copilot icon → select **Agent**).
3. Agent Mode gives Copilot the ability to run terminal commands, create files, and iterate on errors autonomously.

### Step 2: Prompt Copilot to Deploy the Infrastructure

Paste the following prompt into the Agent Mode chat:

```
Deploy a complete Microsoft Discovery infrastructure on Azure using Bicep. 
Use the official quickstart template from:
https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.discovery/discovery-infra-deployment

Configuration:
- Region: eastus
- Supercomputer name: sc-ra-targetlab
- Workspace name: ws-ra-targetlab
- Node pool VM SKU: Standard_D4s_v6 (scale 0-3 nodes)
- Chat model: gpt-5.2 (deployment name: gpt-5-2)
- Storage container: stc-ra-targetlab
- Project name: prj-ra-targetlab
- Enable GitHub Copilot AI features: true
- Enable VS Code extensions: true
- Network isolation: false (public access for lab)

Steps:
1. Create a resource group named "rg-discovery-lab-eastus" in eastus
2. Download/create the Bicep template (main.bicep) with all resources:
   VNet (10.0.0.0/16 with 6 subnets), UAMI with role assignments, 
   storage account with CORS, supercomputer with node pool, workspace, 
   chat model deployment, storage container, and project
3. Deploy using: az deployment group create
4. Validate the deployment succeeded
5. List all created resources

Run the commands in the terminal. Fix any errors that occur.
```

### Step 3: Watch Copilot Execute

In Agent Mode, Copilot will:

1. **Create the Bicep file** — writes `main.bicep` with all resource definitions (VNet, UAMI, storage, supercomputer, workspace, project)
2. **Create the resource group** — runs `az group create --name rg-discovery-lab-eastus --location eastus`
3. **Deploy the template** — runs:
   ```
   az deployment group create \
     --resource-group rg-discovery-lab-eastus \
     --template-file main.bicep \
     --parameters location=eastus \
       supercomputerName=sc-ra-targetlab \
       workspaceName=ws-ra-targetlab \
       nodePoolVmSize=Standard_D4s_v6 \
       chatModelDeploymentName=gpt-5-2 \
       chatModelName=gpt-5.2 \
       storageContainerName=stc-ra-targetlab \
       projectName=prj-ra-targetlab
   ```
4. **Handle errors** — if deployment fails (quota, naming conflict, missing provider), Copilot reads the error message and proposes a fix
5. **Validate** — lists resources to confirm everything was created

### What the Bicep Template Creates

The official template deploys all resources in a single atomic operation:

| Resource | Type | Purpose |
|----------|------|---------|
| Virtual Network | `Microsoft.Network/virtualNetworks` | 6 subnets (nodepool, AKS, workspace, private endpoint, agent, search) with appropriate delegations |
| Managed Identity | `Microsoft.ManagedIdentity/userAssignedIdentities` | UAMI for supercomputer + workspace authentication |
| Role Assignments | `Microsoft.Authorization/roleAssignments` | Storage Blob Data Contributor, Discovery Platform Contributor, AcrPull |
| Storage Account | `Microsoft.Storage/storageAccounts` | Standard_LRS with CORS for Discovery Studio + VS Code |
| Blob Container | `Microsoft.Storage/.../containers` | `discoveryoutputs` container for data |
| Supercomputer | `Microsoft.Discovery/supercomputers` | Compute backbone with cluster/kubelet/workload identities |
| Node Pool | `Microsoft.Discovery/supercomputers/nodePools` | Scalable VM pool (0-3 nodes) for tool execution and indexing |
| Workspace | `Microsoft.Discovery/workspaces` | Collaborative environment linked to supercomputer |
| Chat Model | `Microsoft.Discovery/workspaces/chatModelDeployments` | LLM deployment for agents and Discovery Engine |
| Storage Container | `Microsoft.Discovery/storageContainers` | Discovery-managed storage linked to blob account |
| Project | `Microsoft.Discovery/workspaces/projects` | Research project with agent and session support |

### Step 4: Post-Deployment Setup

After Copilot completes the deployment, you still need to:

1. **Assign persona roles to yourself** — run this in the terminal (or ask Copilot to do it):
   ```powershell
   # Assign Platform Administrator persona roles
   # Use the Set-DiscoveryRoleAssignments.ps1 script:
   Invoke-WebRequest -Uri "https://raw.githubusercontent.com/Azure/microsoft-discovery/main/scripts/Set-DiscoveryRoleAssignments.ps1" -OutFile Set-DiscoveryRoleAssignments.ps1
   ./Set-DiscoveryRoleAssignments.ps1 -SubscriptionId <your-sub-id> -ResourceGroupName rg-discovery-lab-eastus -UserObjectId <your-entra-object-id> -Persona PlatformAdministrator
   ```

2. **Assign Foundry User role on the managed resource group** — find the managed RG name on the workspace overview page, then assign `Foundry User` to your account.

3. **Sign in to Discovery Studio** — navigate to https://studio.discovery.microsoft.com and verify your workspace, project, and agent are visible.

### Troubleshooting with Agent Mode

If deployment fails, Copilot Agent Mode excels at diagnosing and fixing issues. Common scenarios:

| Error | Copilot's Fix |
|-------|--------------|
| `QuotaExceeded` for VM SKU | Switches to a different SKU or region |
| `ResourceProviderNotRegistered` | Runs `az provider register --namespace Microsoft.Discovery` |
| `SubnetDelegationConflict` | Adjusts subnet configuration |
| `NameNotAvailable` (storage account) | Generates a new globally unique name |
| `RoleAssignmentAlreadyExists` | Skips the duplicate and continues |

Simply tell Copilot: "The deployment failed. Fix the error and retry."

### Why Use Agent Mode?

| Aspect | Manual (Steps 5-13) | Agent Mode |
|--------|---------------------|------------|
| **Time** | 30-60 minutes clicking through Azure portal | ~10 minutes (mostly waiting for provisioning) |
| **Errors** | Must interpret and fix manually | Copilot reads errors and auto-fixes |
| **Repeatability** | Must remember all steps | Bicep file is reusable IaC |
| **Learning** | Good for understanding each resource | Good for getting started fast |
| **Customization** | Flexible per-resource control | Modify the prompt or Bicep params |

> **Recommendation**: Use the manual portal walkthrough (Steps 5-13) the first time to understand each resource. Use Agent Mode for subsequent deployments or when spinning up new environments quickly.

### Clean Up Resources (When Done with Lab)

To delete all resources created by this deployment:

```
Ask Copilot Agent Mode:
"Delete the resource group rg-discovery-lab-eastus and all its contents."
```

Or run manually:
```powershell
az group delete --name rg-discovery-lab-eastus --yes --no-wait
```

---

## 8.15 Enterprise vs. Local — Side-by-Side Workflow Comparison

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

## 8.16 Checkpoint

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
