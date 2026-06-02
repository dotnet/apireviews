# API Review 06/02/2026

## Tracing configuration similar to MetricsBuilder

**Approved** | [#runtime/100317](https://github.com/dotnet/runtime/issues/100317#issuecomment-4605843379) | [Video](https://www.youtube.com/watch?v=LXXn05tWbKA&t=0h0m0s)


* Changed IActivitySourceFactory and ActivitySourceFactoryExtensions to abstract ActivitySourceFactory
* Eliminated IActivityListener
* Added an optional name to ActivityListener
* TracingRule.ActivityName => OperationName
  * ActivitySourceName => SourceName
* TracingOptions.Rules, changed from IList to List since it's always List.
* ActivitySourceScope => ActivitySourceScopes for `[Flags]` enum rules.

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
        public string? Name { get; }
        public ActivityListener(string? name);

        public void RefreshSources();
    }
}

namespace Microsoft.Extensions.Diagnostics.Tracing
{
    public interface ITracingBuilder
    {
        IServiceCollection Services { get; }
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
        public static ITracingBuilder AddListener<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] T>(this ITracingBuilder builder)
            where T : class, IActivityListener;

        public static ITracingBuilder AddListener<T>(this ITracingBuilder builder, Func<IServiceProvider, T> factory)
            where T : class, IActivityListener;

        public static ITracingBuilder AddListener(this ITracingBuilder builder, IActivityListener listener);
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
}
```

## Proposal for DNS client and custom DNS queries

**Approved** | [#runtime/19443](https://github.com/dotnet/runtime/issues/19443#issuecomment-4606362524) | [Video](https://www.youtube.com/watch?v=LXXn05tWbKA&t=1h23m25s)

* Changed all of the record types to consistently end in "Record".
* Removed the "Dns" prefix from all "Record" types.
* Since the existing methods, which don't return the TTL, already have sync options, we should have sync options of these, too.
* Changed Ttl from `int` to `TimeSpan` both for representing the unit/scale (seconds) and because the spec allows values in the full uint range.
* Consider adding a Name/Query properfy to each record to indicate what generated it, or at least the Address record so it can convey that the name was handled by CName to Address lookup.
* Sealed DnsResolver and DnsResolverOptions
* Added IPAddress overload to ResolvePtr
* Unified all of the parameters to `name`.
* Renamed DnsResponseCode.NameError to NxDomain to match the common name
* TxtResult.Strings => TxtResult.Values

```csharp
namespace System.Net;

public partial class Dns
{
    public static DnsResult<AddressRecord> ResolveAddresses(
        string name);
    public static Task<DnsResult<AddressRecord>> ResolveAddressesAsync(
        string name, CancellationToken cancellationToken = default);

    public static DnsResult<AddressRecord> ResolveAddresses(
        string name, AddressFamily addressFamily);
    public static Task<DnsResult<AddressRecord>> ResolveAddressesAsync(
        string name, AddressFamily addressFamily, CancellationToken cancellationToken = default);

    public static DnsResult<SrvRecord> ResolveSrv(
        string name);
    public static Task<DnsResult<SrvRecord>> ResolveSrvAsync(
        string name, CancellationToken cancellationToken = default);

    public static DnsResult<MxRecord> ResolveMx(
        string name);
    public static Task<DnsResult<MxRecord>> ResolveMxAsync(
        string name, CancellationToken cancellationToken = default);

    public static DnsResult<TxtRecord> ResolveTxt(
        string name);
    public static Task<DnsResult<TxtRecord>> ResolveTxtAsync(
        string name, CancellationToken cancellationToken = default);

    public static DnsResult<CNameRecord> ResolveCName(
        string name);
    public static Task<DnsResult<CNameRecord>> ResolveCNameAsync(
        string name, CancellationToken cancellationToken = default);

    public static DnsResult<PtrRecord> ResolvePtr(
        string name);
    public static DnsResult<PtrRecord> ResolvePtr(
        IPAddress address);
    public static Task<DnsResult<PtrRecord>> ResolvePtrAsync(
        string name, CancellationToken cancellationToken = default);
    public static Task<DnsResult<PtrRecord>> ResolvePtrAsync(
        IPAddress address, CancellationToken cancellationToken = default);

    public static DnsResult<NsRecord> ResolveNs(
        string name);
    public static Task<DnsResult<NsRecord>> ResolveNsAsync(
        string name, CancellationToken cancellationToken = default);
}

// Instance-based API where you can configure a custom DNS server
//
// In the first implementation, we don't expect it to hold any significant state, just the DnsResolverOptions configuration,
// Having an instance allow not passing the DnsResolverOptions separately for each request, and would simplify things
// in the future should we decide we want to implement Dns-over-HTTPS/TLS or other variants
public sealed class DnsResolver : IAsyncDisposable, IDisposable
{
    // uses default options (system-configured DNS server)
    public DnsResolver();

    public DnsResolver(DnsResolverOptions options);

    public DnsResult<AddressRecord> ResolveAddresses(string name);
    public DnsResult<AddressRecord> ResolveAddresses(string name, AddressFamily addressFamily);
    public DnsResult<SrvRecord> ResolveSrv(string name);
    public DnsResult<MxRecord> ResolveMx(string name);
    public DnsResult<TxtRecord> ResolveTxt(string name);
    public DnsResult<CNameRecord> ResolveCName(string name);
    public DnsResult<PtrRecord> ResolvePtr(string name);
    public DnsResult<NsRecord> ResolveNs(string name);

    public Task<DnsResult<AddressRecord>> ResolveAddressesAsync(string name, CancellationToken cancellationToken = default);
    public Task<DnsResult<AddressRecord>> ResolveAddressesAsync(string name, AddressFamily addressFamily, CancellationToken cancellationToken = default);
    public Task<DnsResult<SrvRecord>> ResolveSrvAsync(string name, CancellationToken cancellationToken = default);
    public Task<DnsResult<MxRecord>> ResolveMxAsync(string name, CancellationToken cancellationToken = default);
    public Task<DnsResult<TxtRecord>> ResolveTxtAsync(string name, CancellationToken cancellationToken = default);
    public Task<DnsResult<CNameRecord>> ResolveCNameAsync(string name, CancellationToken cancellationToken = default);
    public Task<DnsResult<PtrRecord>> ResolvePtrAsync(string name, CancellationToken cancellationToken = default);
    public Task<DnsResult<NsRecord>> ResolveNsAsync(string name, CancellationToken cancellationToken = default);

    public void Dispose();
    public ValueTask DisposeAsync();
}

public sealed class DnsResolverOptions
{
    public IList<IPEndPoint> Servers { get; set; } = new List<IPEndPoint>();

    // Possibly more config options coming in the future, e.g.
    // public bool EnableCache { get; set; }
    // If set, ignore `Servers` and use provided Transport abstraction, allows implementing DoT, DoH, DoQ, ...
    // public DnsTransport? Transport { get; set; }
}

public readonly struct DnsResult<T>
{
    // The actual response code from the DNS message header
    public DnsResponseCode ResponseCode { get; }
    public ReadOnlyList<T> Records { get; }

    //
    // For negative responses (NXDOMAIN/NODATA), `NegativeCacheExpiresAt` is
    // populated from the SOA minimum TTL in the authority section (per RFC 2308
    // §5), enabling callers to cache negative results.
    //
    public TimeSpan NegativeCacheTtl { get; }
}

// Defined by RFC
public enum DnsResponseCode : ushort
{
    NoError        = 0,
    FormatError    = 1,
    ServerFailure  = 2,
    NxDomain       = 3,
    NotImplemented = 4,
    Refused        = 5,
}

public readonly struct AddressRecord
{
    public IPAddress Address { get; }

    public TimeSpan Ttl { get; }
}

public readonly struct SrvRecord
{
    public string Target { get; }
    public ushort Port { get; }
    public ushort Priority { get; }
    public ushort Weight { get; }
    public TimeSpan Ttl { get; }
    public ReadOnlyList<AddressRecord> Addresses { get; }
}

public readonly struct MxRecord
{
    public string Exchange { get; }
    public ushort Preference { get; }
    public TimeSpan Ttl { get; }
}

public readonly struct TxtRecord
{
    public ReadOnlyList<string> Values { get; }
    public TimeSpan Ttl { get; }
}

public readonly struct CNameRecord
{
    public string CanonicalName { get; }
    public TimeSpan Ttl { get; }
}

public readonly struct PtrRecord
{
    public string Name { get; }
    public TimeSpan Ttl { get; }
}

public readonly struct NsRecord
{
    public string Name { get; }
    public TimeSpan Ttl { get; }
}
```
