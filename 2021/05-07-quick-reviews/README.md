# Quick Reviews 05/07/2021

## Developers can trace their code using Open Telemetry Metrics

**NeedsWork** | [#runtime/44445](https://github.com/dotnet/runtime/issues/44445#issuecomment-834688067) | [Video](https://www.youtube.com/watch?v=Cquc3gfSirQ&t=0h0m0s)

* In the Metric.Create* methods, consider swapping the order of unit and description, if (name+unit) seems more likely than (name+description).
* "Observable" in this context has a meaning opposite of what it means in "ObservableCollection"
  * We don't have a solid replacement name for it, though, since it seems that every appropriate word already has a conflicting meaning in OpenTelemetry.
* Consider renaming `Instrument<T>` to `StreamingInstrument<T>`, or some other prefix, to give better separation between `ObservableInstrument<T>` and "the other kind".
* Add a `public Measurement(T value)` constructor so the params one is not called with the empty array.
* For `Counter<T>.Add`, rename `measurement` to `delta` to make it more clear that the caller should provide only the amount to increase since the last call, vs that "Add" means appending to a list.
* Rename `Histogram<T>.Record`'s `measurement` parameter to `value` (to avoid confusion with `Measurement<T>` and associated semantics)
* `Measurement<T>` should be declared readonly
* MeasurementCallback should constraint T to match the rest of the proposal (T : unmanaged)
* Replace all of the T : unmanaged constraints with T : struct

We had to stop before really looking at MetricListener.
## C# 10 interpolated strings support, part 3

**Approved** | [#runtime/51962](https://github.com/dotnet/runtime/issues/51962#issuecomment-834716920) | [Video](https://www.youtube.com/watch?v=Cquc3gfSirQ&t=1h41m3s)

After a long discussion we think the best convention we'll come up with is the suffix "InterpolatedStringHandler"

Therefore, the renames should be

* InterpolatedStringHandlerArgumentAttribute
* InterpolatedStringHandlerAttribute
* DefaultInterpolatedStringHandler
* SpanInterpolatedStringHandler
* StringBuilder+AppendFormatInterpolatedStringHandler

The attribute is otherwise good as proposed

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct, AllowMultiple = false, Inherited = false)]
    public sealed class InterpolatedStringHandlerAttribute : Attribute
    {
        public InterpolatedStringHandlerAttribute()
        {
        }
    }
}
```

