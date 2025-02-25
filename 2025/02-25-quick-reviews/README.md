# API Review 02/25/2025

## Use AddRandomProbabilisticSampler instead of AddProbabilisticSampler

**Approved** | [#extensions/5955](https://github.com/dotnet/extensions/issues/5955#issuecomment-2682933904) | [Video](https://www.youtube.com/watch?v=L10THXOq0l0&t=0h0m0s)

* We discussed a variety of names: "RandomProbabilistic", "Stochastic", "Random", "Linear", "Uniform", "LinearRandom", "UniformRandom", etc.  In the end we stuck with the proposed "RandomProbabilistic"

```diff
namespace Microsoft.Extensions.Logging
{
    public static class SamplingLoggerBuilderExtensions
    {
-      public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, IConfiguration configuration);
-      public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, Action<ProbabilisticSamplerOptions> configure);
-      public static ILoggingBuilder AddProbabilisticSampler(this ILoggingBuilder builder, double probability, LogLevel? level = null);
+      public static ILoggingBuilder AddRandomProbabilisticSampler(this ILoggingBuilder builder, IConfiguration configuration);
+      public static ILoggingBuilder AddRandomProbabilisticSampler(this ILoggingBuilder builder, Action<RandomProbabilisticSamplerOptions> configure);
+      public static ILoggingBuilder AddRandomProbabilisticSampler(this ILoggingBuilder builder, double probability, LogLevel? level = null);
    }
}
namespace Microsoft.Extensions.Diagnostics.Sampling
{
-   public class ProbabilisticSamplerOptions
+   public class RandomProbabilisticSamplerOptions
    {
-       public IList<ProbabilisticSamplerFilterRule> Rules { get; set; } = [];
+       public IList<RandomProbabilisticSamplerFilterRule> Rules { get; set; } = [];
    }

-   public class ProbabilisticSamplerFilterRule
+   public class RandomProbabilisticSamplerFilterRule
    {
-        public ProbabilisticSamplerFilterRule(
+        public RandomProbabilisticSamplerFilterRule(
            double probability,
            string? categoryName = null,
            LogLevel? logLevel = null,
            int? eventId = null,
            string? eventName = null);
    }
}
```
## Obsolete XsltSettings.EnableScript

**Approved** | [#runtime/108287](https://github.com/dotnet/runtime/issues/108287#issuecomment-2682985787) | [Video](https://www.youtube.com/watch?v=L10THXOq0l0&t=0h26m36s)

* On the basis that the property actually has no legitimate value, obsoleting it seems sound.
  * Double check that before making the change, as there are multiple call sites, only one of which throws.
* Use the next available diagnostic ID
* There was a debate as to whether the obsoletion should be at the property level or just on the setter, in end we decided to leave it on the property as a whole.

```diff
namespace System.Xml.Xsl;

public sealed class XsltSettings
{
+   [System.Obsolete("XSLT Script blocks are not supported on .NET Core or .NET 5 or later.", ID = ???)]
    public bool EnableScript { get; set; }
}
```
## void-returning Encode callback for AsnWriter

**Approved** | [#runtime/112675](https://github.com/dotnet/runtime/issues/112675#issuecomment-2683003232) | [Video](https://www.youtube.com/watch?v=L10THXOq0l0&t=0h52m6s)

* Looks good as proposed.
* Since the API exists purely for perf, there's no reason to add the Action overload that doesn't take a state.

```C#
namespace System.Formats.Asn1;

public partial class AsnWriter {
#if NET9_0_OR_GREATER
    public void Encode<TState>(
        TState state,
        Action<TState, ReadOnlySpan<byte>> encodeCallback) where TState : allows ref struct;
#endif
}
```
## Add schema URL support

**Approved** | [#runtime/63651](https://github.com/dotnet/runtime/issues/63651#issuecomment-2683065103) | [Video](https://www.youtube.com/watch?v=L10THXOq0l0&t=1h0m23s)

* Instead of adding new parameters on ActivitySource constructors, we added an Options type to make it more like Meter, and added the ability to set the value only on the options types.

```C#
namespace System.Diagnostics
{
    public sealed partial class ActivitySource
    {
        public ActivitySource(ActivitySourceOptions options);
        
        public string? TelemetrySchemaUrl { get; }
    }

    public class ActivitySourceOptions
    {
        public ActivitySourceOptions(string name);

        public string Name { get; set; }
        public string? Version { get; set; }
        public IEnumerable<KeyValuePair<string!, object?>>? Tags { get; set; }
        public string? TelemetrySchemaUrl { get; set; }
    }
}

namespace System.Diagnostics.Metrics
{
    public partial class Meter : IDisposable
    {
        public string? TelemetrySchemaUrl { get; }
    }

    public partial class MeterOptions
    {
        public string? TelemetrySchemaUrl { get; set; }
    }
}
```
## More `MemoryExtension` methods accepting `SearchValues<T>`

**Approved** | [#runtime/109859](https://github.com/dotnet/runtime/issues/109859#issuecomment-2683175515) | [Video](https://www.youtube.com/watch?v=L10THXOq0l0&t=1h25m28s)

* "CountOfAny" => "CountAny", the Of was deemed unnnecessary
  * Added the IEqualityComparer overload (not params)
* Count(Of)AnyExcept was deemed not necessary at this time.
* The proposed Trim methods would need to either have all 4 overloads, or be changed to return Range.  They also then force "should we also add TrimEnd and TrimStart"?
  * But we felt that there's not a strong need for Trim based on SearchValues at this time.
* In discussion, we felt that ReplaceAnyExcept had higher utility than ReplaceAny, so we added that to this proposal.

```C#
namespace System;

public static class MemoryExtensions
{
    public static int CountAny<T>(this ReadOnlySpan<T> span, SearchValues<T> values) where T : IEquatable<T>;
    public static int CountAny<T>(this ReadOnlySpan<T> span, params ReadOnlySpan<T> values) where T : IEquatable<T>;
    public static int CountAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values, IEqualityComparer<T>? comparer = null);

    public static void ReplaceAny<T>(this Span<T> span, SearchValues<T> oldValues, T newValue) where T : IEquatable<T>;
    public static void ReplaceAny<T>(this ReadOnlySpan<T> source, Span<T> destination, SearchValues<T> oldValues, T newValue) where T : IEquatable<T>;

    public static void ReplaceAnyExcept<T>(this Span<T> span, SearchValues<T> values, T newValue) where T : IEquatable<T>;
    public static void ReplaceAnyExcept<T>(this ReadOnlySpan<T> source, Span<T> destination, SearchValues<T> values, T newValue) where T : IEquatable<T>;
}
```
