# API Review 05/16/2023

## Minor cleanup of approved Avx512 API surface

**Approved** | [#runtime/86168](https://github.com/dotnet/runtime/issues/86168#issuecomment-1550126484) | [Video](https://www.youtube.com/watch?v=h_7hz1o2RGs&t=0h0m0s)

This just needs `MultiShift` exposed and can then be closed. Other work has already been completed as part of implementing:
* https://github.com/dotnet/runtime/issues/73604
* https://github.com/dotnet/runtime/issues/74813
* https://github.com/dotnet/runtime/issues/76579
## Introduce DI friendly IMeter<T> for modern services

**NeedsWork** | [#runtime/77514](https://github.com/dotnet/runtime/issues/77514#issuecomment-1550203450) | [Video](https://www.youtube.com/watch?v=h_7hz1o2RGs&t=0h54m12s)

* Meter doesn't need to have the factory as typed data.  Since it's only needed for ReferenceEquals later, it should just be `object? context`
* `IMeterFactory` has been moved from System.Diagnostics.Metrics to Microsoft.Extensions.Metrics.
* InstrumentRecorder seems like low level functionalty, so it should be in System.Diagnostics.Metrics
  * And the IMeterFactory parameter changed to object
  * And we added a Reset method

The pieces that are in System.Diagnostics.Metrics were discussed and are now effectively approved.
We ran out of time before finishing some issues in IMeterFactory (should the name be Create, GetOrCreate, etc; the ownership and disposal semantics)

Approved:

```C#
namespace System.Diagnostics.Metrics
{
    public abstract partial class Instrument
    {
        protected Instrument(
            Meter meter,
            string name,
            string? unit,
            string? description,
            IEnumerable<KeyValuePair<string, object?>>? tags);

         public IEnumerable<KeyValuePair<string, object?>>? Tags { get; }
    }

    public abstract partial class Instrument<T> : Instrument where T : struct
    {
        protected Instrument(
            Meter meter,
            string name,
            string? unit,
            string? description, IEnumerable<KeyValuePair<string, object?>>? tags);
    }

    public abstract partial class ObservableInstrument<T> : Instrument where T : struct
    {
        protected ObservableInstrument(
            Meter meter,
            string name,
            string? unit,
            string? description,
            IEnumerable<KeyValuePair<string, object?>> tags);
    }

    public partial class Meter : IDisposable
    {
        public Meter(
            string name,
            string? version,
            IEnumerable<KeyValuePair<string, object?>>? tags,
            object? context = null);

        public IEnumerable<KeyValuePair<string, object?>>? Tags { get ; }}

        public object? Context { get; }

        public Counter<T> CreateCounter<T>(
                            string name,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public UpDownCounter<T> CreateUpDownCounter<T>(
                            string name,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public Histogram<T> CreateHistogram<T>(
                            string name,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableCounter<T> CreateObservableCounter<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableCounter<T> CreateObservableCounter<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableCounter<T> CreateObservableCounter<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableGauge<T> CreateObservableGauge<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableGauge<T> CreateObservableGauge<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;

        public ObservableGauge<T> CreateObservableGauge<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit,
                            string? description,
                            IEnumerable<KeyValuePair<string, object?>> tags) where T : struct;
    }

    public sealed class InstrumentRecorder<T> : IDisposable where T : struct
    {
        public InstrumentRecorder(Instrument instrument);
        public InstrumentRecorder(
                        object? contextFilter,
                        string meterName,
                        string instrumentName);
        public InstrumentRecorder(
                        Meter meter,
                        string instrumentName);
    
        public Instrument Instrument { get; }

        public IEnumerable<Measurement<T>> GetMeasurements(bool clear = false);
        public void Dispose();
    }
}
```

Needs Work (future discussion):
```C#
namespace Microsoft.Extensions.Metrics
{
    public interface IMeterFactory : IDisposable
    {
        Meter Create(
            string name,
            string? version = null,
            IEnumerable<KeyValuePair<string, object?>>? tags = null);
    }

    public static class MetricsServiceExtensions
    {
        public static IServiceCollection AddMetrics(this IServiceCollection services);        
    }
}
```
