# MCP C# SDK - Vocabulary Reference

**Part of**: [MCP C# SDK Sample Synthesis](README.md)
**Generated**: 2026-02-16 | **Version**: v1

---

## Core Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| McpServer | ModelContextProtocol.Server | Server instance; injectable as tool method parameter for sampling and notifications |
| McpClient | ModelContextProtocol.Client | Client-side connection handler; factory via `CreateAsync` |
| McpServerTool | ModelContextProtocol.Server | Runtime representation of a tool; created via attributes, reflection, lambda, or manual `Create` |
| McpServerOptions | ModelContextProtocol.Server | Configuration object for server initialization (tools, capabilities, server info) |
| McpServerToolCreateOptions | ModelContextProtocol.Server | Options for `McpServerTool.Create` (Name, Icons) |
| McpException | ModelContextProtocol | Custom exception type for MCP-related errors |
| McpProtocolException | ModelContextProtocol | Protocol exception with `McpErrorCode` for structured error responses |
| McpErrorCode | ModelContextProtocol | Standard MCP error codes (InvalidParams, etc.) |
| IMcpTaskStore | ModelContextProtocol | Interface for durable task storage and lifecycle management (experimental) |
| McpTask | ModelContextProtocol.Protocol | Protocol type representing task metadata and status |
| McpTaskStatus | ModelContextProtocol.Protocol | Enum: Working, Completed, Failed, Cancelled |
| McpTaskMetadata | ModelContextProtocol.Protocol | Task creation parameters including TimeToLive |

---

## Transport Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| StdioClientTransport | ModelContextProtocol.Client | Client transport spawning external process via stdin/stdout |
| HttpClientTransport | ModelContextProtocol.Client | Client transport using HTTP with optional OAuth support |
| HttpClientTransportOptions | ModelContextProtocol.Client | Configuration for HTTP transport (OAuth, session options) |
| StreamServerTransport | ModelContextProtocol.Server | Server transport accepting explicit Stream pairs |
| StreamClientTransport | ModelContextProtocol.Client | Client transport accepting explicit Stream pairs; used internally by Stdio and HTTP transports |
| Pipe | System.IO.Pipelines | In-memory bidirectional communication channel |
| PipeReader / PipeWriter | System.IO.Pipelines | Reader/writer ends of Pipe, convertible to Stream via `AsStream()` |
| SocketsHttpHandler | System.Net.Http | Custom HTTP handler with connection pooling settings |

---

## Protocol Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| ContentBlock | ModelContextProtocol.Protocol | Base class for tool response content |
| TextContentBlock | ModelContextProtocol.Protocol | Text content with optional annotations |
| ImageContentBlock | ModelContextProtocol.Protocol | Image content with base64 data and MIME type |
| Annotations | ModelContextProtocol.Protocol | Content metadata with Audience and Priority |
| TextResourceContents | ModelContextProtocol.Protocol | Resource contents with text/plain data |
| BlobResourceContents | ModelContextProtocol.Protocol | Resource contents with base64-encoded binary data |
| Implementation | ModelContextProtocol.Protocol | Server metadata: Name, Version, Title, Description, WebsiteUrl, Icons |
| Icon | ModelContextProtocol.Protocol | Icon metadata: Source URL, MimeType, Sizes, Theme |
| CreateMessageRequestParams | ModelContextProtocol.Protocol | Parameters for LLM sampling requests |
| SamplingMessage | ModelContextProtocol.Protocol | Message in sampling request with Role and Content |
| ContextInclusion | ModelContextProtocol.Protocol | Enum controlling server context in sampling (ThisServer, None, AllServers) |
| CompleteResult | ModelContextProtocol.Protocol | Completion response with Values, HasMore, Total |
| EmptyResult | ModelContextProtocol.Protocol | Success response with no data (used by handlers) |
| LoggingLevel | ModelContextProtocol.Protocol | MCP logging levels (Debug, Info, Notice, Warning, Error, Critical, Alert, Emergency) |
| ResourceTemplateReference | ModelContextProtocol.Protocol | Reference to resource with URI template parameters |
| PromptReference | ModelContextProtocol.Protocol | Reference to prompt for completion |
| RequestId | ModelContextProtocol.Protocol | JSON-RPC request identifier type |
| CallToolRequestParams | ModelContextProtocol.Protocol | Parameters for tool invocation requests |
| ListTasksResult | ModelContextProtocol.Protocol | Result type containing array of McpTask objects |

---

## Chat/AI Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| IChatClient | Microsoft.Extensions.AI | Chat client abstraction interface |
| IChatClientBuilder | Microsoft.Extensions.AI | Builder interface for configuring IChatClient middleware |
| ChatMessage | Microsoft.Extensions.AI | Single message in chat conversation |
| ChatResponseUpdate | Microsoft.Extensions.AI | Single update chunk from streaming response |
| ChatRole | Microsoft.Extensions.AI | Enum distinguishing user/assistant/system roles |

---

## OAuth/Authentication Types

| Type | Namespace | Purpose (as demonstrated) |
|------|-----------|---------------------------|
| OAuthConfiguration | ModelContextProtocol.Client | OAuth settings: RedirectUri, AuthorizationRedirectDelegate, DynamicClientRegistration |
| McpAuthenticationDefaults | ModelContextProtocol.AspNetCore | Static class providing MCP authentication scheme name |
| ResourceMetadata | ModelContextProtocol.AspNetCore | OAuth resource metadata: ResourceDocumentation, AuthorizationServers, ScopesSupported |
| TokenValidationParameters | Microsoft.IdentityModel.Tokens | JWT validation: issuer, audience, signing key, lifetime |
| JwtBearerEvents | Microsoft.AspNetCore.Authentication.JwtBearer | Authentication lifecycle callbacks |

---

## Core Methods/Operations

### Server Configuration

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| AddMcpServer | IServiceCollection (extension) | Registers MCP server services, returns builder |
| WithHttpTransport | builder (extension) | Configures HTTP transport |
| WithStdioServerTransport | builder (extension) | Configures stdio transport |
| WithTools\<T\> | builder (extension) | Registers tool types via attribute scanning |
| WithResources\<T\> | builder (extension) | Registers resource types |
| WithPrompts\<T\> | builder (extension) | Registers prompt types |
| MapMcp | WebApplication (extension) | Maps MCP endpoints to ASP.NET Core pipeline |
| McpServer.Create | McpServer (static) | Factory method creating server with transport and options |

### Session Management

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| ConfigureSessionOptions | HttpTransportOptions (property) | Pre-protocol callback for per-session tool filtering |
| RunSessionHandler | HttpTransportOptions (property) | Full lifecycle callback with setup/execute/cleanup |

### Tool Creation

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| McpServerTool.Create | McpServerTool (static) | Creates tool from MethodInfo, delegate, or lambda |

### Client Operations

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| McpClient.CreateAsync | McpClient (static) | Creates and initializes client connection |
| ListToolsAsync | McpClient | Retrieves available tools from server |
| CallToolAsync | McpClient | Direct tool invocation with name and arguments dictionary |
| InvokeAsync | tool proxy | Executes tool via proxy object |

### Server-Side Capabilities

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| SendNotificationAsync | McpServer | Fire-and-forget server-to-client notification |
| SampleAsync | McpServer | Requests LLM sampling from connected client |
| AsSamplingChatClient | McpServer (extension) | Wraps sampling in IChatClient abstraction |
| WithSubscribeToResourcesHandler | builder (extension) | Registers resource subscription callback |
| WithUnsubscribeFromResourcesHandler | builder (extension) | Registers resource unsubscription callback |
| WithCompleteHandler | builder (extension) | Registers completion/autocomplete callback |
| WithSetLoggingLevelHandler | builder (extension) | Registers logging level change callback |

### Chat Integration

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| CreateSamplingHandler | IChatClient (extension) | Converts IChatClient to MCP sampling handler |
| AsIChatClient | ChatClient (extension) | Converts OpenAI ChatClient to IChatClient |
| UseFunctionInvocation | IChatClientBuilder (extension) | Enables LLM-mediated tool calling |
| GetStreamingResponseAsync | IChatClient | Streams chat responses incrementally |

### Authentication

| Method | On Type | Purpose (as demonstrated) |
|--------|---------|---------------------------|
| AddMcp | IServiceCollection (extension) | Registers MCP auth scheme with resource metadata |
| RequireAuthorization | endpoint builder (extension) | Applies authorization to MCP endpoint |
| AddAuthentication | IServiceCollection (extension) | Registers authentication services |
| AddJwtBearer | authentication builder (extension) | Configures JWT bearer validation |

---

## Attributes (Discovery Markers)

| Attribute | Target | Purpose |
|-----------|--------|---------|
| `[McpServerToolType]` | Class | Marks class as containing MCP tools |
| `[McpServerTool]` | Method | Marks method as an MCP tool (optional Name override) |
| `[McpServerResourceType]` | Class | Marks class as containing MCP resources |
| `[McpServerResource]` | Method | Marks method as providing an MCP resource (UriTemplate) |
| `[McpServerPromptType]` | Class | Marks class as containing MCP prompts |
| `[McpServerPrompt]` | Method | Marks method as an MCP prompt |
| `[Description]` | Method/Parameter | Human-readable documentation exposed via protocol |
| `[McpMeta]` | Method | Attaches key-value metadata (repeatable) |
| `[DynamicallyAccessedMembers]` | Generic parameter | Preserves reflection metadata for AOT compilation |

---

## Special Injectable Parameters

These types are automatically injected by the MCP framework when they appear as tool method parameters:

| Type | What It Provides |
|------|-----------------|
| `McpServer` | Current server instance (for sampling, notifications) |
| `RequestContext<T>` | Request context with Params, Server, JsonRpcRequest, CancellationToken |
| `CancellationToken` | Cancellation propagation from client disconnect or timeout |

---

## Type Relationships

### Server Creation Paths

- `AddMcpServer()` → builder → `WithTools<T>()` / `WithHttpTransport()` / `WithStdioServerTransport()` → host manages lifetime
- `McpServer.Create(transport, options)` → caller manages lifetime → `_ = server.RunAsync()`

### Transport Hierarchy

- `StreamClientTransport` ← used internally by `StdioClientTransport` and `HttpClientTransport`
- `StreamServerTransport` ← used directly only in InMemoryTransport sample
- Higher-level transports (`WithStdioServerTransport`, `WithHttpTransport`) abstract stream management completely

### Session Lifecycle

- Implicit sessions (HTTP default) → `ConfigureSessionOptions` (pre-protocol filtering) → `RunSessionHandler` (full lifecycle control)
- `RunSessionHandler` takes ownership; must call `mcpServer.RunAsync(token)` explicitly

### Content Type Hierarchy

- `ContentBlock` (base) → `TextContentBlock`, `ImageContentBlock`
- `Annotations` attached to `ContentBlock` with `Audience` (User, Assistant) and `Priority` (0-1 float)
- `TextResourceContents` / `BlobResourceContents` for resource-specific responses

---

## See Also

- [patterns.md](patterns.md) — How these types are used in practice
- [tensions.md](tensions.md) — Alternative approaches to similar problems
