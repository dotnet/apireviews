# API Review 08/10/2023

## Meter Configuration and Pipeline

**Approved** | [#runtime/85684](https://github.com/dotnet/runtime/issues/85684#issuecomment-1673597867) | [Video](https://www.youtube.com/watch?v=XNFWSZ0eJek&t=0h0m0s)

* Looks good as proposed.

```diff
 namespace Microsoft.Extensions.Diagnostics.Metrics.Configuration
 {
-    public interface IMetricListenerConfigurationFactory
-    {
-        IConfiguration GetConfiguration(Type listenerType);
-    }
-    public interface IMetricListenerConfiguration<T>
-    {
-        IConfiguration Configuration { get; }
-    }
+    public interface IMetricListenerConfigurationFactory
+    {
+        IConfiguration GetConfiguration(string listenerName);
+    }
 }
```
## Future of Numerics and AI - Provide a downlevel `System.Numerics.Tensor` class

**Approved** | [#runtime/89639](https://github.com/dotnet/runtime/issues/89639#issuecomment-1673707529) | [Video](https://www.youtube.com/watch?v=XNFWSZ0eJek&t=0h6m15s)

* The `T` should be `float`
* We should replace `left` and `right` with `x` and `y` for consistency
* `ConvertToXxx`: first parameter should be named `source`
* All span-returning methods should return `void`
* For `Half` and `MathF` we'll follow up offline; they might not be needed.

```C#
namespace System.Numerics.Tensors;

public static partial class Tensor
{
    public static void Add(ReadOnlySpan<float> x, ReadOnlySpan<float> y, Span<float> destination);
    public static void Add(ReadOnlySpan<float> x, float y, Span<float> destination);

    public static void Subtract(ReadOnlySpan<float> x, ReadOnlySpan<float> y, Span<float> destination);
    public static void Subtract(ReadOnlySpan<float> x, float y, Span<float> destination);

    public static void Multiply(ReadOnlySpan<float> x, ReadOnlySpan<float> y, Span<float> destination);

    public static void Divide(ReadOnlySpan<float> x, ReadOnlySpan<float> y, Span<float> destination);
    public static void Divide(ReadOnlySpan<float> x, float y, Span<float> destination);

    public static void Negate(ReadOnlySpan<float> values, Span<float> destination);

    public static void AddMultiply(ReadOnlySpan<float> x, ReadOnlySpan<float> y, ReadOnlySpan<float> multiplier, Span<float> destination);
    public static void AddMultiply(ReadOnlySpan<float> x, ReadOnlySpan<float> y, T multiplier, Span<float> destination);
    public static void AddMultiply(ReadOnlySpan<float> x, float y, ReadOnlySpan<float> multiplier, Span<float> destination);

    public static void MultiplyAdd(ReadOnlySpan<float> x, ReadOnlySpan<float> y, ReadOnlySpan<float> addend, Span<float> destination);

    public static void MultiplyAdd(ReadOnlySpan<float> x, float y, ReadOnlySpan<float> addend, Span<float> destination);

    public static void Exp(ReadOnlySpan<float> x, Span<float> destination);
    public static void Log(ReadOnlySpan<float> x, Span<float> destination);

    public static void Cosh(ReadOnlySpan<float> x, Span<float> destination);
    public static void Sinh(ReadOnlySpan<float> x, Span<float> destination);
    public static void Tanh(ReadOnlySpan<float> x, Span<float> destination);

    public static float CosineSimilarity(ReadOnlySpan<float> x, ReadOnlySpan<float> y);

    public static float Distance(ReadOnlySpan<float> x, ReadOnlySpan<float> y);

    public static float SoftMax(ReadOnlySpan<float> x);

    public static Span<float> Sigmoid(ReadOnlySpan<float> x, Span<float> destination);

    public static float Max(ReadOnlySpan<float> value);
    public static float Min(ReadOnlySpan<float> value);

    public static float MaxMagnitude(ReadOnlySpan<float> value);
    public static float MinMagnitude(ReadOnlySpan<float> value);

    public static int IndexOfMax(ReadOnlySpan<float> value);
    public static int IndexOfMin(ReadOnlySpan<float> value);

    public static int IndexOfMaxMagnitude(ReadOnlySpan<float> value);
    public static int IndexOfMinMagnitude(ReadOnlySpan<float> value);

    public static float Sum(ReadOnlySpan<float> value);
    public static float SumOfSquares(ReadOnlySpan<float> value);

    public static float Product(ReadOnlySpan<float> value);
    public static float ProductOfSums(ReadOnlySpan<float> x, ReadOnlySpan<float> y);
    public static float ProductOfDifferences(ReadOnlySpan<float> x, ReadOnlySpan<float> y);

    public static void ConvertToHalf(ReadOnlySpan<float> source, Span<Half> destination);
    public static void ConvertToSingle(ReadOnlySpan<Half> source, Span<float> destination);
}
```
