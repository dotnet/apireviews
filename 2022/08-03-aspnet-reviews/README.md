# API Review 08/03/2022

## [Blazor] Enable cancellation of navigation events

**Approved** | [#aspnetcore/42877](https://github.com/dotnet/aspnetcore/issues/42877#issuecomment-1204589541)

There was more offline feedback about this API, so we're going to edit it hopefully one last time.

API Review Notes:

- @BrennanConroy asks, "Should the context be required init without a ctor? I know we’ve been playing with that in other APIs."
  - [RateLimiting's OnRejectedContext](https://github.com/dotnet/aspnetcore/blob/acff643b706bba701cb728011b37b39c759b3d4c/src/Middleware/RateLimiting/src/OnRejectedContext.cs) is an example of where we've done this.
   - Sounds good
- Would everything in `LocationChangingContext` but `HistoryEntryState` be required? Should we provide defaults for `IsNavigationIntercepted` and/or `CancellationToken`?
  - Providing defaults for IsNavigationIntercepted and CancellationToken seems good to everyone
- @MackinnonBuck says, "I think “HandleException” implies something more general-purpose than what’s really going on. For example, exceptions thrown when invoking the `LocationChanged` event won’t get caught and passed to the new `HandleException` method (they could, but that would be a breaking change). "
  - `HandleException` could probably be made non-breaking by having it return a `bool` to opt-in to marking the `LocationChanged` event as handled.
- @MackinnonBuck also says, "Furthermore, providing a `LocationChangingContext` in this method allows for NavigationManager implementations to log more meaningful error messages (e.g. “An exception was thrown while navigating to location XYZ”). We would lose information about the fact that the exception was thrown in a location changing handler if we went the HandleException route."
  - I see the benefit to passing in the `LocationChangingContext` which preclude the same handler handling `LocationChanged` exceptions where this context does not exist.

Approved!

```diff
namespace Microsoft.AspNetCore.Components;

public abstract class NavigationManager
{
+    public IDisposable RegisterLocationChangingHandler(Func<LocationChangingContext, ValueTask> locationChangingHandler);
+    protected ValueTask<bool> NotifyLocationChangingAsync(string uri, string? state, bool isNavigationIntercepted);
+    protected virtual void HandleLocationChangingHandlerException(Exception ex, LocationChangingContext context);
+    protected virtual void SetNavigationLockState(bool value);
}

namespace Microsoft.AspNetCore.Components.Routing;

+ public sealed class LocationChangingContext
+ {
+     public required string TargetLocation { get; init; }
+     public string? HistoryEntryState { get; init; }
+     public bool IsNavigationIntercepted { get; init; }
+     public CancellationToken CancellationToken { get; init; }
+     public void PreventNavigation();
+ }

+ public sealed class NavigationLock : IComponent, IAsyncDisposable
+ {
+     public NavigationLock();
+     public EventCallback<LocationChangingContext> OnBeforeInternalNavigation { get; set; }
+     public bool ConfirmExternalNavigation { get; set; }
+ }
```
