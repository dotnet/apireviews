# API Review 05/18/2023

## Introduce DI friendly IMeter<T> for modern services

**Approved** | [#runtime/77514](https://github.com/dotnet/runtime/issues/77514#issuecomment-1553414738) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=0h0m0s)

* As MeterOptions.Name is required, we added a ctor to specify it.
* We're OK with the name Create for the factory, but need to ensure that it is OK to call Dispose on the returned Meter.
  * During the discussion we realized that Meter (an unsealed type) did not do the Basic Dispose Pattern, so we're adding the protected virtual Dispose to facilitate this.

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
            string? description,
            IEnumerable<KeyValuePair<string, object?>>? tags);
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

    public class MeterOptions
    {
        public string Name { get; set; }
        public string? Version { get; set; }
        public IEnumerable<KeyValuePair<string,object?>>? Tags { get; set; }
        public object? Scope { get; set; }

        public MeterOptions(string name);
    }

    public partial class Meter : IDisposable
    {
        public Meter(
            string name,
            string? version,
            IEnumerable<KeyValuePair<string, object?>>? tags,
            object? scope = null);

        public Meter(MeterOptions options);

        public IEnumerable<KeyValuePair<string, object?>>? Tags { get ; }

        public object? Scope { get; }

        protected virtual void Dispose(bool disposing);

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
            object? scopeFilter,
            string meterName,
            string instrumentName);
        public InstrumentRecorder(
            Meter meter,
            string instrumentName);
    
        public Instrument? Instrument { get; }

        public IEnumerable<Measurement<T>> GetMeasurements(bool clear = false);
        public void Dispose();
    }
}

namespace Microsoft.Extensions.Metrics
{
    public interface IMeterFactory : IDisposable
    {
        Meter Create(MeterOptions options);
    }

    public static class MetricsServiceExtensions
    {
        public static IServiceCollection AddMetrics(this IServiceCollection services);        
    }

    public static class MeterFactoryExtensions
    {
        // Adding extra extension method helper for creating the meter with flat parameters. 
        public static Meter Create(this IMeterFactory, string name, string? version = null, IEnumerable<KeyValuePair<string,object?>> tags = null,  object? scope = null);
    }
}
```
## Add a `JsonTypeInfoResolver.WithModifier` extension method

**NeedsWork** | [#runtime/86440](https://github.com/dotnet/runtime/issues/86440#issuecomment-1553432673) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=0h49m3s)

This came up in API Review, and we were unclear what the semantics of the method should be.

If it's creating a clone and then appending the new modifier, then we think a name like `WithExtraModifier`/`WithAdditionalModifier`/etc is appropriate.  If it's clearing and assigning only the one, then a different name would be better.

The alternative proposal (params `WithModifiers`) definitely suggests full replacement.
## Convert between degrees and radians

**Approved** | [#runtime/86402](https://github.com/dotnet/runtime/issues/86402#issuecomment-1553447132) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=1h3m38s)

* We changed ToRadians to DegreesToRadians and ToDegrees to RadiansToDegrees so they read better (`double.RadiansToDegrees(double.Pi)` vs `double.ToDegrees(double.Pi)`)
* The method will be specifically implemented (and thus visible) on Single, Double, Half, and NFloat.
* The proposal also mentioned adding these to `Math` and `MathF`, but that is redundant with the methods being exposed on Double and Single already.

```diff
  namespace System.Numerics {
      public interface ITrigonometricFunctions<TSelf> {
+         public static virtual TSelf RadiansToDegrees(TSelf radians) { imprecise, but working, body }
+         public static virtual TSelf DegreesToRadians(TSelf degrees) { imprecise, but working, body }
      }
  }
```


## Add Length and IsEmpty properties to System.BinaryData

**Approved** | [#runtime/77970](https://github.com/dotnet/runtime/issues/77970#issuecomment-1553454588) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=1h17m6s)

Looks good as proposed.

We briefly discussed `Length` vs `Count`, and feel that `Length` is more appropriate here because it's representing a fixed size window of data, like Span/Memory/Array, vs a point-in-time number like List and the other collections.

```C#
namespace System;

public class BinaryData
{
    public int Length { get; }
    public bool IsEmpty { get; }
}
```
## CngProperty constructor with ReadOnlySpan<byte> overload

**Approved** | [#runtime/69061](https://github.com/dotnet/runtime/issues/69061#issuecomment-1553463524) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=1h22m10s)

Looks good as proposed.

There may be some interesting edge cases around null vs empty (and therefore the default ReadOnlySpan vs a zero-width slice) which probably warrant testing (or proving that null and empty are the same to CNG)

```C#
namespace System.Security.Cryptography {
    public partial struct CngProperty {
       public CngProperty(string name, ReadOnlySpan<byte> value, CngPropertyOptions options);
    }
}
```
## Analyzer for cases when the `Dispose()` pattern is introduced after the fact

**Approved** | [#runtime/70188](https://github.com/dotnet/runtime/issues/70188#issuecomment-1553471826) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=1h26m12s)

Looks good as proposed.

For both the override diagnostic and the finalizer diagnostic

Default Severity: Warning
Category: Usage
## Allow specifying indent size and whitespace character when writing JSON with Utf8JsonWriter

**Approved** | [#runtime/63882](https://github.com/dotnet/runtime/issues/63882#issuecomment-1553487279) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=1h29m45s)

* Because `char`+`int` is limiting (it doesn't allow anything with varied indentation, like "->  ", or anything outside the Basic Multilingual Plane) we combined them into one field.
* While we typed the field as `string`, `JsonEncodedText` might be more appropriate
* The property is proposed as non-nullable, defaulting to whatever the current indentation is (two spaces? four?)

```C#
namespace System.Text.Json
{
    public sealed class JsonSerializerOptions 
    {
        public string IndentText { get; set; }
    }
    
    public struct JsonWriterOptions 
    {
        public string IndentText { get; set; }
    }
}
```


## TryFindSystemTimeZoneById

**Approved** | [#runtime/66928](https://github.com/dotnet/runtime/issues/66928#issuecomment-1553491747) | [Video](https://www.youtube.com/watch?v=XFfdBdOE-mQ&t=1h44m11s)

Looks good as proposed (ignoring that the body says to do a try/catch).

```C#
namespace System
{
    public class TimeZoneInfo
    {
        public static bool TryFindSystemTimeZoneById(string id, out TimeZoneInfo timeZoneInfo);
    }
}
```
