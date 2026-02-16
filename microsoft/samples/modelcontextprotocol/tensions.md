# MCP C# SDK - Tensions and Design Choices

**Part of**: [MCP C# SDK Sample Synthesis](README.md)
**Generated**: 2026-02-16 | **Version**: v1

---

## Tension Summary

| # | Tension | Domain | Approaches |
|---|---------|--------|------------|
| 1 | Generic Host + Stdio vs. ASP.NET Core + HTTP | ServerArchitecture | Child process vs. web service |
| 2 | Builder Pattern vs. Factory Method | ServerArchitecture | DI-integrated vs. caller-managed |
| 3 | Serilog vs. OpenTelemetry | ServerArchitecture | Structured logging vs. distributed tracing |
| 4 | ConfigureSessionOptions vs. RunSessionHandler | ServerArchitecture | Pre-protocol filtering vs. full lifecycle |
| 5 | Implicit vs. Explicit Session Management | ServerArchitecture | HTTP default vs. RunSessionHandler |
| 6 | OAuth 2.0 vs. Process Isolation | SecurityModel | Network auth vs. OS boundary |
| 7 | Scope Declaration vs. Enforcement | SecurityModel | Metadata vs. policy |
| 8 | IHttpContextAccessor Registration vs. Consumption | SecurityModel | Provisioned vs. exercised |
| 9 | Explicit Auth vs. Implicit Trust | SecurityModel | HTTP OAuth vs. stdio/in-memory |
| 10 | Session Isolation: Data vs. State vs. Capability | SecurityModel | Three isolation guarantees |
| 11 | Builder vs. Factory Registration | ToolManagement | DI builder vs. McpServer.Create |
| 12 | Global vs. Per-Session Tool Scope | ToolManagement | WithTools vs. ConfigureSessionOptions |
| 13 | Attribute vs. Manual Tool Discovery | ToolManagement | Attribute scanning vs. MethodInfo+options |
| 14 | Static vs. Instance Tool Methods | ToolManagement | Stateless vs. DI-injected |
| 15 | Proxy vs. Direct vs. LLM Invocation | ToolManagement | Three client calling patterns |
| 16 | Simple String vs. Rich ContentBlock Returns | ToolManagement | Plain text vs. annotated blocks |
| 17 | Factory vs. Builder Server Creation | TransportAbstraction | McpServer.Create vs. AddMcpServer |
| 18 | Stdio stdout Exclusivity vs. Normal Logging | TransportAbstraction | Stderr redirect vs. standard output |
| 19 | Single-Session vs. Multi-Session Model | TransportAbstraction | Process/pipe vs. HTTP connections |
| 20 | Client-Managed vs. Independent Service Lifecycle | TransportAbstraction | Child process vs. persistent service |
| 21 | Explicit vs. Abstracted Stream Wiring | TransportAbstraction | Pipe management vs. hidden streams |
| 22 | Transport-Specific Auth vs. Transport-Agnostic Tools | TransportAbstraction | Endpoint security vs. portable tools |
| 23 | Proxy vs. Direct vs. LLM Tool Calling | ClientServerContract | Three invocation patterns |
| 24 | Ephemeral Progress vs. Durable Tasks | ClientServerContract | ProgressToken vs. IMcpTaskStore |
| 25 | Simple vs. Rich Content Returns | ClientServerContract | String vs. ContentBlock+Annotations |
| 26 | Sampling vs. No Sampling | ClientServerContract | Bidirectional vs. unidirectional |
| 27 | One-Time vs. Dynamic Tool Discovery | ClientServerContract | Cache once vs. re-list |
| 28 | Comprehensive vs. Minimal Protocol Surface | ClientServerContract | All features vs. tools only |
| 29 | Sync vs. Async Tool Methods | AsyncOperations | string return vs. Task\<string\> |
| 30 | CancellationToken Acceptance vs. Omission | AsyncOperations | Propagated vs. omitted in same server |
| 31 | Fire-and-Forget vs. Awaited Server Execution | AsyncOperations | `_ = RunAsync()` vs. `await RunAsync(token)` |
| 32 | Ephemeral Progress vs. Durable Tasks (Async) | AsyncOperations | Push vs. poll model |
| 33 | Direct SampleAsync vs. AsSamplingChatClient | AsyncOperations | MCP-specific vs. IChatClient abstraction |
| 34 | Per-Session BackgroundService vs. No Background Services | AsyncOperations | using-based vs. none |

---

## ServerArchitecture Tensions

### Tension 1: Generic Host + Stdio vs. ASP.NET Core + HTTP

**Domain**: ServerArchitecture
**Manifestation**: The most fundamental architectural decision across all samples

#### Approach A: Generic Host + Stdio

- **Observed in**: FileBasedMcpServer, TestServerWithHosting, LongRunningTasks
- **Assumes**: Server runs as child process; one session per process; stdin/stdout for protocol; process isolation for security
- **Mechanism**: `Host.CreateApplicationBuilder` + `WithStdioServerTransport()` + `app.RunAsync()`

```csharp
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddMcpServer()
    .WithStdioServerTransport()
    .WithTools<MyTools>();
var app = builder.Build();
await app.RunAsync();
```

#### Approach B: ASP.NET Core + HTTP

- **Observed in**: AspNetCoreMcpServer, AspNetCoreMcpPerSessionTools, EverythingServer, ProtectedMcpServer
- **Assumes**: Server runs as persistent web service; multiple concurrent sessions; middleware pipeline; network-accessible
- **Mechanism**: `WebApplication.CreateBuilder` + `WithHttpTransport()` + `MapMcp()` + `app.RunAsync()`

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddMcpServer()
    .WithHttpTransport()
    .WithTools<MyTools>();
var app = builder.Build();
app.MapMcp();
app.Run();
```

#### Nature of Tension

Fundamentally different deployment models. Transport choice cascades into session model (single vs. multi), security model (process isolation vs. OAuth), observability approach (Serilog/stderr vs. OpenTelemetry), and middleware capabilities (none vs. full pipeline). A server cannot simultaneously use both for the same connection.

**Resolution**: Not provided — design choice for the implementer.

---

### Tension 2: Builder Pattern vs. Factory Method

**Domain**: ServerArchitecture
**Manifestation**: Two server creation paths with different infrastructure requirements

#### Approach A: DI-Integrated Builder

- **Observed in**: 7 of 8 server samples
- **Assumes**: Hosting framework with DI container; declarative configuration
- **Mechanism**: `AddMcpServer().With*()` chain

#### Approach B: Factory Method

- **Observed in**: InMemoryTransport
- **Assumes**: No hosting framework; caller manages lifetime
- **Mechanism**: `McpServer.Create(transport, options)`

```csharp
await using var server = McpServer.Create(transport, new McpServerOptions
{
    ToolCollection = [McpServerTool.Create((string msg) => $"Echo: {msg}", new() { Name = "Echo" })]
});
_ = server.RunAsync();
```

#### Nature of Tension

Builder requires hosting framework and DI container; factory bypasses both. Tool implementations are portable between both, but registration and lifetime management code is not.

**Resolution**: Not provided — builder for production, factory for testing/embedding.

---

### Tension 3: Serilog vs. OpenTelemetry

**Domain**: ServerArchitecture
**Manifestation**: Different observability libraries correlate with transport type

#### Approach A: Serilog

- **Observed in**: TestServerWithHosting (stdio)
- **Assumes**: Structured logging with file/debug/console(stderr) sinks; stderr separation critical for stdio

#### Approach B: OpenTelemetry

- **Observed in**: AspNetCoreMcpServer, EverythingServer (HTTP)
- **Assumes**: Distributed tracing and metrics; vendor-neutral OTLP export

#### Nature of Tension

Different libraries and configuration patterns. No sample combines both. The correlation with transport type may be coincidental or intentional.

**Resolution**: Not provided — implementer choice.

---

### Tension 4: ConfigureSessionOptions vs. RunSessionHandler

**Domain**: ServerArchitecture
**Manifestation**: Two HTTP session customization mechanisms with different capabilities

#### Approach A: ConfigureSessionOptions

- **Observed in**: AspNetCoreMcpPerSessionTools
- **Assumes**: Pre-protocol filtering is sufficient; simpler callback
- **Mechanism**: `async (httpContext, mcpOptions, ct) => { /* filter tools */ }`

#### Approach B: RunSessionHandler

- **Observed in**: EverythingServer
- **Assumes**: Full lifecycle control; must call `mcpServer.RunAsync()` explicitly; cleanup in `finally`
- **Mechanism**: `async (httpContext, mcpServer, token) => { /* setup, run, cleanup */ }`

#### Nature of Tension

RunSessionHandler takes ownership of session execution. Whether both can coexist in the same server is not demonstrated.

**Resolution**: Not provided — ConfigureSessionOptions for simple filtering, RunSessionHandler for full lifecycle.

---

### Tension 5: Implicit vs. Explicit Session Management

**Domain**: ServerArchitecture

#### Approach A: Implicit

- **Observed in**: AspNetCoreMcpServer (default HTTP)
- **Assumes**: HTTP transport provides session-per-connection automatically

#### Approach B: Explicit

- **Observed in**: EverythingServer (RunSessionHandler)
- **Assumes**: Sessions need initialization, state tracking, background services, cleanup

#### Nature of Tension

Moving from implicit to explicit requires RunSessionHandler, fundamentally changing session execution.

**Resolution**: Not provided — implicit for simple servers, explicit for advanced features.

---

## SecurityModel Tensions

### Tension 6: OAuth 2.0 vs. Process Isolation

**Domain**: SecurityModel
**Manifestation**: Transport determines security model

#### Approach A: OAuth 2.0 (HTTP)

- **Observed in**: ProtectedMcpClient, ProtectedMcpServer
- **Assumes**: Network-based HTTP transport; untrusted clients; OAuth server infrastructure exists
- **Mechanism**: Authorization code flow, JWT bearer validation, resource metadata discovery

#### Approach B: Process Isolation (Stdio)

- **Observed in**: FileBasedMcpServer, TestServerWithHosting
- **Assumes**: Process-based stdio transport; OS process boundary provides security; no network exposure
- **Mechanism**: Client spawns server process; implicit trust

#### Nature of Tension

Stdio has no mechanism for bearer token exchange. OAuth requires HTTP infrastructure. A server cannot use both for the same connection.

**Resolution**: Not provided — determined by deployment context.

---

### Tension 7: Scope Declaration vs. Enforcement

**Domain**: SecurityModel

#### Approach A: Declared

- **Observed in**: ProtectedMcpServer
- **Mechanism**: `ScopesSupported = ["mcp:tools"]` in ResourceMetadata

#### Approach B: Enforced

- **Observed in**: ProtectedMcpServer
- **Mechanism**: `RequireAuthorization()` with no parameters — default policy requires authenticated user only

#### Nature of Tension

Declared scopes imply granular access control, but implementation enforces only authentication. Any valid token accesses all tools regardless of scopes.

**Resolution**: Not provided — may be intentional simplification.

---

### Tension 8: IHttpContextAccessor Registration vs. Consumption

**Domain**: SecurityModel

- **Registered in**: ProtectedMcpServer (`AddHttpContextAccessor()`)
- **Consumed by**: No tools — WeatherTools identical in protected and unprotected servers

#### Nature of Tension

Capability to access security context is provisioned but not exercised. Security boundary exists at endpoint level only, not tool level.

**Resolution**: Not provided — infrastructure without demonstrated consumption.

---

### Tension 9: Explicit Auth vs. Implicit Trust

**Domain**: SecurityModel

- **Explicit**: HTTP + OAuth (ProtectedMcpServer)
- **Implicit**: Stdio (TestServerWithHosting), in-memory (InMemoryTransport)

#### Nature of Tension

Code for trusted environments makes no auth provision; cannot move to untrusted without adding security. Security model determined by transport, not tool implementation.

**Resolution**: Not provided — implementer choice.

---

### Tension 10: Session Isolation Mechanisms

**Domain**: SecurityModel

- **Data isolation**: LongRunningTasks — `sessionId` filtering in storage operations
- **State isolation**: EverythingServer — per-session dictionaries and background services via RunSessionHandler
- **Capability isolation**: AspNetCoreMcpPerSessionTools — different tool subsets per route via ConfigureSessionOptions

#### Nature of Tension

Three different isolation guarantees addressing different concerns. No sample combines all three.

**Resolution**: Not provided — each addresses a different isolation concern.

---

## ToolManagement Tensions

### Tension 11: Builder vs. Factory Registration

**Domain**: ToolManagement

- **Builder**: `AddMcpServer().WithTools<T>()` — requires DI and hosting framework
- **Factory**: `McpServer.Create` with `McpServerOptions.ToolCollection` — no framework needed

Tool implementations are portable; registration mechanism is not.

**Resolution**: Not provided — implementer choice.

---

### Tension 12: Global vs. Per-Session Tool Scope

**Domain**: ToolManagement

- **Global**: `WithTools<T>()` exposes all tools to all sessions
- **Per-Session**: `ConfigureSessionOptions` assigns per-session `ToolCollection` based on route

Whether both coexist (filtering a subset of globally registered tools) is not demonstrated.

**Resolution**: Not provided — implementer choice.

---

### Tension 13: Attribute vs. Manual Discovery

**Domain**: ToolManagement

- **Attribute**: `[McpServerToolType]`/`[McpServerTool]` via `WithTools<T>()`
- **Manual**: `McpServerTool.Create(MethodInfo, target, options)` with rich metadata (multi-icon)

EverythingServer demonstrates both coexisting. Manual creation enables features unavailable via attributes.

**Resolution**: Not provided — both are valid.

---

### Tension 14: Static vs. Instance Tool Methods

**Domain**: ToolManagement

- **Static**: No constructor dependencies; `target: null` (AspNetCoreMcpPerSessionTools)
- **Instance**: Constructor-injected dependencies; DI resolves (AspNetCoreMcpServer WeatherTools, LongRunningTasks TaskTools)

Static methods cannot receive constructor-injected dependencies. The choice affects whether tools can access external services.

**Resolution**: Not provided — implementer choice.

---

### Tension 15: Proxy vs. Direct vs. LLM Invocation

**Domain**: ToolManagement / ClientServerContract

- **Proxy**: `tool.InvokeAsync(args)` — typed, programmatic (InMemoryTransport)
- **Direct**: `CallToolAsync(name, dict)` — string-based (ProtectedMcpClient)
- **LLM**: `UseFunctionInvocation` — LLM selects tools (ChatWithTools)

Each structures client code differently. Code written for one cannot switch to another without structural changes.

**Resolution**: Not provided — implementer choice.

---

### Tension 16: Simple String vs. Rich ContentBlock

**Domain**: ToolManagement / ClientServerContract

- **Simple**: Tools return plain strings (most samples)
- **Rich**: `IEnumerable<ContentBlock>` with `Annotations` (Audience, Priority) (EverythingServer)

Annotation system meaningful only with rich content. Simple tools cannot express audience-specific content.

**Resolution**: Not provided — implementer choice.

---

## TransportAbstraction Tensions

### Tension 17: Factory vs. Builder Server Creation

Equivalent to Tension 2, manifesting in transport context. See Tension 2.

---

### Tension 18: Stdio stdout Exclusivity vs. Normal Logging

**Domain**: TransportAbstraction

- **Stdio**: stdout reserved for MCP JSON-RPC; logs must go to stderr
- **HTTP**: No stdout constraint; normal logging channels

Log configuration valid for HTTP would corrupt MCP protocol on stdio. Logging code not naively shareable.

**Resolution**: Not provided — transport determines logging constraints.

---

### Tension 19: Single-Session vs. Multi-Session

**Domain**: TransportAbstraction

- **Single**: One client per process (stdio) or scope (in-memory)
- **Multi**: Multiple concurrent clients via HTTP connections

Session management code for HTTP (`ConfigureSessionOptions`, `RunSessionHandler`) has no equivalent in stdio/in-memory.

**Resolution**: Not provided — transport determines session model.

---

### Tension 20: Client-Managed vs. Independent Service

**Domain**: TransportAbstraction

- **Client-Managed**: `StdioClientTransport` spawns server process; server lifetime bound to client
- **Independent**: HTTP server runs independently; client connects via URL

Fundamentally different deployment models.

**Resolution**: Not provided — implementer choice.

---

### Tension 21: Explicit vs. Abstracted Stream Wiring

**Domain**: TransportAbstraction

- **Explicit**: `Pipe`, `AsStream()`, `StreamServerTransport` (InMemoryTransport)
- **Abstracted**: `WithStdioServerTransport()`, `WithHttpTransport()` hide streams entirely

**Resolution**: Not provided — explicit for understanding/testing, abstracted for production.

---

### Tension 22: Transport-Specific Auth vs. Transport-Agnostic Tools

**Domain**: TransportAbstraction

- Auth is transport-layer concern (OAuth in `HttpClientTransportOptions`)
- Tools are transport-agnostic (WeatherTools identical in protected and unprotected servers)

If tools need auth decisions (per-user data), they must access `IHttpContextAccessor`, breaking transport-agnosticism.

**Resolution**: Not provided — current samples show purely transport-agnostic tools.

---

## ClientServerContract Tensions

### Tensions 23-28

Tensions 23 (Proxy vs. Direct vs. LLM), 25 (Simple vs. Rich Content), and 24 (Ephemeral vs. Durable) overlap with ToolManagement tensions above. Additional unique tensions:

### Tension 26: Sampling vs. No Sampling

- **With Sampling**: Server calls `SampleAsync`; client registers handler (EverythingServer, TestServerWithHosting)
- **Without Sampling**: Unidirectional client→server only (ProtectedMcpClient, InMemoryTransport, FileBasedMcpServer)

Servers requiring sampling fail without client handler. Whether client supports sampling is not discoverable at connection time.

---

### Tension 27: One-Time vs. Dynamic Tool Discovery

- **One-Time**: `ListToolsAsync` called once, cached (ChatWithTools)
- **Dynamic**: Per-session tool filtering can vary tools across connections (AspNetCoreMcpPerSessionTools)

No protocol mechanism for tool change notifications demonstrated.

---

### Tension 28: Comprehensive vs. Minimal Protocol Surface

- **Comprehensive**: Tools, resources, prompts, subscriptions, completion, logging, progress, sampling (EverythingServer)
- **Minimal**: Tools only (FileBasedMcpServer, TestServerWithHosting)

No capability negotiation protocol demonstrated. Clients must handle absent capabilities defensively.

---

## AsyncOperations Tensions

### Tension 29: Sync vs. Async Tool Methods

Both styles coexist in single servers (EchoTool sync + WeatherTools async in AspNetCoreMcpServer). Framework supports both transparently.

---

### Tension 30: CancellationToken Acceptance vs. Omission

SampleLlmTool always accepts CancellationToken; WeatherTools always omits it — in the same server. Inconsistency unexplained.

---

### Tension 31: Fire-and-Forget vs. Awaited Server Execution

- **Fire-and-Forget**: `_ = server.RunAsync()` — silently swallows exceptions (InMemoryTransport)
- **Awaited**: `await mcpServer.RunAsync(token)` — blocks until session ends, propagates errors (EverythingServer)

---

### Tension 33: Direct SampleAsync vs. AsSamplingChatClient

- **Direct**: `server.SampleAsync(params, token)` with MCP-specific parameters (TestServerWithHosting, EverythingServer)
- **Abstracted**: `server.AsSamplingChatClient().GetResponseAsync(...)` with IChatClient interface (AspNetCoreMcpServer)

Both produce same protocol behavior; code uses different parameter types.

---

### Tension 34: Per-Session BackgroundService vs. None

- **Per-Session**: `using` statements in RunSessionHandler, manual `StartAsync(token)` (EverythingServer)
- **None**: All other samples — no per-session background work

Per-session BackgroundService requires RunSessionHandler. Unconventional pattern vs. DI-registered `IHostedService`.

---

## See Also

- [patterns.md](patterns.md) — Related implementation patterns
- [vocabulary.md](vocabulary.md) — Types involved in these tensions
