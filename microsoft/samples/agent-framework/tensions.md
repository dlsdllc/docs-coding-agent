# Microsoft Agent Framework - Tensions and Design Choices

**Part of**: [Microsoft Agent Framework Sample Synthesis](README.md)
**Generated**: 2026-01-29 | **Version**: v1

---

## Tension Summary

| Tension | Domain | Approaches | Resolution |
|---------|--------|------------|------------|
| Client-Side vs Server-Side MCP | Protocol Integration | McpClient vs HostedMcpServerTool | Implementer choice |
| Thread State vs Workflow State | State Management | AgentThread vs QueueStateUpdateAsync | Nested hierarchies |
| Programmatic vs Declarative Workflows | Workflow Orchestration | C# vs YAML | Eject as bridge |
| Full History vs Extracted Memories | Memory Systems | Messages vs discrete facts | Implementer choice |
| Implicit vs Explicit Agent Lifecycle | Agent Hosting | Process-scoped vs DeleteAgentAsync | Hosting target determines |
| Per-Skill vs Whole-Agent Tool Granularity | Protocol Integration | Individual AIFunction vs AsAIFunction | Implementer choice |
| Standard vs Private A2A Discovery | Protocol Integration | Well-known path vs direct endpoint | Implementer choice |
| Sync vs Streaming Execution | Workflow Execution | RunAsync vs StreamAsync | StreamAsync preferred |
| Checkpoint Rehydration vs Resume | State Management | New instance vs same instance | Use case determines |
| Storage Scope vs Search Scope | Memory Systems | Per-thread vs cross-thread | Scope design choice |
| Generic Host vs WebApplication | Agent Hosting | DI services vs HTTP endpoints | Requirements determine |
| MCP Auto-Execution vs Approval | Protocol Integration | NeverRequire vs AlwaysRequire | Trust model |
| Stdio vs HTTP MCP Transport | Protocol Integration | Local process vs network service | Topology determines |
| Type-Driven vs Message-Based Routing | Workflow Orchestration | Executor\<T\> vs YAML variables | Adapter pattern |
| In-Thread vs External State Storage | State Management | Serialized vs vector store | Implementer choice |

---

## Tension: Client-Side vs Server-Side MCP Tool Invocation

**Domain**: Protocol Integration
**Manifestation**: ModelContextProtocol samples show two incompatible patterns

### Approach A: Client-Side (McpClient)

- **Observed in**: Agent_MCP_Server, Agent_MCP_Server_Auth
- **Assumes**: Application controls tool discovery and invocation
- **Mechanism**: McpClient instantiated in application; ListToolsAsync() discovers tools; application invokes tools via agent; tool results flow through application code

```csharp
var mcpClient = await McpClient.CreateAsync(
    new StdioClientTransport(new StdioClientTransportOptions
    {
        Name = "server", Command = "npx", Arguments = ["-y", "@pkg/server"]
    }));

var tools = (await mcpClient.ListToolsAsync()).Cast<AITool>().ToArray();
var agent = chatClient.AsAIAgent(instructions: "...", tools: tools);
```

### Approach B: Server-Side (HostedMcpServerTool)

- **Observed in**: ResponseAgent_Hosted_MCP, FoundryAgent_Hosted_MCP, AgentWithHostedMCP
- **Assumes**: Backend service controls tool invocation
- **Mechanism**: HostedMcpServerTool configured with static AllowedTools; no McpClient in application; backend service (OpenAI Responses or Azure AI Foundry) invokes tools; application never sees tool results directly

```csharp
var mcpTool = new HostedMcpServerTool
{
    ServerName = "microsoft_learn",
    ServerAddress = "https://learn.microsoft.com/api/mcp",
    AllowedTools = ["microsoft_docs_search"],
    ApprovalMode = HostedMcpServerToolApprovalMode.NeverRequire
};

var agent = responsesClient.CreateAIAgent(instructions: "...", tools: [mcpTool]);
```

### Nature of Tension

- Different trust boundaries: Client-side allows observability of tool calls; server-side delegates entirely
- Different dependencies: Client-side requires MCP SDK packages; server-side has no MCP packages
- Different tool discovery: Client-side is dynamic (ListToolsAsync); server-side is static (AllowedTools)
- Cannot be mixed within same agent invocation

### Resolution

**Not provided** — this is a design choice for the implementer based on deployment architecture and trust requirements.

---

## Tension: Thread State vs Workflow State

**Domain**: State Management
**Manifestation**: Agents and workflows use different state containers

### Approach A: AgentThread State

- **Observed in**: Agents/Step02, Step06, Step07, AgentWithMemory/*
- **Assumes**: State persists across multiple RunAsync calls; caller manages thread lifecycle
- **Mechanism**: `thread.Serialize()` returns JsonElement; `agent.DeserializeThreadAsync()` restores; ChatHistoryProvider embedded in thread serialization

```csharp
var thread = await agent.GetNewThreadAsync();
await agent.RunAsync("message 1", thread);
await agent.RunAsync("message 2", thread);  // state preserved

var state = thread.Serialize();  // persist
var restored = await agent.DeserializeThreadAsync(state);  // resume
```

### Approach B: Workflow State

- **Observed in**: _Foundational/07-08, Checkpoint/*, Concurrent/*
- **Assumes**: State tied to workflow run; checkpointing required for persistence
- **Mechanism**: `context.QueueStateUpdateAsync(key, value, scope)` writes; `context.ReadStateAsync<T>()` reads; state lifetime matches run unless checkpointed

```csharp
// Inside executor
await context.QueueStateUpdateAsync("counter", count, "MyScope");
var value = await context.ReadStateAsync<int>("counter", "MyScope");

// Checkpointing for persistence
var run = await InProcessExecution.StreamAsync(workflow, input, CheckpointManager.Default);
```

### Nature of Tension

- Different lifecycles: Thread state exists across process restarts (if serialized); workflow state exists within run only (without checkpointing)
- Different APIs: Serialize/Deserialize vs QueueStateUpdateAsync/ReadStateAsync
- Workflows can wrap agents, creating nested state hierarchies where agent thread state is inside workflow execution state

### Resolution

**Not provided** — workflows that contain agents have nested state models; the framework does not unify them.

---

## Tension: Programmatic vs Declarative Workflow Definition

**Domain**: Workflow Orchestration
**Manifestation**: Two syntaxes for defining identical workflows

### Approach A: Programmatic (C# Code)

- **Observed in**: _Foundational/*, Concurrent/*, ConditionalEdges/*, Checkpoint/*
- **Assumes**: Developer writes C# code; compile-time type safety; full IDE support
- **Mechanism**: WorkflowBuilder with AddEdge, AddSwitch, AddFanOutEdge; custom Executor<T> classes

```csharp
var workflow = new WorkflowBuilder(executor1)
    .AddEdge(executor1, executor2)
    .AddSwitch(executor2, sw => {
        sw.AddCase(x => x.IsValid, validHandler);
        sw.WithDefault(invalidHandler);
    })
    .Build();
```

### Approach B: Declarative (YAML)

- **Observed in**: Declarative/* (12 projects)
- **Assumes**: Workflow designers use YAML; Power FX expressions; runtime interpretation
- **Mechanism**: DeclarativeWorkflowBuilder.Build() parses YAML; actions, triggers, variables in YAML syntax

```yaml
kind: Workflow
trigger:
  type: OnConversationStart
  actions:
    - actionId: analyze
      kind: InvokeAzureAgent
      agent: AnalyzerAgent
      output:
        responseObject: Local.Result
    - actionId: branch
      kind: ConditionGroup
      conditions:
        - condition: =Local.Result.IsValid
          actions:
            - kind: SendActivity
              text: "Valid input"
```

### Nature of Tension

- Different skills required: C# expertise vs YAML + Power FX expertise
- Different flexibility: Programmatic allows any C# logic; declarative limited to YAML actions
- Eject feature generates C# from YAML but loses declarative flexibility
- Concurrency patterns (AddFanOutEdge/AddFanInEdge) not demonstrated in YAML

### Resolution

**Not provided** — Eject exists as a one-way bridge; choice depends on team skills and requirements.

---

## Tension: Full History vs Extracted Memories

**Domain**: Memory Systems
**Manifestation**: Different memory models require different retrieval strategies

### Approach A: Full Message History

- **Observed in**: AgentWithMemory/Step01 (ChatHistoryMemoryProvider)
- **Assumes**: Store complete ChatMessage objects; retrieve via vector similarity
- **Mechanism**: Messages stored with embeddings; similarity search retrieves relevant prior messages

```csharp
var provider = new ChatHistoryMemoryProvider(
    vectorStore, collectionName, dimensions,
    storageScope: new { UserId, ThreadId },
    searchScope: new { UserId });
```

### Approach B: Discrete Extracted Memories

- **Observed in**: AgentWithMemory/Step02 (Mem0Provider)
- **Assumes**: External service extracts discrete facts; retrieves relevant memories
- **Mechanism**: Mem0 service automatically extracts memories; returns relevant facts not raw messages

```csharp
var provider = new Mem0Provider(
    httpClient,
    new Mem0ProviderScope { ApplicationId = appId, UserId = userId });
```

### Approach C: Custom Structured Extraction

- **Observed in**: AgentWithMemory/Step03 (UserInfoMemory)
- **Assumes**: Custom extraction logic with structured output; typed properties
- **Mechanism**: GetResponseAsync<T>() extracts typed data; properties stored on provider instance

```csharp
// In InvokedAsync
var userInfo = await _chatClient.GetResponseAsync<UserInfo>(
    context.RequestMessages,
    "Extract user information");
```

### Nature of Tension

- Different storage models: Raw messages vs extracted facts vs typed objects
- Different retrieval: Similarity search vs fact relevance vs direct property access
- Different cross-thread behavior: Scope-based automatic vs explicit copy required

### Resolution

**Not provided** — memory model is a design choice based on use case requirements.

---

## Tension: Implicit vs Explicit Agent Lifecycle

**Domain**: Agent Hosting
**Manifestation**: Different hosting models require different cleanup patterns

### Approach A: Implicit (Process-Scoped)

- **Observed in**: Console samples, DI hosted services (Agents/Step09)
- **Assumes**: Process termination cleans up resources
- **Mechanism**: Agent created at startup; used throughout process; no explicit cleanup

```csharp
var agent = chatClient.AsAIAgent(instructions: "...");
// Use agent throughout process lifetime
// Cleanup implicit when process exits
```

### Approach B: Explicit (DeleteAgentAsync)

- **Observed in**: Agents/Step18, FoundryAgent_Hosted_MCP, AgentWithRAG/Step04
- **Assumes**: Server-side resources require explicit cleanup
- **Mechanism**: CreateAIAgentAsync provisions agent; DeleteAgentAsync required in finally block

```csharp
PersistentAgent? agent = null;
try
{
    agent = await client.CreateAIAgentAsync(...);
    // Use agent
}
finally
{
    if (agent != null)
        await client.DeleteAgentAsync(agent.Id);
}
```

### Nature of Tension

- Resource leaks: Implicit cleanup causes leaks with persistent agents
- Complexity: Explicit cleanup requires try/finally patterns
- Cost implications: Persistent agents may be billed until deleted

### Resolution

**Not provided** — hosting target determines required lifecycle pattern.

---

## Tension: Sync vs Streaming Workflow Execution

**Domain**: Workflow Execution
**Manifestation**: Different execution entry points with different capabilities

### Approach A: RunAsync (Synchronous)

- **Observed in**: _Foundational/01, simple workflow samples
- **Assumes**: Block until workflow completes; return final result
- **Mechanism**: `InProcessExecution.RunAsync(workflow, input)` blocks; returns when complete

```csharp
var result = await InProcessExecution.RunAsync(workflow, "input");
// No intermediate visibility
```

### Approach B: StreamAsync (Streaming)

- **Observed in**: Most workflow samples
- **Assumes**: Return immediately; stream events as they occur
- **Mechanism**: `StreamAsync` returns `StreamingRun`; `WatchStreamAsync` yields events

```csharp
var run = await InProcessExecution.StreamAsync(workflow, "input");
await foreach (var evt in run.WatchStreamAsync())
{
    // Observe intermediate events
    if (evt is WorkflowOutputEvent output)
        Console.WriteLine(output.Data);
}
```

### Nature of Tension

- Observability: RunAsync provides no intermediate visibility
- Control: StreamAsync enables human-in-the-loop via SendResponseAsync
- Checkpointing: Only StreamAsync supports checkpoint capture

### Resolution

**StreamAsync appears to be the preferred default** based on sample prevalence and capability support.

---

## Tension: Checkpoint Rehydration vs Resume

**Domain**: State Management
**Manifestation**: Two patterns for restoring workflow from checkpoint

### Approach A: Rehydration (ResumeStreamAsync)

- **Observed in**: Checkpoint/CheckpointAndRehydrate
- **Assumes**: Create new workflow instance from checkpoint
- **Mechanism**: New `StreamingRun` created; fresh object allocation

```csharp
var newRun = await InProcessExecution.ResumeStreamAsync(
    workflow, checkpointInfo, CheckpointManager.Default);
```

### Approach B: Resume (RestoreCheckpointAsync)

- **Observed in**: Checkpoint/CheckpointAndResume
- **Assumes**: Reuse same workflow instance; rewind state
- **Mechanism**: Existing `StreamingRun` restored; same object references

```csharp
await existingRun.RestoreCheckpointAsync(checkpointInfo);
```

### Nature of Tension

- Memory implications: Rehydration allows garbage collection; resume retains objects
- Process boundaries: Rehydration works across processes; resume requires same process
- Instance affinity: Some executors may have instance-specific state

### Resolution

**Not provided** — use case determines appropriate approach.

---

## Tension: Per-Skill vs Whole-Agent Tool Granularity (A2A)

**Domain**: Protocol Integration
**Manifestation**: Two ways to expose A2A agent capabilities as tools

### Approach A: Per-Skill

- **Observed in**: A2AAgent_AsFunctionTools
- **Assumes**: Each AgentSkill becomes separate AIFunction; AI model decides which skill
- **Mechanism**: Iterate skills; create AIFunction per skill; metadata in JSON description

### Approach B: Whole-Agent

- **Observed in**: A2AClientServer/A2AClient
- **Assumes**: Entire agent is single AIFunction; agent routes internally
- **Mechanism**: `agent.AsAIFunction()` creates single tool

### Nature of Tension

- Granularity: Per-skill enables fine-grained tool selection; whole-agent is coarser
- Schema workaround: Per-skill embeds metadata in description (skills lack schemas)
- Complexity: Per-skill requires iteration and sanitization; whole-agent is simpler

### Resolution

**Not provided** — this is a design choice based on agent capability structure.

---

## See Also

- [patterns.md](patterns.md) — Related implementation patterns
- [vocabulary.md](vocabulary.md) — Types involved in these tensions
- [coverage.md](coverage.md) — Samples demonstrating each approach
