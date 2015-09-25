# API Review 2015-09-22

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c4t5gher6o0mhgqnmn53np51nto).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#telemetrysource) | TelemetrySource
* [Notes](#3054-assemblyloadcontextloadunmanageddllfrompath) | [#3054](https://github.com/dotnet/corefx/issues/3054) `AssemblyLoadContext.LoadUnmanagedDllFromPath`

## Notes

### TelemetrySource

Status: **Approved with Feedback** |
[Proposal](https://github.com/dotnet/corefx/tree/master/src/System.Diagnostics.Tracing.Telemetry/src/System/Diagnostics/Tracing) |
[Video](https://plus.google.com/events/c4t5gher6o0mhgqnmn53np51nto)

* Should be renamed to `DiagnosticSource`
* `WriteTelemetry(string telemetryName, object parameters)`
    - should be renamed to `Write`
    - `telemetryName`  should be `key` (same for `IsEnabled`)
    - `parameters` should be named `value`
* `IDisposable` should be implemented correctly
    - Needs a finalizer
    - Needs `Dispose(bool isDiposing)` should be virtual
    - `Dispose()` shouldn't be virtual

#### Details

`TelemetrySource` is a new API that has been added to the CoreFX framework. It
lives in the DLL `System.Diagnostics.Tracing.Telemetry`.

We would have preferred to not have this class, and instead simply use
EventSource. However there is a desire for 'Rich Telemetry' where the consumer
of the telemetry is IN PROCESS but gets RICH data (basically as detailed as what
a debugger would get). This scenario is not handled by EventSource well because
EventSource requires that the payload be serializable (it assumes it is likely
to leave the process (e.g. written to a file). While we could have tried to make
EventSource support this other mode as well, it seems more confusing than
simplifying for a single class to support both scenarios.

Thus we created `TelemetrySource`, which implements a 'fat' data pipe (where you
can pass any object, but you have to live with the restriction that the object
can't leave the process (`AppDomain`). Note that for `TelemetrySources` the
instrumentation code can mostly defer the question of what is important to log
(log 'everything'), where users of EventSource do need to make that choice. This
is another reason for having two classes (since the design of their use is
different).

It does mean we have two classes: see `EventSource` / `TelemetrySource` Guidance
for guidance about when to use each.

It is also possible to 'bridge the two. See `TelemetrySources` as `EventSources`
on how `TelemetrySource` can be used as `EventSources` (this feature however is
not complete at this time).

Here is the API surface:

```C#
public abstract class TelemetrySource
{
    // A default source for light duty logging
    public static TelemetrySource DefaultSource { get { return TelemetryListener.DefaultListener; } }

    // Writing
    public abstract void WriteTelemetry(string telemetryName, object parameters);
    public abstract bool IsEnabled(string telemetryName);
}

public class TelemetryListener : TelemetrySource, IObservable<KeyValuePair<string, object>>, IDisposable
{
    // A default listener for light duty logging.
    public static TelemetryListener DefaultListener { get; }

    // Creation
    public TelemetryListener(string name)
    virtual public void Dispose()
    public string Name { get; }
    public override string ToString();


    // Discovery
    public static IObservable<TelemetryListener> AllListeners

    // Subscription
    virtual public IDisposable Subscribe(IObserver<KeyValuePair<string, object>> observer, Predicate<string> isEnabled);
    public IDisposable Subscribe(IObserver<KeyValuePair<string, object>> observer);
}
```

**Basic use**

To log an event:

```C#
TelemetrySource.DefaultSource.WriteTelemetry("IntPayload", 5);
```

To receive an event:

```C#
using (TelemetryListener.DefaultListener.Subscribe(MyObserver))
{
    // Do something
}
```

See [TelemetryTests.cs](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.Tracing.Telemetry/tests/TelemetryTests.cs)
for a complete working example.

## #3054 AssemblyLoadContext.LoadUnmanagedDllFromPath

Status: **Approved** |
[Issue](https://github.com/dotnet/corefx/issues/3054) |
[Video](https://plus.google.com/events/c4t5gher6o0mhgqnmn53np51nto)

* Agreed for as-is