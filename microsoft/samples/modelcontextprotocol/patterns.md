# MCP C# SDK - Pattern Catalog

**Part of**: [MCP C# SDK Sample Synthesis](README.md)
**Generated**: 2026-02-16 | **Version**: v1

---

## Pattern Summary

| Pattern | Category | Frequency | Key Samples |
|---------|----------|-----------|-------------|
| Fluent Builder Configuration | Server Initialization | 7 samples | AspNetCoreMcpServer, TestServerWithHosting, FileBasedMcpServer |
| Attribute-Based Tool Discovery | Tool Registration | 8 samples | AspNetCoreMcpServer, EverythingServer, FileBasedMcpServer |
| HTTP Transport Configuration | Transport | 5 samples | AspNetCoreMcpServer, EverythingServer, ProtectedMcpServer |
| Stdio Transport Configuration | Transport | 3 samples | FileBasedMcpServer, TestServerWithHosting, LongRunningTasks |
| Per-Session Tool Filtering | Tool Registration | 1 sample | AspNetCoreMcpPerSessionTools |
| Factory-Based Server Creation | Server Initialization | 1 sample | InMemoryTransport |
| Manual Tool Creation with Icons | Tool Registration | 1 sample | EverythingServer |
| Session Lifecycle via RunSessionHandler | Session Management | 1 sample | EverythingServer |
| OAuth 2.0 Authorization Code Flow | Security | 2 samples | ProtectedMcpClient, ProtectedMcpServer |
| Background Service Periodic Loops | Async Operations | 1 sample | EverythingServer |
| Durable Task Management (IMcpTaskStore) | Task Management | 1 sample | LongRunningTasks |
| LLM-Mediated Tool Invocation | Client Integration | 1 sample | ChatWithTools |
| Streaming Chat Response | Client Integration | 1 sample | ChatWithTools |
| In-Memory Pipe Communication | Transport | 1 sample | InMemoryTransport |

---

## Pattern: Fluent Builder Configuration

**Category**: Server Initialization
**Observed In**: AspNetCoreMcpServer, TestServerWithHosting, FileBasedMcpServer, LongRunningTasks, ProtectedMcpServer, AspNetCoreMcpPerSessionTools, EverythingServer
**Frequency**: 7 of 8 server samples (all except InMemoryTransport)

### Mechanism

MCP server is registered via `AddMcpServer()` on `IServiceCollection`, returning a builder. Subsequent `With*` extension methods configure transport, tools, resources, and prompts via method chaining. The hosting framework (Generic Host or ASP.NET Core) manages the server lifetime.

### Code Signature

```csharp
builder.Services
    .AddMcpServer()
    .WithHttpTransport()           // or .WithStdioServerTransport()
    .WithTools<EchoTool>()
    .WithTools<WeatherTools>()
    .WithResources<SimpleResourceType>();
```

### Assumptions

- `AddMcpServer()` must be called first to obtain the builder
- Multiple `WithTools<T>()` calls accumulate (do not replace)
- Builder requires hosting framework and DI container
- Transport must be configured exactly once

### Variations

| Variation | Samples | Difference |
|-----------|---------|------------|
| HTTP transport | AspNetCoreMcpServer, EverythingServer, ProtectedMcpServer | Uses `WithHttpTransport()`, requires `MapMcp()` on WebApplication |
| Stdio transport | FileBasedMcpServer, TestServerWithHosting, LongRunningTasks | Uses `WithStdioServerTransport()`, requires stderr log redirection |
| With Serilog | TestServerWithHosting | `AddSerilog()` chained before `AddMcpServer()` |
| With authentication | ProtectedMcpServer | `AddAuthentication().AddJwtBearer()` and `AddMcp()` before MCP server |

---

## Pattern: Attribute-Based Tool Discovery

**Category**: Tool Registration
**Observed In**: AspNetCoreMcpServer, TestServerWithHosting, FileBasedMcpServer, LongRunningTasks, ProtectedMcpServer, AspNetCoreMcpPerSessionTools, EverythingServer, ChatWithTools (server-side Node.js)
**Frequency**: 8 of 10 samples

### Mechanism

Classes are marked with `[McpServerToolType]` and methods with `[McpServerTool]`. The framework scans these at registration time via `WithTools<T>()`. `[Description]` attributes provide documentation exposed via the MCP protocol.

### Code Signature

```csharp
[McpServerToolType]
public class WeatherTools(IHttpClientFactory httpClientFactory)
{
    [McpServerTool, Description("Get weather alerts for a US state")]
    [McpMeta("ReadOnlyHint", "true")]
    public async Task<string> GetAlerts(
        [Description("Two-letter US state code")] string state)
    {
        // implementation
    }
}
```

### Assumptions

- Types are known at compile time
- DI container resolves constructor dependencies for instance tool classes
- All `[McpServerTool]`-attributed public methods within a type are registered
- Private methods in tool classes are NOT exposed

### Variations

| Variation | Samples | Difference |
|-----------|---------|------------|
| Static methods (no DI) | FileBasedMcpServer, AspNetCoreMcpPerSessionTools | `target: null`; no constructor dependencies |
| Instance with constructor DI | AspNetCoreMcpServer (WeatherTools), LongRunningTasks (TaskTools) | Constructor receives IHttpClientFactory, IMcpTaskStore |
| Named tool override | AspNetCoreMcpServer (SampleLlmTool) | `[McpServerTool(Name = "sampleLLM")]` overrides method name |
| McpMeta key-value metadata | AspNetCoreMcpServer (WeatherTools) | `[McpMeta("ReadOnlyHint", "true")]` repeatable |
| File-scoped class | FileBasedMcpServer | `file class` (C# 11) visibility modifier works with attributes |

---

## Pattern: Factory-Based Server Creation

**Category**: Server Initialization
**Observed In**: InMemoryTransport
**Frequency**: 1 sample

### Mechanism

`McpServer.Create(transport, options)` creates an MCP server directly without hosting framework or DI container. Transport is constructed explicitly. Tools are provided via `McpServerOptions.ToolCollection`.

### Code Signature

```csharp
await using var server = McpServer.Create(
    new StreamServerTransport(serverInput, serverOutput),
    new McpServerOptions
    {
        ToolCollection = [McpServerTool.Create(
            (string message) => $"Echo: {message}",
            new() { Name = "Echo" })]
    });
_ = server.RunAsync(); // fire-and-forget
```

### Assumptions

- No DI container or hosting framework needed
- Lambda parameters become tool parameters (names inferred from delegate)
- Server lifetime managed by caller via `await using`
- Fire-and-forget execution is acceptable for testing

---

## Pattern: Per-Session Tool Filtering

**Category**: Tool Registration
**Observed In**: AspNetCoreMcpPerSessionTools
**Frequency**: 1 sample

### Mechanism

No `WithTools<T>()` calls in builder. Instead, `ConfigureSessionOptions` callback on HTTP transport reads route parameters, looks up pre-cached tool arrays, and assigns them to `mcpOptions.ToolCollection`.

### Code Signature

```csharp
.WithHttpTransport(options =>
{
    options.ConfigureSessionOptions = async (httpContext, mcpOptions, ct) =>
    {
        var category = httpContext.Request.RouteValues["category"]?.ToString();
        var tools = toolDictionary.GetValueOrDefault(category);
        mcpOptions.Capabilities = new() { Tools = new() };
        mcpOptions.ToolCollection = new();
        foreach (var tool in tools) mcpOptions.ToolCollection.Add(tool);
    };
})
```

### Assumptions

- All tools discovered at startup via reflection and cached in `ConcurrentDictionary`
- `ConfigureSessionOptions` runs once per session initialization
- Direct `ToolCollection` assignment replaces any builder-registered tools
- `[DynamicallyAccessedMembers]` annotation preserves reflection metadata for AOT

---

## Pattern: Manual Tool Creation with Icon Configuration

**Category**: Tool Registration
**Observed In**: EverythingServer
**Frequency**: 1 sample (coexists with attribute-based registration)

### Mechanism

`McpServerTool.Create(MethodInfo, target, options)` constructs tools with rich metadata not available via attributes, including multi-icon configuration with themes, sizes, and data URIs.

### Code Signature

```csharp
var sampleTool = McpServerTool.Create(
    typeof(SampleLlmTool).GetMethod(nameof(SampleLlmTool.SampleLLM))!,
    null,
    new McpServerToolCreateOptions
    {
        Icons = [
            new Icon { Source = "https://example.com/icon.png", MimeType = "image/png" },
            new Icon { Source = "data:image/webp;base64,...", MimeType = "image/webp",
                       Sizes = ["1x1"], Theme = "dark" }
        ]
    });
// Passed alongside attribute-based tools:
.WithTools<AddTool>()
.WithTools([sampleTool])
```

### Assumptions

- `WithTools()` accepts both generic type parameters and explicit tool arrays
- Manual creation enables features not available via attributes (multi-icon)
- Both registration approaches coexist in the same server

---

## Pattern: Session Lifecycle via RunSessionHandler

**Category**: Session Management
**Observed In**: EverythingServer
**Frequency**: 1 sample

### Mechanism

`RunSessionHandler` callback receives `HttpContext`, `McpServer`, and `CancellationToken`. The handler performs setup (create per-session state, start background services), calls `mcpServer.RunAsync(token)` to process the session, then runs cleanup in a `finally` block.

### Code Signature

```csharp
options.RunSessionHandler = async (httpContext, mcpServer, token) =>
{
    var sessionId = mcpServer.SessionId;
    // Setup: per-session state, background services
    subscriptions.TryAdd(sessionId!, new());
    await using var sender = new SubscriptionMessageSender(mcpServer, subscriptions);
    await sender.StartAsync(token);
    try
    {
        await mcpServer.RunAsync(token);
    }
    finally
    {
        subscriptions.TryRemove(sessionId!, out _);
    }
};
```

### Assumptions

- `RunSessionHandler` takes ownership of session execution
- `mcpServer.RunAsync(token)` must be explicitly called
- `finally` block runs on disconnect, error, or cancellation
- Background services use `using` statements for per-session lifetime (not DI registration)
- `SessionId` may be null in stateless mode

---

## Pattern: OAuth 2.0 Authorization Code Flow

**Category**: Security
**Observed In**: ProtectedMcpClient (client), ProtectedMcpServer (server)
**Frequency**: 2 samples (client-server pair)

### Mechanism

**Client-side**: `HttpClientTransport` configured with `OAuthConfiguration`. When authentication needed, transport invokes `AuthorizationRedirectDelegate` which launches browser, starts local `HttpListener`, receives authorization code.

**Server-side**: Dual authentication scheme (MCP challenge + JWT Bearer authenticate), `AddMcp()` with `ResourceMetadata`, `RequireAuthorization()` on `MapMcp()`.

### Code Signature (Client)

```csharp
var transport = new HttpClientTransport(httpClient, new()
{
    Endpoint = new Uri("http://localhost:7071/sse"),
    OAuth = new()
    {
        RedirectUri = new Uri("http://localhost:1179"),
        AuthorizationRedirectDelegate = HandleAuthorizationUrlAsync,
        DynamicClientRegistration = new() { ClientName = "ProtectedMcpClient" }
    }
});
```

### Code Signature (Server)

```csharp
builder.Services.AddAuthentication(options =>
{
    options.DefaultChallengeScheme = McpAuthenticationDefaults.AuthenticationScheme;
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
}).AddJwtBearer(options => { /* TokenValidationParameters */ });

builder.Services.AddMcp(options =>
{
    options.ResourceMetadata = new ResourceMetadata
    {
        AuthorizationServers = [new Uri("https://localhost:7029")],
        ScopesSupported = ["mcp:tools"]
    };
});
// ...
app.MapMcp().RequireAuthorization();
```

### Assumptions

- External OAuth server running separately
- Browser available for interactive consent
- `HttpListener` requires URL reservation or admin on Windows
- Scope declared (`mcp:tools`) but only default policy enforced (authenticated user)

---

## Pattern: Durable Task Management (IMcpTaskStore)

**Category**: Task Management
**Observed In**: LongRunningTasks
**Frequency**: 1 sample

### Mechanism

Custom `FileBasedMcpTaskStore` implements `IMcpTaskStore`. Tasks persisted as JSON files with session isolation, TTL-based expiration, exclusive file locking with retry, and time-based automatic completion.

### Code Signature

```csharp
builder.Services.AddMcpServer(options =>
{
    options.TaskStore = new FileBasedMcpTaskStore(taskStorePath);
})
.WithStdioServerTransport()
.WithTools<TaskTools>();

// Tool creates task:
[McpServerTool]
public Task<McpTask> SubmitJob(IMcpTaskStore taskStore,
    [Description("Duration")] int executionSeconds,
    CancellationToken cancellationToken)
{
    return taskStore.CreateTaskAsync(new McpTaskMetadata
    {
        TimeToLive = TimeSpan.FromMinutes(10)
    }, cancellationToken: cancellationToken);
}
```

### Assumptions

- Tasks survive server restarts (file-based persistence)
- Clients poll for task status (not push-based)
- Session isolation enforced via `sessionId` filtering
- Experimental API (MCPEXP001 warning suppression)

---

## Pattern: Ephemeral Progress via ProgressToken

**Category**: Async Operations
**Observed In**: EverythingServer (LongRunningTool)
**Frequency**: 1 sample

### Mechanism

Tool reads `ProgressToken` from `RequestContext.Params`. During operation loop, sends progress notifications via `SendNotificationAsync` if token is not null.

### Code Signature

```csharp
[McpServerTool]
public static async Task<string> LongRunningOperation(
    RequestContext<CallToolRequestParams> context,
    int duration = 10, int steps = 5)
{
    var progressToken = context.Params?.ProgressToken;
    for (int i = 0; i < steps; i++)
    {
        await Task.Delay(duration * 1000 / steps);
        if (progressToken != null)
        {
            await context.Server!.SendNotificationAsync(
                "notifications/progress",
                new { Progress = i + 1, Total = steps, progressToken });
        }
    }
    return "Operation complete";
}
```

### Assumptions

- ProgressToken optionally provided by client
- Notifications are fire-and-forget (lost on disconnect)
- Client must be connected to receive progress updates

---

## Pattern: LLM-Mediated Tool Invocation

**Category**: Client Integration
**Observed In**: ChatWithTools
**Frequency**: 1 sample

### Mechanism

Tools retrieved from MCP server are passed to LLM via `IChatClient`. `UseFunctionInvocation` middleware enables automatic tool calling when LLM requests it.

### Code Signature

```csharp
var tools = await mcpClient.ListToolsAsync();

IChatClient chatClient = openAiClient
    .AsIChatClient("gpt-4o-mini")
    .AsBuilder()
    .UseFunctionInvocation()
    .UseOpenTelemetry()
    .Build();

await foreach (var update in chatClient.GetStreamingResponseAsync(
    messages, new() { Tools = [.. tools] }))
{
    Console.Write(update);
}
```

### Assumptions

- All tools passed on every request (no filtering)
- LLM selects which tools to call based on conversation
- Middleware transparently handles MCP tool invocation
- Tools compatible between MCP format and LLM function calling format

---

## Pattern: Streaming Chat Response

**Category**: Client Integration
**Observed In**: ChatWithTools
**Frequency**: 1 sample

### Mechanism

`GetStreamingResponseAsync` returns `IAsyncEnumerable<ChatResponseUpdate>`. Client consumes incrementally with `await foreach`.

### Code Signature

```csharp
await foreach (var update in chatClient.GetStreamingResponseAsync(
    messages, new() { Tools = [.. tools] }))
{
    Console.Write(update);
}
messages.AddMessages(updates);
```

---

## Pattern: In-Memory Pipe Communication

**Category**: Transport
**Observed In**: InMemoryTransport
**Frequency**: 1 sample

### Mechanism

Two `Pipe` instances with cross-wired reader/writer ends. `AsStream()` converts to streams for `StreamServerTransport` and `StreamClientTransport`.

### Code Signature

```csharp
Pipe clientToServerPipe = new(), serverToClientPipe = new();

var serverTransport = new StreamServerTransport(
    clientToServerPipe.Reader.AsStream(),
    serverToClientPipe.Writer.AsStream());

var clientTransport = new StreamClientTransport(
    serverToClientPipe.Reader.AsStream(),
    clientToServerPipe.Writer.AsStream());
```

---

## Pattern: Serilog with Stderr Separation

**Category**: Observability
**Observed In**: TestServerWithHosting
**Frequency**: 1 sample

### Mechanism

Serilog configured with three sinks: File (rolling daily), Debug, and Console (redirected to stderr). Stderr separation is required for stdio transport to avoid corrupting MCP protocol on stdout.

### Code Signature

```csharp
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Verbose()
    .WriteTo.File(Path.Combine(logDir, "TestServer_.log"),
        rollingInterval: RollingInterval.Day,
        outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss} [{Level}] {Message:lj}{NewLine}{Exception}")
    .WriteTo.Debug()
    .WriteTo.Console(standardErrorFromLevel: Serilog.Events.LogEventLevel.Verbose)
    .CreateLogger();

builder.Services.AddSerilog();
```

---

## Pattern: Resource Subscriptions with Push Notifications

**Category**: Server Capabilities
**Observed In**: EverythingServer
**Frequency**: 1 sample

### Mechanism

`WithSubscribeToResourcesHandler` and `WithUnsubscribeFromResourcesHandler` manage per-session subscription state. Background service (`SubscriptionMessageSender`) periodically sends `notifications/resource/updated` via `SendNotificationAsync`.

### Code Signature

```csharp
.WithSubscribeToResourcesHandler(async (context, ct) =>
{
    var sessionId = context.Server!.SessionId!;
    var uri = context.Params!.Uri;
    subscriptions[sessionId].TryAdd(uri, 0);
    return new EmptyResult();
})
```

---

## Pattern: Completion/Autocomplete Handler

**Category**: Server Capabilities
**Observed In**: EverythingServer
**Frequency**: 1 sample

### Mechanism

`WithCompleteHandler` registers a callback that receives `Ref` (ResourceTemplateReference or PromptReference) and `Argument` (name + current value). Handler filters suggestions by prefix and returns `CompleteResult`.

### Code Signature

```csharp
.WithCompleteHandler(async (context, ct) =>
{
    var values = possibleCompletions[context.Params.Argument.Name]
        .Where(v => v.StartsWith(context.Params.Argument.Value))
        .ToArray();
    return new CompleteResult
    {
        Completion = new() { Values = values, HasMore = false, Total = values.Length }
    };
})
```

---

## See Also

- [vocabulary.md](vocabulary.md) — Types and methods used in these patterns
- [tensions.md](tensions.md) — When patterns conflict or represent different choices
