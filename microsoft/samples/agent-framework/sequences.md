# Microsoft Agent Framework - Sequence Progressions

**Part of**: [Microsoft Agent Framework Sample Synthesis](README.md)
**Generated**: 2026-01-29 | **Version**: v1

---

## Sequence Overview

| Sequence | Samples | Theme | Key Evolution |
|----------|---------|-------|---------------|
| Agents | Step01 → Step20 | Agent Fundamentals | Basic invocation → Tools → Middleware → Advanced patterns |
| _Foundational | 01 → 08 | Workflow Basics | Executors → Streaming → Agents → Composition → State |
| AgentWithRAG | Step01 → Step04 | RAG Patterns | Local vector → Qdrant → Mock → Foundry hosted |
| AgentWithMemory | Step01 → Step03 | Memory Systems | Vector memory → Mem0 → Custom extraction |
| ModelContextProtocol | 4 projects | MCP Integration | Client Stdio → OAuth → Hosted Responses → Hosted Foundry |

---

## Sequence: Agents (20 Steps)

**Samples**: Agent_Step01 → Agent_Step20
**Theme**: Comprehensive agent fundamentals from basic invocation to advanced patterns

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 01 | Agent_Step01 | Basic agent creation, sync/streaming invocation | — |
| 02 | Agent_Step02 | Multi-turn with AgentThread | Step01 |
| 03 | Agent_Step03 | Function tools via AIFunctionFactory | Step01 |
| 04 | Agent_Step04 | Human-in-the-loop function approval | Step03 |
| 05 | Agent_Step05 | Structured output with generic RunAsync | Step02 |
| 06 | Agent_Step06 | Thread serialization for persistence | Step02 |
| 07 | Agent_Step07 | Custom ChatHistoryProvider with vector store | Step06 |
| 08 | Agent_Step08 | OpenTelemetry instrumentation | Step01 |
| 09 | Agent_Step09 | DI Hosted Service with Generic Host | Step01 |
| 10 | Agent_Step10 | Agent exposed as MCP tool | Step01 |
| 11 | Agent_Step11 | Image input processing | Step01 |
| 12 | Agent_Step12 | Nested agents (agent-as-tool) | Step03 |
| 13 | Agent_Step13 | Background responses with polling | Step06 |
| 14 | Agent_Step14 | Middleware stack (chat + function) | Step03 |
| 15 | Agent_Step15 | Plugin with dependency injection | Step03, Step09 |
| 16 | Agent_Step16 | Chat history reducer for size limits | Step07 |
| 17 | Agent_Step17 | Streaming with background responses | Step13 |
| 18 | Agent_Step18 | Deep Research with Foundry | Step10 |
| 19 | Agent_Step19 | Declarative agent from YAML | Step05 |
| 20 | Agent_Step20 | AIContextProvider for context injection | Step07 |

### Key Evolution

1. **Foundation** (01-02): Basic agent creation and multi-turn conversation
2. **Tools** (03-04): Function calling with optional human approval
3. **Output Control** (05): Structured output via generics and JSON schema
4. **Persistence** (06-07): Thread serialization and custom storage
5. **Infrastructure** (08-09): Telemetry and DI hosting
6. **Interoperability** (10-12): MCP exposure and nested agents
7. **Long-Running** (13, 17): Background responses and streaming resumption
8. **Extension** (14-16): Middleware pipeline and history management
9. **Advanced** (18-20): Deep Research, declarative definition, context injection

### Vocabulary Introduced

| Sample | New Types | New Methods |
|--------|-----------|-------------|
| Step01 | AzureOpenAIClient, ChatClientAgent | AsAIAgent, RunAsync, RunStreamingAsync |
| Step02 | AgentThread | GetNewThreadAsync |
| Step03 | AIFunctionFactory | AIFunctionFactory.Create |
| Step04 | ApprovalRequiredAIFunction, FunctionApprovalRequestContent | CreateResponse |
| Step05 | AgentResponse\<T\>, ChatResponseFormat | ForJsonSchema\<T\> |
| Step06 | JsonElement | Serialize, DeserializeThreadAsync |
| Step07 | ChatHistoryProvider, VectorStore | InvokingAsync, InvokedAsync |
| Step14 | FunctionInvocationContext | Use (middleware) |
| Step16 | IChatReducer, MessageCountingChatReducer | WithReducer |
| Step20 | AIContextProvider, AIContext | InvokingAsync (context), InvokedAsync (context) |

---

## Sequence: _Foundational (8 Steps)

**Samples**: 01_ExecutorsAndEdges → 08_WriterCriticWorkflow
**Theme**: Workflow construction from basic executors to complex patterns

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 01 | 01_ExecutorsAndEdges | WorkflowBuilder, Executor\<T\>, AddEdge | — |
| 02 | 02_Streaming | StreamAsync, WatchStreamAsync | Step01 |
| 03 | 03_AgentsInWorkflows | ChatClientAgent as executor, TurnToken | Step02 |
| 04 | 04_AgentWorkflowPatterns | BuildSequential, BuildConcurrent, Handoffs, GroupChat | Step03 |
| 05 | 05_MultiModelService | Workflow.AsAgent(), HostedWebSearchTool | Step04 |
| 06 | 06_SubWorkflows | BindAsExecutor for workflow composition | Step01 |
| 07 | 07_MixedWorkflowAgentsAndExecutors | Adapter executors, type conversion | Step03, Step06 |
| 08 | 08_WriterCriticWorkflow | AddSwitch, loops, shared state | Step07 |

### Key Evolution

1. **Graph Basics** (01): Executors, edges, Build pattern
2. **Execution Models** (02): Streaming with event enumeration
3. **Agent Integration** (03): TurnToken protocol for agents in workflows
4. **Built-In Patterns** (04): Factory methods for common workflow shapes
5. **Protocol Bridge** (05): Workflow-as-Agent transformation
6. **Composition** (06): Hierarchical sub-workflow embedding
7. **Type Bridging** (07): Adapter executors between typed boundaries
8. **Control Flow** (08): Conditional routing and iteration

### Patterns Introduced

| Sample | Pattern |
|--------|---------|
| 01 | Basic Sequential Workflow, Delegate-as-Executor |
| 02 | Streaming Workflow Execution |
| 03 | TurnToken Protocol, Agents as Executors |
| 04 | Sequential/Concurrent/Handoff/GroupChat factories |
| 05 | Workflow-as-Agent, Multi-Model Composition |
| 06 | Sub-Workflow Composition |
| 07 | Adapter Executor (String ↔ ChatMessage) |
| 08 | Switch-Case Routing, Feedback Loops, Shared State |

---

## Sequence: AgentWithRAG (4 Steps)

**Samples**: Step01 → Step04
**Theme**: Retrieval-Augmented Generation from local to cloud

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 01 | Step01 | TextSearchProvider, TextSearchStore, InMemoryVectorStore | — |
| 02 | Step02 | QdrantVectorStore, custom schema with attributes | Step01 |
| 03 | Step03 | Mock search for testing RAG logic | Step01 |
| 04 | Step04 | Foundry hosted vector store with file upload | — (Foundry path) |

### Key Evolution

1. **Local Vector** (01): InMemoryVectorStore with dynamic schema
2. **Production Vector** (02): Qdrant with custom POCO schema
3. **Testing** (03): Mock search function for unit testing
4. **Cloud Hosted** (04): Foundry-managed vector store with automatic chunking

### Vector Store Progression

| Step | Store Type | Schema Definition | Embedding |
|------|-----------|-------------------|-----------|
| 01 | InMemoryVectorStore | VectorStoreCollectionDefinition (dynamic) | Client-side |
| 02 | QdrantVectorStore | POCO attributes (compile-time) | Client-side |
| 03 | (mock) | N/A | N/A |
| 04 | Foundry hosted | Automatic | Server-side |

---

## Sequence: AgentWithMemory (3 Steps)

**Samples**: Step01 → Step03
**Theme**: Memory systems from built-in to custom

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 01 | Step01 | ChatHistoryMemoryProvider, storageScope vs searchScope | — |
| 02 | Step02 | Mem0Provider (external service) | Step01 |
| 03 | Step03 | Custom AIContextProvider with GetResponseAsync\<T\> extraction | Step01, Step02 |

### Key Evolution

1. **Vector Memory** (01): Store complete messages, retrieve via similarity
2. **External Memory** (02): Delegate extraction to Mem0 service
3. **Custom Memory** (03): In-process extraction with structured output

### Memory Model Comparison

| Step | Storage | Extraction | Cross-Thread |
|------|---------|------------|--------------|
| 01 | Vector store (local) | None (raw messages) | searchScope without ThreadId |
| 02 | External service (Mem0) | Automatic (service) | Shared scope |
| 03 | Provider instance | Structured output LLM call | Manual copy |

---

## Sequence: ModelContextProtocol (4 Projects)

**Samples**: Agent_MCP_Server → FoundryAgent_Hosted_MCP
**Theme**: MCP integration from client-controlled to server-delegated

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | Agent_MCP_Server | McpClient, StdioClientTransport, ListToolsAsync | — |
| 2 | Agent_MCP_Server_Auth | HttpClientTransport, OAuth 2.0 flow | Step1 |
| 3 | ResponseAgent_Hosted_MCP | HostedMcpServerTool, ApprovalMode | — (server path) |
| 4 | FoundryAgent_Hosted_MCP | HostedMcpServerTool with PersistentAgentsClient | Step3 |

### Key Evolution

1. **Client Stdio** (1): Application spawns MCP server process, controls invocation
2. **Client OAuth** (2): Network-based MCP with authentication
3. **Server Responses** (3): Backend service (OpenAI Responses) invokes tools
4. **Server Foundry** (4): Backend service (Azure AI Foundry) invokes tools

### Architectural Shift

```
Steps 1-2: Client-Side Control
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Application │───▶│  McpClient  │───▶│  MCP Server │
└─────────────┘    └─────────────┘    └─────────────┘
      │                                      │
      └─────────── tool results ◀────────────┘

Steps 3-4: Server-Side Delegation
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Application │───▶│ Backend Svc │───▶│  MCP Server │
└─────────────┘    │ (Responses/ │    └─────────────┘
                   │  Foundry)   │
                   └─────────────┘
                         │
        tool invocation entirely within backend
```

### Transport Evolution

| Step | Transport | Authentication | Tool Discovery |
|------|-----------|----------------|----------------|
| 1 | Stdio (process) | None | Runtime (ListToolsAsync) |
| 2 | HTTP/SSE | OAuth 2.0 | Runtime (ListToolsAsync) |
| 3 | N/A (backend) | Azure CLI | Static (AllowedTools) |
| 4 | N/A (backend) | Azure CLI | Static (AllowedTools) |

---

## Cross-Sequence Observations

### Common Progression Patterns

1. **Simple → Complex**: All sequences start with minimal viable examples
2. **Local → External**: Memory and RAG progress from in-memory to services
3. **Manual → Factory**: Repeated patterns become factory methods
4. **Sync → Async/Streaming**: Execution models evolve to streaming
5. **Control → Delegation**: MCP shows shift from client control to server delegation

### Vocabulary Accumulation

| Sequence | New Types Introduced | New Patterns |
|----------|---------------------|--------------|
| Agents (20) | 40+ types | 50+ patterns |
| _Foundational (8) | 20+ types | 25+ patterns |
| AgentWithRAG (4) | 15+ types | 10+ patterns |
| AgentWithMemory (3) | 8+ types | 8+ patterns |
| ModelContextProtocol (4) | 10+ types | 12+ patterns |

### Sequence Dependencies

```
Agents (core)
   │
   ├──▶ AgentWithRAG (context injection)
   │
   ├──▶ AgentWithMemory (context providers)
   │
   └──▶ ModelContextProtocol (tool patterns)

_Foundational (workflows)
   │
   └──▶ Agents Step09-10 (hosting patterns)
```

---

## See Also

- [vocabulary.md](vocabulary.md) — Complete vocabulary reference
- [patterns.md](patterns.md) — Full pattern catalog
- [coverage.md](coverage.md) — Complete sample index
