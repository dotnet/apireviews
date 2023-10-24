# API Review 10/24/2023

## Make OpCode.StackChange() public

**Approved** | [#runtime/93419](https://github.com/dotnet/runtime/issues/93419#issuecomment-1777684948) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Reflection.Emit

public readonly partial struct OpCode : System.IEquatable<System.Reflection.Emit.OpCode>
{
    public int EvaluationStackDelta { get }
}
```
## Add protected factory method for creating Label in ILGenerator, abstract out LocalBuilder

**Approved** | [#runtime/93497](https://github.com/dotnet/runtime/issues/93497#issuecomment-1777707890) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=0h10m40s)

There was discussion around `LocalBuilder.LocalMethod` being an abstract property or a value passed in via the constructor.  Since everything else in the hierarchy is abstract properties, it is locally consistent as proposed.

Since the method isn't a feature of the local, but instead describes the context where it exists, the property should be `Method` not `LocalMethod`

```diff
namespace System.Reflection.Emit;

-public sealed class LocalBuilder : System.Reflection.LocalVariableInfo
+public abstract class LocalBuilder : System.Reflection.LocalVariableInfo
{
-   internal LocalBuilder(int localIndex, Type localType, MethodInfo method, bool isPinned) { }
+   protected LocalBuilder() { }
+   public abstract MethodInfo Method { get; }
}

public abstract class ILGenerator
{
    protected ILGenerator() { }
    ...
    public abstract Label DefineLabel();
+   protected static Label CreateLabel(int id) { throw null; }
}

public readonly partial struct Label : System.IEquatable<System.Reflection.Emit.Label>
{
    ...
+   public int Id { get; }
}
```
## New TimeSpan.From overloads which take integers.

**Approved** | [#runtime/93890](https://github.com/dotnet/runtime/issues/93890#issuecomment-1777761585) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=0h24m40s)

* We discussed the edge cases of when FromDays(literal) might give a different value on a recompile (versus that number treated as a double).  Unless Jeremy did bad math, that is approximately at FromDays(67_108_864), which seems big enough to not be a concern.
* The methods should be overloaded to have no default parameters per our normal default parameters usability guideline
* We changed minutes and smaller to be `long` because their values can mathematically exceed `int.MaxValue` without exceeding `DateTime.MaxValue`

```C#
namespace System;

public partial struct TimeSpan
{
    public static TimeSpan FromDays(int days);
    public static TimeSpan FromDays(int days, int hours = 0, long minutes = 0, long seconds = 0, long milliseconds = 0, long  microseconds = 0);
    public static TimeSpan FromHours(int hours);
    public static TimeSpan FromHours(int hours, long minutes = 0, long seconds = 0, long milliseconds = 0, long microseconds = 0);
    public static TimeSpan FromMinutes(long minutes);
    public static TimeSpan FromMinutes(long minutes, long seconds = 0, long milliseconds = 0, long microseconds = 0);
    public static TimeSpan FromSeconds(long seconds);
    public static TimeSpan FromSeconds(long seconds, long milliseconds = 0, long microseconds = 0);
    public static TimeSpan FromMilliseconds(long milliseconds, long microseconds = 0);
    public static TimeSpan FromMicroseconds(long microseconds);
}
```
## Add a `Remove` method to `PriorityQueue`

**Approved** | [#runtime/93925](https://github.com/dotnet/runtime/issues/93925#issuecomment-1777792429) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=0h59m15s)

* We added an out for the removed element to forestall future requests for it.
* We added some nullability annotations.

```C#
namespace System.Collections.Generic;

public partial class PriorityQueue<TElement, TPriority>
{
    // Scans the heap for elements equal to the provided value and removes the first occurrence.
    public bool Remove(
        TElement element,
        [MaybeNullWhen(false)] out TElement removedElement,
        [MaybeNullWhen(false)] out TPriority priority,
        IEqualityComparer<TElement>? equalityComparer = null);
}
```
## add `JsonSerializerOptions.Web` for `JsonSerializerOptions`

**Approved** | [#runtime/92181](https://github.com/dotnet/runtime/issues/92181#issuecomment-1777811022) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=1h17m24s)

* We discussed several naming options (`Web`, `WebDefaults`, `DefaultWeb`, and `DefaultsForWeb`) and stuck with `Web` as straight parity from the enum.

```diff
namespace System.Text.Json;

public sealed partial class JsonSerializerOptions
{
    public static JsonSerializerOptions Default { get; }
+   public static JsonSerializerOptions Web { get; }
}
```
## Math float vectorized functions for Vector64/128/256/512 and  Vector2/3/4

**Approved** | [#runtime/93513](https://github.com/dotnet/runtime/issues/93513#issuecomment-1777827729) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=1h27m26s)

Looks good as proposed. Modulo some generic constraints specified on non-generic methods.

```C#
namespace System.Runtime.Intrinsics;

public static class Vector64
{
    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector64<float>>
    public static Vector64<float> Acosh(Vector64<float> x);
    public static Vector64<float> Asinh(Vector64<float> x);
    public static Vector64<float> Atanh(Vector64<float> x);
    public static Vector64<float> Cosh(Vector64<float> x);
    public static Vector64<float> Sinh(Vector64<float> x);
    public static Vector64<float> Tanh(Vector64<float> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector64<float>>
    public static Vector64<float> Acos(Vector64<float> x);
    public static Vector64<float> AcosPi(Vector64<float> x);
    public static Vector64<float> Asin(Vector64<float> x);
    public static Vector64<float> AsinPi(Vector64<float> x);
    public static Vector64<float> Atan(Vector64<float> x);
    public static Vector64<float> AtanPi(Vector64<float> x);
    public static Vector64<float> Cos(Vector64<float> x);
    public static Vector64<float> CosPi(Vector64<float> x);
    public static Vector64<float> DegreesToRadians(Vector64<float> degrees);
    public static Vector64<float> RadiansToDegrees(Vector64<float> radians);
    public static Vector64<float> Sin(Vector64<float> x);
    public static (Vector64<float> Sin, Vector64<float> Cos) SinCos(Vector64<float> x);
    public static (Vector64<float> SinPi, Vector64<float> CosPi) SinCosPi(Vector64<float> x);
    public static Vector64<float> SinPi(Vector64<float> x);
    public static Vector64<float> Tan(Vector64<float> x);
    public static Vector64<float> TanPi(Vector64<float> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector64<float>>
    public static Vector64<float> Log(Vector64<float> x);
    public static Vector64<float> Log(Vector64<float> x, Vector64<float> newBase);
    public static Vector64<float> LogP1(Vector64<float> x) => Log(x + Vector64<float>.One);
    public static Vector64<float> Log2(Vector64<float> x);
    public static Vector64<float> Log2P1(Vector64<float> x) => Log2(x + Vector64<float>.One);
    public static Vector64<float> Log10(Vector64<float> x);
    public static Vector64<float> Log10P1(Vector64<float> x) => Log10(x + Vector64<float>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector64<float>>
    public static Vector64<float> Exp(Vector64<float> x);
    public static Vector64<float> ExpM1(Vector64<float> x) => Exp(x) - Vector64<float>.One;
    public static Vector64<float> Exp2(Vector64<float> x);
    public static Vector64<float> Exp2M1(Vector64<float> x) => Exp2(x) - Vector64<float>.One;
    public static Vector64<float> Exp10(Vector64<float> x);
    public static Vector64<float> Exp10M1(Vector64<float> x) => Exp10(x) - Vector64<float>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector64<float>>
    public static Vector64<float> Pow(Vector64<float> x, Vector64<float> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector64<float>>
    public static Vector64<float> Cbrt(Vector64<float> x);
    public static Vector64<float> Hypot(Vector64<float> x, Vector64<float> y);
    public static Vector64<float> RootN(Vector64<float> x, int n);
    public static Vector64<float> Sqrt(Vector64<float> x);

    // IFloatingPoint<TSelf>
    public static Vector64<float> Round(Vector64<float> x);
    public static Vector64<float> Round(Vector64<float> x, int digits);
    public static Vector64<float> Round(Vector64<float> x, MidpointRounding mode);
    public static Vector64<float> Round(Vector64<float> x, int digits, MidpointRounding mode);
    public static Vector64<float> Truncate(Vector64<float> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector64<float> Atan2(Vector64<float> y, Vector64<float> x);
    public static Vector64<float> Atan2Pi(Vector64<float> y, Vector64<float> x);
    public static Vector64<float> BitDecrement(Vector64<float> x);
    public static Vector64<float> BitIncrement(Vector64<float> x);
    public static Vector64<float> FusedMultiplyAdd(Vector64<float> left, Vector64<float> right, Vector64<float> addend);
    public static Vector64<float> Lerp(Vector64<float> value1, Vector64<float> value2, Vector64<float> amount);
    public static Vector64<float> ReciprocalEstimate(Vector64<float> x);
    public static Vector64<float> ReciprocalSqrtEstimate(Vector64<float> x);

    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector64<double>>
    public static Vector64<double> Acosh(Vector64<double> x);
    public static Vector64<double> Asinh(Vector64<double> x);
    public static Vector64<double> Atanh(Vector64<double> x);
    public static Vector64<double> Cosh(Vector64<double> x);
    public static Vector64<double> Sinh(Vector64<double> x);
    public static Vector64<double> Tanh(Vector64<double> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector64<double>>
    public static Vector64<double> Acos(Vector64<double> x);
    public static Vector64<double> AcosPi(Vector64<double> x);
    public static Vector64<double> Asin(Vector64<double> x);
    public static Vector64<double> AsinPi(Vector64<double> x);
    public static Vector64<double> Atan(Vector64<double> x);
    public static Vector64<double> AtanPi(Vector64<double> x);
    public static Vector64<double> Cos(Vector64<double> x);
    public static Vector64<double> CosPi(Vector64<double> x);
    public static Vector64<double> DegreesToRadians(Vector64<double> degrees);
    public static Vector64<double> RadiansToDegrees(Vector64<double> radians);
    public static Vector64<double> Sin(Vector64<double> x);
    public static (Vector64<double> Sin, Vector64<double> Cos) SinCos(Vector64<double> x);
    public static (Vector64<double> SinPi, Vector64<double> CosPi) SinCosPi(Vector64<double> x);
    public static Vector64<double> SinPi(Vector64<double> x);
    public static Vector64<double> Tan(Vector64<double> x);
    public static Vector64<double> TanPi(Vector64<double> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector64<double>>
    public static Vector64<double> Log(Vector64<double> x);
    public static Vector64<double> Log(Vector64<double> x, Vector64<double> newBase);
    public static Vector64<double> LogP1(Vector64<double> x) => Log(x + Vector64<double>.One);
    public static Vector64<double> Log2(Vector64<double> x);
    public static Vector64<double> Log2P1(Vector64<double> x) => Log2(x + Vector64<double>.One);
    public static Vector64<double> Log10(Vector64<double> x);
    public static Vector64<double> Log10P1(Vector64<double> x) => Log10(x + Vector64<double>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector64<double>>
    public static Vector64<double> Exp(Vector64<double> x);
    public static Vector64<double> ExpM1(Vector64<double> x) => Exp(x) - Vector64<double>.One;
    public static Vector64<double> Exp2(Vector64<double> x);
    public static Vector64<double> Exp2M1(Vector64<double> x) => Exp2(x) - Vector64<double>.One;
    public static Vector64<double> Exp10(Vector64<double> x);
    public static Vector64<double> Exp10M1(Vector64<double> x) => Exp10(x) - Vector64<double>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector64<double>>
    public static Vector64<double> Pow(Vector64<double> x, Vector64<double> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector64<double>>
    public static Vector64<double> Cbrt(Vector64<double> x);
    public static Vector64<double> Hypot(Vector64<double> x, Vector64<double> y);
    public static Vector64<double> RootN(Vector64<double> x, int n);
    public static Vector64<double> Sqrt(Vector64<double> x);

    // IFloatingPoint<TSelf>
    public static Vector64<double> Round(Vector64<double> x);
    public static Vector64<double> Round(Vector64<double> x, int digits);
    public static Vector64<double> Round(Vector64<double> x, MidpointRounding mode);
    public static Vector64<double> Round(Vector64<double> x, int digits, MidpointRounding mode);
    public static Vector64<double> Truncate(Vector64<double> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector64<double> Atan2(Vector64<double> y, Vector64<double> x);
    public static Vector64<double> Atan2Pi(Vector64<double> y, Vector64<double> x);
    public static Vector64<double> BitDecrement(Vector64<double> x);
    public static Vector64<double> BitIncrement(Vector64<double> x);
    public static Vector64<double> FusedMultiplyAdd(Vector64<double> left, Vector64<double> right, Vector64<double> addend);
    public static Vector64<double> Lerp(Vector64<double> value1, Vector64<double> value2, Vector64<double> amount);
    public static Vector64<double> ReciprocalEstimate(Vector64<double> x);
    public static Vector64<double> ReciprocalSqrtEstimate(Vector64<double> x);

    // INumber<T> -- these also work for integer types
    public static Vector64<T> Clamp<T>(Vector64<T> value, Vector64<T> min, Vector64<T> max) where T: unmanaged;
    public static Vector64<T> CopySign<T>(Vector64<T> value, Vector64<T> sign) where T: unmanaged;
    public static Vector64<T> MaxNumber<T>(Vector64<T> x, Vector64<T> y) where T: unmanaged;
    public static Vector64<T> MinNumber<T>(Vector64<T> x, Vector64<T> y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector64<T> MaxMagnitude<T>(Vector64<T> x, Vector64<T> y) where T: unmanaged;
    public static Vector64<T> MaxMagnitudeNumber<T>(Vector64<T> x, Vector64<T> y) where T: unmanaged;
    public static Vector64<T> MinMagnitude<T>(Vector64<T> x, Vector64<T> y) where T: unmanaged;
    public static Vector64<T> MinMagnitudeNumber<T>(Vector64<T> x, Vector64<T> y) where T: unmanaged; 
}

namespace System.Runtime.Intrinsics;

public static class Vector128
{
    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector128<float>>
    public static Vector128<float> Acosh(Vector128<float> x);
    public static Vector128<float> Asinh(Vector128<float> x);
    public static Vector128<float> Atanh(Vector128<float> x);
    public static Vector128<float> Cosh(Vector128<float> x);
    public static Vector128<float> Sinh(Vector128<float> x);
    public static Vector128<float> Tanh(Vector128<float> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector128<float>>
    public static Vector128<float> Acos(Vector128<float> x);
    public static Vector128<float> AcosPi(Vector128<float> x);
    public static Vector128<float> Asin(Vector128<float> x);
    public static Vector128<float> AsinPi(Vector128<float> x);
    public static Vector128<float> Atan(Vector128<float> x);
    public static Vector128<float> AtanPi(Vector128<float> x);
    public static Vector128<float> Cos(Vector128<float> x);
    public static Vector128<float> CosPi(Vector128<float> x);
    public static Vector128<float> DegreesToRadians(Vector128<float> degrees);
    public static Vector128<float> RadiansToDegrees(Vector128<float> radians);
    public static Vector128<float> Sin(Vector128<float> x);
    public static (Vector128<float> Sin, Vector128<float> Cos) SinCos(Vector128<float> x);
    public static (Vector128<float> SinPi, Vector128<float> CosPi) SinCosPi(Vector128<float> x);
    public static Vector128<float> SinPi(Vector128<float> x);
    public static Vector128<float> Tan(Vector128<float> x);
    public static Vector128<float> TanPi(Vector128<float> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector128<float>>
    public static Vector128<float> Log(Vector128<float> x);
    public static Vector128<float> Log(Vector128<float> x, Vector128<float> newBase);
    public static Vector128<float> LogP1(Vector128<float> x) => Log(x + Vector128<float>.One);
    public static Vector128<float> Log2(Vector128<float> x);
    public static Vector128<float> Log2P1(Vector128<float> x) => Log2(x + Vector128<float>.One);
    public static Vector128<float> Log10(Vector128<float> x);
    public static Vector128<float> Log10P1(Vector128<float> x) => Log10(x + Vector128<float>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector128<float>>
    public static Vector128<float> Exp(Vector128<float> x);
    public static Vector128<float> ExpM1(Vector128<float> x) => Exp(x) - Vector128<float>.One;
    public static Vector128<float> Exp2(Vector128<float> x);
    public static Vector128<float> Exp2M1(Vector128<float> x) => Exp2(x) - Vector128<float>.One;
    public static Vector128<float> Exp10(Vector128<float> x);
    public static Vector128<float> Exp10M1(Vector128<float> x) => Exp10(x) - Vector128<float>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector128<float>>
    public static Vector128<float> Pow(Vector128<float> x, Vector128<float> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector128<float>>
    public static Vector128<float> Cbrt(Vector128<float> x);
    public static Vector128<float> Hypot(Vector128<float> x, Vector128<float> y);
    public static Vector128<float> RootN(Vector128<float> x, int n);
    public static Vector128<float> Sqrt(Vector128<float> x);

    // IFloatingPoint<TSelf>
    public static Vector128<float> Round(Vector128<float> x);
    public static Vector128<float> Round(Vector128<float> x, int digits);
    public static Vector128<float> Round(Vector128<float> x, MidpointRounding mode);
    public static Vector128<float> Round(Vector128<float> x, int digits, MidpointRounding mode);
    public static Vector128<float> Truncate(Vector128<float> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector128<float> Atan2(Vector128<float> y, Vector128<float> x);
    public static Vector128<float> Atan2Pi(Vector128<float> y, Vector128<float> x);
    public static Vector128<float> BitDecrement(Vector128<float> x);
    public static Vector128<float> BitIncrement(Vector128<float> x);
    public static Vector128<float> FusedMultiplyAdd(Vector128<float> left, Vector128<float> right, Vector128<float> addend);
    public static Vector128<float> Lerp(Vector128<float> value1, Vector128<float> value2, Vector128<float> amount);
    public static Vector128<float> ReciprocalEstimate(Vector128<float> x);
    public static Vector128<float> ReciprocalSqrtEstimate(Vector128<float> x);

    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector128<double>>
    public static Vector128<double> Acosh(Vector128<double> x);
    public static Vector128<double> Asinh(Vector128<double> x);
    public static Vector128<double> Atanh(Vector128<double> x);
    public static Vector128<double> Cosh(Vector128<double> x);
    public static Vector128<double> Sinh(Vector128<double> x);
    public static Vector128<double> Tanh(Vector128<double> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector128<double>>
    public static Vector128<double> Acos(Vector128<double> x);
    public static Vector128<double> AcosPi(Vector128<double> x);
    public static Vector128<double> Asin(Vector128<double> x);
    public static Vector128<double> AsinPi(Vector128<double> x);
    public static Vector128<double> Atan(Vector128<double> x);
    public static Vector128<double> AtanPi(Vector128<double> x);
    public static Vector128<double> Cos(Vector128<double> x);
    public static Vector128<double> CosPi(Vector128<double> x);
    public static Vector128<double> DegreesToRadians(Vector128<double> degrees);
    public static Vector128<double> RadiansToDegrees(Vector128<double> radians);
    public static Vector128<double> Sin(Vector128<double> x);
    public static (Vector128<double> Sin, Vector128<double> Cos) SinCos(Vector128<double> x);
    public static (Vector128<double> SinPi, Vector128<double> CosPi) SinCosPi(Vector128<double> x);
    public static Vector128<double> SinPi(Vector128<double> x);
    public static Vector128<double> Tan(Vector128<double> x);
    public static Vector128<double> TanPi(Vector128<double> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector128<double>>
    public static Vector128<double> Log(Vector128<double> x);
    public static Vector128<double> Log(Vector128<double> x, Vector128<double> newBase);
    public static Vector128<double> LogP1(Vector128<double> x) => Log(x + Vector128<double>.One);
    public static Vector128<double> Log2(Vector128<double> x);
    public static Vector128<double> Log2P1(Vector128<double> x) => Log2(x + Vector128<double>.One);
    public static Vector128<double> Log10(Vector128<double> x);
    public static Vector128<double> Log10P1(Vector128<double> x) => Log10(x + Vector128<double>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector128<double>>
    public static Vector128<double> Exp(Vector128<double> x);
    public static Vector128<double> ExpM1(Vector128<double> x) => Exp(x) - Vector128<double>.One;
    public static Vector128<double> Exp2(Vector128<double> x);
    public static Vector128<double> Exp2M1(Vector128<double> x) => Exp2(x) - Vector128<double>.One;
    public static Vector128<double> Exp10(Vector128<double> x);
    public static Vector128<double> Exp10M1(Vector128<double> x) => Exp10(x) - Vector128<double>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector128<double>>
    public static Vector128<double> Pow(Vector128<double> x, Vector128<double> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector128<double>>
    public static Vector128<double> Cbrt(Vector128<double> x);
    public static Vector128<double> Hypot(Vector128<double> x, Vector128<double> y);
    public static Vector128<double> RootN(Vector128<double> x, int n);
    public static Vector128<double> Sqrt(Vector128<double> x);

    // IFloatingPoint<TSelf>
    public static Vector128<double> Round(Vector128<double> x);
    public static Vector128<double> Round(Vector128<double> x, int digits);
    public static Vector128<double> Round(Vector128<double> x, MidpointRounding mode);
    public static Vector128<double> Round(Vector128<double> x, int digits, MidpointRounding mode);
    public static Vector128<double> Truncate(Vector128<double> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector128<double> Atan2(Vector128<double> y, Vector128<double> x);
    public static Vector128<double> Atan2Pi(Vector128<double> y, Vector128<double> x);
    public static Vector128<double> BitDecrement(Vector128<double> x);
    public static Vector128<double> BitIncrement(Vector128<double> x);
    public static Vector128<double> FusedMultiplyAdd(Vector128<double> left, Vector128<double> right, Vector128<double> addend);
    public static Vector128<double> Lerp(Vector128<double> value1, Vector128<double> value2, Vector128<double> amount);
    public static Vector128<double> ReciprocalEstimate(Vector128<double> x);
    public static Vector128<double> ReciprocalSqrtEstimate(Vector128<double> x);

    // INumber<T> -- these also work for integer types
    public static Vector128<T> Clamp<T>(Vector128<T> value, Vector128<T> min, Vector128<T> max) where T: unmanaged;
    public static Vector128<T> CopySign<T>(Vector128<T> value, Vector128<T> sign) where T: unmanaged;
    public static Vector128<T> MaxNumber<T>(Vector128<T> x, Vector128<T> y) where T: unmanaged;
    public static Vector128<T> MinNumber<T>(Vector128<T> x, Vector128<T> y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector128<T> MaxMagnitude<T>(Vector128<T> x, Vector128<T> y) where T: unmanaged;
    public static Vector128<T> MaxMagnitudeNumber<T>(Vector128<T> x, Vector128<T> y) where T: unmanaged;
    public static Vector128<T> MinMagnitude<T>(Vector128<T> x, Vector128<T> y) where T: unmanaged;
    public static Vector128<T> MinMagnitudeNumber<T>(Vector128<T> x, Vector128<T> y) where T: unmanaged; 
}

namespace System.Runtime.Intrinsics;

public static class Vector256
{
    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector256<float>>
    public static Vector256<float> Acosh(Vector256<float> x);
    public static Vector256<float> Asinh(Vector256<float> x);
    public static Vector256<float> Atanh(Vector256<float> x);
    public static Vector256<float> Cosh(Vector256<float> x);
    public static Vector256<float> Sinh(Vector256<float> x);
    public static Vector256<float> Tanh(Vector256<float> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector256<float>>
    public static Vector256<float> Acos(Vector256<float> x);
    public static Vector256<float> AcosPi(Vector256<float> x);
    public static Vector256<float> Asin(Vector256<float> x);
    public static Vector256<float> AsinPi(Vector256<float> x);
    public static Vector256<float> Atan(Vector256<float> x);
    public static Vector256<float> AtanPi(Vector256<float> x);
    public static Vector256<float> Cos(Vector256<float> x);
    public static Vector256<float> CosPi(Vector256<float> x);
    public static Vector256<float> DegreesToRadians(Vector256<float> degrees);
    public static Vector256<float> RadiansToDegrees(Vector256<float> radians);
    public static Vector256<float> Sin(Vector256<float> x);
    public static (Vector256<float> Sin, Vector256<float> Cos) SinCos(Vector256<float> x);
    public static (Vector256<float> SinPi, Vector256<float> CosPi) SinCosPi(Vector256<float> x);
    public static Vector256<float> SinPi(Vector256<float> x);
    public static Vector256<float> Tan(Vector256<float> x);
    public static Vector256<float> TanPi(Vector256<float> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector256<float>>
    public static Vector256<float> Log(Vector256<float> x);
    public static Vector256<float> Log(Vector256<float> x, Vector256<float> newBase);
    public static Vector256<float> LogP1(Vector256<float> x) => Log(x + Vector256<float>.One);
    public static Vector256<float> Log2(Vector256<float> x);
    public static Vector256<float> Log2P1(Vector256<float> x) => Log2(x + Vector256<float>.One);
    public static Vector256<float> Log10(Vector256<float> x);
    public static Vector256<float> Log10P1(Vector256<float> x) => Log10(x + Vector256<float>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector256<float>>
    public static Vector256<float> Exp(Vector256<float> x);
    public static Vector256<float> ExpM1(Vector256<float> x) => Exp(x) - Vector256<float>.One;
    public static Vector256<float> Exp2(Vector256<float> x);
    public static Vector256<float> Exp2M1(Vector256<float> x) => Exp2(x) - Vector256<float>.One;
    public static Vector256<float> Exp10(Vector256<float> x);
    public static Vector256<float> Exp10M1(Vector256<float> x) => Exp10(x) - Vector256<float>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector256<float>>
    public static Vector256<float> Pow(Vector256<float> x, Vector256<float> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector256<float>>
    public static Vector256<float> Cbrt(Vector256<float> x);
    public static Vector256<float> Hypot(Vector256<float> x, Vector256<float> y);
    public static Vector256<float> RootN(Vector256<float> x, int n);
    public static Vector256<float> Sqrt(Vector256<float> x);

    // IFloatingPoint<TSelf>
    public static Vector256<float> Round(Vector256<float> x);
    public static Vector256<float> Round(Vector256<float> x, int digits);
    public static Vector256<float> Round(Vector256<float> x, MidpointRounding mode);
    public static Vector256<float> Round(Vector256<float> x, int digits, MidpointRounding mode);
    public static Vector256<float> Truncate(Vector256<float> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector256<float> Atan2(Vector256<float> y, Vector256<float> x);
    public static Vector256<float> Atan2Pi(Vector256<float> y, Vector256<float> x);
    public static Vector256<float> BitDecrement(Vector256<float> x);
    public static Vector256<float> BitIncrement(Vector256<float> x);
    public static Vector256<float> FusedMultiplyAdd(Vector256<float> left, Vector256<float> right, Vector256<float> addend);
    public static Vector256<float> Lerp(Vector256<float> value1, Vector256<float> value2, Vector256<float> amount);
    public static Vector256<float> ReciprocalEstimate(Vector256<float> x);
    public static Vector256<float> ReciprocalSqrtEstimate(Vector256<float> x);

    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector256<double>>
    public static Vector256<double> Acosh(Vector256<double> x);
    public static Vector256<double> Asinh(Vector256<double> x);
    public static Vector256<double> Atanh(Vector256<double> x);
    public static Vector256<double> Cosh(Vector256<double> x);
    public static Vector256<double> Sinh(Vector256<double> x);
    public static Vector256<double> Tanh(Vector256<double> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector256<double>>
    public static Vector256<double> Acos(Vector256<double> x);
    public static Vector256<double> AcosPi(Vector256<double> x);
    public static Vector256<double> Asin(Vector256<double> x);
    public static Vector256<double> AsinPi(Vector256<double> x);
    public static Vector256<double> Atan(Vector256<double> x);
    public static Vector256<double> AtanPi(Vector256<double> x);
    public static Vector256<double> Cos(Vector256<double> x);
    public static Vector256<double> CosPi(Vector256<double> x);
    public static Vector256<double> DegreesToRadians(Vector256<double> degrees);
    public static Vector256<double> RadiansToDegrees(Vector256<double> radians);
    public static Vector256<double> Sin(Vector256<double> x);
    public static (Vector256<double> Sin, Vector256<double> Cos) SinCos(Vector256<double> x);
    public static (Vector256<double> SinPi, Vector256<double> CosPi) SinCosPi(Vector256<double> x);
    public static Vector256<double> SinPi(Vector256<double> x);
    public static Vector256<double> Tan(Vector256<double> x);
    public static Vector256<double> TanPi(Vector256<double> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector256<double>>
    public static Vector256<double> Log(Vector256<double> x);
    public static Vector256<double> Log(Vector256<double> x, Vector256<double> newBase);
    public static Vector256<double> LogP1(Vector256<double> x) => Log(x + Vector256<double>.One);
    public static Vector256<double> Log2(Vector256<double> x);
    public static Vector256<double> Log2P1(Vector256<double> x) => Log2(x + Vector256<double>.One);
    public static Vector256<double> Log10(Vector256<double> x);
    public static Vector256<double> Log10P1(Vector256<double> x) => Log10(x + Vector256<double>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector256<double>>
    public static Vector256<double> Exp(Vector256<double> x);
    public static Vector256<double> ExpM1(Vector256<double> x) => Exp(x) - Vector256<double>.One;
    public static Vector256<double> Exp2(Vector256<double> x);
    public static Vector256<double> Exp2M1(Vector256<double> x) => Exp2(x) - Vector256<double>.One;
    public static Vector256<double> Exp10(Vector256<double> x);
    public static Vector256<double> Exp10M1(Vector256<double> x) => Exp10(x) - Vector256<double>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector256<double>>
    public static Vector256<double> Pow(Vector256<double> x, Vector256<double> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector256<double>>
    public static Vector256<double> Cbrt(Vector256<double> x);
    public static Vector256<double> Hypot(Vector256<double> x, Vector256<double> y);
    public static Vector256<double> RootN(Vector256<double> x, int n);
    public static Vector256<double> Sqrt(Vector256<double> x);

    // IFloatingPoint<TSelf>
    public static Vector256<double> Round(Vector256<double> x);
    public static Vector256<double> Round(Vector256<double> x, int digits);
    public static Vector256<double> Round(Vector256<double> x, MidpointRounding mode);
    public static Vector256<double> Round(Vector256<double> x, int digits, MidpointRounding mode);
    public static Vector256<double> Truncate(Vector256<double> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector256<double> Atan2(Vector256<double> y, Vector256<double> x);
    public static Vector256<double> Atan2Pi(Vector256<double> y, Vector256<double> x);
    public static Vector256<double> BitDecrement(Vector256<double> x);
    public static Vector256<double> BitIncrement(Vector256<double> x);
    public static Vector256<double> FusedMultiplyAdd(Vector256<double> left, Vector256<double> right, Vector256<double> addend);
    public static Vector256<double> Lerp(Vector256<double> value1, Vector256<double> value2, Vector256<double> amount);
    public static Vector256<double> ReciprocalEstimate(Vector256<double> x);
    public static Vector256<double> ReciprocalSqrtEstimate(Vector256<double> x);

    // INumber<T> -- these also work for integer types
    public static Vector256<T> Clamp<T>(Vector256<T> value, Vector256<T> min, Vector256<T> max) where T: unmanaged;
    public static Vector256<T> CopySign<T>(Vector256<T> value, Vector256<T> sign) where T: unmanaged;
    public static Vector256<T> MaxNumber<T>(Vector256<T> x, Vector256<T> y) where T: unmanaged;
    public static Vector256<T> MinNumber<T>(Vector256<T> x, Vector256<T> y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector256<T> MaxMagnitude<T>(Vector256<T> x, Vector256<T> y) where T: unmanaged;
    public static Vector256<T> MaxMagnitudeNumber<T>(Vector256<T> x, Vector256<T> y) where T: unmanaged;
    public static Vector256<T> MinMagnitude<T>(Vector256<T> x, Vector256<T> y) where T: unmanaged;
    public static Vector256<T> MinMagnitudeNumber<T>(Vector256<T> x, Vector256<T> y) where T: unmanaged; 
}

namespace System.Runtime.Intrinsics;

public static class Vector512
{
    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector512<float>>
    public static Vector512<float> Acosh(Vector512<float> x);
    public static Vector512<float> Asinh(Vector512<float> x);
    public static Vector512<float> Atanh(Vector512<float> x);
    public static Vector512<float> Cosh(Vector512<float> x);
    public static Vector512<float> Sinh(Vector512<float> x);
    public static Vector512<float> Tanh(Vector512<float> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector512<float>>
    public static Vector512<float> Acos(Vector512<float> x);
    public static Vector512<float> AcosPi(Vector512<float> x);
    public static Vector512<float> Asin(Vector512<float> x);
    public static Vector512<float> AsinPi(Vector512<float> x);
    public static Vector512<float> Atan(Vector512<float> x);
    public static Vector512<float> AtanPi(Vector512<float> x);
    public static Vector512<float> Cos(Vector512<float> x);
    public static Vector512<float> CosPi(Vector512<float> x);
    public static Vector512<float> DegreesToRadians(Vector512<float> degrees);
    public static Vector512<float> RadiansToDegrees(Vector512<float> radians);
    public static Vector512<float> Sin(Vector512<float> x);
    public static (Vector512<float> Sin, Vector512<float> Cos) SinCos(Vector512<float> x);
    public static (Vector512<float> SinPi, Vector512<float> CosPi) SinCosPi(Vector512<float> x);
    public static Vector512<float> SinPi(Vector512<float> x);
    public static Vector512<float> Tan(Vector512<float> x);
    public static Vector512<float> TanPi(Vector512<float> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector512<float>>
    public static Vector512<float> Log(Vector512<float> x);
    public static Vector512<float> Log(Vector512<float> x, Vector512<float> newBase);
    public static Vector512<float> LogP1(Vector512<float> x) => Log(x + Vector512<float>.One);
    public static Vector512<float> Log2(Vector512<float> x);
    public static Vector512<float> Log2P1(Vector512<float> x) => Log2(x + Vector512<float>.One);
    public static Vector512<float> Log10(Vector512<float> x);
    public static Vector512<float> Log10P1(Vector512<float> x) => Log10(x + Vector512<float>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector512<float>>
    public static Vector512<float> Exp(Vector512<float> x);
    public static Vector512<float> ExpM1(Vector512<float> x) => Exp(x) - Vector512<float>.One;
    public static Vector512<float> Exp2(Vector512<float> x);
    public static Vector512<float> Exp2M1(Vector512<float> x) => Exp2(x) - Vector512<float>.One;
    public static Vector512<float> Exp10(Vector512<float> x);
    public static Vector512<float> Exp10M1(Vector512<float> x) => Exp10(x) - Vector512<float>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector512<float>>
    public static Vector512<float> Pow(Vector512<float> x, Vector512<float> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector512<float>>
    public static Vector512<float> Cbrt(Vector512<float> x);
    public static Vector512<float> Hypot(Vector512<float> x, Vector512<float> y);
    public static Vector512<float> RootN(Vector512<float> x, int n);
    public static Vector512<float> Sqrt(Vector512<float> x);

    // IFloatingPoint<TSelf>
    public static Vector512<float> Round(Vector512<float> x);
    public static Vector512<float> Round(Vector512<float> x, int digits);
    public static Vector512<float> Round(Vector512<float> x, MidpointRounding mode);
    public static Vector512<float> Round(Vector512<float> x, int digits, MidpointRounding mode);
    public static Vector512<float> Truncate(Vector512<float> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector512<float> Atan2(Vector512<float> y, Vector512<float> x);
    public static Vector512<float> Atan2Pi(Vector512<float> y, Vector512<float> x);
    public static Vector512<float> BitDecrement(Vector512<float> x);
    public static Vector512<float> BitIncrement(Vector512<float> x);
    public static Vector512<float> FusedMultiplyAdd(Vector512<float> left, Vector512<float> right, Vector512<float> addend);
    public static Vector512<float> Lerp(Vector512<float> value1, Vector512<float> value2, Vector512<float> amount);
    public static Vector512<float> ReciprocalEstimate(Vector512<float> x);
    public static Vector512<float> ReciprocalSqrtEstimate(Vector512<float> x);

    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector512<double>>
    public static Vector512<double> Acosh(Vector512<double> x);
    public static Vector512<double> Asinh(Vector512<double> x);
    public static Vector512<double> Atanh(Vector512<double> x);
    public static Vector512<double> Cosh(Vector512<double> x);
    public static Vector512<double> Sinh(Vector512<double> x);
    public static Vector512<double> Tanh(Vector512<double> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector512<double>>
    public static Vector512<double> Acos(Vector512<double> x);
    public static Vector512<double> AcosPi(Vector512<double> x);
    public static Vector512<double> Asin(Vector512<double> x);
    public static Vector512<double> AsinPi(Vector512<double> x);
    public static Vector512<double> Atan(Vector512<double> x);
    public static Vector512<double> AtanPi(Vector512<double> x);
    public static Vector512<double> Cos(Vector512<double> x);
    public static Vector512<double> CosPi(Vector512<double> x);
    public static Vector512<double> DegreesToRadians(Vector512<double> degrees);
    public static Vector512<double> RadiansToDegrees(Vector512<double> radians);
    public static Vector512<double> Sin(Vector512<double> x);
    public static (Vector512<double> Sin, Vector512<double> Cos) SinCos(Vector512<double> x);
    public static (Vector512<double> SinPi, Vector512<double> CosPi) SinCosPi(Vector512<double> x);
    public static Vector512<double> SinPi(Vector512<double> x);
    public static Vector512<double> Tan(Vector512<double> x);
    public static Vector512<double> TanPi(Vector512<double> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector512<double>>
    public static Vector512<double> Log(Vector512<double> x);
    public static Vector512<double> Log(Vector512<double> x, Vector512<double> newBase);
    public static Vector512<double> LogP1(Vector512<double> x) => Log(x + Vector512<double>.One);
    public static Vector512<double> Log2(Vector512<double> x);
    public static Vector512<double> Log2P1(Vector512<double> x) => Log2(x + Vector512<double>.One);
    public static Vector512<double> Log10(Vector512<double> x);
    public static Vector512<double> Log10P1(Vector512<double> x) => Log10(x + Vector512<double>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector512<double>>
    public static Vector512<double> Exp(Vector512<double> x);
    public static Vector512<double> ExpM1(Vector512<double> x) => Exp(x) - Vector512<double>.One;
    public static Vector512<double> Exp2(Vector512<double> x);
    public static Vector512<double> Exp2M1(Vector512<double> x) => Exp2(x) - Vector512<double>.One;
    public static Vector512<double> Exp10(Vector512<double> x);
    public static Vector512<double> Exp10M1(Vector512<double> x) => Exp10(x) - Vector512<double>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector512<double>>
    public static Vector512<double> Pow(Vector512<double> x, Vector512<double> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector512<double>>
    public static Vector512<double> Cbrt(Vector512<double> x);
    public static Vector512<double> Hypot(Vector512<double> x, Vector512<double> y);
    public static Vector512<double> RootN(Vector512<double> x, int n);
    public static Vector512<double> Sqrt(Vector512<double> x);

    // IFloatingPoint<TSelf>
    public static Vector512<double> Round(Vector512<double> x);
    public static Vector512<double> Round(Vector512<double> x, int digits);
    public static Vector512<double> Round(Vector512<double> x, MidpointRounding mode);
    public static Vector512<double> Round(Vector512<double> x, int digits, MidpointRounding mode);
    public static Vector512<double> Truncate(Vector512<double> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector512<double> Atan2(Vector512<double> y, Vector512<double> x);
    public static Vector512<double> Atan2Pi(Vector512<double> y, Vector512<double> x);
    public static Vector512<double> BitDecrement(Vector512<double> x);
    public static Vector512<double> BitIncrement(Vector512<double> x);
    public static Vector512<double> FusedMultiplyAdd(Vector512<double> left, Vector512<double> right, Vector512<double> addend);
    public static Vector512<double> Lerp(Vector512<double> value1, Vector512<double> value2, Vector512<double> amount);
    public static Vector512<double> ReciprocalEstimate(Vector512<double> x);
    public static Vector512<double> ReciprocalSqrtEstimate(Vector512<double> x);

    // INumber<T> -- these also work for integer types
    public static Vector512<T> Clamp<T>(Vector512<T> value, Vector512<T> min, Vector512<T> max) where T: unmanaged;
    public static Vector512<T> CopySign<T>(Vector512<T> value, Vector512<T> sign) where T: unmanaged;
    public static Vector512<T> MaxNumber<T>(Vector512<T> x, Vector512<T> y) where T: unmanaged;
    public static Vector512<T> MinNumber<T>(Vector512<T> x, Vector512<T> y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector512<T> MaxMagnitude<T>(Vector512<T> x, Vector512<T> y) where T: unmanaged;
    public static Vector512<T> MaxMagnitudeNumber<T>(Vector512<T> x, Vector512<T> y) where T: unmanaged;
    public static Vector512<T> MinMagnitude<T>(Vector512<T> x, Vector512<T> y) where T: unmanaged;
    public static Vector512<T> MinMagnitudeNumber<T>(Vector512<T> x, Vector512<T> y) where T: unmanaged; 
}

namespace System.Runtime.Intrinsics;

public static class Vector
{
    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector<float>>
    public static Vector<float> Acosh(Vector<float> x);
    public static Vector<float> Asinh(Vector<float> x);
    public static Vector<float> Atanh(Vector<float> x);
    public static Vector<float> Cosh(Vector<float> x);
    public static Vector<float> Sinh(Vector<float> x);
    public static Vector<float> Tanh(Vector<float> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector<float>>
    public static Vector<float> Acos(Vector<float> x);
    public static Vector<float> AcosPi(Vector<float> x);
    public static Vector<float> Asin(Vector<float> x);
    public static Vector<float> AsinPi(Vector<float> x);
    public static Vector<float> Atan(Vector<float> x);
    public static Vector<float> AtanPi(Vector<float> x);
    public static Vector<float> Cos(Vector<float> x);
    public static Vector<float> CosPi(Vector<float> x);
    public static Vector<float> DegreesToRadians(Vector<float> degrees);
    public static Vector<float> RadiansToDegrees(Vector<float> radians);
    public static Vector<float> Sin(Vector<float> x);
    public static (Vector<float> Sin, Vector<float> Cos) SinCos(Vector<float> x);
    public static (Vector<float> SinPi, Vector<float> CosPi) SinCosPi(Vector<float> x);
    public static Vector<float> SinPi(Vector<float> x);
    public static Vector<float> Tan(Vector<float> x);
    public static Vector<float> TanPi(Vector<float> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector<float>>
    public static Vector<float> Log(Vector<float> x);
    public static Vector<float> Log(Vector<float> x, Vector<float> newBase);
    public static Vector<float> LogP1(Vector<float> x) => Log(x + Vector<float>.One);
    public static Vector<float> Log2(Vector<float> x);
    public static Vector<float> Log2P1(Vector<float> x) => Log2(x + Vector<float>.One);
    public static Vector<float> Log10(Vector<float> x);
    public static Vector<float> Log10P1(Vector<float> x) => Log10(x + Vector<float>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector<float>>
    public static Vector<float> Exp(Vector<float> x);
    public static Vector<float> ExpM1(Vector<float> x) => Exp(x) - Vector<float>.One;
    public static Vector<float> Exp2(Vector<float> x);
    public static Vector<float> Exp2M1(Vector<float> x) => Exp2(x) - Vector<float>.One;
    public static Vector<float> Exp10(Vector<float> x);
    public static Vector<float> Exp10M1(Vector<float> x) => Exp10(x) - Vector<float>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector<float>>
    public static Vector<float> Pow(Vector<float> x, Vector<float> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector<float>>
    public static Vector<float> Cbrt(Vector<float> x);
    public static Vector<float> Hypot(Vector<float> x, Vector<float> y);
    public static Vector<float> RootN(Vector<float> x, int n);
    public static Vector<float> Sqrt(Vector<float> x);

    // IFloatingPoint<TSelf>
    public static Vector<float> Round(Vector<float> x);
    public static Vector<float> Round(Vector<float> x, int digits);
    public static Vector<float> Round(Vector<float> x, MidpointRounding mode);
    public static Vector<float> Round(Vector<float> x, int digits, MidpointRounding mode);
    public static Vector<float> Truncate(Vector<float> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector<float> Atan2(Vector<float> y, Vector<float> x);
    public static Vector<float> Atan2Pi(Vector<float> y, Vector<float> x);
    public static Vector<float> BitDecrement(Vector<float> x);
    public static Vector<float> BitIncrement(Vector<float> x);
    public static Vector<float> FusedMultiplyAdd(Vector<float> left, Vector<float> right, Vector<float> addend);
    public static Vector<float> Lerp(Vector<float> value1, Vector<float> value2, Vector<float> amount);
    public static Vector<float> ReciprocalEstimate(Vector<float> x);
    public static Vector<float> ReciprocalSqrtEstimate(Vector<float> x);

    // Equivalent implementing IHyperbolicFunctions<System.Runtime.Intrinsics.Vector<double>>
    public static Vector<double> Acosh(Vector<double> x);
    public static Vector<double> Asinh(Vector<double> x);
    public static Vector<double> Atanh(Vector<double> x);
    public static Vector<double> Cosh(Vector<double> x);
    public static Vector<double> Sinh(Vector<double> x);
    public static Vector<double> Tanh(Vector<double> x);

    // Equivalent implementing ITrigonometricFunctions<System.Runtime.Intrinsics.Vector<double>>
    public static Vector<double> Acos(Vector<double> x);
    public static Vector<double> AcosPi(Vector<double> x);
    public static Vector<double> Asin(Vector<double> x);
    public static Vector<double> AsinPi(Vector<double> x);
    public static Vector<double> Atan(Vector<double> x);
    public static Vector<double> AtanPi(Vector<double> x);
    public static Vector<double> Cos(Vector<double> x);
    public static Vector<double> CosPi(Vector<double> x);
    public static Vector<double> DegreesToRadians(Vector<double> degrees);
    public static Vector<double> RadiansToDegrees(Vector<double> radians);
    public static Vector<double> Sin(Vector<double> x);
    public static (Vector<double> Sin, Vector<double> Cos) SinCos(Vector<double> x);
    public static (Vector<double> SinPi, Vector<double> CosPi) SinCosPi(Vector<double> x);
    public static Vector<double> SinPi(Vector<double> x);
    public static Vector<double> Tan(Vector<double> x);
    public static Vector<double> TanPi(Vector<double> x);

    // Equivalent implementing ILogarithmicFunctions<System.Runtime.Intrinsics.Vector<double>>
    public static Vector<double> Log(Vector<double> x);
    public static Vector<double> Log(Vector<double> x, Vector<double> newBase);
    public static Vector<double> LogP1(Vector<double> x) => Log(x + Vector<double>.One);
    public static Vector<double> Log2(Vector<double> x);
    public static Vector<double> Log2P1(Vector<double> x) => Log2(x + Vector<double>.One);
    public static Vector<double> Log10(Vector<double> x);
    public static Vector<double> Log10P1(Vector<double> x) => Log10(x + Vector<double>.One);

    // Equivalent implementing IExponentialFunctions<System.Runtime.Intrinsics.Vector<double>>
    public static Vector<double> Exp(Vector<double> x);
    public static Vector<double> ExpM1(Vector<double> x) => Exp(x) - Vector<double>.One;
    public static Vector<double> Exp2(Vector<double> x);
    public static Vector<double> Exp2M1(Vector<double> x) => Exp2(x) - Vector<double>.One;
    public static Vector<double> Exp10(Vector<double> x);
    public static Vector<double> Exp10M1(Vector<double> x) => Exp10(x) - Vector<double>.One;

    // Equivalent implementing IPowerFunctions<System.Runtime.Intrinsics.Vector<double>>
    public static Vector<double> Pow(Vector<double> x, Vector<double> y);

    // Equivalent implementing IRootFunctions<System.Runtime.Intrinsics.Vector<double>>
    public static Vector<double> Cbrt(Vector<double> x);
    public static Vector<double> Hypot(Vector<double> x, Vector<double> y);
    public static Vector<double> RootN(Vector<double> x, int n);
    public static Vector<double> Sqrt(Vector<double> x);

    // IFloatingPoint<TSelf>
    public static Vector<double> Round(Vector<double> x);
    public static Vector<double> Round(Vector<double> x, int digits);
    public static Vector<double> Round(Vector<double> x, MidpointRounding mode);
    public static Vector<double> Round(Vector<double> x, int digits, MidpointRounding mode);
    public static Vector<double> Truncate(Vector<double> x);

    // IFloatingPointIeee754<TSelf>
    public static Vector<double> Atan2(Vector<double> y, Vector<double> x);
    public static Vector<double> Atan2Pi(Vector<double> y, Vector<double> x);
    public static Vector<double> BitDecrement(Vector<double> x);
    public static Vector<double> BitIncrement(Vector<double> x);
    public static Vector<double> FusedMultiplyAdd(Vector<double> left, Vector<double> right, Vector<double> addend);
    public static Vector<double> Lerp(Vector<double> value1, Vector<double> value2, Vector<double> amount);
    public static Vector<double> ReciprocalEstimate(Vector<double> x);
    public static Vector<double> ReciprocalSqrtEstimate(Vector<double> x);

    // INumber<T> -- these also work for integer types
    public static Vector<T> Clamp<T>(Vector<T> value, Vector<T> min, Vector<T> max) where T: unmanaged;
    public static Vector<T> CopySign<T>(Vector<T> value, Vector<T> sign) where T: unmanaged;
    public static Vector<T> MaxNumber<T>(Vector<T> x, Vector<T> y) where T: unmanaged;
    public static Vector<T> MinNumber<T>(Vector<T> x, Vector<T> y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector<T> MaxMagnitude<T>(Vector<T> x, Vector<T> y) where T: unmanaged;
    public static Vector<T> MaxMagnitudeNumber<T>(Vector<T> x, Vector<T> y) where T: unmanaged;
    public static Vector<T> MinMagnitude<T>(Vector<T> x, Vector<T> y) where T: unmanaged;
    public static Vector<T> MinMagnitudeNumber<T>(Vector<T> x, Vector<T> y) where T: unmanaged; 
}

namespace System.Numerics;

public struct Vector2
{
    // IFloatingPointIeee754
    public static Vector2 Epsilon { get; }
    public static Vector2 NaN { get; }
    public static Vector2 NegativeInfinity { get; }
    public static Vector2 NegativeZero { get; }
    public static Vector2 PositiveInfinity { get; }

    // IFloatingPointConstants
    public static Vector2 E { get; }
    public static Vector2 Pi { get; }
    public static Vector2 Tau { get; }

    // INumberBase
    public static Vector2 Radix { get; }

    // Equivalent implementing IHyperbolicFunctions<System.Numerics.Vector2>
    public static Vector2 Acosh(Vector2 x);
    public static Vector2 Asinh(Vector2 x);
    public static Vector2 Atanh(Vector2 x);
    public static Vector2 Cosh(Vector2 x);
    public static Vector2 Sinh(Vector2 x);
    public static Vector2 Tanh(Vector2 x);

    // Equivalent implementing ITrigonometricFunctions<System.Numerics.Vector2>
    public static Vector2 Acos(Vector2 x);
    public static Vector2 AcosPi(Vector2 x);
    public static Vector2 Asin(Vector2 x);
    public static Vector2 AsinPi(Vector2 x);
    public static Vector2 Atan(Vector2 x);
    public static Vector2 AtanPi(Vector2 x);
    public static Vector2 Cos(Vector2 x);
    public static Vector2 CosPi(Vector2 x);
    public static Vector2 DegreesToRadians(Vector2 degrees);
    public static Vector2 RadiansToDegrees(Vector2 radians);
    public static Vector2 Sin(Vector2 x);
    public static (Vector2 Sin, Vector2 Cos) SinCos(Vector2 x);
    public static (Vector2 SinPi, Vector2 CosPi) SinCosPi(Vector2 x);
    public static Vector2 SinPi(Vector2 x);
    public static Vector2 Tan(Vector2 x);
    public static Vector2 TanPi(Vector2 x);

    // Equivalent implementing ILogarithmicFunctions<System.Numerics.Vector2>
    public static Vector2 Log(Vector2 x);
    public static Vector2 Log(Vector2 x, Vector2 newBase);
    public static Vector2 LogP1(Vector2 x) => Log(x + Vector2.One);
    public static Vector2 Log2(Vector2 x);
    public static Vector2 Log2P1(Vector2 x) => Log2(x + Vector2.One);
    public static Vector2 Log10(Vector2 x);
    public static Vector2 Log10P1(Vector2 x) => Log10(x + Vector2.One);

    // Equivalent implementing IExponentialFunctions<System.Numerics.Vector2>
    public static Vector2 Exp(Vector2 x);
    public static Vector2 ExpM1(Vector2 x) => Exp(x) - Vector2.One;
    public static Vector2 Exp2(Vector2 x);
    public static Vector2 Exp2M1(Vector2 x) => Exp2(x) - Vector2.One;
    public static Vector2 Exp10(Vector2 x);
    public static Vector2 Exp10M1(Vector2 x) => Exp10(x) - Vector2.One;

    // Equivalent implementing IPowerFunctions<System.Numerics.Vector2>
    public static Vector2 Pow(Vector2 x, Vector2 y);

    // Equivalent implementing IRootFunctions<System.Numerics.Vector2>
    public static Vector2 Cbrt(Vector2 x);
    public static Vector2 Hypot(Vector2 x, Vector2 y);
    public static Vector2 RootN(Vector2 x, int n);
    public static Vector2 Sqrt(Vector2 x);

    // IFloatingPoint<TSelf>
    public static Vector2 Round(Vector2 x);
    public static Vector2 Round(Vector2 x, int digits);
    public static Vector2 Round(Vector2 x, MidpointRounding mode);
    public static Vector2 Round(Vector2 x, int digits, MidpointRounding mode);
    public static Vector2 Truncate(Vector2 x);

    // IFloatingPointIeee754<TSelf>
    public static Vector2 Atan2(Vector2 y, Vector2 x);
    public static Vector2 Atan2Pi(Vector2 y, Vector2 x);
    public static Vector2 BitDecrement(Vector2 x);
    public static Vector2 BitIncrement(Vector2 x);
    public static Vector2 FusedMultiplyAdd(Vector2 left, Vector2 right, Vector2 addend);
    public static Vector2 Lerp(Vector2 value1, Vector2 value2, Vector2 amount);
    public static Vector2 ReciprocalEstimate(Vector2 x);
    public static Vector2 ReciprocalSqrtEstimate(Vector2 x);

    // INumber<T> -- these also work for integer types
    public static Vector2 Clamp(Vector2 value, Vector2 min, Vector2 max) where T: unmanaged;
    public static Vector2 CopySign(Vector2 value, Vector2 sign) where T: unmanaged;
    public static Vector2 MaxNumber(Vector2 x, Vector2 y) where T: unmanaged;
    public static Vector2 MinNumber(Vector2 x, Vector2 y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector2 MaxMagnitude(Vector2 x, Vector2 y) where T: unmanaged;
    public static Vector2 MaxMagnitudeNumber(Vector2 x, Vector2 y) where T: unmanaged;
    public static Vector2 MinMagnitude(Vector2 x, Vector2 y) where T: unmanaged;
    public static Vector2 MinMagnitudeNumber(Vector2 x, Vector2 y) where T: unmanaged; 
}

namespace System.Numerics;

public struct Vector3
{
    // IFloatingPointIeee754
    public static Vector3 Epsilon { get; }
    public static Vector3 NaN { get; }
    public static Vector3 NegativeInfinity { get; }
    public static Vector3 NegativeZero { get; }
    public static Vector3 PositiveInfinity { get; }

    // IFloatingPointConstants
    public static Vector3 E { get; }
    public static Vector3 Pi { get; }
    public static Vector3 Tau { get; }

    // INumberBase
    public static Vector3 Radix { get; }

    // Equivalent implementing IHyperbolicFunctions<System.Numerics.Vector3>
    public static Vector3 Acosh(Vector3 x);
    public static Vector3 Asinh(Vector3 x);
    public static Vector3 Atanh(Vector3 x);
    public static Vector3 Cosh(Vector3 x);
    public static Vector3 Sinh(Vector3 x);
    public static Vector3 Tanh(Vector3 x);

    // Equivalent implementing ITrigonometricFunctions<System.Numerics.Vector3>
    public static Vector3 Acos(Vector3 x);
    public static Vector3 AcosPi(Vector3 x);
    public static Vector3 Asin(Vector3 x);
    public static Vector3 AsinPi(Vector3 x);
    public static Vector3 Atan(Vector3 x);
    public static Vector3 AtanPi(Vector3 x);
    public static Vector3 Cos(Vector3 x);
    public static Vector3 CosPi(Vector3 x);
    public static Vector3 DegreesToRadians(Vector3 degrees);
    public static Vector3 RadiansToDegrees(Vector3 radians);
    public static Vector3 Sin(Vector3 x);
    public static (Vector3 Sin, Vector3 Cos) SinCos(Vector3 x);
    public static (Vector3 SinPi, Vector3 CosPi) SinCosPi(Vector3 x);
    public static Vector3 SinPi(Vector3 x);
    public static Vector3 Tan(Vector3 x);
    public static Vector3 TanPi(Vector3 x);

    // Equivalent implementing ILogarithmicFunctions<System.Numerics.Vector3>
    public static Vector3 Log(Vector3 x);
    public static Vector3 Log(Vector3 x, Vector3 newBase);
    public static Vector3 LogP1(Vector3 x) => Log(x + Vector3.One);
    public static Vector3 Log2(Vector3 x);
    public static Vector3 Log2P1(Vector3 x) => Log2(x + Vector3.One);
    public static Vector3 Log10(Vector3 x);
    public static Vector3 Log10P1(Vector3 x) => Log10(x + Vector3.One);

    // Equivalent implementing IExponentialFunctions<System.Numerics.Vector3>
    public static Vector3 Exp(Vector3 x);
    public static Vector3 ExpM1(Vector3 x) => Exp(x) - Vector3.One;
    public static Vector3 Exp2(Vector3 x);
    public static Vector3 Exp2M1(Vector3 x) => Exp2(x) - Vector3.One;
    public static Vector3 Exp10(Vector3 x);
    public static Vector3 Exp10M1(Vector3 x) => Exp10(x) - Vector3.One;

    // Equivalent implementing IPowerFunctions<System.Numerics.Vector3>
    public static Vector3 Pow(Vector3 x, Vector3 y);

    // Equivalent implementing IRootFunctions<System.Numerics.Vector3>
    public static Vector3 Cbrt(Vector3 x);
    public static Vector3 Hypot(Vector3 x, Vector3 y);
    public static Vector3 RootN(Vector3 x, int n);
    public static Vector3 Sqrt(Vector3 x);

    // IFloatingPoint<TSelf>
    public static Vector3 Round(Vector3 x);
    public static Vector3 Round(Vector3 x, int digits);
    public static Vector3 Round(Vector3 x, MidpointRounding mode);
    public static Vector3 Round(Vector3 x, int digits, MidpointRounding mode);
    public static Vector3 Truncate(Vector3 x);

    // IFloatingPointIeee754<TSelf>
    public static Vector3 Atan2(Vector3 y, Vector3 x);
    public static Vector3 Atan2Pi(Vector3 y, Vector3 x);
    public static Vector3 BitDecrement(Vector3 x);
    public static Vector3 BitIncrement(Vector3 x);
    public static Vector3 FusedMultiplyAdd(Vector3 left, Vector3 right, Vector3 addend);
    public static Vector3 Lerp(Vector3 value1, Vector3 value2, Vector3 amount);
    public static Vector3 ReciprocalEstimate(Vector3 x);
    public static Vector3 ReciprocalSqrtEstimate(Vector3 x);

    // INumber<T> -- these also work for integer types
    public static Vector3 Clamp(Vector3 value, Vector3 min, Vector3 max) where T: unmanaged;
    public static Vector3 CopySign(Vector3 value, Vector3 sign) where T: unmanaged;
    public static Vector3 MaxNumber(Vector3 x, Vector3 y) where T: unmanaged;
    public static Vector3 MinNumber(Vector3 x, Vector3 y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector3 MaxMagnitude(Vector3 x, Vector3 y) where T: unmanaged;
    public static Vector3 MaxMagnitudeNumber(Vector3 x, Vector3 y) where T: unmanaged;
    public static Vector3 MinMagnitude(Vector3 x, Vector3 y) where T: unmanaged;
    public static Vector3 MinMagnitudeNumber(Vector3 x, Vector3 y) where T: unmanaged; 
}

namespace System.Numerics;

public struct Vector4
{
    // IFloatingPointIeee754
    public static Vector4 Epsilon { get; }
    public static Vector4 NaN { get; }
    public static Vector4 NegativeInfinity { get; }
    public static Vector4 NegativeZero { get; }
    public static Vector4 PositiveInfinity { get; }

    // IFloatingPointConstants
    public static Vector4 E { get; }
    public static Vector4 Pi { get; }
    public static Vector4 Tau { get; }

    // INumberBase
    public static Vector4 Radix { get; }

    // Equivalent implementing IHyperbolicFunctions<System.Numerics.Vector4>
    public static Vector4 Acosh(Vector4 x);
    public static Vector4 Asinh(Vector4 x);
    public static Vector4 Atanh(Vector4 x);
    public static Vector4 Cosh(Vector4 x);
    public static Vector4 Sinh(Vector4 x);
    public static Vector4 Tanh(Vector4 x);

    // Equivalent implementing ITrigonometricFunctions<System.Numerics.Vector4>
    public static Vector4 Acos(Vector4 x);
    public static Vector4 AcosPi(Vector4 x);
    public static Vector4 Asin(Vector4 x);
    public static Vector4 AsinPi(Vector4 x);
    public static Vector4 Atan(Vector4 x);
    public static Vector4 AtanPi(Vector4 x);
    public static Vector4 Cos(Vector4 x);
    public static Vector4 CosPi(Vector4 x);
    public static Vector4 DegreesToRadians(Vector4 degrees);
    public static Vector4 RadiansToDegrees(Vector4 radians);
    public static Vector4 Sin(Vector4 x);
    public static (Vector4 Sin, Vector4 Cos) SinCos(Vector4 x);
    public static (Vector4 SinPi, Vector4 CosPi) SinCosPi(Vector4 x);
    public static Vector4 SinPi(Vector4 x);
    public static Vector4 Tan(Vector4 x);
    public static Vector4 TanPi(Vector4 x);

    // Equivalent implementing ILogarithmicFunctions<System.Numerics.Vector4>
    public static Vector4 Log(Vector4 x);
    public static Vector4 Log(Vector4 x, Vector4 newBase);
    public static Vector4 LogP1(Vector4 x) => Log(x + Vector4.One);
    public static Vector4 Log2(Vector4 x);
    public static Vector4 Log2P1(Vector4 x) => Log2(x + Vector4.One);
    public static Vector4 Log10(Vector4 x);
    public static Vector4 Log10P1(Vector4 x) => Log10(x + Vector4.One);

    // Equivalent implementing IExponentialFunctions<System.Numerics.Vector4>
    public static Vector4 Exp(Vector4 x);
    public static Vector4 ExpM1(Vector4 x) => Exp(x) - Vector4.One;
    public static Vector4 Exp2(Vector4 x);
    public static Vector4 Exp2M1(Vector4 x) => Exp2(x) - Vector4.One;
    public static Vector4 Exp10(Vector4 x);
    public static Vector4 Exp10M1(Vector4 x) => Exp10(x) - Vector4.One;

    // Equivalent implementing IPowerFunctions<System.Numerics.Vector4>
    public static Vector4 Pow(Vector4 x, Vector4 y);

    // Equivalent implementing IRootFunctions<System.Numerics.Vector4>
    public static Vector4 Cbrt(Vector4 x);
    public static Vector4 Hypot(Vector4 x, Vector4 y);
    public static Vector4 RootN(Vector4 x, int n);
    public static Vector4 Sqrt(Vector4 x);

    // IFloatingPoint<TSelf>
    public static Vector4 Round(Vector4 x);
    public static Vector4 Round(Vector4 x, int digits);
    public static Vector4 Round(Vector4 x, MidpointRounding mode);
    public static Vector4 Round(Vector4 x, int digits, MidpointRounding mode);
    public static Vector4 Truncate(Vector4 x);

    // IFloatingPointIeee754<TSelf>
    public static Vector4 Atan2(Vector4 y, Vector4 x);
    public static Vector4 Atan2Pi(Vector4 y, Vector4 x);
    public static Vector4 BitDecrement(Vector4 x);
    public static Vector4 BitIncrement(Vector4 x);
    public static Vector4 FusedMultiplyAdd(Vector4 left, Vector4 right, Vector4 addend);
    public static Vector4 Lerp(Vector4 value1, Vector4 value2, Vector4 amount);
    public static Vector4 ReciprocalEstimate(Vector4 x);
    public static Vector4 ReciprocalSqrtEstimate(Vector4 x);

    // INumber<T> -- these also work for integer types
    public static Vector4 Clamp(Vector4 value, Vector4 min, Vector4 max) where T: unmanaged;
    public static Vector4 CopySign(Vector4 value, Vector4 sign) where T: unmanaged;
    public static Vector4 MaxNumber(Vector4 x, Vector4 y) where T: unmanaged;
    public static Vector4 MinNumber(Vector4 x, Vector4 y) where T: unmanaged;

    // INumberBase<T> -- these also work for integer types
    public static Vector4 MaxMagnitude(Vector4 x, Vector4 y) where T: unmanaged;
    public static Vector4 MaxMagnitudeNumber(Vector4 x, Vector4 y) where T: unmanaged;
    public static Vector4 MinMagnitude(Vector4 x, Vector4 y) where T: unmanaged;
    public static Vector4 MinMagnitudeNumber(Vector4 x, Vector4 y) where T: unmanaged; 
}
```
## Stringbuilder.Replace(ROS oldValue, ROS newValue, int index, int count)

**Approved** | [#runtime/77837](https://github.com/dotnet/runtime/issues/77837#issuecomment-1777838026) | [Video](https://www.youtube.com/watch?v=O9v-lgJkqm8&t=1h38m35s)

Looks good as proposed.

```C#
namespace System.Text;

partial class StringBuilder
{
   public StringBuilder Replace(ReadOnlySpan<char> oldValue, ReadOnlySpan<char> newValue, int startIndex, int count);
   public StringBuilder Replace(ReadOnlySpan<char> oldValue, ReadOnlySpan<char> newValue);
}
```
