# MCP C# SDK - Coverage and Methodology

**Part of**: [MCP C# SDK Sample Synthesis](README.md)
**Generated**: 2026-02-16 | **Version**: v1

---

## Gaps and Limitations

### Not Demonstrated in Samples

- **Hybrid transport hosting**: No sample combines stdio and HTTP in a single server
- **Custom transport implementations**: No gRPC, named pipes, Unix domain sockets, or WebSocket transport
- **Transport fallback or negotiation**: No retry logic or transport type negotiation
- **Capability negotiation**: Clients cannot query which optional features a server supports
- **Tool change notifications**: No protocol mechanism for servers to notify clients of tool set changes
- **Tool unregistration or hot-reload**: No runtime tool removal or modification after startup
- **Multiple OAuth flows**: Only authorization code with browser; no device code, client credentials, refresh tokens
- **Intermediate authentication**: Samples jump from zero auth to full OAuth 2.0; no API keys, basic auth, mTLS
- **Scope enforcement**: Scopes declared but only default policy (authenticated user) enforced
- **Claim-based tool behavior**: IHttpContextAccessor registered but tools never access claims
- **Role-based access control**: RoleClaimType configured but no role-based policies
- **Graceful shutdown**: No coordinated shutdown with request draining or session cleanup across multiple sessions
- **CPU-bound tool offloading**: No `Task.Run` or thread pool usage for compute-intensive tools
- **ValueTask optimization**: All async operations use `Task<T>`, never `ValueTask<T>`
- **Async server initialization**: No async factory or initialization middleware for servers needing async setup
- **Prompt protocol (client-side)**: No client sample retrieves or uses prompts
- **Resource protocol (client-side)**: No client sample lists, reads, or subscribes to resources
- **Completion pagination**: HasMore/Total properties exist but full pagination not demonstrated
- **Protocol version negotiation**: Version appears in headers but no negotiation shown
- **Cross-session security**: No session fixation, hijacking, or cross-session data access protection shown

### Partial Coverage

- **OAuth lifecycle**: Token acquisition demonstrated but not refresh, expiration handling, revocation, or persistence
- **Error handling**: McpProtocolException shown (EverythingServer) but comprehensive tool-level error patterns not demonstrated
- **Background services**: Only EverythingServer demonstrates per-session BackgroundService; no DI-registered IHostedService pattern for MCP
- **Lambda tool limitations**: Only simple single-parameter synchronous lambdas; no async lambdas, multiple parameters, or injectable parameters shown
- **Icon handling**: Icons configured on server but client-side icon selection/display not demonstrated
- **McpMeta metadata**: Used once (AspNetCoreMcpServer WeatherTools); consumption pattern not shown

### Assumed Knowledge

- .NET development (C#, async/await, dependency injection)
- ASP.NET Core fundamentals (middleware pipeline, WebApplication, hosting)
- OAuth 2.0 concepts (authorization code flow, JWT, OIDC)
- MCP protocol specification (JSON-RPC, tool/resource/prompt concepts)
- NuGet package management and .NET SDK build system

---

## Sample Index

| Sample | Path | Tier 1 | Sequences | Domains |
|--------|------|--------|-----------|---------|
| AspNetCoreMcpServer | samples/AspNetCoreMcpServer | ✓ | ServerHostingPatterns, ToolRegistrationStrategies, TransportMechanisms | All 6 |
| AspNetCoreMcpPerSessionTools | samples/AspNetCoreMcpPerSessionTools | ✓ | ServerHostingPatterns, ToolRegistrationStrategies, AdvancedFeatures | All 6 |
| ChatWithTools | samples/ChatWithTools | ✓ | TransportMechanisms, ClientIntegration | All 6 |
| EverythingServer | samples/EverythingServer | ✓ | ServerHostingPatterns, ToolRegistrationStrategies, AdvancedFeatures | All 6 |
| FileBasedMcpServer | samples/FileBasedMcpServer | ✓ | ServerHostingPatterns, TransportMechanisms | All 6 |
| InMemoryTransport | samples/InMemoryTransport | ✓ | ToolRegistrationStrategies, TransportMechanisms, ClientIntegration | All 6 |
| LongRunningTasks | samples/LongRunningTasks | ✓ | ServerHostingPatterns, AdvancedFeatures | All 6 |
| ProtectedMcpClient | samples/ProtectedMcpClient | ✓ | AuthenticationSecurity, TransportMechanisms, ClientIntegration | All 6 |
| ProtectedMcpServer | samples/ProtectedMcpServer | ✓ | ServerHostingPatterns, AuthenticationSecurity | All 6 |
| TestServerWithHosting | samples/TestServerWithHosting | ✓ | ServerHostingPatterns, TransportMechanisms | All 6 |

---

## Methodology

This synthesis was produced using a 4-tier descriptive analysis:

1. **Tier 1 (Single Sample)**: Each of the 10 samples analyzed independently for vocabulary, patterns, assumptions, and dependencies
2. **Tier 2 (Sequence Groups)**: 6 sequences analyzed for progressive evolution of concepts and patterns across related samples
3. **Tier 3 (Capability Domains)**: 6 cross-cutting capability domains analyzed for alignment, divergence, tensions, and inter-domain relationships
4. **Tier 4 (Cross-Domain Synthesis)**: Final synthesis producing 7 self-contained documents optimized for RAG retrieval

**Constraints applied:**
- Descriptive only, not prescriptive
- No recommendations or preferences stated
- Tensions noted without resolution
- All observed approaches documented equally
- Minority patterns preserved (even single-sample patterns)
- Gaps and limitations honestly acknowledged

---

## Coverage Summary

### Tier 1: Samples Analyzed (10/10)

| Sample | Key Focus |
|--------|-----------|
| AspNetCoreMcpServer | ASP.NET Core + HTTP, fluent builder, dual tool styles, OpenTelemetry |
| AspNetCoreMcpPerSessionTools | Per-session tool filtering, ConfigureSessionOptions, reflection caching, AOT |
| ChatWithTools | Client + LLM, StdioClientTransport, streaming, sampling handler |
| EverythingServer | Comprehensive feature showcase: prompts, subscriptions, completion, lifecycle |
| FileBasedMcpServer | Minimal stdio server, single-file program, preprocessor directives |
| InMemoryTransport | Factory creation, in-memory pipes, lambda tools, proxy invocation |
| LongRunningTasks | IMcpTaskStore, file-based persistence, session isolation, TTL |
| ProtectedMcpClient | OAuth client, authorization code flow, HttpClientTransport, CallToolAsync |
| ProtectedMcpServer | JWT Bearer auth, MCP auth scheme, ResourceMetadata, RequireAuthorization |
| TestServerWithHosting | Generic Host, Serilog (3 sinks), stderr separation, dual tool styles |

### Tier 2: Sequences Analyzed (6/6)

| Sequence | Samples | Theme |
|----------|---------|-------|
| ServerHostingPatterns | 6 samples | Console → web server hosting evolution |
| AuthenticationSecurity | 2 samples | Client + server OAuth perspectives |
| ToolRegistrationStrategies | 4 samples | Builder → per-session → lambda → manual creation |
| TransportMechanisms | 5 samples | Pipes → stdio → HTTP → OAuth HTTP |
| ClientIntegration | 3 samples | In-process → stdio+LLM → HTTP+OAuth clients |
| AdvancedFeatures | 3 samples | Tasks → per-session tools → full capabilities |

### Tier 3: Domains Analyzed (6/6)

| Domain | Samples Covered | Variations | Tensions |
|--------|----------------|------------|----------|
| ServerArchitecture | 8 | 4 hosting models | 5 |
| SecurityModel | 10 (2 primary) | 5 security models | 5 |
| ToolManagement | 10 | 7 registration/invocation | 6 |
| TransportAbstraction | 10 | 4 transport types | 6 |
| ClientServerContract | 10 | 16 protocol variations | 6 |
| AsynchronousOperations | 10 | 13 async patterns | 6 |

### Known Gaps

- **External dependencies not analyzed**: TestOAuthServer (in tests directory) is not part of the sample set
- **Test samples not analyzed**: Unit tests and integration tests in the SDK are outside scope
- **Non-C# samples not analyzed**: Only C# SDK samples; Node.js MCP servers referenced by ChatWithTools not analyzed
- **SDK internals not analyzed**: Internal implementation of McpServer, McpClient, transport base classes not examined
- **Official documentation not cross-referenced**: Synthesis based solely on sample code, not specification or docs

---

## See Also

- [README.md](README.md) — Navigation hub and quick reference
- [tensions.md](tensions.md) — Unresolved design choices
