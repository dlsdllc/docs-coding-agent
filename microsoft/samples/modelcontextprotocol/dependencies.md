# MCP C# SDK - Dependency Landscape

**Part of**: [MCP C# SDK Sample Synthesis](README.md)
**Generated**: 2026-02-16 | **Version**: v1

---

## Required Dependencies (Universal)

These packages are required for all MCP C# SDK usage:

| Package | Observed Version(s) | Purpose |
|---------|--------------------:|---------|
| ModelContextProtocol | (project reference) | Core MCP types: McpServer, McpClient, protocol types, attributes |

**Note**: All samples reference MCP packages as project references (not NuGet), with versions managed centrally (no version attributes in PackageReference elements). This suggests the samples are part of the SDK repository, referencing source directly.

---

## Conditional Dependencies (Per Capability)

These packages are needed only for specific capabilities:

### Server Hosting

| Capability | Package | When Needed |
|------------|---------|-------------|
| Generic Host (stdio servers) | `Microsoft.Extensions.Hosting` | Console/daemon MCP servers with stdio transport |
| ASP.NET Core (HTTP servers) | `Microsoft.NET.Sdk.Web` (SDK) | Web-hosted MCP servers with HTTP transport |
| ASP.NET Core MCP integration | `ModelContextProtocol.AspNetCore` | HTTP transport, MapMcp, ConfigureSessionOptions, RunSessionHandler |

### Observability

| Capability | Package | When Needed |
|------------|---------|-------------|
| Serilog logging | `Serilog`, `Serilog.Sinks.File`, `Serilog.Sinks.Debug`, `Serilog.Sinks.Console` | Structured logging in stdio servers |
| Serilog + DI integration | `Serilog.Extensions.Hosting` | Integrating Serilog with Generic Host |
| OpenTelemetry tracing | `OpenTelemetry.Exporter.OpenTelemetryProtocol` | Distributed tracing for HTTP servers |
| OpenTelemetry ASP.NET Core | `OpenTelemetry.Instrumentation.AspNetCore` | HTTP request instrumentation |
| OpenTelemetry HTTP | `OpenTelemetry.Instrumentation.Http` | Outbound HTTP call instrumentation |

### Authentication

| Capability | Package | When Needed |
|------------|---------|-------------|
| JWT Bearer validation | `Microsoft.AspNetCore.Authentication.JwtBearer` | Server-side OAuth 2.0 token validation |
| MCP authentication scheme | `ModelContextProtocol.AspNetCore.Authentication` | MCP-specific auth scheme + resource metadata |

### Client Integration

| Capability | Package | When Needed |
|------------|---------|-------------|
| Chat client abstraction | `Microsoft.Extensions.AI` | IChatClient, UseFunctionInvocation, streaming |
| OpenAI provider | `Microsoft.Extensions.AI.OpenAI`, `OpenAI` | Connecting to OpenAI models |
| In-memory transport | `System.IO.Pipelines` | Pipe-based in-process communication |
| HTTP client transport | `System.Net.Http` (framework) | HttpClientTransport, SocketsHttpHandler |

### Advanced Features

| Capability | Package | When Needed |
|------------|---------|-------------|
| AOT compilation | (build configuration) | `PublishAot=true` in .csproj; conditional on .NET 9.0 |
| AOT reflection preservation | `System.Diagnostics.CodeAnalysis` (framework) | `[DynamicallyAccessedMembers]` attribute |
| JSON source generation | `System.Text.Json` (framework) | `[JsonSourceGeneration]` for AOT-compatible serialization |
| File-based programs | .NET 10+ SDK | `#:package` and `#:project` preprocessor directives |

---

## Dependency Patterns by Hosting Model

### Generic Host + Stdio Server

```
Microsoft.Extensions.Hosting
ModelContextProtocol (project ref)
├── Optional: Serilog.* (structured logging)
└── Optional: OpenTelemetry.* (tracing)
```

**Samples**: FileBasedMcpServer, TestServerWithHosting, LongRunningTasks

### ASP.NET Core + HTTP Server

```
Microsoft.NET.Sdk.Web
ModelContextProtocol.AspNetCore (project ref)
├── Optional: OpenTelemetry.* (tracing)
├── Optional: Microsoft.AspNetCore.Authentication.JwtBearer (auth)
└── Optional: ModelContextProtocol.AspNetCore.Authentication (MCP auth scheme)
```

**Samples**: AspNetCoreMcpServer, AspNetCoreMcpPerSessionTools, EverythingServer, ProtectedMcpServer

### Factory + In-Memory (No Host)

```
ModelContextProtocol (project ref)
System.IO.Pipelines
```

**Samples**: InMemoryTransport

### MCP Client

```
ModelContextProtocol (project ref)
├── Optional: Microsoft.Extensions.AI (LLM integration)
├── Optional: Microsoft.Extensions.AI.OpenAI (OpenAI provider)
└── Optional: OpenTelemetry.* (tracing)
```

**Samples**: ChatWithTools, ProtectedMcpClient, InMemoryTransport (client portion)

---

## Capability Domain Summary

| Domain | Patterns | Samples | Key Tensions |
|--------|----------|---------|--------------|
| ServerArchitecture | ~15 | 8 | Generic Host vs. ASP.NET Core; Builder vs. Factory; Serilog vs. OpenTelemetry |
| SecurityModel | ~10 | 10 (2 primary) | OAuth vs. Process Isolation; Scope declaration vs. enforcement |
| ToolManagement | ~20 | 10 | Builder vs. Factory registration; Global vs. per-session scope; 3 invocation patterns |
| TransportAbstraction | ~10 | 10 | Stdio vs. HTTP vs. In-Memory; Single vs. multi-session |
| ClientServerContract | ~16 | 10 | 3 invocation patterns; Ephemeral vs. durable progress; Sampling vs. no sampling |
| AsynchronousOperations | ~13 | 10 | Sync vs. async tools; CancellationToken acceptance; Fire-and-forget vs. awaited |

---

## Build System Observations

- **Central Package Management**: All project-based samples omit version attributes in PackageReference elements, relying on Directory.Build.props or Directory.Packages.props for version resolution
- **Project References**: MCP packages are referenced as `ProjectReference`, not NuGet packages (consistent with samples being part of the SDK repo)
- **AOT Compilation**: Conditional on target framework (`net9.0`), configured via `PublishAot=true` with reflection annotations
- **SDK Type**: Web samples use `Microsoft.NET.Sdk.Web`; console samples use `Microsoft.NET.Sdk`
- **File-Based Programs**: FileBasedMcpServer uses `#:package` and `#:project` preprocessor directives (.NET 10+), eliminating .csproj entirely

---

## See Also

- [patterns.md](patterns.md) — How these dependencies are used
- [coverage.md](coverage.md) — What capabilities were analyzed
