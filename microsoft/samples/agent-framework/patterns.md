# Microsoft Agent Framework - Pattern Catalog

**Part of**: [Microsoft Agent Framework Sample Synthesis](README.md)
**Generated**: 2026-01-29 | **Version**: v1

---

## Pattern Summary

| Category | Pattern Count | Key Samples |
|----------|---------------|-------------|
| Agent Creation & Invocation | 15 | Agents/Step01-05, HostedAgents |
| Thread & Conversation | 8 | Agents/Step02, 06-07, 16 |
| Function Tools | 6 | Agents/Step03-04, 15 |
| Middleware | 5 | Agents/Step14 |
| Memory & RAG | 12 | AgentWithMemory, AgentWithRAG |
| Workflow Construction | 20 | _Foundational/*, Concurrent/* |
| Workflow Execution | 10 | Checkpoint/*, ConditionalEdges/* |
| Protocol Integration | 15 | A2A*, ModelContextProtocol/* |
| Agent Hosting | 10 | HostedAgents, A2AClientServer |
| Observability | 8 | AgentOpenTelemetry, Observability/* |

---

## Agent Creation Patterns

### Pattern: Fluent Azure OpenAI Agent Creation

**Category**: Agent Creation
**Observed In**: Agents/Step01, Agent_With_AzureOpenAIChatCompletion, HostedAgents/*
**Frequency**: Most common (15+ samples)

#### Mechanism

Single fluent expression creates configured agent from Azure OpenAI service.

#### Code Signature

```csharp
var agent = new AzureOpenAIClient(
        new Uri(endpoint), 
        new AzureCliCredential())
    .GetChatClient(deploymentName)
    .AsAIAgent(
        instructions: "System prompt here",
        name: "AgentName");
```

#### Variations

| Variation | Samples | Difference |
|-----------|---------|------------|
| AzureCliCredential | Agents/* | Local development |
| DefaultAzureCredential | HostedAgents/* | Multi-source credential chain |
| ResponsesClient path | Agent_With_AzureOpenAIResponses | GetResponsesClient() instead |

---

### Pattern: Single-Turn vs Multi-Turn Invocation

**Category**: Agent Invocation
**Observed In**: All agent samples
**Frequency**: Universal

#### Mechanism

Single-turn uses no thread; multi-turn preserves state via AgentThread.

#### Code Signature

```csharp
// Single-turn (no state)
var response = await agent.RunAsync("message");

// Multi-turn (preserves state)
var thread = await agent.GetNewThreadAsync();
var response1 = await agent.RunAsync("first message", thread);
var response2 = await agent.RunAsync("follow-up", thread);
```

#### Assumptions

- Thread persists conversation history
- Caller manages thread lifecycle
- Thread can be serialized for persistence

---

### Pattern: Streaming Agent Invocation

**Category**: Agent Invocation
**Observed In**: Agents/Step01, AgentOpenTelemetry, _Foundational/02
**Frequency**: Common (10+ samples)

#### Code Signature

```csharp
await foreach (var update in agent.RunStreamingAsync("message", thread))
{
    Console.Write(update.Contents);
}
```

---

## Thread & Persistence Patterns

### Pattern: Thread Serialization for Persistence

**Category**: State Management
**Observed In**: Agents/Step06, Step13, AgentWithMemory/*
**Frequency**: Common

#### Code Signature

```csharp
// Serialize
var state = thread.Serialize();
var json = JsonSerializer.Serialize(state);
await File.WriteAllTextAsync("thread.json", json);

// Deserialize
var json = await File.ReadAllTextAsync("thread.json");
var state = JsonDocument.Parse(json).RootElement;
var thread = await agent.DeserializeThreadAsync(state);
```

---

### Pattern: ChatHistoryProvider with External Storage

**Category**: State Management
**Observed In**: Agents/Step07
**Frequency**: Specialized

#### Mechanism

Factory creates provider per thread; provider stores messages externally.

#### Code Signature

```csharp
var options = new ChatClientAgentOptions
{
    ChatHistoryProviderFactory = (serializedState, jsonOptions) =>
        new VectorChatHistoryProvider(vectorStore, serializedState, jsonOptions)
};
```

---

## Function Tool Patterns

### Pattern: Function Tool Registration

**Category**: Tool Integration
**Observed In**: Agents/Step03, A2AClientServer
**Frequency**: Common

#### Code Signature

```csharp
var tools = new AITool[]
{
    AIFunctionFactory.Create(
        [Description("Gets current weather")]
        (string location) => $"Weather in {location}: Sunny, 72°F")
};

var agent = chatClient.AsAIAgent(
    instructions: "You help with weather queries",
    tools: tools);
```

---

### Pattern: Human-in-the-Loop Function Approval

**Category**: Tool Integration
**Observed In**: Agents/Step04
**Frequency**: Specialized

#### Code Signature

```csharp
var approvalTool = new ApprovalRequiredAIFunction(originalFunction);

var response = await agent.RunAsync("invoke the function", thread);

foreach (var request in response.UserInputRequests
    .OfType<FunctionApprovalRequestContent>())
{
    Console.WriteLine($"Approve {request.FunctionName}? (Y/N)");
    var approved = Console.ReadLine()?.ToUpper() == "Y";
    var approval = request.CreateResponse(approved);
    
    response = await agent.RunAsync(
        [new ChatMessage(ChatRole.User, [approval])], 
        thread);
}
```

---

## Workflow Patterns

### Pattern: Basic Sequential Workflow

**Category**: Workflow Construction
**Observed In**: _Foundational/01, Declarative/Marketing
**Frequency**: Common

#### Code Signature

```csharp
var workflow = new WorkflowBuilder(executor1)
    .AddEdge(executor1, executor2)
    .AddEdge(executor2, executor3)
    .WithOutputFrom(executor3)
    .Build();

var result = await InProcessExecution.RunAsync(workflow, "input");
```

---

### Pattern: Concurrent Fan-Out/Fan-In

**Category**: Workflow Construction
**Observed In**: Concurrent/*, _Foundational/04
**Frequency**: Specialized

#### Code Signature

```csharp
var workflow = new WorkflowBuilder(startExecutor)
    .AddFanOutEdge(startExecutor, [workerA, workerB, workerC])
    .AddFanInEdge([workerA, workerB, workerC], aggregator)
    .WithOutputFrom(aggregator)
    .Build();
```

#### Assumptions

- Targets execute concurrently
- Fan-in receives `List<TOutput>`
- Execution time determined by slowest worker

---

### Pattern: Conditional Switch Routing

**Category**: Workflow Control Flow
**Observed In**: _Foundational/08, ConditionalEdges/02
**Frequency**: Common

#### Code Signature

```csharp
var workflow = new WorkflowBuilder(analyzer)
    .AddSwitch(analyzer, switchBuilder =>
    {
        switchBuilder.AddCase(
            output => output is Result { IsSpam: true },
            spamHandler);
        switchBuilder.AddCase(
            output => output is Result { IsSpam: false },
            normalHandler);
        switchBuilder.WithDefault(uncertainHandler);
    })
    .WithOutputFrom(spamHandler)
    .WithOutputFrom(normalHandler)
    .WithOutputFrom(uncertainHandler)
    .Build();
```

---

### Pattern: Agents as Workflow Executors

**Category**: Workflow Construction
**Observed In**: _Foundational/03-05, Workflow-Agents/*
**Frequency**: Common

#### Mechanism

Agents added directly to WorkflowBuilder; TurnToken triggers processing.

#### Code Signature

```csharp
var agent = chatClient.AsAIAgent(instructions: "...");

var workflow = new WorkflowBuilder(agent)
    .AddEdge(agent, nextExecutor)
    .Build();

var run = await InProcessExecution.StreamAsync(workflow, "input");

// Send message and trigger
await run.TrySendMessageAsync(new ChatMessage(ChatRole.User, "hello"));
await run.TrySendMessageAsync(new TurnToken(emitEvents: true));
```

---

### Pattern: Workflow-as-Agent Transformation

**Category**: Agent Hosting
**Observed In**: _Foundational/05, HostedAgents/AgentsInWorkflows
**Frequency**: Specialized

#### Code Signature

```csharp
var workflow = AgentWorkflowBuilder.BuildSequential(
    [agent1, agent2, agent3]);

var workflowAgent = workflow.AsAgent("workflow-agent", "Translation Pipeline");

// Use like any agent
var response = await workflowAgent.RunAsync("Translate this text");
```

---

### Pattern: Human-in-the-Loop with RequestPort

**Category**: Workflow Execution
**Observed In**: HumanInTheLoopBasic, Checkpoint/CheckpointWithHumanInTheLoop
**Frequency**: Specialized

#### Code Signature

```csharp
var requestPort = RequestPort.Create<ApprovalRequest, ApprovalResponse>("approval");

var workflow = new WorkflowBuilder(analyzer)
    .AddEdge(analyzer, requestPort)
    .AddEdge(requestPort, processor)
    .Build();

var run = await InProcessExecution.StreamAsync(workflow, "input");

await foreach (var evt in run.WatchStreamAsync())
{
    if (evt is RequestInfoEvent requestInfo)
    {
        var request = requestInfo.Request.DataAs<ApprovalRequest>();
        Console.WriteLine($"Approve: {request.Description}? (Y/N)");
        
        var approved = Console.ReadLine()?.ToUpper() == "Y";
        await run.SendResponseAsync(
            requestInfo.Request.CreateResponse(new ApprovalResponse(approved)));
    }
}
```

---

### Pattern: Workflow Checkpointing

**Category**: Workflow Execution
**Observed In**: Checkpoint/*
**Frequency**: Specialized

#### Code Signature

```csharp
// Execute with checkpointing
var run = await InProcessExecution.StreamAsync(
    workflow, input, CheckpointManager.Default);

CheckpointInfo? lastCheckpoint = null;
await foreach (var evt in run.WatchStreamAsync())
{
    if (evt is SuperStepCompletedEvent sse)
    {
        lastCheckpoint = sse.CompletionInfo.Checkpoint;
    }
}

// Resume from checkpoint (rehydration - new instance)
var newRun = await InProcessExecution.ResumeStreamAsync(
    workflow, lastCheckpoint, CheckpointManager.Default);

// Or restore (same instance)
await run.RestoreCheckpointAsync(lastCheckpoint);
```

---

## Protocol Integration Patterns

### Pattern: A2A Client with Card Resolver

**Category**: Protocol Integration
**Observed In**: A2AClientServer, A2AAgent_AsFunctionTools
**Frequency**: Common for A2A

#### Code Signature

```csharp
var resolver = new A2ACardResolver(new Uri("https://agent.example.com"));
var agent = await resolver.GetAIAgentAsync();

var response = await agent.RunAsync("Hello from A2A");
```

---

### Pattern: Agent as A2A Function Tools (Per-Skill)

**Category**: Protocol Integration
**Observed In**: A2AAgent_AsFunctionTools
**Frequency**: Specialized

#### Code Signature

```csharp
var card = await resolver.GetAgentCardAsync();
var tools = new List<AITool>();

foreach (var skill in card.Skills ?? [])
{
    var skillName = FunctionNameSanitizer.Sanitize(skill.Name);
    var description = JsonSerializer.Serialize(new
    {
        skill.Description,
        skill.Tags,
        skill.Examples,
        skill.InputModes,
        skill.OutputModes
    });
    
    tools.Add(AIFunctionFactory.Create(
        async (string input) => (await a2aAgent.RunAsync(input)).ToString(),
        new AIFunctionFactoryOptions
        {
            Name = skillName,
            Description = description
        }));
}
```

---

### Pattern: Client-Side MCP with Stdio Transport

**Category**: Protocol Integration
**Observed In**: ModelContextProtocol/Agent_MCP_Server
**Frequency**: Common for local MCP

#### Code Signature

```csharp
var mcpClient = await McpClient.CreateAsync(
    new StdioClientTransport(new StdioClientTransportOptions
    {
        Name = "my-mcp-server",
        Command = "npx",
        Arguments = ["-y", "@package/mcp-server"]
    }));

var tools = (await mcpClient.ListToolsAsync()).Cast<AITool>().ToArray();
var agent = chatClient.AsAIAgent(instructions: "...", tools: tools);
```

---

### Pattern: Server-Side MCP with HostedMcpServerTool

**Category**: Protocol Integration
**Observed In**: HostedAgents/AgentWithHostedMCP, ResponseAgent_Hosted_MCP
**Frequency**: Common for production

#### Code Signature

```csharp
var mcpTool = new HostedMcpServerTool
{
    ServerName = "microsoft_learn",
    ServerAddress = "https://learn.microsoft.com/api/mcp",
    AllowedTools = ["microsoft_docs_search"],
    ApprovalMode = HostedMcpServerToolApprovalMode.NeverRequire
};

var agent = responsesClient.CreateAIAgent(
    instructions: "Search documentation when needed",
    name: "DocSearch Agent",
    tools: [mcpTool]);
```

---

### Pattern: MCP Tool Approval Loop

**Category**: Protocol Integration
**Observed In**: ResponseAgent_Hosted_MCP, FoundryAgent_Hosted_MCP
**Frequency**: Specialized

#### Code Signature

```csharp
var mcpTool = new HostedMcpServerTool
{
    ApprovalMode = HostedMcpServerToolApprovalMode.AlwaysRequire,
    // ... other config
};

var response = await agent.RunAsync("search for docs", thread);

while (response.UserInputRequests.Any())
{
    foreach (var request in response.UserInputRequests
        .OfType<McpServerToolApprovalRequestContent>())
    {
        Console.WriteLine($"Approve {request.ToolName}? (Y/N)");
        var approved = Console.ReadLine()?.ToUpper() == "Y";
        
        response = await agent.RunAsync(
            [new ChatMessage(ChatRole.User, [request.CreateResponse(approved)])],
            thread);
    }
}
```

---

## Memory & RAG Patterns

### Pattern: TextSearchProvider as AIContextProvider

**Category**: RAG
**Observed In**: AgentWithRAG/*, HostedAgents/AgentWithTextSearchRag
**Frequency**: Common

#### Code Signature

```csharp
var options = new ChatClientAgentOptions
{
    AIContextProviderFactory = (ctx, ct) => new ValueTask<AIContextProvider>(
        new TextSearchProvider(
            searchFunction,
            ctx.SerializedState,
            ctx.JsonSerializerOptions,
            new TextSearchProviderOptions
            {
                SearchTime = TextSearchBehavior.BeforeAIInvoke,
                RecentMessageMemoryLimit = 6
            }))
};
```

---

### Pattern: Memory Scope Separation

**Category**: Memory
**Observed In**: AgentWithMemory/Step01
**Frequency**: Specialized

#### Mechanism

Storage scope isolates per-thread; search scope enables cross-thread retrieval.

#### Code Signature

```csharp
var provider = new ChatHistoryMemoryProvider(
    vectorStore,
    collectionName,
    vectorDimensions,
    storageScope: new { UserId = userId, ThreadId = threadId },  // isolated
    searchScope: new { UserId = userId });  // cross-thread retrieval
```

---

## Observability Patterns

### Pattern: OpenTelemetry Agent Instrumentation

**Category**: Observability
**Observed In**: AgentOpenTelemetry, Observability/*
**Frequency**: Specialized

#### Code Signature

```csharp
var chatClient = azureOpenAIClient
    .GetChatClient(deploymentName)
    .AsIChatClient()
    .AsBuilder()
    .UseFunctionInvocation()
    .UseOpenTelemetry()
    .Build();

var agent = new ChatClientAgent(chatClient, instructions)
    .AsBuilder()
    .UseOpenTelemetry()
    .Build();
```

---

## See Also

- [vocabulary.md](vocabulary.md) — Types and methods used in these patterns
- [tensions.md](tensions.md) — When patterns conflict or represent different choices
- [coverage.md](coverage.md) — Which samples demonstrate each pattern
