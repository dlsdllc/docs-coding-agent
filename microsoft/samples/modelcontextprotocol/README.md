# MCP C# SDK - Sample Synthesis

**Generated**: 2026-02-16
**Version**: v1
**Project ID**: project-0001

---

## Overview

This synthesis maps patterns, vocabulary, and capabilities observed in the Model Context Protocol (MCP) C# SDK samples. It is **descriptive, not prescriptive**. Multiple approaches are documented without preference. Tensions between approaches are noted without resolution.

| Metric | Count |
|--------|-------|
| Samples Analyzed | 10 |
| Sequences Analyzed | 6 |
| Domains Analyzed | 6 |
| Patterns Documented | ~137 |
| Tensions Identified | 34 |

---

## Document Index

| Document | Purpose | When to Read |
|----------|---------|--------------|
| [vocabulary.md](vocabulary.md) | Core types, methods, domain terms | Understanding the SDK's API surface and type system |
| [patterns.md](patterns.md) | Implementation patterns with code signatures | How things are typically done in the samples |
| [sequences.md](sequences.md) | Learning progressions across samples | Understanding sample ordering and feature evolution |
| [tensions.md](tensions.md) | Design choices and alternative approaches | When facing architectural decisions |
| [dependencies.md](dependencies.md) | Package requirements per capability | Setting up a project with correct packages |
| [coverage.md](coverage.md) | Gaps, methodology, sample index | Understanding synthesis scope and limitations |

---

## Quick Reference

### Most Common Patterns

1. **Fluent Builder Configuration** — `AddMcpServer().WithHttpTransport().WithTools<T>()` chain used in 7 of 8 server samples
2. **Attribute-Based Tool Discovery** — `[McpServerToolType]` / `[McpServerTool]` / `[Description]` used in 8 of 10 samples
3. **Tool Discovery via ListToolsAsync** — All 3 client samples use `McpClient.ListToolsAsync()` for capability enumeration
4. **Generic Host or ASP.NET Core Hosting** — All builder-based samples use one of two hosting models
5. **Transport-Agnostic Tool Implementations** — Tool code is portable across stdio, HTTP, and in-memory transports

### Key Tensions (Unresolved)

1. **Generic Host + Stdio vs. ASP.NET Core + HTTP**: Fundamentally different deployment models with cascading effects on session management, security, and observability
2. **Builder Pattern vs. Factory Method**: DI-integrated builder chain vs. `McpServer.Create` with caller-managed lifetime
3. **Global Tools vs. Per-Session Filtering**: `WithTools<T>()` exposes all tools unconditionally vs. `ConfigureSessionOptions` selects per-session
4. **Ephemeral Progress vs. Durable Tasks**: ProgressToken push notifications vs. IMcpTaskStore poll-based persistence
5. **Proxy vs. Direct vs. LLM-Mediated Invocation**: Three incompatible client-side tool calling patterns

### Required Dependencies (Core)

- `ModelContextProtocol` — Core MCP types (server, client, protocol)
- `ModelContextProtocol.AspNetCore` — ASP.NET Core integration (HTTP transport, MapMcp)
- `Microsoft.Extensions.Hosting` — Generic Host for stdio-based servers

---

## Architecture at a Glance

### Transport Determines Architecture

The most impactful decision observed across all samples is **transport choice**. This single decision cascades into:

| Transport | Hosting Model | Session Model | Security Model | Observability |
|-----------|---------------|---------------|----------------|---------------|
| Stdio | Generic Host | Single session per process | Process isolation | Serilog (stderr) |
| HTTP | ASP.NET Core | Multi-session concurrent | Open or OAuth 2.0 | OpenTelemetry |
| In-Memory | No host (factory) | Single session in-process | Implicit trust | None configured |

### Server-Side Registration Approaches

Four coexisting tool registration mechanisms are observed:

| Mechanism | Entry Point | Used In |
|-----------|-------------|---------|
| Builder attribute discovery | `WithTools<T>()` | 7 samples |
| Per-session dynamic | `ConfigureSessionOptions` + `ToolCollection` | 1 sample |
| Factory inline lambda | `McpServer.Create` + `McpServerTool.Create(lambda)` | 1 sample |
| Manual creation with options | `McpServerTool.Create(MethodInfo, target, options)` | 1 sample (coexists with builder) |

### Client-Side Invocation Approaches

Three incompatible client patterns:

| Pattern | Mechanism | Used In |
|---------|-----------|---------|
| Proxy objects | `ListToolsAsync()` + `tool.InvokeAsync(args)` | InMemoryTransport |
| Direct protocol | `CallToolAsync(name, dictionary)` | ProtectedMcpClient |
| LLM-mediated | `UseFunctionInvocation` middleware + all tools passed to LLM | ChatWithTools |

---

## Usage Notes

**This synthesis can be used for:**
- LLM context when working with MCP C# SDK
- Onboarding new contributors to the SDK samples
- API design reference for MCP server/client implementations
- Documentation source material for MCP C# patterns

**This synthesis should NOT be used as:**
- Prescriptive guidance on "the right way" to use MCP
- Architectural recommendations for production systems
- A replacement for official MCP documentation
- A complete API reference (only sample-demonstrated APIs are covered)
