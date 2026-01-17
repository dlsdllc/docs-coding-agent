***

## VERTICAL SLICE IMPLEMENTATION USING MICROSOFT-SUPPORTED PRIMITIVES

This document explains how to build vertical, feature‑scoped slices in ASP.NET Core using only **Microsoft-supported primitives**. Vertical Slice Architecture itself is a **community pattern**, but Microsoft fully supports the underlying mechanisms.

Each section includes short narrative + official docs links.

***

1.  MINIMAL API ROUTE GROUPS

***

**Narrative:**  
Minimal APIs allow you to encapsulate a slice by placing all related endpoints into a static `Map{Feature}` class. Route Groups (`MapGroup`) provide a feature root URL, shared authorization, CORS, rate limiting, and logical grouping. This is the simplest and most officially supported primitive for vertical slices.

**Key Microsoft Docs:**

*   Route handlers and grouping:  
    <https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis/route-handlers>  
    (Minimal API route handlers and route groups)    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/route-handlers?view=aspnetcore-10.0)

*   Minimal API quick reference:  
    <https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-10.0)

***

2.  APPLICATION PARTS (DISCOVER CONTROLLERS FROM OTHER ASSEMBLIES)

***

**Narrative:**  
If your slice includes MVC controllers or Razor Pages, Application Parts let the host auto‑discover them at startup. The slice assembly can contain isolated controllers while the host composes them via `.AddApplicationPart()`.

**Key Microsoft Docs:**

*   Application Parts overview:  
    <https://learn.microsoft.com/aspnet/core/mvc/advanced/app-parts>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/mvc/advanced/app-parts?view=aspnetcore-10.0)

***

3.  RAZOR CLASS LIBRARIES (RCLs)

***

**Narrative:**  
If a slice includes UI (Views, Pages, Razor components), ship them in a Razor Class Library. Hosts can override views, and RCLs support embedded static assets. This is the official model for UI‑inclusive feature packages.

**Key Microsoft Docs:**

*   RCL documentation:  
    <https://learn.microsoft.com/aspnet/core/razor-pages/ui-class>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/razor-pages/ui-class?view=aspnetcore-10.0)

***

4.  DI EXTENSION GUIDANCE (IServiceCollection)

***

**Narrative:**  
Each slice should expose one DI entry point, such as `AddOrders()` or `AddInventory()`. Follow Microsoft’s guidance for naming, structure, options, and overriding. This makes slices predictable for your LLMs.

**Key Microsoft Docs:**

*   Options pattern & DI extension best practices:  
    <https://learn.microsoft.com/dotnet/core/extensions/options-library-authors>  
    (Covers naming `Add{Feature}`, options, DI patterns)    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/dotnet/core/extensions/options-library-authors)

***

5.  RECOMMENDED SLICE LAYOUT (ABBREVIATED ASCII SKELETON)

***

Your slice assembly (example: Orders):

    /Orders
      OrdersEndpoints.cs         -> static class MapOrders(app)
      OrdersServiceCollection.cs -> static class AddOrders(services)
      Handlers/
          CreateOrderHandler.cs
          GetOrderHandler.cs
      Models/
          CreateOrderRequest.cs
          OrderDto.cs
      Data/
          OrdersDbContext.cs  (optional)
      Controllers/             (optional if using MVC+Parts)
          OrdersController.cs
      Views/ (RCL optional)

In host app:

```csharp
// Program.cs (Minimal API)
builder.Services.AddOrders();
var app = builder.Build();
app.MapOrders();
app.Run();
```

For MVC-based slices:

```csharp
builder.Services.AddControllersWithViews()
    .AddApplicationPart(typeof(OrdersController).Assembly);
```

***

6.  QUICK LINK INDEX (ASCII CLEAN)

***

Minimal API Route Groups:  
<https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis/route-handlers>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/route-handlers?view=aspnetcore-10.0)

Minimal APIs Reference:  
<https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-10.0)

Application Parts:  
<https://learn.microsoft.com/aspnet/core/mvc/advanced/app-parts>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/mvc/advanced/app-parts?view=aspnetcore-10.0)

Razor Class Libraries:  
<https://learn.microsoft.com/aspnet/core/razor-pages/ui-class>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/aspnet/core/razor-pages/ui-class?view=aspnetcore-10.0)

DI Extension Guidance:  
<https://learn.microsoft.com/dotnet/core/extensions/options-library-authors>    [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/dotnet/core/extensions/options-library-authors)

***
