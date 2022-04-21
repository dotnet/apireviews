# API Review 04/21/2022

## Exposing the total paused duration in GC.

**Approved** | [#runtime/66036](https://github.com/dotnet/runtime/issues/66036#issuecomment-1105487825) | [Video](https://www.youtube.com/watch?v=CXPqfXN3vmg&t=0h0m0s)

* Looks good as proposed

```C#
namespace System;

public partial class GC
{
    public static TimeSpan GetTotalPauseDuration();
}
```
## Let the application author tell us to be idle.

**Approved** | [#runtime/66037](https://github.com/dotnet/runtime/issues/66037#issuecomment-1105506174) | [Video](https://www.youtube.com/watch?v=CXPqfXN3vmg&t=0h12m58s)

* Makes sense
    - We should rename it because `Maximal` makes it a bit difficult to add more modes later
    - `Aggressive` seems like a good name
* What is currently framed as the alternative design seems mostly complementary and helpful for libraries to participate in an overall decommission of resources when an application goes idle
    - @grabyourpitchforks mentioned that we did a similar API in .NET Framework 4.5 for ASP.NET.
    - This might be especially helpful for containers

```C#
namespace System;

public enum GCCollectionMode
{
    // Existing:
    // Default = 0,
    // Forced = 1,
    // Optimized = 2,
    Aggressive = 3
}
```
## Retrieve GC configuration values

**Approved** | [#runtime/67471](https://github.com/dotnet/runtime/issues/67471#issuecomment-1105611143) | [Video](https://www.youtube.com/watch?v=CXPqfXN3vmg&t=0h35m10s)

* As @jkotas says, we should come up with a pattern here that we can replicate elsewhere.
    - It seems a single method that returns a value is easiest
    - Unlikely to be called in a tight loop
* Currently, the values can never change but we might decide to make these values mutable in the future. For that, it makes sense to have snapshot semantics in that we the method returns the values at this point in time.
* The GC configuration settings in particular do parsing and coercion of values; it seems valuable to expose the configured value and the parsed/coerced value.
* It seems the runtime config isn't entirely consistent in which values can be configured in the runtime json config vs. what can come from environment variables. It seems we should unify this and maybe even have a centralized API on `AppContext`

```C#
namespace System;

public partial class GC
{
    // Returns a snapshot of the values at this particular point in time.
    public static IReadOnlyDictionary<string, object> GetConfigurationVariables();
}
```

## NoGC callback

**Approved** | [#runtime/66039](https://github.com/dotnet/runtime/issues/66039#issuecomment-1105631442) | [Video](https://www.youtube.com/watch?v=CXPqfXN3vmg&t=1h23m40s)

* We should rename the method to make it clear that's tied to the no-GC region feature
* The API should if not currently in a no-GC region
* The callback is automatically removed when the no-GC region ends

```C#
namespace System;

public partial class GC
{
    public static void RegisterNoGCRegionCallback(long totalSize, Action callback);
}
```

