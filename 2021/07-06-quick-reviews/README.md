# API Review 07/06/2021

## Developers will be able to control how distributed tracing information is propagated

**Approved** | [#runtime/50658](https://github.com/dotnet/runtime/issues/50658#issuecomment-875007158)

* Since there's already a strong delegate type for the getter, let's use a strong delegate type for the setter to name the string parameters.
* Extract(out id, out state) should use the "trace" prefix to help callers understand the alignment to (e.g.) Activity.TraceStateString
* Instead of relying on the shape of the out parameters, the different Extract methods should use purpose names
* We renamed CreateLegacyPropagator() to CreateDefaultPropagator()

```C#
namespace System.Diagnostics
{
    public abstract class TextMapPropagator
    {
      public delegate void PropagatorGetterCallback(object carrier, string fieldName, out string? fieldValue, out IEnumerable<string>? fieldValues);
      public delegate void PropagatorSetterCallback(object carrier, string fieldName, string fieldValue);

      public abstract IReadOnlyCollection<string> Fields { get; }
      public abstract void Inject(Activity activity, object carrier, PropagatorSetterCallback setter);
      public abstract void ExtractTraceIdAndState(object carrier, PropagatorGetterCallback getter, out string? traceId, out string? traceState);
      public abstract IEnumerable<KeyValuePair<string, string?>>? ExtractBaggage(object carrier, PropagatorGetterCallback getter);

      public static TextMapPropagator Current { get; set; }
      public static TextMapPropagator CreateDefaultPropagator() { throw null; }
      public static TextMapPropagator CreatePassThroughPropagator() { throw null; }
      public static TextMapPropagator CreateNoOutputPropagator() { throw null; }
    }
}
```
