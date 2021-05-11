# Quick Reviews 05/11/2021

## Add LoggerMessage.Define overloads accepting up to 14 arguments.

**NeedsWork** | [#runtime/50913](https://github.com/dotnet/runtime/issues/50913#issuecomment-838958162)

The new overloads need more work, @davidfowl has concerns about the return type, and some explosion.

The new LogOptions class should be LogDefineOptions

```C#
namespace Microsoft.Extensions.Logging
{ 
    public class LogDefineOptions
    {
        public bool SkipEnabledCheck { get; set; }
    }
}
```

Updating the not-yet-released API:

```diff
namespace Microsoft.Extensions.Logging
{ 
    public static partial class LoggerMessage
    {
-        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, System.Exception?> Define<T1>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, bool skipEnabledCheck) { throw null; }
+        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, System.Exception?> Define<T1>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, Microsoft.Extensions.Logging.LogOptions options) {​ throw null; }​
-        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, System.Exception?> Define<T1, T2>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, bool skipEnabledCheck) { throw null; }
+        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, System.Exception?> Define<T1, T2>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, Microsoft.Extensions.Logging.LogOptions options) {​ throw null; }​
-        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, System.Exception?> Define<T1, T2, T3>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, bool skipEnabledCheck) { throw null; }
+        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, System.Exception?> Define<T1, T2, T3>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, Microsoft.Extensions.Logging.LogOptions options) {​ throw null; }​
-        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, System.Exception?> Define<T1, T2, T3, T4>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, bool skipEnabledCheck) { throw null; }
+        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, System.Exception?> Define<T1, T2, T3, T4>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, Microsoft.Extensions.Logging.LogOptions options) {​ throw null; }​
-        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, T5, System.Exception?> Define<T1, T2, T3, T4, T5>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, bool skipEnabledCheck) { throw null; }
+        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, T5, System.Exception?> Define<T1, T2, T3, T4, T5>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, Microsoft.Extensions.Logging.LogOptions options) {​ throw null; }​
-        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, T5, T6, System.Exception?> Define<T1, T2, T3, T4, T5, T6>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, bool skipEnabledCheck) { throw null; }
+        public static System.Action<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, T5, T6, System.Exception?> Define<T1, T2, T3, T4, T5, T6>(Microsoft.Extensions.Logging.LogLevel logLevel, Microsoft.Extensions.Logging.EventId eventId, string formatString, Microsoft.Extensions.Logging.LogOptions options) {​ throw null; }​
    }
}
```
## Developers can trace their code using Open Telemetry Metrics

**Approved** | [#runtime/44445](https://github.com/dotnet/runtime/issues/44445#issuecomment-839028589)

In addition to the previous feedback:

* Do MeterListener.InstrumentPublished and MeasurementsCompleted need to be get/set? Should they be ctor parameters instead?
  * It seems there are theoretical scenarios for replacing them later, so get/set is fine
* What is the experience when a SetMeasurementCallback wasn't called for the T that is being reported?
  * Consider something like `public Action<Instrument>? UnregisteredMeasurementCallback { get; set; }` to get called in that case instead of just data loss.

```C#
namespace System.Diagnostics.Metrics
{
    public class Meter : IDisposable
    {

        /// <summary>
        /// The constructor allows creating the Meter class with the name and optionally the version.
        /// The name should be validated as described by the OpenTelemetry specs.
        /// </summary>
        public Meter(string name)  { throw null;  }
        public Meter(string name, string? version) { throw null; }

        /// <summary>
        /// Getter properties to retrieve the Meter name and version
        /// </summary>
        public string Name { get; }
        public string? Version { get; }

        /// <summary>
        /// Factory methods to create Counter and Histogram instruments.
        /// </summary>
        public Counter<T> CreateCounter<T>(
                            string name,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        public Histogram<T> CreateHistogram<T>(
                            string name,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        /// <summary>
        /// Factory methods to create an ObservableCounter instrument.
        /// </summary>

        public ObservableCounter<T> CreateObservableCounter<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        public ObservableCounter<T> CreateObservableCounter<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit = null,
                            string? description = null,) where T : struct { throw null; }

        public ObservableCounter<T> CreateObservableCounter<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        /// <summary>
        /// Factory methods to create ObservableGauge instrument.
        /// </summary>
        public ObservableGauge<T> CreateObservableGauge<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit = null,
                            string? description = null,) where T : struct { throw null; }

        public ObservableGauge<T> CreateObservableGauge<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        public ObservableGauge<T> CreateObservableGauge<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        /// <summary>
        /// Factory methods to create ObservableUpDownCounter instrument.
        /// </summary>
        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit = null,
                            string? description = null,) where T : struct { throw null; }

        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit = null,
                            string? description = null,) where T : struct { throw null; }

        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit = null,
                            string? description = null) where T : struct { throw null; }

        public void Dispose() { throw null; }
    }

/// <summary>
    /// Is the base class which contains all common properties between different types of instruments.
    /// It contains the protected constructor and the Publish method allows activating the instrument
    /// to start recording measurements.
    /// </summary>

    public abstract class Instrument
    {
        /// <summary>
        /// Protected constructor to initialize the common instrument properties.
        /// </summary>
        protected Instrument(Meter meter, string name, string? unit, string? description) { throw null; }

        /// <summary>
        /// Publish is to allow activating the instrument to start recording measurements and to allow
        /// listeners to start listening to such measurements.
        /// </summary>
        protected void Publish() { throw null; }

        /// <summary>
        /// Getters to retrieve the properties that the instrument is created with.
        /// </summary>
        public Meter Meter { get; }
        public string Name { get; }
        public string? Unit { get; }
        public string? Description { get; }

        /// <summary>
        /// A property tells if a listener is listening to this instrument measurement recording.
        /// </summary>
        public bool Enabled => throw null;

        /// <summary>
        /// A property tells if the instrument is a regular instrument or an observable instrument.
        /// </summary>
        public virtual bool IsObservable => throw null;
    }


    /// <summary>
    /// Instrument<T> is the base class from which all instruments that report measurements in the context of the request will inherit from.
    /// Mainly it will support the CLS compliant numerical types.
    /// </summary>
    public abstract class Instrument<T> : Instrument where T : struct
    {
        /// <summary>
        /// Protected constructor to create the instrument with the common properties.
        /// </summary>
        protected Instrument(Meter meter, string name, string? unit, string? description) :
                        base(meter, name, unit, description) { throw null; }

        /// <summary>
        /// Record measurement overloads allowing passing different numbers of tags.
        /// </summary>

        protected void RecordMeasurement(T measurement) { throw null; }

        protected void RecordMeasurement(
                            T measurement,
                            KeyValuePair<string, object?> tag) { throw null; }

        protected void RecordMeasurement(
                            T measurement,
                            KeyValuePair<string, object?> tag1,
                            KeyValuePair<string, object?> tag2) { throw null; }

        protected void RecordMeasurement(
                            T measurement,
                            KeyValuePair<string, object?> tag1,
                            KeyValuePair<string, object?> tag2,
                            KeyValuePair<string, object?> tag3) { throw null; }

        protected void RecordMeasurement(
                            T measurement,
                            ReadOnlySpan<KeyValuePair<string, object?>> tags) { throw null; }
    }


    /// <summary>
    /// ObservableInstrument<T> is the base class from which all observable instruments will inherit from.
    /// It will only support the CLS compliant numerical types.
    /// </summary>
    public abstract class ObservableInstrument<T> : Instrument where T : struct
    {
        /// <summary>
        /// Protected constructor to create the instrument with the common properties.
        /// </summary>
        protected ObservableInstrument(
                    Meter meter,
                    string name,
                    string? unit,
                    string? description) : base(meter, name, unit, description) { throw null; }

        /// <summary>
        /// Observe() fetches the current measurements being tracked by this instrument.
        /// </summary>
        protected abstract IEnumerable<Measurement<T>> Observe();

        public override bool IsObservable => true;
    }


    /// <summary>
    /// A measurement stores one observed value and its associated tags. This type is used by Observable instruments' Observe() method when reporting current measurements with associated tags.
    /// with the associated tags.
    /// </summary>
    public readonly struct Measurement<T> where T : struct
    {
        /// <summary>
        /// Construct the Measurement using the value and the list of tags.
        /// We'll always copy the input list as this is not perf hot path.
        /// </summary>
        public Measurement(T value) { throw null; }
        public Measurement(T value, IEnumerable<KeyValuePair<string, object?>> tags) { throw null; }
        public Measurement(T value, params KeyValuePair<string, object?>[] tags) { throw null; }
        public Measurement(T value, ReadOnlySpan<KeyValuePair<string, object?>> tags) { throw null; }

        public ReadOnlySpan<KeyValuePair<string, object?>> Tags { get { throw null;  } }
        public T Value { get; }
    }


    /// <summary>
    /// The counter is an Instrument that supports non-negative increments.
    /// e.g. Number of completed requests.
    /// </summary>
    public sealed class Counter<T> : Instrument<T> where T : struct
    {
        public void Add(T delta) { throw null; }
        public void Add(T delta,
                        KeyValuePair<string, object?> tag) { throw null; }
        public void Add(T delta,
                        KeyValuePair<string, object?> tag1,
                        KeyValuePair<string, object?> tag2) { throw null; }
        public void Add(T delta,
                        KeyValuePair<string, object?> tag1,
                        KeyValuePair<string, object?> tag2,
                        KeyValuePair<string, object?> tag3) { throw null; }
        public void Add(T delta,
                        ReadOnlySpan<KeyValuePair<string, object?>> tags) { throw null; }
        public void Add(T delta,
                        params KeyValuePair<string, object?>[] tags) { throw null; }
    }

    /// <summary>
    /// The histogram is an Instrument that can be used to report arbitrary values
    /// that are likely to be statistically meaningful. It is intended for statistics such as the request duration.
    /// e.g. the request duration.
    /// </summary>
    public sealed class Histogram<T> : Instrument<T> where T : struct
    {
        public void Record(T value) { throw null; }
        public void Record(
                        T value,
                        KeyValuePair<string, object?> tag) { throw null; }
        public void Record(
                        T value,
                        KeyValuePair<string, object?> tag1,
                        KeyValuePair<string, object?> tag2) { throw null; }
        public void Record(
                        T value,
                        KeyValuePair<string, object?> tag1,
                        KeyValuePair<string, object?> tag2,
                        KeyValuePair<string, object?> tag3) { throw null; }
        public void Record(T value,
                            ReadOnlySpan<KeyValuePair<string, object?>> tags) { throw null; }
        public void Record(
                        T value,
                        params KeyValuePair<string, object?>[] tags) { throw null; }
    }


    /// <summary>
    /// ObservableCounter is an observable Instrument that reports monotonically increasing value(s)
    /// when the instrument is being observed.
    /// e.g. CPU time (for different processes, threads, user mode or kernel mode).
    /// </summary>
    public sealed class ObservableCounter<T> : ObservableInstrument<T> where T : struct
    {
        protected override IEnumerable<Measurement<T>> Observe() { throw null; }
    }

    /// <summary>
    /// ObservableUpDownCounter is an observable Instrument that reports additive value(s)
    /// when the instrument is being observed.
    /// e.g. the process heap size
    /// </summary>
    public sealed class ObservableUpDownCounter<T> : ObservableInstrument<T> where T : struct
    {
        protected override IEnumerable<Measurement<T>> Observe() { throw null; }
    }

    /// <summary>
    /// ObservableGauge is an observable Instrument that reports non-additive value(s)
    /// when the instrument is being observed.
    /// e.g. the current room temperature
    /// </summary>
    public sealed class ObservableGauge<T> : ObservableInstrument<T> where T : struct
    {
        protected override IEnumerable<Measurement<T>> Observe() { throw null; }
    }


    /// <summary>
    /// A delegate to represent the callbacks signatures used in the listener.
    /// </summary>
    public delegate void MeasurementCallback<T>(
                            Instrument instrument,
                            T measurement,
                            ReadOnlySpan<KeyValuePair<string, object?>> tags,
                            object? state) where T : struct;


    /// <summary>
    /// The listener class can be used to listen to kinds of instruments.
    /// recorded measurements.
    /// </summary>
    public sealed class MeterListener : IDisposable
    {
        /// <summary>
        /// Simple constructor
        /// </summary>
        public MeterListener() { throw null;  }

        /// <summary>
        /// Callbacks to get notification when an instrument is published
        /// </summary>
        public Action<Instrument, MeterListener>? InstrumentPublished { get; set; }

        /// <summary>
        /// Callbacks to get notification when stopping the measurement on some instrument
        /// this can happen when the Meter or the Listener is disposed of. Or calling Stop()
        /// on the listener.
        /// </summary>
        public Action<Instrument, object?>? MeasurementsCompleted { get; set; }

        /// <summary>
        /// Start listening to a specific instrument measurement recording.
        /// </summary>
        public void EnableMeasurementEvents(Instrument instrument, object? state = null) { throw null; }

        /// <summary>
        /// Stop listening to a specific instrument measurement recording.
        /// returns the associated state.
        /// </summary>
        public object? DisableMeasurementEvents(Instrument instrument) { throw null; }

        /// <summary>
        /// Set a callback for a specific numeric type to get the measurement recording notification
        /// from all instruments which enabled listened to and was created with the same specified
        /// numeric type. If a measurement of type T is recorded and a callback of type T is registered, that callback is used. If there is no callback for type T but there is a callback for type object, the measured value is boxed and reported via the object typed callback. If there is neither type T callback nor object callback then the measurement will not be reported.
        /// </summary>
        public void SetMeasurementEventCallback<T>(MeasurementCallback<T>? measurementCallback) where T : struct { throw null; }

        public void Start() { throw null; }

        /// <summary>
        /// Call all Observable instruments to get the recorded measurements reported to the
        /// callbacks enabled by SetMeasurementEventCallback<T>
        /// </summary>
        public void RecordObservableInstruments() { throw null; }

        public void Dispose() { throw null; }
    }
}
```
