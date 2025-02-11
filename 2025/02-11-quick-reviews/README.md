# API Review 02/11/2025

## Introduce logging sampling and buffering

**NeedsWork** | [#extensions/5123](https://github.com/dotnet/extensions/issues/5123#issuecomment-2651934320)

* SamplingParameters: Should this just be renamed to LogMessage and moved down to a lower assembly?
  * Is it compatible with the `LogEntry<TState>` struct, (specifically `LogEntry<IReadOnlyList<KeyValuePair<string, object>>?>`)
  * It looks too large for our normal struct guidelines.
* AddProbabilitySampler => AddProbabilisticSampler (and similar renames)
* Remove the AddSampler overload taking a delegate as there doesn't seem to be high need
* ProbabilisticSamplerOptions.Rules should be consistent with other options classes
  * Should it be IList instead of List?
  * Should it be get-only instead of get/set?
* ProbabilisticSamplerFilterRule should not be settable (or all the others should be... but they should be consistent)
* LoggerSampler.ShouldSample during discussion it was said that the parameter should be passed by reference.
  * And the parameter was renamed to `entry` to match the type name change.
* LoggerSampler => LoggingSampler
* AddGlobalBuffer split the LogLevel parameter and the delegate parameter to separate overloads.
* GlobalBufferOptions => GlobalLogBufferingOptions
  * BufferSizeInBytes => MaxBufferSizeInBytes
* GlobalBufferLoggerBuilderExtensions => GlobalBufferingLoggingBuilderExtensions
  * AddGlobalBuffer => AddGlobalBuffering
* Everywhere that took EventId should also take EventName
* BufferFilterRule => LogBufferingFilterRule
* Consider renaming the Global Buffer, as multiple people were surprised that it's intended as a "throw things away" buffer, rather than a "minimize the number of pushes to the wire" buffer.
  * Consider naming it for the purpose, rather than the implementation; but calling it a circular buffer does at least imply "overwrite" vs "auto-flush".
* IBufferManager => ILogBufferingManager
  * The first parameter should have two 'g's in 'Logger' => bufferedLogger
  * The rest of the parameters are just `LogEntry<IReadOnlyList<KeyValuePair<string, object>>?>`, so they should use that.
  * It then turned into a class, and then to LogBuffer
  * It has been asked if TryEnqueue needs the bufferedLogger parameter at all.

```csharp
namespace Microsoft.Extensions.Diagnostics.Sampling
{
    public static class SamplingLoggerBuilderExtensions
    {
        public static ILoggingBuilder AddTraceBasedSampler(this ILoggingBuilder builder);
        public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, IConfiguration configuration);
        public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, double probability, LogLevel? level = null);
        public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, Action<ProbabilisticSamplerOptions> configure);

        public static ILoggingBuilder AddSampler<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] T>(
            this ILoggingBuilder builder)
            where T : LoggerSampler;

        public static ILoggingBuilder AddSampler(this ILoggingBuilder builder, LoggerSampler sampler);
    }

    public class ProbabilisticSamplerOptions
    {
        public List<ProbabilisticSamplerFilterRule> Rules { get; set; } = [];
    }

    public class ProbabilisticSamplerFilterRule
    {
        public ProbabilisticSamplerFilterRule()
            : this(1.0, null, null, null)
        {
        }

        public ProbabilisticSamplerFilterRule(double probability)
            : this(probability, null, null, null)
        {
        }

        public ProbabilisticSamplerFilterRule(
            double probability,
            string? categoryName = null,
            LogLevel? logLevel = null,
            int? eventId = null,
            string? eventName = null)
        {
            Probability = probability;
            Category = categoryName;
            LogLevel = logLevel;
            EventId = eventId;
            EventName = eventName;
        }

        public string? Category { get; }
        public LogLevel? LogLevel { get; }
        public int? EventId { get; }
        public string? EventName { get; set; }
        public double Probability { get; }
    }
}

namespace Microsoft.Extensions.Logging
{
    public abstract class LoggerSampler
    {
        public abstract bool ShouldSample(ref LogEntry<IReadOnlyList<KeyValuePair<string, object>>?> entry);
    }

    public static class GlobalBufferingLoggingBuilderExtensions
    {
        public static ILoggingBuilder AddGlobalBuffering(this ILoggingBuilder builder, IConfiguration configuration);
        public static ILoggingBuilder AddGlobalBuffering(this ILoggingBuilder builder, LogLevel level);
        public static ILoggingBuilder AddGlobalBuffering(this ILoggingBuilder builder, Action<GlobalLogBufferingOptions>? configure);
    }

    public class GlobalLogBufferingOptions
    {
        public TimeSpan SuspendAfterFlushDuration { get; set; } = TimeSpan.FromSeconds(30);
        public int MaxLogRecordSizeInBytes { get; set; } = 50_000;
        public int MaxBufferSizeInBytes { get; set; } = 500_000_000;
        public List<BufferFilterRule> Rules { get; } = [];
    }
}

namespace Microsoft.Extensions.Diagnostics.Buffering
{
    public class LogBufferingFilterRule
    {
        public LogBufferingFilterRule()
            : this(null, null, null, null)
        {
        }

        public LogBufferingFilterRule(
            string? categoryName = null,
            LogLevel? logLevel = null,
            int? eventId = null,
            string? eventName = null,
            IReadOnlyList<KeyValuePair<string, object?>>? attributes = null)
        {
            Category = categoryName;
            LogLevel = logLevel;
            EventId = eventId;
            EventName = eventName;
            Attributes = attributes ?? [];
        }

        /// <summary>
        /// Gets or sets the logger category this rule applies to.
        /// </summary>
        public string? Category { get; set; }

        /// <summary>
        /// Gets or sets the maximum <see cref="LogLevel"/> of messages.
        /// </summary>
        public LogLevel? LogLevel { get; set; }

        /// <summary>
        /// Gets or sets the <see cref="EventId"/> of messages where this rule applies to.
        /// </summary>
        public int? EventId { get; set; }
        public string? EventName { get; set; }

        /// <summary>
        /// Gets or sets the log state attributes of messages where this rules applies to.
        /// </summary>
        public IReadOnlyList<KeyValuePair<string, object?>> Attributes { get; set; }
    }

    public abstract class LogBuffer
    {
        public abstract bool TryEnqueue(
            IBufferedLogger bufferedLogger,
            ref LogEntry<IReadOnlyList<KeyValuePair<string, object>>?> entry);
    }
}

// Unreviewed

namespace Microsoft.Extensions.Diagnostics.Buffering;

 /// <summary>
 /// Interface for a global buffer manager.
 /// </summary>
 public interface IGlobalBufferManager : IBufferManager
 {
     /// <summary>
     /// Flushes the buffer and emits all buffered logs.
     /// </summary>
     void Flush();
 }

 namespace Microsoft.Extensions.Logging;

/// <summary>
/// Lets you register log buffers in a dependency injection container.
/// </summary>
public static class HttpRequestBufferLoggerBuilderExtensions
{
    /// <summary>
    /// Adds HTTP request-aware buffer to the logging infrastructure. Matched logs will be buffered in
    /// a buffer specific to each HTTP request and can optionally be flushed and emitted during the request lifetime./>.
    /// </summary>
    /// <param name="builder">The <see cref="ILoggingBuilder" />.</param>
    /// <param name="configuration">The <see cref="IConfiguration" /> to add.</param>
    /// <returns>The value of <paramref name="builder"/>.</returns>
    /// <exception cref="ArgumentNullException"><paramref name="builder"/> is <see langword="null"/>.</exception>
    public static ILoggingBuilder AddHttpRequestBuffering(this ILoggingBuilder builder, IConfiguration configuration);

    /// <summary>
    /// Adds HTTP request-aware buffering to the logging infrastructure. Matched logs will be buffered in
    /// a buffer specific to each HTTP request and can optionally be flushed and emitted during the request lifetime./>.
    /// </summary>
    /// <param name="builder">The <see cref="ILoggingBuilder" />.</param>
    /// <param name="level">The log level (and below) to apply the buffer to.</param>
    /// <param name="configure">The buffer configuration options.</param>
    /// <returns>The value of <paramref name="builder"/>.</returns>
    /// <exception cref="ArgumentNullException"><paramref name="builder"/> is <see langword="null"/>.</exception>
    public static ILoggingBuilder AddHttpRequestBuffering(this ILoggingBuilder builder, LogLevel? level = null, Action<HttpRequestBufferOptions>? configure = null);
}

namespace Microsoft.AspNetCore.Diagnostics.Buffering;

/// <summary>
/// The options for Http logging buffering.
/// </summary>
public class HttpRequestBufferOptions
{
    /// <summary>
    /// Gets or sets the size in bytes of the buffer for a request. If the buffer size exceeds this limit, the oldest buffered log records will be dropped.
    /// </summary>
    public int PerRequestBufferSizeInBytes { get; set; } = 5_000_000;

#pragma warning disable CA1002 // Do not expose generic lists - List is necessary to be able to call .AddRange()
    /// <summary>
    /// Gets the collection of <see cref="BufferFilterRule"/> used for filtering log messages for the purpose of further buffering.
    /// </summary>
    public List<BufferFilterRule> Rules { get; } = [];
}

namespace Microsoft.AspNetCore.Diagnostics.Buffering;

/// <summary>
/// Interface for an HTTP request buffer manager.
/// </summary>
public interface IHttpRequestBufferManager : IBufferManager
{
    /// <summary>
    /// Flushes the buffer and emits non-request logs.
    /// </summary>
    void FlushNonRequestLogs();

    /// <summary>
    /// Flushes the buffer and emits buffered logs for the current request.
    /// </summary>
    void FlushCurrentRequestLogs();
}
```
