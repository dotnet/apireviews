# API Review 01/19/2023

## Rate limiter context HttpContext feature API proposal

**NeedsWork** | [#aspnetcore/45658](https://github.com/dotnet/aspnetcore/issues/45658#issuecomment-1397735591)

API Review Notes:

- We like removing the `Microsoft.AspNetCore.RateLimiting.Features` namespace.
- We still are not sure what we want to do about the confusing nature of `IRateLimiterStatisticsFeature` only referencing the partition of the current request. We think it's fine to do that, but it might not be obvious enough what's really going on given the current API naming.

We will try to get back to this in API review Monday. I'm leaving it to @BrennanConroy to improve the proposal since he doesn't like my proposal 😆 .
## Allow Dispatching to Blazor Renderer Sync Context

**Approved** | [#aspnetcore/46068](https://github.com/dotnet/aspnetcore/issues/46068#issuecomment-1397749256)

API Review Notes:

- Why doesn't InvokeAsync call DispatchExceptionAsync internally? Will this ever be used for exceptions thrown from something other than InvokeAsync?
    - InvokeAsync is the primary scenario, but this makes this a non-breaking opt-in change.
- How are people going to know to call try/catch? Documentation.
- Is this behavior we even want on every call to InvokeAsync? No. Oftentimes, the error can be handled outside of the renderer.
- Can RenderHandle.DispatchExceptionAsync be internal? ~Maybe.~ Edit: No.
- Can this be worked around? Maybe using ExceptionDispatchInfo and SynchronizatinContext.Post? Maybe not that workaround, but there are ugly workarounds that we do not want to suggest to users.
- What does the `Task` returned by `DispatchExceptionAsync` represent? When the exception has completed dispatching. So any handlers should have completed handling the error on the UI thread. This should never really throw except maybe an `ObjectDisposeException` or something.

API Approved!

```diff
namespace Microsoft.AspNetCore.Components;

public abstract class ComponentBase : IComponent, IHandleEvent, IHandleAfterRender
{
+    protected Task DispatchExceptionAsync(Exception exception);
}

public readonly struct RenderHandle
{
+    public Task DispatchExceptionAsync(Exception exception);
}
```
