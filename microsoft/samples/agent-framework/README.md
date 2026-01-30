# Microsoft Agent Framework - Sample Synthesis

**Generated**: 2026-01-29
**Version**: v1
**Project ID**: project-0001

---

## Overview

This synthesis maps patterns, vocabulary, and capabilities observed in Microsoft Agent Framework .NET samples. It is **descriptive, not prescriptive**. Multiple approaches are documented without preference. Tensions between approaches are noted without resolution.

| Metric | Count |
|--------|-------|
| Samples Analyzed | 22 |
| Sequences Analyzed | 5 |
| Domains Analyzed | 13 |
| Patterns Documented | 199 |
| Pattern Categories | 18 |
| Types Documented | 200+ |
| Tensions Identified | 15+ |

---

## Document Index

| Document | Purpose | When to Read |
|----------|---------|--------------|
| [vocabulary.md](vocabulary.md) | Core types, methods, domain terms | Understanding the framework's API surface |
| [patterns.md](patterns.md) | Implementation patterns with code signatures | How things are typically done |
| [sequences.md](sequences.md) | Learning progressions | Understanding sample ordering and evolution |
| [tensions.md](tensions.md) | Design choices and alternatives | When facing architectural decisions |
| [dependencies.md](dependencies.md) | Package requirements | Setting up a project |
| [coverage.md](coverage.md) | Gaps, methodology, sample index | Understanding synthesis scope |

---

## Quick Reference

### Most Common Patterns
1. **Fluent Agent Creation** — `AzureOpenAIClient → GetChatClient() → AsAIAgent()` chain
2. **Thread-Based Conversation** — `GetNewThreadAsync()` for multi-turn state
3. **Executor-Based Workflows** — `WorkflowBuilder.AddEdge()` for graph construction
4. **AIContextProvider Injection** — Custom context via `InvokingAsync`/`InvokedAsync` hooks
5. **Streaming Execution** — `RunStreamingAsync()` / `StreamAsync()` for incremental results

### Key Tensions (Unresolved)

1. **Client-Side vs Server-Side MCP**: Application controls tools vs backend service invokes tools — different trust boundaries, observability, and dependencies
2. **Thread State vs Workflow State**: `AgentThread` serialization vs `QueueStateUpdateAsync`/checkpointing — different lifecycles and persistence models
3. **Programmatic vs Declarative Workflows**: C# code vs YAML definitions — compile-time safety vs runtime flexibility
4. **Full History vs Extracted Memories**: Complete message storage vs discrete fact extraction — different retrieval and storage patterns
5. **Implicit vs Explicit Agent Lifecycle**: Process-scoped cleanup vs `DeleteAgentAsync()` — resource management models

### Required Dependencies (Universal)

- `Microsoft.Agents.AI` — Core agent abstractions
- `Azure.AI.OpenAI` — Azure OpenAI integration (most samples)
- `Azure.Identity` — Azure credential management

### Domain Summary

| Domain | Purpose | Key Patterns |
|--------|---------|--------------|
| Agent Hosting | Deployment models | Console REPL, DI Hosted Service, A2A Server, Foundry Persistent |
| State Management | Persistence | Thread serialization, ChatHistoryProvider, Workflow checkpointing |
| Workflow Orchestration | Graph execution | Sequential, Concurrent, Conditional, Human-in-the-Loop |
| Protocol Integration | Interoperability | A2A, MCP (client-side and server-side) |
| Tool Integration | Function calling | AIFunctionFactory, ApprovalRequiredAIFunction, Plugin pattern |
| Observability | Telemetry | OpenTelemetry tracing, metrics, Azure Monitor |

---

## Usage Notes

**This synthesis can be used for:**
- LLM context when working with Microsoft Agent Framework
- Onboarding new contributors to the framework
- API design reference and pattern discovery
- Documentation source material
- Understanding cross-cutting concerns and tensions

**This synthesis should NOT be used as:**
- Prescriptive guidance on "the right way"
- Architectural recommendations
- A replacement for official documentation
- Production code reference (samples are educational)

---

## Architecture at a Glance

### Core Abstractions

```
AIAgent (abstract)
├── ChatClientAgent         (wraps IChatClient)
├── PersistentAgent         (Azure AI Foundry)
├── Workflow.AsAgent()      (workflow wrapped as agent)
└── Custom implementations  (extend AIAgent)

AgentThread
├── InMemoryAgentThread     (default)
├── Custom implementations  (with ChatHistoryProvider)
└── Serializable state      (via Serialize()/DeserializeThreadAsync())

Workflow
├── WorkflowBuilder         (programmatic construction)
├── DeclarativeWorkflowBuilder (YAML parsing)
├── Executor<TIn, TOut>     (typed processing units)
└── InProcessExecution      (runtime entry point)
```

### Protocol Boundaries

```
┌─────────────────────────────────────────────────────────────────┐
│  Application Layer                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │ A2A Client  │  │ MCP Client  │  │ Direct Chat │              │
│  │ (discovery) │  │ (tools)     │  │ (IChatClient)│             │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Agent Framework (AIAgent interface)                            │
│  - GetNewThreadAsync(), RunAsync(), RunStreamingAsync()         │
│  - ChatHistoryProvider, AIContextProvider hooks                 │
│  - Middleware pipeline (chat + function invocation)             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Backend Services                                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │ Azure OpenAI│  │ Azure AI    │  │ Hosted MCP  │              │
│  │ (ChatClient)│  │ Foundry     │  │ (HostedMcp  │              │
│  │             │  │ (Persistent)│  │  ServerTool)│              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
└─────────────────────────────────────────────────────────────────┘
```

---

## See Also

- [vocabulary.md](vocabulary.md) — Complete type and method reference
- [patterns.md](patterns.md) — Implementation patterns with code
- [tensions.md](tensions.md) — Design decisions left to implementers
