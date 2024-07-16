# API Review 07/16/2024

## HttpClientFactory support for keyed DI

**Approved** | [#runtime/89755](https://github.com/dotnet/runtime/issues/89755#issuecomment-2231477356) | [Video](https://www.youtube.com/watch?v=DKMLWjOrDT8&t=0h0m0s)

* We would like to align the names and take in the lifetime
* The default behavior won't change for .NET 9, the expectation is that .NET 10 will change the default add it keyed lifetime by default.

```C#
namespace Microsoft.Extensions.DependencyInjection;

public static partial class HttpClientBuilderExtensions
{
    public static IHttpClientBuilder AddAsKeyed(this IHttpClientBuilder builder, ServiceLifetime lifetime = ServiceLifetime.Scoped);
    public static IHttpClientBuilder RemoveAsKeyed(this IHttpClientBuilder builder);
}
```
## Introducing Log buffering abstraction

**Approved** | [#runtime/104129](https://github.com/dotnet/runtime/issues/104129#issuecomment-2231512146) | [Video](https://www.youtube.com/watch?v=DKMLWjOrDT8&t=0h37m40s)

* `LogRecords` should take an `IEnumerable<T>`

```C#
namespace Microsoft.Extensions.Logging.Abstractions;

public interface IBufferedLogger
{
    void LogRecords(IEnumerable<BufferedLogRecord> records);
}

public abstract class BufferedLogRecord
{
    public abstract DateTimeOffset Timestamp { get; }
    public abstract LogLevel LogLevel { get; }
    public abstract EventId EventId { get; }
    public virtual string? Exception { get; }
    public virtual ActivitySpanId? ActivitySpanId { get; }
    public virtual ActivityTraceId? ActivityTraceId { get; }
    public virtual ActivityTraceFlags? ActivityTraceFlags { get; }
    public virtual int? ManagedThreadId { get; }
    public virtual string? FormattedMessage { get; }
    public virtual string? MessageTemplate { get; }
    public virtual IReadOnlyList<KeyValuePair<string, object?>>? Attributes { get; }
}
```
## Introducing Environment.ProcessorUserTime & ProcessorPrivilegedTime 

**Approved** | [#runtime/104844](https://github.com/dotnet/runtime/issues/104844#issuecomment-2231556034) | [Video](https://www.youtube.com/watch?v=DKMLWjOrDT8&t=1h8m14s)

* We'd prefer to have a nested type, as opposed to putting it in the `System` namespace
* Might as well also expose `TotalTime`

```C#
namespace System;

public partial class Environment
{
    [SupportedOSPlatform("maccatalyst")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    public static ProcessCpuUsage CpuUsage { get; }

    public readonly struct ProcessCpuUsage
    {
        public TimeSpan UserTime { get; }
        public TimeSpan PrivilegedTime { get; }
        public TimeSpan TotalTime => UserTime + PrivilegedTime;
    }
}
```
## Add `TagList` constructor to `Measurement`

**Approved** | [#runtime/104015](https://github.com/dotnet/runtime/issues/104015#issuecomment-2231566599) | [Video](https://www.youtube.com/watch?v=DKMLWjOrDT8&t=1h25m11s)

* Looks good as proposed

```C#
namespace System.Diagnostics.Metrics;

public partial struct Measurement<T>
{
    // Existing
    // public Measurement(T value);
    // public Measurement(T value, IEnumerable<KeyValuePair<string, object?>>? tags);
    // public Measurement(T value, params KeyValuePair<string, object?>[]? tags);
    // public Measurement(T value, params ReadOnlySpan<KeyValuePair<string, object?>> tags);
    public Measurement(T value, in TagList tags);
}
```
## In TarEntry objects emitted by TarReader, make the offset to the entry data in enclosing stream a public property

**Approved** | [#runtime/101314](https://github.com/dotnet/runtime/issues/101314#issuecomment-2231640123) | [Video](https://www.youtube.com/watch?v=DKMLWjOrDT8&t=1h29m6s)

* We should call it `DataOffset`
    - Sadly `Length` should have been called `DataLength` but that ship has sailed
* We should make sure that `(DataOffset, Length)` always point to the size of the entry's body, even for entries that don't have a `DataStream`

```C#
namespace System.Formats.Tar;

public abstract partial class TarEntry
{
    public long DataOffset { get; }
}
```
## Remove Interlocked.{Compare}Exchange generic constraint

**Approved** | [#runtime/65184](https://github.com/dotnet/runtime/issues/65184#issuecomment-2231643225) | [Video](https://www.youtube.com/watch?v=DKMLWjOrDT8&t=1h58m22s)

* Looks good as proposed

```C#
namespace System.Threading;

public static class Interlocked
{
    public static T CompareExchange<T> (ref T location1, T value, T comparand);
    public static T Exchange<T>(ref T location1, T value);
}
```
