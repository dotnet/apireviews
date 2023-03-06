# API Review 03/06/2023

## Blazor Sections API Proposal

**Approved** | [#aspnetcore/46937](https://github.com/dotnet/aspnetcore/issues/46937#issuecomment-1456766192)

API Review Notes:
* How easy is it to pass objects around in Blazor?
  - You can have a parameter in a child component or inject via a service/using static or Cascading Value
* What's wrong with `string Name`?
  - Easy to conflict with someone else, e.g. I use "head" and another library uses "head", we'll step on eachother.
  - Can't avoid conflicts with an error since it's not necessarily the users fault and wouldn't be fixable if multiple libraries conflicted. Plus you might want multiple SectionOutlets with the same "name" in a single app
* Namespace? Is `.Sections` fine instead of the "main" `Components` namespace?
  - Yes, we think it'll be less commonly used than the "core" components, and other components do use custom namespaces as well, .Web, .Authorization, e.g.

API approved!

```diff
namespace Microsoft.AspNetCore.Components.Sections;

+ public sealed class SectionOutlet : IComponent, IDisposable
+ {
+     [Parameter, EditorRequired] public object SectionId { get; set; }

+     // <inheritdoc/>
+     void Attach(RenderHandle renderHandle);

+     // <inheritdoc/>
+     Task SetParametersAsync(ParameterView parameters);

+     // <inheritdoc/>
+     public void Dispose();
+ }

+ public sealed class SectionContent : IComponent, IDisposable
+ {
+     [Parameter, EditorRequired] public object SectionId { get; set; }
+     [Parameter] public RenderFragment? ChildContent { get; set; }

+     // <inheritdoc/>
+     void Attach(RenderHandle renderHandle);

+     // <inheritdoc/>
+     Task SetParametersAsync(ParameterView parameters);

+     // <inheritdoc/>
+     public void Dispose();
+ }
```
## Add parameterless overload for `RequireCors` API

**Approved** | [#aspnetcore/46922](https://github.com/dotnet/aspnetcore/issues/46922#issuecomment-1456775589)

API Review Notes:
* Makes sense, I have no concerns/questions

API Approved!

```diff
namespace Microsoft.AspNetCore.Builder;

public static class CorsEndpointConventionBuilderExtensions
{
+  public static TBuilder RequireCors<TBuilder>(this TBuilder builder) where TBuilder : IEndpointConventionBuilder
}
```
## Add generic versions of attributes in ASP.NET Core

**Approved** | [#aspnetcore/37767](https://github.com/dotnet/aspnetcore/issues/37767#issuecomment-1456806701)

API Review Notes:
* We prefer `T` over `TType` for generic arguments, unless they are constrained, then we add a suffix to `T`.

API Approved!

```diff

namespace Microsoft.AspNetCore.Mvc;

+[AttributeUsage(AttributeTargets.Class, AllowMultiple = false, Inherited = true)]
+public class ModelMetadataTypeAttribute<T> : ModelMetadataTypeAttribute
+{
+    public ModelMetadataTypeAttribute() : base(typeof(T))
+    { }
+}

+[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true, Inherited = true)]
+public class MiddlewareFilterAttribute<T> : MiddlewareFilterAttribute
+{
+    public MiddlewareFilterAttribute() : base(typeof(T))
+    { }
+}

+[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true, Inherited = true)]
+[System.Diagnostics.DebuggerDisplay("ServiceFilter: Type={ServiceType} Order={Order}")]
+public class ServiceFilterAttribute<TFilter> : ServiceFilterAttribute
+ where TFilter : IFilterMetadata
+{
+    public ServiceFilterAttribute() : base(typeof(TFilter))
+    { }
+}

+[AttributeUsage(AttributeTargets.Parameter | AttributeTargets.Property | AttributeTargets.Class | AttributeTargets.Enum | AttributeTargets.Struct, AllowMultiple = false, Inherited = true)]
+public class ModelBinderAttribute<TBinder> : ModelBinderAttribute
+ where TBinder : IModelBinder
+{
+    public ModelBinderAttribute() : base(typeof(TBinder))
+    { }
+}

+[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
+public class ProducesAttribute<T> : ProducesAttribute
+{
+    public ProducesAttribute() : base(typeof(T))
+    { }
+}

+[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true, Inherited = true)]
+public class ProducesResponseTypeAttribute<T> : ProducesResponseTypeAttribute
+{
+    public ProducesResponseTypeAttribute(int statusCode) : base(typeof(T), statusCode)
+    { }

+    public ProducesResponseTypeAttribute(int statusCode, string contentType, params string[] additionalContentTypes)
+             : base(typeof(T), statusCode, contentType, additionalContentTypes)
+    { }
+}

```
