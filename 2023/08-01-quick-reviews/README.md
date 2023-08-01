# API Review 08/01/2023

## Meter Configuration and Pipeline

**Approved** | [#runtime/85684](https://github.com/dotnet/runtime/issues/85684#issuecomment-1660907845) | [Video](https://www.youtube.com/watch?v=sdOk6E8p0Hg&t=0h0m0s)

* `IMetricsListener.GetMeasurementHandler()`
    - This method should return a class that has a callback for all `T`s that we expect listeners to support (7 as of today). We should probably have those be nullable. If they are, we'll drop the measurements on the floor.
* `InstrumentEnableRule`
    - Should probably just be `InstrumentRule`
* `MetricsEnableOptions`
    - Should probably just `MetricsOptions`
* `MetricScope`
    - Should have a zero bit named `None` and `Global` should be non-zero
* `MetricsBuilderEnableExtensions`
    - Consider simplifying the overload using default parameters
    - But you want to keep the simplest ones to avoid confusing people
    - `DisableMetrics` should probably mirror `EnableMetrics`
* `IMetricsSubscriptionManager`
    - Does this need to be public?

```C#
namespace Microsoft.Extensions.DependencyInjection;

public static class MetricsServiceCollectionExtensions
{
    public static IServiceCollection AddMetrics(this IServiceCollection services);
    public static IServiceCollection AddMetrics(this IServiceCollection services, Action<IMetricsBuilder> configure);
}
```

```C#
namespace Microsoft.Extensions.Diagnostics.Metrics;

public interface IMetricsBuilder
{
    IServiceCollection Services { get; }
}

public static class MetricsBuilderExtensions
{
    public static IMetricsBuilder AddListener(this IMetricsBuilder builder, IMetricsListener listener);
    public static IMetricsBuilder AddListener<T>(this IMetricsBuilder builder) where T : class, IMetricsListener;
    public static IMetricsBuilder ClearListeners(this IMetricsBuilder builder);
}

public interface IMetricsSource
{
    void RecordObservableInstruments();
}

public interface IMetricsListener
{
    string Name { get; }
    void SetSource(IMetricsSource source);
    bool InstrumentPublished(Instrument instrument, out object? userState);
    void MeasurementsCompleted(Instrument instrument, object? userState);
    MeasurementCallback<T> GetMeasurementHandler<T>();
}

public class MetricsEnableOptions
{
    public IList<InstrumentEnableRule> Rules { get; }
}

public class InstrumentEnableRule
{
    public InstrumentEnableRule(string? listenerName, string? meterName, MeterScope scopes, string? instrumentName, bool enable);
    public string? ListenerName { get; }
    public string? MeterName { get; }
    public MeterScope Scopes { get; }
    public string? InstrumentName { get; }
    public bool Enable { get; }
}

[Flags]
public enum MeterScope
{
    None = 0x00,
    Global = 0x01
    Local = 0x02
}

public interface IMetricsSubscriptionManager
{
    void Start();
}

public static class MetricsBuilderEnableExtensions
{
    public static IMetricsBuilder EnableMetrics(this IMetricsBuilder builder);
    public static IMetricsBuilder EnableMetrics(this IMetricsBuilder builder, string? meterName);
    public static IMetricsBuilder EnableMetrics(this IMetricsBuilder builder, string? meterName, string? instrumentName);
    public static IMetricsBuilder EnableMetrics(this IMetricsBuilder builder, string? meterName, string? instrumentName, string? listenerName);
    public static IMetricsBuilder EnableMetrics(this IMetricsBuilder builder, string? meterName, string? instrumentName, string? listenerName, MeterScope scopes);

    public static MetricsEnableOptions EnableMetrics(this MetricsEnableOptions options);
    public static MetricsEnableOptions EnableMetrics(this MetricsEnableOptions options, string? meterName);
    public static MetricsEnableOptions EnableMetrics(this MetricsEnableOptions options, string? meterName, string? instrumentName);
    public static MetricsEnableOptions EnableMetrics(this MetricsEnableOptions options, string? meterName, string? instrumentName, string? listenerName);
    public static MetricsEnableOptions EnableMetrics(this MetricsEnableOptions options, string? meterName, string? instrumentName, string? listenerName, MeterScope scopes);

    // Which overloads of this do we want?
    public static MetricsEnableOptions DisableMetrics(this MetricsEnableOptions options, string? meterName, string? instrumentName, string? listenerName, MeterScope scopes);
}

public static class MetricsBuilderExtensions
{
    public static IMetricsBuilder AddConfiguration(this IMetricsBuilder builder, IConfiguration configuration);
}
```

```C#
namespace Microsoft.Extensions.Hosting
{
    public partial class HostApplicationBuilder
    {
        public IMetricsBuilder Metrics { get; }
    }
    public partial static class HostingHostBuilderExtensions
    {
        public static IHostBuilder ConfigureMetrics(this IHostBuilder hostBuilder, Action<IMetricsBuilder> configureMetrics);
        public static IHostBuilder ConfigureMetrics(this IHostBuilder hostBuilder, Action<HostBuilderContext, IMetricsBuilder> configureMetrics);
    }
}

namespace Microsoft.AspNetCore.Builder
{
    public partial class WebApplicationBuilder
    {
        public IMetricsBuilder Metrics { get; }
    }
}
```

<details>
<summary>Unreviewed</summary>

```C#
namespace Microsoft.Extensions.Diagnostics.Metrics.Configuration
{
    public interface IMetricListenerConfigurationFactory
    {
        IConfiguration GetConfiguration(Type listenerType);
    }

    public interface IMetricListenerConfiguration<T>
    {
        IConfiguration Configuration { get; }
    }
}

namespace Microsoft.Extensions.Diagnostics.Metrics
{
    public static class MetricsBuilderExtensions
    {
        public static IMetricsBuilder AddConfiguration(this IMetricsBuilder builder, IConfiguration configuration);
    }
}

namespace Microsoft.Extensions.Diagnostics.Metrics.Configuration
{
    public interface IMetricListenerConfigurationFactory
    {
        IConfiguration GetConfiguration(Type listenerType);
    }
    
    public interface IMetricListenerConfiguration<T>
    {
        IConfiguration Configuration { get; }
    }
}
```
</details>
