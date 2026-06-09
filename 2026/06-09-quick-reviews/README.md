# API Review 06/09/2026

## Tracing configuration similar to MetricsBuilder

**Approved** | [#runtime/100317](https://github.com/dotnet/runtime/issues/100317#issuecomment-4662388202) | [Video](https://www.youtube.com/watch?v=a6wW6HT_1B8&t=0h0m0s)

* Moved ActivityListenerConfigurationFactory up to the Microsoft.Extensions.Diagnostics.Tracing namespace directly.

Combined with the previous approval, the net result is

```diff
namespace System.Diagnostics
{
-   public sealed partial class ActivitySource
+   public partial class ActivitySource
    {
    }
}
```

```csharp
namespace Microsoft.Extensions.DependencyInjection
{
    public static class TracingServiceExtensions
    {
        public static ITracingBuilder AddTracing(this IServiceCollection services);
        public static ITracingBuilder AddTracing(this IServiceCollection services, Action<ITracingBuilder> configure);
    }
}

namespace System.Diagnostics
{
    public abstract class ActivitySourceFactory : IDisposable
    {
        public ActivitySource Create(ActivitySourceOptions options);
        public ActivitySource Create(string name, string? version = null, IEnumerable<KeyValuePair<string, object?>>? tags = null);
        public void Dispose();

        protected abstract ActivitySource CreateCore(ActivitySourceOptions options);
        protected virtual void Dispose(bool disposing);
    }

    public partial class ActivitySource : IDisposable
    {
        public object? Scope { get; }
        protected virtual void Dispose(bool disposing);
    }

    public partial class ActivitySourceOptions
    {
        public object? Scope { get; set; }
    }

    public partial class ActivityListener
    {
        public void RefreshSources();
    }
}

namespace Microsoft.Extensions.Diagnostics.Tracing
{
    public interface ITracingBuilder
    {
        IServiceCollection Services { get; }
    }

    public sealed class ActivityListenerBuilder
    {
        public string Name { get; }
        public SampleActivity<ActivityContext>? Sample { get; set; }
        public SampleActivity<string>? SampleUsingParentId { get; set; }
        public Action<Activity>? ActivityStarted { get; set; }
        public Action<Activity>? ActivityStopped { get; set; }
        public ExceptionRecorder? ExceptionRecorder { get; set; }
    }

    [Flags]
    public enum ActivitySourceScopes
    {
        None   = 0,
        Global = 1,
        Local  = 2,
    }

    public class TracingRule
    {
        public TracingRule(string? sourceName, string? operationName, string? listenerName, ActivitySourceScopes scopes, bool enable);
        public string? SourceName { get; }
        public string? OperationName { get; }
        public string? ListenerName { get; }
        public ActivitySourceScopes Scopes { get; }
        public bool Enable { get; }
    }

    public class TracingOptions
    {
        public List<TracingRule> Rules { get; }
    }

    public static class TracingBuilderExtensions
    {
        public static ITracingBuilder AddListener(this ITracingBuilder builder, string name, Action<ActivityListenerBuilder> configure);
        public static ITracingBuilder AddListener(this ITracingBuilder builder, string name, Action<IServiceProvider, ActivityListenerBuilder> configure);
        public static ITracingBuilder ClearListeners(this ITracingBuilder builder);
        public static ITracingBuilder EnableTracing(this ITracingBuilder builder, string? sourceName = null, string? operationName = null, string? listenerName = null, ActivitySourceScopes scopes = ActivitySourceScopes.Global | ActivitySourceScopes.Local);
        public static TracingOptions EnableTracing(this TracingOptions options, string? sourceName = null, string? operationName = null, string? listenerName = null, ActivitySourceScopes scopes = ActivitySourceScopes.Global | ActivitySourceScopes.Local);
        public static ITracingBuilder DisableTracing(this ITracingBuilder builder, string? sourceName = null, string? operationName = null, string? listenerName = null, ActivitySourceScopes scopes = ActivitySourceScopes.Global | ActivitySourceScopes.Local);
        public static TracingOptions DisableTracing(this TracingOptions options, string? sourceName = null, string? operationName = null, string? listenerName = null, ActivitySourceScopes scopes = ActivitySourceScopes.Global | ActivitySourceScopes.Local);
    }

    public static class TracingBuilderConfigurationExtensions
    {
        public static ITracingBuilder AddConfiguration(this ITracingBuilder builder, IConfiguration configuration);
    }   

    public abstract class ActivityListenerConfigurationFactory
    {
        public abstract IConfiguration GetConfiguration(string listenerName);
    }
}
```
## Async DataAnnotations bridge for Microsoft.Extensions.Options

**Approved** | [#runtime/129056](https://github.com/dotnet/runtime/issues/129056#issuecomment-4662588914) | [Video](https://www.youtube.com/watch?v=a6wW6HT_1B8&t=0h26m57s)

* Let's just add IAsyncValidateOptions to the existing `DataAnnotationValidateOptions<TOptions>`

```csharp
namespace Microsoft.Extensions.Options
{
    public partial class DataAnnotationValidateOptions<TOptions> :
        IAsyncValidateOptions<TOptions>
    {
        // implicitly implement the interface
    }
}
```
## X25519DiffieHellman.DeriveRawSecretAgreement with bytes public key

**Approved** | [#runtime/128040](https://github.com/dotnet/runtime/issues/128040#issuecomment-4662679948) | [Video](https://www.youtube.com/watch?v=a6wW6HT_1B8&t=0h52m51s)

Looks good as proposed

```csharp
namespace System.Security.Cryptography

public partial class X25519DiffieHellman
{
    public byte[] DeriveRawSecretAgreement(byte[] otherPartyPublicKey);
    public void DeriveRawSecretAgreement(ReadOnlySpan<byte> otherPartyPublicKey, Span<byte> destination);
    protected abstract void DeriveRawSecretAgreementCore(ReadOnlySpan<byte> otherPartyPublicKey, Span<byte> destination);
}
```
## Support non-locking assembly loading with preserved Assembly.Location

**Approved** | [#runtime/127097](https://github.com/dotnet/runtime/issues/127097#issuecomment-4662846955) | [Video](https://www.youtube.com/watch?v=a6wW6HT_1B8&t=1h4m10s)

Looks good as proposed

```csharp
namespace System.Runtime.Loader
{
    public partial class AssemblyLoadContext
    {
        public static void SetAssemblyLocationOverride(Func<Assembly, string, string> locationOverride);
    }
}
```
## Add MaxDepth property to CborReader

**Approved** | [#runtime/128087](https://github.com/dotnet/runtime/issues/128087#issuecomment-4663060715) | [Video](https://www.youtube.com/watch?v=a6wW6HT_1B8&t=1h22m20s)


* Changed the options types to sealed classes.

```csharp
namespace System.Formats.Cbor;

public partial class CborReader
{
    public CborReader(ReadOnlyMemory<byte> data, CborReaderOptions? options);

    public int MaxDepth { get; }
}

public sealed class CborReaderOptions
{
    public bool AllowMultipleRootLevelValues { get; set; }
    public CborConformanceMode ConformanceMode { get; set; }
    public int MaxDepth { get; set; }
}

public partial class CborWriter
{
    public CborWriter(CborWriterOptions? options);

    public int MaxDepth { get; }
}

public sealed class CborWriterOptions
{
    public bool AllowMultipleRootLevelValues { get; set; }
    public CborConformanceMode ConformanceMode { get; set; }
    public bool ConvertIndefiniteLengthEncodings { get; set; }
    public int InitialCapacity { get; set; }
    public int MaxDepth { get; set; }
}
```
