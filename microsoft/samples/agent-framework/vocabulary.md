# Microsoft Agent Framework - Vocabulary Reference

**Part of**: [Microsoft Agent Framework Sample Synthesis](README.md)
**Generated**: 2026-01-29 | **Version**: v1

---

## Core Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| AIAgent | Microsoft.Agents.AI | Abstract base class for all agents; defines `RunAsync`, `RunStreamingAsync`, `GetNewThreadAsync` |
| AgentThread | Microsoft.Agents.AI | Conversation state container; supports serialization for persistence |
| AgentResponse | Microsoft.Agents.AI | Return type from agent execution; contains Messages collection |
| AgentResponseUpdate | Microsoft.Agents.AI | Single update chunk in streaming responses |
| ChatClientAgent | Microsoft.Agents.AI | AIAgent implementation wrapping IChatClient |
| ChatClientAgentOptions | Microsoft.Agents.AI | Configuration for ChatClientAgent with factory delegates |
| AIContext | Microsoft.Agents.AI | Container returned by AIContextProvider with Tools, Messages, Instructions |
| AIContextProvider | Microsoft.Agents.AI | Abstract base for injecting context before/after agent execution |
| ChatHistoryProvider | Microsoft.Agents.AI | Manages chat history lifecycle with InvokingAsync/InvokedAsync hooks |
| AgentRunOptions | Microsoft.Agents.AI | Per-invocation configuration including ContinuationToken |

### Workflow Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| Workflow | Microsoft.Agents.AI.Workflows | Built workflow graph ready for execution |
| WorkflowBuilder | Microsoft.Agents.AI.Workflows | Fluent builder for constructing workflow graphs |
| Executor\<TIn, TOut\> | Microsoft.Agents.AI.Workflows | Base class for typed processing units |
| ExecutorBinding | Microsoft.Agents.AI.Workflows | Configuration wrapper for executor as workflow node |
| IWorkflowContext | Microsoft.Agents.AI.Workflows | Context providing state, messaging, output services |
| WorkflowEvent | Microsoft.Agents.AI.Workflows | Base class for all workflow events |
| WorkflowOutputEvent | Microsoft.Agents.AI.Workflows | Terminal event emitted when workflow produces output |
| TurnToken | Microsoft.Agents.AI.Workflows | Signal triggering agent execution in workflows |
| InProcessExecution | Microsoft.Agents.AI.Workflows | Entry point for workflow execution |
| Run / StreamingRun | Microsoft.Agents.AI.Workflows | Execution handles for sync/streaming workflows |
| CheckpointManager | Microsoft.Agents.AI.Workflows | Manages checkpoint storage and retrieval |
| CheckpointInfo | Microsoft.Agents.AI.Workflows | Contains checkpoint state and metadata |
| RequestPort | Microsoft.Agents.AI.Workflows | Executor enabling human-in-the-loop external requests |

### Azure AI Integration Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| AzureOpenAIClient | Azure.AI.OpenAI | Azure OpenAI service client |
| AzureCliCredential | Azure.Identity | Authentication using local Azure CLI login |
| DefaultAzureCredential | Azure.Identity | Multi-source credential chain for hosted environments |
| PersistentAgentsClient | Azure.AI.Agents.Persistent | Client for Azure AI Foundry agents |
| PersistentAgent | Azure.AI.Agents.Persistent | Azure AI Foundry agent representation |
| AIProjectClient | Azure.AI.Projects | Client for Azure AI Projects (Foundry) |

### Protocol Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| A2ACardResolver | Microsoft.Agents.AI.A2A | Retrieves agent card from A2A endpoint |
| A2AClient | A2A | Direct client for A2A agent endpoint |
| AgentCard | A2A | Metadata descriptor for A2A agent capabilities |
| AgentSkill | A2A | Named capability descriptor with examples and tags |
| McpClient | ModelContextProtocol.Client | Client for connecting to MCP servers |
| StdioClientTransport | ModelContextProtocol.Client | MCP transport via stdin/stdout |
| HttpClientTransport | ModelContextProtocol.Client | MCP transport via HTTP/SSE |
| HostedMcpServerTool | Azure.AI.Agents.Persistent | Server-side MCP tool configuration |
| McpServerTool | ModelContextProtocol.Server | MCP tool wrapper for agents/functions |

### Tool and Function Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| AITool | Microsoft.Extensions.AI | Abstraction for functions/tools an agent can invoke |
| AIFunctionFactory | Microsoft.Extensions.AI | Factory for creating AIFunction from methods |
| ApprovalRequiredAIFunction | Microsoft.Agents.AI | Wrapper requiring human approval before execution |
| FunctionInvocationContext | Microsoft.Agents.AI | Context passed to function invocation middleware |

### Memory and RAG Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| ChatHistoryMemoryProvider | Microsoft.Agents.AI | AIContextProvider storing messages with embeddings |
| Mem0Provider | Microsoft.Agents.AI.Mem0 | AIContextProvider integrating Mem0 service |
| TextSearchProvider | Microsoft.Agents.AI | AIContextProvider performing text search |
| VectorStore | Microsoft.Extensions.VectorData | Vector database abstraction |
| TextSearchStore | Microsoft.Agents.AI.Samples | Sample storage with opinionated schema |

### Observability Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| ActivitySource | System.Diagnostics | Creates Activity instances for tracing |
| Activity | System.Diagnostics | Single span in distributed tracing |
| Meter | System.Diagnostics.Metrics | Creates metric instruments |
| TracerProviderBuilder | OpenTelemetry | Configures tracing pipeline |
| MeterProviderBuilder | OpenTelemetry | Configures metrics pipeline |

---

## Core Methods/Operations

### Agent Lifecycle Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| AsAIAgent | ChatClient (extension) | Converts ChatClient to AIAgent with instructions/tools |
| AsAIAgent | ResponsesClient (extension) | Converts ResponsesClient to AIAgent |
| AsAIAgent | A2AClient (extension) | Converts A2A client to AIAgent |
| GetNewThreadAsync | AIAgent | Creates new conversation thread |
| RunAsync | AIAgent | Processes message in agent thread (sync) |
| RunStreamingAsync | AIAgent | Processes message with streaming updates |
| DeserializeThreadAsync | AIAgent | Reconstructs thread from serialized state |
| Serialize | AgentThread | Returns JsonElement containing thread state |

### Workflow Construction Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| AddEdge | WorkflowBuilder | Connects two executors with data flow path |
| AddSwitch | WorkflowBuilder | Creates conditional routing based on predicate |
| AddFanOutEdge | WorkflowBuilder | Sends same message to multiple executors |
| AddFanInEdge | WorkflowBuilder | Waits for all sources before invoking target |
| WithOutputFrom | WorkflowBuilder | Designates output executor |
| Build | WorkflowBuilder | Creates immutable Workflow |
| BindAsExecutor | Func / Workflow | Converts delegate/workflow to ExecutorBinding |
| AsAgent | Workflow | Converts workflow to AIAgent interface |

### Workflow Execution Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| RunAsync | InProcessExecution | Executes workflow synchronously |
| StreamAsync | InProcessExecution | Executes workflow with event streaming |
| ResumeStreamAsync | InProcessExecution | Creates new instance from checkpoint |
| WatchStreamAsync | StreamingRun | Returns IAsyncEnumerable of events |
| TrySendMessageAsync | StreamingRun | Sends message after run started |
| SendResponseAsync | StreamingRun | Sends external response (human-in-the-loop) |

### State Management Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| QueueStateUpdateAsync | IWorkflowContext | Writes value to workflow state |
| ReadStateAsync\<T\> | IWorkflowContext | Reads typed value from state |
| SendMessageAsync | IWorkflowContext | Sends message to connected executors |
| YieldOutputAsync | IWorkflowContext | Emits output value from executor |
| OnCheckpointingAsync | Executor | Override to save state during checkpoint |
| OnCheckpointRestoredAsync | Executor | Override to restore state from checkpoint |

### Provider Lifecycle Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| InvokingAsync | ChatHistoryProvider | Called before agent execution |
| InvokedAsync | ChatHistoryProvider | Called after agent execution |
| InvokingAsync | AIContextProvider | Returns AIContext with Tools/Messages/Instructions |
| InvokedAsync | AIContextProvider | Post-execution hook for state extraction |

### Tool and Function Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| AIFunctionFactory.Create | AIFunctionFactory | Wraps method as AIFunction |
| AsAIFunction | AIAgent (extension) | Converts agent to AITool |
| AsAITools | Plugin | Returns AIFunction array from plugin |
| ListToolsAsync | McpClient | Retrieves tools from MCP server |

### Protocol Methods

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| MapA2A | WebApplication (extension) | Registers A2A endpoints in ASP.NET Core |
| MapWellKnownAgentCard | TaskManager (extension) | Registers agent card discovery endpoint |
| GetAIAgentAsync | A2ACardResolver | Retrieves card and creates AIAgent |
| McpClient.CreateAsync | McpClient | Creates MCP client with transport |

---

## Domain-Specific Vocabulary

### A2A Protocol Terms

| Term | Meaning |
|------|---------|
| Agent Card | Metadata document describing agent capabilities, skills, and modes |
| Well-Known Path | Standard path `/.well-known/agent-card.json` for discovery |
| Background Responses | Long-running task pattern using continuation tokens |
| Direct/Private Discovery | Bypassing well-known path with explicit endpoint |
| InputModes/OutputModes | MIME types supported by agent skills |

### MCP Protocol Terms

| Term | Meaning |
|------|---------|
| Stdio Transport | Process-based MCP communication via stdin/stdout |
| HTTP Transport | Network-based MCP via HTTP/SSE |
| HostedMcpServerTool | Configuration for server-side MCP invocation |
| AllowedTools | Whitelist filtering available tools from MCP server |
| ApprovalMode | NeverRequire vs AlwaysRequire for tool execution |

### Workflow Terms

| Term | Meaning |
|------|---------|
| Executor | Processing unit in workflow graph |
| Edge | Connection between executors |
| Fan-Out | Broadcasting to multiple concurrent executors |
| Fan-In | Collecting results from multiple sources |
| Super Step | Checkpoint boundary in workflow execution |
| Turn Token | Signal triggering agent processing in workflows |

### Declarative Workflow Terms

| Term | Meaning |
|------|---------|
| Action | Individual step in workflow YAML |
| Local Scope | Workflow variable scope (Local.VariableName) |
| System Scope | Read-only system properties (System.LastMessage) |
| Power FX Expression | Formula prefixed with `=` in YAML values |
| GotoAction | Jump to earlier action for loops |
| ConditionGroup | If/elseif/else branching in YAML |
| Eject | Converting YAML to executable C# code |

### Memory Terms

| Term | Meaning |
|------|---------|
| storageScope | Defines where memories are stored (UserId + ThreadId) |
| searchScope | Defines retrieval boundary (UserId only enables cross-thread) |
| Hybrid Search | Combined vector + keyword search |
| Chunking | Splitting documents into embeddable segments |

### Hosting Terms

| Term | Meaning |
|------|---------|
| kind: hosted | YAML property for hosted agent deployment |
| Generic Host | .NET hosting infrastructure (HostApplicationBuilder) |
| IHostedService | Interface for long-running background services |
| AddKeyedChatClient | DI registration with string key for multi-provider |

---

## Type Relationships

### Agent Composition

- `AIAgent` → produces `AgentResponse` / `AgentResponseUpdate`
- `AIAgent` → manages `AgentThread` lifecycle
- `AgentThread` → contains `ChatHistoryProvider`
- `ChatClientAgent` → wraps `IChatClient`
- `AIContextProvider` → returns `AIContext` (Tools + Messages + Instructions)

### Workflow Composition

- `WorkflowBuilder` → produces `Workflow`
- `Workflow` → contains `ExecutorBinding` nodes
- `Executor<T>` → wrapped by `ExecutorBinding`
- `Workflow.AsAgent()` → produces `AIAgent`
- `InProcessExecution` → produces `Run` / `StreamingRun`

### Protocol Composition

- `A2ACardResolver` → retrieves `AgentCard`
- `AgentCard` → contains `AgentSkill` collection
- `McpClient` → uses `StdioClientTransport` / `HttpClientTransport`
- `AIAgent.AsAIFunction()` → produces `AITool`
- `McpServerTool.Create()` → wraps `AIFunction`

---

## See Also

- [patterns.md](patterns.md) — How these types are used in practice
- [tensions.md](tensions.md) — Alternative approaches to similar problems
