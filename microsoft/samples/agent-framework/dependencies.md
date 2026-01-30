# Microsoft Agent Framework - Dependency Landscape

**Part of**: [Microsoft Agent Framework Sample Synthesis](README.md)
**Generated**: 2026-01-29 | **Version**: v1

---

## Required Dependencies (Universal)

These packages are required for most Microsoft Agent Framework usage:

| Package | Purpose |
|---------|---------|
| Microsoft.Agents.AI | Core agent abstractions (AIAgent, AgentThread, ChatClientAgent) |
| Azure.AI.OpenAI | Azure OpenAI service client (AzureOpenAIClient, ChatClient) |
| Azure.Identity | Azure credential management (AzureCliCredential, DefaultAzureCredential) |
| Microsoft.Extensions.AI | AI abstractions (IChatClient, AITool, AIFunctionFactory) |

---

## Conditional Dependencies (Per Capability)

These packages are needed only for specific capabilities:

| Capability | Package | When Needed |
|------------|---------|-------------|
| **Workflow Orchestration** | Microsoft.Agents.AI.Workflows | Building programmatic workflows with WorkflowBuilder |
| **Declarative Workflows** | Microsoft.Agents.AI.Workflows.Declarative | Parsing YAML workflow definitions |
| **Declarative with Foundry** | Microsoft.Agents.AI.Workflows.Declarative.AzureAI | InvokeAzureAgent action in YAML |
| **Azure AI Foundry Agents** | Azure.AI.Agents.Persistent | PersistentAgentsClient, HostedMcpServerTool |
| **Azure AI Projects** | Azure.AI.Projects | AIProjectClient for Foundry projects |
| **A2A Protocol (Client)** | Microsoft.Agents.AI.A2A | A2ACardResolver, A2AClient |
| **A2A Protocol (Server)** | Microsoft.Agents.AI.A2A | MapA2A extension, AgentCard |
| **MCP Client (Stdio)** | ModelContextProtocol, ModelContextProtocol.Client | McpClient, StdioClientTransport |
| **MCP Client (HTTP)** | ModelContextProtocol, ModelContextProtocol.Client | HttpClientTransport, OAuth flow |
| **MCP Server** | ModelContextProtocol.Server | McpServerTool, hosting agent as MCP |
| **Memory (Mem0)** | Microsoft.Agents.AI.Mem0 | Mem0Provider integration |
| **Vector Store (In-Memory)** | Microsoft.SemanticKernel.Connectors.InMemory | InMemoryVectorStore |
| **Vector Store (Qdrant)** | Microsoft.SemanticKernel.Connectors.Qdrant | QdrantVectorStore |
| **OpenTelemetry (Core)** | OpenTelemetry, OpenTelemetry.Extensions.Hosting | Tracing and metrics infrastructure |
| **OpenTelemetry (OTLP)** | OpenTelemetry.Exporter.OpenTelemetryProtocol | OTLP export to Aspire Dashboard |
| **OpenTelemetry (Azure)** | Azure.Monitor.OpenTelemetry.Exporter | Azure Application Insights export |
| **ASP.NET Core** | Microsoft.AspNetCore.* | A2A server hosting (WebApplication) |
| **Generic Host** | Microsoft.Extensions.Hosting | DI hosted services (IHostedService) |
| **Hosted Agent Deployment** | Azure.AI.AgentServer.AgentFramework | RunAIAgentAsync extension |

---

## Dependency Matrix by Sample Category

| Sample Category | Core | Workflows | Foundry | Protocols | Observability |
|-----------------|------|-----------|---------|-----------|---------------|
| Agents/Step01-08 | ✓ | — | — | — | Step08 only |
| Agents/Step09-20 | ✓ | — | Step10, 18 | Step10 | — |
| _Foundational/* | ✓ | ✓ | — | — | — |
| AgentWithRAG/* | ✓ | — | Step04 | — | — |
| AgentWithMemory/* | ✓ | — | — | — | — |
| ModelContextProtocol/* | ✓ | — | 3-4 only | ✓ | — |
| A2AClientServer | ✓ | — | optional | ✓ (A2A) | — |
| HostedAgents/* | ✓ | optional | — | MCP only | — |
| Declarative/* | ✓ | ✓ (Decl.) | most | — | — |
| Concurrent/* | — | ✓ | — | — | — |
| Checkpoint/* | — | ✓ | — | — | — |
| Observability/* | ✓ | ✓ | — | — | ✓ |

---

## Capability Domain Summary

| Domain | Patterns | Samples | Dependencies | Key Tensions |
|--------|----------|---------|--------------|--------------|
| Agent Creation | 15 | 22 | Core only | AzureCliCredential vs DefaultAzureCredential |
| State Management | 9 | 15 | Core + optional VectorStore | Thread vs Workflow state |
| Workflow Orchestration | 20 | 12 | Workflows package | Programmatic vs Declarative |
| Protocol Integration | 15 | 10 | A2A and/or MCP packages | Client vs Server-side invocation |
| Tool Integration | 6 | 10 | Core only | Approval vs auto-execution |
| Memory Systems | 12 | 7 | Core + VectorStore/Mem0 | Full history vs extracted memories |
| Agent Hosting | 10 | 8 | Varies by host type | Implicit vs explicit lifecycle |
| Observability | 8 | 4 | OpenTelemetry packages | OTLP vs Azure Monitor export |
| Streaming | 8 | 15 | Core/Workflows | Sync vs streaming execution |
| Authentication | 3 | 22 | Azure.Identity | Credential source selection |

---

## Package Version Observations

Based on sample csproj files:

| Package Family | Notes |
|----------------|-------|
| Microsoft.Agents.AI.* | Core framework packages; typically same version |
| Azure.AI.OpenAI | Major dependency; API surface drives agent patterns |
| Azure.AI.Agents.Persistent | Foundry integration; requires Foundry project |
| ModelContextProtocol.* | MCP SDK; separate versioning from Agent Framework |
| Microsoft.SemanticKernel.Connectors.* | Vector store implementations |
| OpenTelemetry.* | Standard OpenTelemetry packages |

---

## Runtime Dependencies

| Capability | Runtime Requirement |
|------------|---------------------|
| MCP (Stdio) | Node.js and npx installed |
| MCP (OAuth) | Browser for authorization flow |
| Qdrant Vector Store | Qdrant server running |
| Mem0 | Mem0 service accessible at configured endpoint |
| Azure AI Foundry | Azure subscription with Foundry project |
| OTLP Export | OTLP receiver (e.g., Aspire Dashboard) |
| Azure Monitor | Application Insights resource |
| Hosted Agents | Azure AI AgentServer infrastructure |

---

## Docker Build Considerations

Observed in HostedAgents samples:

| Constraint | Reason | Mitigation |
|------------|--------|------------|
| `ManagePackageVersionsCentrally=false` | Docker context lacks parent folders | Disable central versioning in project |
| Explicit analyzer removal | Inherited via MSBuild imports | `<PackageReference Remove="..."/>` |
| Inline package versions | No Directory.Packages.props access | Specify versions in project file |

---

## Minimal Dependency Sets

### Scenario: Basic Agent with Chat

```xml
<PackageReference Include="Microsoft.Agents.AI" />
<PackageReference Include="Azure.AI.OpenAI" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: Agent with Function Tools

```xml
<PackageReference Include="Microsoft.Agents.AI" />
<PackageReference Include="Microsoft.Extensions.AI" />
<PackageReference Include="Azure.AI.OpenAI" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: Programmatic Workflows

```xml
<PackageReference Include="Microsoft.Agents.AI.Workflows" />
<PackageReference Include="Microsoft.Agents.AI" />
<PackageReference Include="Azure.AI.OpenAI" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: Declarative Workflows with Foundry

```xml
<PackageReference Include="Microsoft.Agents.AI.Workflows.Declarative.AzureAI" />
<PackageReference Include="Microsoft.Agents.AI.Workflows.Declarative" />
<PackageReference Include="Azure.AI.Projects" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: A2A Client

```xml
<PackageReference Include="Microsoft.Agents.AI.A2A" />
<PackageReference Include="Microsoft.Agents.AI" />
<PackageReference Include="Azure.AI.OpenAI" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: MCP Client (Stdio)

```xml
<PackageReference Include="ModelContextProtocol" />
<PackageReference Include="ModelContextProtocol.Client" />
<PackageReference Include="Microsoft.Agents.AI" />
<PackageReference Include="Azure.AI.OpenAI" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: MCP Hosted (Server-Side)

```xml
<PackageReference Include="Azure.AI.Agents.Persistent" />
<PackageReference Include="Azure.Identity" />
<!-- No ModelContextProtocol packages needed -->
```

### Scenario: RAG with Vector Store

```xml
<PackageReference Include="Microsoft.Agents.AI" />
<PackageReference Include="Microsoft.SemanticKernel.Connectors.InMemory" />
<!-- or Microsoft.SemanticKernel.Connectors.Qdrant -->
<PackageReference Include="Microsoft.Extensions.VectorData" />
<PackageReference Include="Azure.AI.OpenAI" />
<PackageReference Include="Azure.Identity" />
```

### Scenario: OpenTelemetry Instrumentation

```xml
<PackageReference Include="OpenTelemetry" />
<PackageReference Include="OpenTelemetry.Extensions.Hosting" />
<PackageReference Include="OpenTelemetry.Exporter.OpenTelemetryProtocol" />
<!-- or Azure.Monitor.OpenTelemetry.Exporter -->
```

---

## See Also

- [patterns.md](patterns.md) — How these dependencies are used
- [coverage.md](coverage.md) — What capabilities were analyzed
