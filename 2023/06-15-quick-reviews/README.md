# API Review 06/15/2023

## pass custom tags for metrics enrichment in HttpClientHandler

**Approved** | [#runtime/86281](https://github.com/dotnet/runtime/issues/86281#issuecomment-1593460628) | [Video](https://www.youtube.com/watch?v=DV_fl-UIE5w&t=0h0m0s)

Generally looks good as proposed.  Ideally, this method will apply these tags to all metrics that are produced with "the request" (duration, size, etc) but probably not with "the client" (total number of requests, etc).  If it's just cherry-picking individual metrics then it probably shouldn't be public API, as 3rd party callers wouldn't know when they can/can't add things sensibly.

```C#
namespace System.Net.Http;

// Consider a Metrics-specific HttpRequestOptionsMetricsExtensions
public static class HttpRequestOptionsExtensions
{
    // Returns the value assigned to the well known key if it's present:
    // HttpRequestOptionsKey<ICollection<KeyValuePair<string, object?>>>("CustomMetricsTags")
    // Creates a new collection if it doesn't. Throws if a collection is present but readonly.
    // Alternative name ideas: GetMetricsEnrichmentTags, GetEnrichmentTags
    public static ICollection<KeyValuePair<string, object?>> GetCustomMetricsTags(this HttpRequestOptions options);
}
```
## Pass Meter to HttpClientHandler and SocketsHttpHandler

**NeedsWork** | [#runtime/86961](https://github.com/dotnet/runtime/issues/86961#issuecomment-1593517226) | [Video](https://www.youtube.com/watch?v=DV_fl-UIE5w&t=0h18m30s)

There are a lot of scenario based questions that were taking up a lot of time in the meeting and we weren't getting answers, so this is being sent back for followup.

One particular question was what the expected behaviors are around a pipeline of delegating message handlers, and are they all supposed to use the same customized Meter, or different Meters, and if the same, how is it supposed to flow?  As proposed, "forwarding" doesn't seem possible.  https://github.com/Pixeval/Pixeval/blob/main/src/Pixeval.CoreApi/Net/RetryHttpClientHandler.cs was a randomly searched out example, if this type wanted to add metrics, how should it participate in the pipeline?

```C#
namespace System.Net.Http;

public class HttpClientHandler
{
    public Meter Meter { get; set; } // = DefaultGlobalMeterForHttpClient;
}

public class SocketsHttpHandler
{
    public Meter Meter { get; set; } // = DefaultGlobalMeterForHttpClient;
}
```
## Add additional LoggerMessageAttribute constructors

**Approved** | [#runtime/87254](https://github.com/dotnet/runtime/issues/87254#issuecomment-1593561695) | [Video](https://www.youtube.com/watch?v=DV_fl-UIE5w&t=1h3m35s)

Given that we've already gone against guidelines by having the `(int, LogLevel, string)` secondary constructor, we feel that this is a reasonable enhancement to usability of people creating these logger functions.

```C#
namespace Microsoft.Extensions.Logging;

public sealed partial class LoggerMessageAttribute : Attribute
{
    // Existing:
    // public LoggerMessageAttribute();
    // public LoggerMessageAttribute(int eventId, LogLevel level, string message);

    public LoggerMessageAttribute(LogLevel level, string message);
    public LoggerMessageAttribute(LogLevel level);
    public LoggerMessageAttribute(string message);
}
```
## Expose options to only generate one direction for generated COM interfaces

**Approved** | [#runtime/86360](https://github.com/dotnet/runtime/issues/86360#issuecomment-1593567753) | [Video](https://www.youtube.com/watch?v=DV_fl-UIE5w&t=1h42m17s)

namespace System.Runtime.InteropServices.Marshalling;

```C#
[Flags]
public enum ComInterfaceOptions
{
     None = 0,
     ManagedObjectWrapper = 0x1,
     ComObjectWrapper = 0x2,
}

[AttributeUsage(AttributeTargets.Interface)]
public sealed partial class GeneratedComInterfaceAttribute
{
    public ComInterfaceOptions Options { get; set; } = ComInterfaceOptions.ManagedObjectWrapper | ComInterfaceOptions.ComObjectWrapper;
}
```
## Stopwatch.ToString()

**Approved** | [#runtime/87449](https://github.com/dotnet/runtime/issues/87449#issuecomment-1593579450) | [Video](https://www.youtube.com/watch?v=DV_fl-UIE5w&t=1h47m45s)

Looks good as proposed

```C#
namespace System.Diagnostics;

public partial class Stopwatch
{
    public override string ToString() => Elapsed.ToString();
}
```
