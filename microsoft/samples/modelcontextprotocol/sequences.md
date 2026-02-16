# MCP C# SDK - Sequence Progressions

**Part of**: [MCP C# SDK Sample Synthesis](README.md)
**Generated**: 2026-02-16 | **Version**: v1

---

## Sequence Overview

| Sequence | Samples | Theme | Key Evolution |
|----------|---------|-------|---------------|
| ServerHostingPatterns | FileBasedMcpServer → ProtectedMcpServer (6) | Server hosting from console to web with auth | Generic Host → ASP.NET Core → Session lifecycle → OAuth |
| AuthenticationSecurity | ProtectedMcpClient → ProtectedMcpServer (2) | OAuth 2.0 client-server protection | Client token acquisition ↔ Server token validation |
| ToolRegistrationStrategies | AspNetCoreMcpServer → EverythingServer (4) | Tool registration approaches | Builder attributes → Per-session → Lambda → Manual creation |
| TransportMechanisms | InMemoryTransport → ProtectedMcpClient (5) | Transport layer progression | Pipes → Stdio → HTTP → OAuth HTTP |
| ClientIntegration | InMemoryTransport → ProtectedMcpClient (3) | Client implementation patterns | In-process → Stdio + LLM → HTTP + OAuth |
| AdvancedFeatures | LongRunningTasks → EverythingServer (3) | Advanced MCP capabilities | Task persistence → Per-session tools → Full feature showcase |

---

## Sequence: ServerHostingPatterns

**Samples**: FileBasedMcpServer → TestServerWithHosting → AspNetCoreMcpServer → AspNetCoreMcpPerSessionTools → EverythingServer → ProtectedMcpServer
**Theme**: Evolution from minimal console applications to full-featured web servers with authentication

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | FileBasedMcpServer | Generic Host, stdio transport, file-based program, preprocessor directives | — |
| 2 | TestServerWithHosting | Serilog (3 sinks), stderr redirection, dual tool styles (sync/async), try-catch-finally lifecycle | Generic Host foundation |
| 3 | AspNetCoreMcpServer | ASP.NET Core WebApplication, HTTP transport, MapMcp, OpenTelemetry, constructor DI tools | Alternative host model |
| 4 | AspNetCoreMcpPerSessionTools | ConfigureSessionOptions, ToolCollection, per-session filtering, reflection caching, AOT annotations | HTTP transport |
| 5 | EverythingServer | RunSessionHandler, background services, prompts, subscriptions, completion, logging level, progress, icons, server metadata | Session management |
| 6 | ProtectedMcpServer | JWT Bearer auth, MCP auth scheme, ResourceMetadata, RequireAuthorization, IHttpContextAccessor | HTTP transport + middleware |

### Key Evolution

Two hosting models branch at step 3:

- **Generic Host + Stdio** (steps 1-2): Console applications with process isolation, stderr logging, single session
- **ASP.NET Core + HTTP** (steps 3-6): Web services with multi-session, middleware pipeline, progressive capability additions

Session management progresses within HTTP:
1. Implicit (HTTP connections) — steps 3-4 default
2. ConfigureSessionOptions (pre-protocol filtering) — step 4
3. RunSessionHandler (full lifecycle control) — step 5

### Vocabulary Introduced

| Sample | New Types/Terms |
|--------|----------------|
| FileBasedMcpServer | Host, WithStdioServerTransport, `#:package`, `file class` |
| TestServerWithHosting | LoggerConfiguration, AddSerilog, RollingInterval, standardErrorFromLevel |
| AspNetCoreMcpServer | WebApplication, WithHttpTransport, MapMcp, McpServerToolType, McpMeta |
| AspNetCoreMcpPerSessionTools | ConfigureSessionOptions, ToolCollection, McpServerTool.Create, DynamicallyAccessedMembers |
| EverythingServer | Implementation, Icon, RunSessionHandler, WithPrompts, WithSubscribeToResourcesHandler |
| ProtectedMcpServer | McpAuthenticationDefaults, ResourceMetadata, AddMcp, RequireAuthorization |

---

## Sequence: AuthenticationSecurity

**Samples**: ProtectedMcpClient → ProtectedMcpServer
**Theme**: OAuth 2.0 protection for MCP communication — complementary client and server perspectives

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | ProtectedMcpClient | OAuth authorization code flow, AuthorizationRedirectDelegate, HttpListener, browser launch, DynamicClientRegistration | HTTP transport |
| 2 | ProtectedMcpServer | Dual auth scheme (MCP + JWT Bearer), ResourceMetadata, .well-known endpoint, TokenValidationParameters, JwtBearerEvents | ASP.NET Core + HTTP |

### Key Evolution

Client and server demonstrate complementary OAuth perspectives:
- **Client**: Acquires tokens via interactive browser flow, presents bearer tokens
- **Server**: Validates tokens via OIDC discovery, enforces authentication at endpoint level

Notable gaps: No refresh tokens, no scope enforcement (declared but not validated), no claim-based tool behavior, single OAuth flow only (authorization code).

### Vocabulary Introduced

| Sample | New Types/Terms |
|--------|----------------|
| ProtectedMcpClient | OAuthConfiguration, AuthorizationRedirectDelegate, HttpListener, DynamicClientRegistration, SocketsHttpHandler |
| ProtectedMcpServer | McpAuthenticationDefaults, ResourceMetadata, Authority, TokenValidationParameters, JwtBearerEvents |

---

## Sequence: ToolRegistrationStrategies

**Samples**: AspNetCoreMcpServer → AspNetCoreMcpPerSessionTools → InMemoryTransport → EverythingServer
**Theme**: Evolution of tool registration from builder-based attribute discovery to manual creation with rich metadata

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | AspNetCoreMcpServer | `WithTools<T>()` with attribute scanning, dual styles (static + DI instance) | — |
| 2 | AspNetCoreMcpPerSessionTools | Startup reflection caching, ConfigureSessionOptions, ToolCollection assignment, static-only constraint | McpServerTool.Create |
| 3 | InMemoryTransport | `McpServer.Create` factory, inline lambda tools, collection expression syntax | McpServerTool.Create |
| 4 | EverythingServer | Manual creation via MethodInfo + options, multi-icon config (themes/sizes/data URIs), coexists with attribute approach | McpServerTool.Create + options |

### Key Evolution

Four coexisting registration mechanisms emerge without convergence:

1. **Builder + Attributes** (`WithTools<T>()`) — dominant pattern, 7 samples
2. **Per-Session Dynamic** (`ConfigureSessionOptions` + `ToolCollection`) — introduced once, not revisited
3. **Factory + Lambda** (`McpServer.Create`) — testing/embedding only
4. **Manual + Options** (`McpServerTool.Create(MethodInfo, target, options)`) — coexists with builder

Tool implementation style oscillates: dual (step 1) → static-only (step 2) → lambda (step 3) → instance (step 4).

### Patterns Introduced

| Sample | Pattern |
|--------|---------|
| AspNetCoreMcpServer | Attribute-based discovery, Description-driven docs, McpMeta metadata |
| AspNetCoreMcpPerSessionTools | Per-session filtering, startup pre-population, AOT reflection annotations |
| InMemoryTransport | Factory creation, inline lambda tools, collection expressions |
| EverythingServer | Manual creation, data URI icons, attribute + manual coexistence |

---

## Sequence: TransportMechanisms

**Samples**: InMemoryTransport → TestServerWithHosting → ChatWithTools → AspNetCoreMcpServer → ProtectedMcpClient
**Theme**: Transport layer progression from in-process pipes to authenticated network communication

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | InMemoryTransport | Pipe, StreamServerTransport, StreamClientTransport, AsStream, factory creation | — |
| 2 | TestServerWithHosting | WithStdioServerTransport, stderr logging, Generic Host lifetime | Stream abstraction (hidden) |
| 3 | ChatWithTools | StdioClientTransport, external process spawning, sampling handler | Stdio server (connects to) |
| 4 | AspNetCoreMcpServer | WithHttpTransport, MapMcp, WebApplication, network endpoint | — (independent branch) |
| 5 | ProtectedMcpClient | HttpClientTransport, OAuthConfiguration, SocketsHttpHandler, connection pooling | HTTP transport |

### Key Evolution

Transport progresses through three distinct layers:
1. **Stream-based** (step 1): Explicit pipe wiring, developer sees streams
2. **Process-based** (steps 2-3): Stdio abstracts streams, cross-process communication
3. **Network-based** (steps 4-5): HTTP abstracts entirely, optional OAuth layer

`StreamClientTransport` serves as internal foundation used by both `StdioClientTransport` and `HttpClientTransport`, but this layering is only visible in step 1.

---

## Sequence: ClientIntegration

**Samples**: InMemoryTransport → ChatWithTools → ProtectedMcpClient
**Theme**: Evolution of MCP client patterns from in-process to authenticated network communication

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | InMemoryTransport | McpClient.CreateAsync, ListToolsAsync, InvokeAsync (proxy), fire-and-forget server | — |
| 2 | ChatWithTools | StdioClientTransport, dual IChatClient, sampling handler, UseFunctionInvocation, streaming | McpClient foundation |
| 3 | ProtectedMcpClient | HttpClientTransport, OAuth flow, CallToolAsync (direct), tool availability check | McpClient foundation |

### Key Evolution

Three incompatible tool invocation patterns emerge:

| Step | Invocation Pattern | Mechanism |
|------|-------------------|-----------|
| 1 | Proxy objects | `tool.InvokeAsync(args)` — typed, programmatic |
| 2 | LLM-mediated | `UseFunctionInvocation` — LLM selects tools, middleware invokes |
| 3 | Direct protocol | `CallToolAsync(name, dict)` — string-based, manual |

LLM integration (step 2) appears then disappears, demonstrating it is an optional capability, not a required client pattern.

---

## Sequence: AdvancedFeatures

**Samples**: LongRunningTasks → AspNetCoreMcpPerSessionTools → EverythingServer
**Theme**: Progressive introduction of advanced MCP server capabilities

### Progression

| Step | Sample | Introduces | Builds On |
|------|--------|------------|-----------|
| 1 | LongRunningTasks | IMcpTaskStore, FileBasedMcpTaskStore, session isolation (data-level), TTL expiration, exclusive file locking | TaskStore configuration |
| 2 | AspNetCoreMcpPerSessionTools | ConfigureSessionOptions, per-session tool filtering, route-based categorization, reflection caching | Session isolation (capability-level) |
| 3 | EverythingServer | RunSessionHandler, background services, prompts, subscriptions, completion, logging, progress, annotations, icons | Session lifecycle (state-level) |

### Key Evolution

Session configuration evolves through three levels:
1. **Data isolation** (step 1): Storage operations filtered by `sessionId`
2. **Capability isolation** (step 2): Different tool subsets per session via `ConfigureSessionOptions`
3. **State isolation** (step 3): Per-session dictionaries, background services, lifecycle control via `RunSessionHandler`

Two long-running operation paradigms without convergence:
- **Durable** (step 1): `IMcpTaskStore` with file persistence, polling, TTL
- **Ephemeral** (step 3): `ProgressToken` with push notifications, lost on disconnect

---

## Cross-Sequence Observations

### Sample Overlap

Several samples appear in multiple sequences:

| Sample | Sequences |
|--------|-----------|
| InMemoryTransport | TransportMechanisms, ClientIntegration, ToolRegistrationStrategies |
| EverythingServer | ServerHostingPatterns, ToolRegistrationStrategies, AdvancedFeatures |
| AspNetCoreMcpPerSessionTools | ServerHostingPatterns, ToolRegistrationStrategies, AdvancedFeatures |
| ProtectedMcpClient | AuthenticationSecurity, TransportMechanisms, ClientIntegration |
| ChatWithTools | TransportMechanisms, ClientIntegration |

### No Convergence

Across all sequences, multiple approaches emerge for the same concern without converging to a single canonical pattern. This is observable in:

- Tool registration (4 approaches)
- Client invocation (3 approaches)
- Session management (3 levels)
- Long-running operations (2 paradigms)
- Observability (Serilog vs. OpenTelemetry)
- Security (process isolation vs. OAuth vs. none)

---

## See Also

- [vocabulary.md](vocabulary.md) — Complete vocabulary reference
- [patterns.md](patterns.md) — Full pattern catalog
