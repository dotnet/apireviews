# API Review 02/18/2025

## M.E.C.M. MemoryCache - add ReadOnlySpan<char> get API

**Approved** | [#runtime/110504](https://github.com/dotnet/runtime/issues/110504#issuecomment-2666574197) | [Video](https://www.youtube.com/watch?v=fkAIx197zUo&t=0h0m0s)

* There was an alternative proposal involving interpolated string handlers.  It is mothballed for now.
* OverloadResolutionPriority is required since strings are now equally `ReadOnlySpan<char>` and `object`.
* It was asked if GetOrCreate should also be overloaded at this time, and the answer was no (for now).
* The new methods will not be available in the .NET Standard TFM, due to a lack of IAlternateLookup.
  * .NET 9+

```csharp
namespace Microsoft.Extensions.Caching.Memory
{
  public class MemoryCache : IMemoryCache
  {
      [OverloadResolutionPriority(1)]
      public bool TryGetValue(ReadOnlySpan<char> key, out object? value);
      [OverloadResolutionPriority(1)]
      public bool TryGetValue<TItem>(ReadOnlySpan<char> key, out TItem? value);
  }
}
```
## Introduce logging sampling and buffering

**Approved** | [#extensions/5123](https://github.com/dotnet/extensions/issues/5123#issuecomment-2666832463) | [Video](https://www.youtube.com/watch?v=fkAIx197zUo&t=0h34m18s)


* AddHttpRequestBuffering looks like it buffers requests.
  * Along with other feedback, it ended up as AddPerIncomingRequestLogBuffer
* The FlushCurrentRequestLogs() method was removed from HttpRequestLogBuffer as that functionality is already encompassed in the inherited Flush() method.
* A new GlobalLogBuffer type was added to make it easier for DI to distingush the two.
* AddGlobalBuffering was renamed to AddGlobalBuffer to match other "ing" trimming
* The types with "Http" in their names were changed to "Per"
* SuspendAfterFlushDuration => AutoFlushDuration

```csharp
namespace Microsoft.Extensions.Logging
{
    public abstract class LoggingSampler
    {
        public abstract bool ShouldSample<TState>(in LogEntry<TState> logEntry);
    }

    public static class SamplingLoggerBuilderExtensions
    {
        public static ILoggingBuilder AddTraceBasedSampler(this ILoggingBuilder builder);
        public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, IConfiguration configuration);
        public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, Action<ProbabilisticSamplerOptions> configure);
        public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, double probability, LogLevel? level = null);
        public static ILoggingBuilder AddSampler<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] T>(this ILoggingBuilder builder)
            where T : LoggingSampler;
        public static ILoggingBuilder AddSampler(this ILoggingBuilder builder, LoggingSampler sampler);
    }

    public static class GlobalBufferLoggingBuilderExtensions
    {
        public static ILoggingBuilder AddGlobalBuffer(this ILoggingBuilder builder, IConfiguration configuration);
        public static ILoggingBuilder AddGlobalBuffer(this ILoggingBuilder builder, Action<GlobalLogBufferingOptions> configure);
        public static ILoggingBuilder AddGlobalBuffer(this ILoggingBuilder builder, LogLevel? logLevel = null);
    }

    public static class PerIncomingRequestLoggingBuilderExtensions
    {
        public static ILoggingBuilder AddPerIncomingRequestBuffer(this ILoggingBuilder builder, IConfiguration configuration);
        public static ILoggingBuilder AddPerIncomingRequestBuffer(this ILoggingBuilder builder, Action<PerRequestLogBufferingOptions> configure);
        public static ILoggingBuilder AddPerIncomingRequestBuffer(this ILoggingBuilder builder, LogLevel? logLevel = null);
    }
}

namespace Microsoft.Extensions.Diagnostics.Sampling
{
    public class ProbabilisticSamplerOptions
    {
        public IList<ProbabilisticSamplerFilterRule> Rules { get; set; } = [];
    }

    public class ProbabilisticSamplerFilterRule
    {
        public ProbabilisticSamplerFilterRule(
            double probability,
            string? categoryName = null,
            LogLevel? logLevel = null,
            int? eventId = null,
            string? eventName = null)
        {
            Probability = probability;
            CategoryName = categoryName;
            LogLevel = logLevel;
            EventId = eventId;
            EventName = eventName;
        }

        public double Probability { get; }
        public string? CategoryName { get; }
        public LogLevel? LogLevel { get; }
        public int? EventId { get; }
        public string? EventName { get; }
    }
}

namespace Microsoft.Extensions.Diagnostics.Buffering
{
    public class GlobalLogBufferingOptions
    {
        public TimeSpan AutoFlushDuration { get; set; } = TimeSpan.FromSeconds(30);
        public int MaxLogRecordSizeInBytes { get; set; } = 50_000;
        public int MaxBufferSizeInBytes { get; set; } = 500_000_000;
        public IList<LogBufferingFilterRule> Rules { get; set; } = [];
    }

    public class LogBufferingFilterRule
    {
        public LogBufferingFilterRule(
            string? categoryName = null,
            LogLevel? logLevel = null,
            int? eventId = null,
            string? eventName = null,
            IReadOnlyList<KeyValuePair<string, object?>>? attributes = null)
        {
            CategoryName = categoryName;
            LogLevel = logLevel;
            EventId = eventId;
            EventName = eventName;
            Attributes = attributes;
        }

        public string? CategoryName { get; }
        public LogLevel? LogLevel { get; }
        public int? EventId { get; }
        public string? EventName { get; }
        public IReadOnlyList<KeyValuePair<string, object?>>? Attributes { get; }
    }
    
    public class PerRequestLogBufferingOptions
    {
        public TimeSpan AutoFlushDuration { get; set; } = TimeSpan.FromSeconds(30);
        public int MaxLogRecordSizeInBytes { get; set; } = 50_000;
        public int MaxPerRequestBufferSizeInBytes { get; set; } = 5_000_000;
        public IList<LogBufferingFilterRule> Rules { get; set; } = [];
    }

    public abstract class LogBuffer
    {
        public abstract void Flush();
        public abstract bool TryEnqueue<TState>(IBufferedLogger bufferedLogger, in LogEntry<TState> logEntry);
    }

    public abstract class PerRequestLogBuffer : LogBuffer
    {
    }

    public abstract class GlobalLogBuffer : LogBuffer
    {
    }
}
```
