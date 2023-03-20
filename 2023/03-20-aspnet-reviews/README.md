# API Review 03/20/2023

## Ability to monitor circuit activity

**Approved** | [#aspnetcore/47325](https://github.com/dotnet/aspnetcore/issues/47325#issuecomment-1476709142)

API Review Notes:

- How long does a DI scope last for server-side blazor? The lifetime of the circuit.
- Is encouraging AsyncLocal usage like IHttpContextAccessor really a good idea? AsyncLocal can be confusing.
- Why not make `HandleInboundActivityAsync` a new virtual on `CircuitHandler`? So we don't have a bunch of wrapping next calls that do nothing when it's not customized.
  - It's not as discoverable on an interface.
  - Could we make it virtual and return `Func<CircuitInboundActivityContext, Task>` instead of `Task`?
    - This would avoid redundant wrapping when not overridden.
    - Might be less convenient when you do want to override it.
- Do we want to expose SignalR's ConnectionContext to store state? It would be hard to flow this everywhere.
- Is this even useful if you don't set an AsyncLocal? Maybe for logging. Not much else.
  - Could we just expose an `IBlazorServiceAccessor` and not the `HandleInboundActivity`?
    - `HandleInboundActivity` could be useful if we add more API to Circuit like the ability to close it.
- Is `CircuitInboundActivityContext` testable? Can we just use `Circuit` directly? There's some internal state outside of the circuit we store in the context.
  - It's not really possible mock a `Circuit` today anyway. If one day that's a thing, we can add a constructor.
- Do we need a `CancellationToken`? We wouldn't use it right now.
  - Could we add it to `CircuitInboundActivityContext` later if we need it? Possibly. It's not super standard, but it's what we do for `HttpContext` and middleware.

API Approved!

```diff
namespace Microsoft.AspNetCore.Components.Server.Circuits;

+public sealed class CircuitInboundActivityContext
+{
+    public Circuit Circuit { get; }
+}

public abstract CircuitHandler
{
+    public virtual Func<CircuitInboundActivityContext, Task> CreateInboundActivityHandler(Func<CircuitInboundActivityContext, Task> next) => next;
}
```
## Add overloads for validation problem results that accept IReadOnlyDictionary<string, string[]>

**Approved** | [#aspnetcore/41899](https://github.com/dotnet/aspnetcore/issues/41899#issuecomment-1476727981)

API Review Notes:

- Is this a breaking change? [Yes.](https://sharplab.io/#v2:CYLg1APgAgDABFAjAFgNwFgBQWoGYEBMcAwnAN5ZxUL5TJwCyAFAJIBKApgIbADyAdgBsAngBEAlgGMALuID2/LgCdhAHiQwANAkQwA2gF0AfHGBTZC5cICU5StQC+9qs5oJ6zFhJnzFK9braGoYmZj6WKrYUmNRwTjHUrnjucAAqHADO0kzWrtGxscz8HADucN4WfmoaQbohOdYYCVTxDkA) It's ambiguous which overload to pick if you pass in `Dictionary<string, string[]>`. 
- Can we make it `IEnumerable<KeyValuePair<string, string[]>>` instead? [Yes.](https://sharplab.io/#v2:CYLg1APgAgDABFAjAFgNwFgBQWoGYEBMcAwnAN5ZxUL5TJwCyAFErgDwDSApgJ4BqAQwA2AVy4AFAQEsATmyQwANAkQwA2gF0AfFrjApAYwAuUgPYA7ATJ4BKStQqZqcAL72q7mgnrMAkgBFDEwsrHnlVZQVNXX1jM0trOycHTzdkj3SvOjgAFS4AZyMmJOdHZ2dmJl8AJS4BYAB5cyEeQLiQ63ClFXVtGzhzLgB3ODbghLCFSNVo4psMTLSXIA=)
- Do we want to change both the `errors` and `extensions` parameters? Might as well. It will accept more arguments, and we're not modifying either dictionary.

API Approved!

```diff
namespace Microsoft.AspNetCore.Http;

public static class Results
{
+    public static IResult ValidationProblem(IEnumerable<KeyValuePair<string, string[]>> errors,
+        string? detail = null,
+        string? instance = null,
+        int? statusCode = null,
+        string? title = null,
+        string? type = null,
+        IEnumerable<KeyValuePair<string, object?>>? extensions = null);

+    public static IResult Problem(
+        string? detail = null,
+        string? instance = null,
+        int? statusCode = null,
+        string? title = null,
+        string? type = null,
+        IEnumerable<KeyValuePair<string, object?>>? extensions = null);
}

public static class TypedResults
{
+    public static ValidationProblem ValidationProblem(IEnumerable<KeyValuePair<string, string[]>> errors,
+        string? detail = null,
+        string? instance = null,
+        string? title = null,
+        string? type = null,
+        IEnumerable<KeyValuePair<string, object?>>? extensions = null);

+    public static ProblemHttpResult Problem(
+        string? detail = null,
+        string? instance = null,
+        string? title = null,
+        string? type = null,
+        IEnumerable<KeyValuePair<string, object?>>? extensions = null);
}
```
