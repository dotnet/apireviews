# API Review 02/06/2024

## Add PipeReader and PipeWriter overloads to JsonSerializer.SerilizeAsync/DeserializeAsync

**Approved** | [#runtime/68586](https://github.com/dotnet/runtime/issues/68586#issuecomment-1930534029)

* We kept all existing overloads of SerializeAsync/DeserializeAsync (and added the missing one)
* We also feel that DeserializeAsyncEnumerable is in scope, so it was added as well.
* Double check the nullable annotations, and linker annotations.
* Congratulations, System.IO.Pipelines.dll for moving into the shared runtime.

```c#
namespace System.Collections.Generic;

public static class JsonSerializer
{
    public static Task SerializeAsync<TValue>(
           PipeWriter utf8Json, 
           TValue value,  
           JsonTypeInfo<TValue> jsonTypeInfo, 
           CancellationToken cancellationToken = default);

    public static Task SerializeAsync<TValue>(
           PipeWriter utf8Json, 
           TValue value, 
           JsonSerializerOptions? options = null, 
           CancellationToken cancellationToken = default);
   
    public static Task SerializeAsync(
            PipeWriter utf8Json,
            object? value,
            JsonTypeInfo jsonTypeInfo,
            CancellationToken cancellationToken = default);
            
    public static Task SerializeAsync(
            PipeWriter utf8Json,
            object? value,
            Type inputType,
            JsonSerializerContext context,
            CancellationToken cancellationToken = default);

    public static Task SerializeAsync(
            Stream utf8Json,
            object? value,
            Type inputType,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
    
    public static ValueTask<TValue?> DeserializeAsync<TValue>(
            PipeReader utf8Json, 
            JsonSerializerOptions? options = null, 
            CancellationToken cancellationToken = default);
    
    public static ValueTask<TValue?> DeserializeAsync<TValue>(
            PipeReader utf8Json,
            JsonTypeInfo<TValue> jsonTypeInfo,
            CancellationToken cancellationToken = default);

    public static ValueTask<object?> DeserializeAsync(
            PipeReader utf8Json,
            JsonTypeInfo jsonTypeInfo,
            CancellationToken cancellationToken = default);
            
    public static ValueTask<object?> DeserializeAsync(
            PipeReader utf8Json,
            Type returnType,
            JsonSerializerContext context,
            CancellationToken cancellationToken = default);

    public static ValueTask<object?> DeserializeAsync(
           Stream utf8Json,
           Type returnType,
           JsonSerializerOptions? options = null,
           CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue> DeserializeAsyncEnumerable<TValue>(
            Stream utf8Json,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue> DeserializeAsyncEnumerable<TValue>(
            Stream utf8Json,
            JsonTypeInfo<TValue> jsonTypeInfo,
            CancellationToken cancellationToken = default);

}
```
## Add new overload to Uri.EscapeDataString

**Approved** | [#runtime/40603](https://github.com/dotnet/runtime/issues/40603#issuecomment-1930539476)

Looks good as proposed

```C#
namespace System
{
    partial class Uri
    {
        // Existing
        public static string EscapeDataString(string stringToEscape);
        public static string UnescapeDataString(string stringToUnescape);

        // New
        public static string EscapeDataString(ReadOnlySpan<char> charsToEscape);
        public static bool TryEscapeDataString(ReadOnlySpan<char> charsToEscape, Span<char> destination, out int charsWritten);

        public static string UnescapeDataString(ReadOnlySpan<char> charsToUnescape);
        public static bool TryUnescapeDataString(ReadOnlySpan<char> charsToUnescape, Span<char> destination, out int charsWritten);
    }
}
```
## Add AssemblyBuilder.SetEntryPoint(MethodInfo) method

**Approved** | [#runtime/97015](https://github.com/dotnet/runtime/issues/97015#issuecomment-1930567382)

* We like the template method pattern approach better, especially because the SetEntryPoint method is already non-virtual in .NET Framework.
* Since this is being written for future-proofing, we should go fully future-proof and include the PEFileKinds overloads.

```diff
namespace System.Reflection.Emit;

+ public enum PEFileKinds
+ {
+     Dll                = 0x0001,
+     ConsoleApplication = 0x0002,
+     WindowApplication = 0x0003,
+ }

public abstract partial class AssemblyBuilder
{
    // Previously approved new APIs
    public static AssemblyBuilder DefinePersistedAssembly(AssemblyName name, Assembly coreAssembly, IEnumerable<CustomAttributeBuilder>? assemblyAttributes = null);

    public void Save(Stream stream);
    public void Save(string assemblyFileName);
    protected abstract void SaveCore(Stream stream);
+   public void SetEntryPoint(MethodInfo entryMethod);
+   public void SetEntryPoint(MethodInfo entryMethod, PEFileKinds fileKind);
+   protected virtual void SetEntryPointCore(MethodInfo entryMethod, PEFileKinds fileKind);
}
```
## TensorPrimitives brought up to par with Math and generic math interfaces

**Approved** | [#runtime/96451](https://github.com/dotnet/runtime/issues/96451#issuecomment-1930602131)

Looks good as proposed.

```diff
namespace System.Numerics.Tensors

public static class TensorPrimitives
{
    // Methods without existing overloads
+   public static void Acos<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void AcosPi<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void Acosh<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IHyperbolicFunctions<T>;
+   public static void Asin<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void AsinPi<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void Asinh<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IHyperbolicFunctions<T>;
+   public static void Atan<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void AtanPi<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void Atan2<T>(ReadOnlySpan<T> y, ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Atan2Pi<T>(ReadOnlySpan<T> y, ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Atanh<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IHyperbolicFunctions<T>;
+   public static void BitwiseAnd<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IBitwiseOperators<T, T, T>;
+   public static void BitwiseAnd<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IBitwiseOperators<T, T, T>;
+   public static void BitwiseOr<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IBitwiseOperators<T, T, T>;
+   public static void BitwiseOr<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IBitwiseOperators<T, T, T>;
+   public static void Cbrt<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IRootFunctions<T>;
+   public static void Ceiling<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void ConvertChecked<TFrom, TTo>(ReadOnlySpan<TFrom> source, Span<TTo> destination) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;
+   public static void ConvertSaturating<TFrom, TTo>(ReadOnlySpan<TFrom> source, Span<TTo> destination) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;
+   public static void ConvertTruncating<TFrom, TTo>(ReadOnlySpan<TFrom> source, Span<TTo> destination) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;
+   public static void CopySign<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> sign, Span<T> destination) where T : INumber<T>;
+   public static void CopySign<T>(ReadOnlySpan<T> x, T sign, Span<T> destination) where T : INumber<T>;
+   public static void Cos<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void CosPi<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void DegreesToRadians<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void ExpM1<T>(ReadOnlySpan<T> x) where T : IExponentialFunctions<TSelf>;
+   public static void Exp2<T>(ReadOnlySpan<T> x) where T : IExponentialFunctions<TSelf>;
+   public static void Exp2M1<T>(ReadOnlySpan<T> x) where T : IExponentialFunctions<TSelf>;
+   public static void Exp10<T>(ReadOnlySpan<T> x) where T : IExponentialFunctions<TSelf>;
+   public static void Exp10M1<T>(ReadOnlySpan<T> x) where T : IExponentialFunctions<TSelf>;
+   public static void Floor<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void FusedMultiplyAdd<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, ReadOnlySpan<T> addend, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
+   public static void FusedMultiplyAdd<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, T addend, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
+   public static void FusedMultiplyAdd<T>(ReadOnlySpan<T> x, T y, ReadOnlySpan<T> addend, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
+   public static void Hypot<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IRootFunctions<T>;
+   public static void Ieee754Remainder<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Ieee754Remainder<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Ieee754Remainder<T>(T x, ReadOnlySpan<T> y, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void LeadingZeroCount<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void ILogB<T>(ReadOnlySpan<T> x, Span<int> destination) where T : IFloatingPointIeee754<T>;
+   public static void Lerp<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, ReadOnlySpan<T> amount, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Lerp<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, T amount, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Lerp<T>(ReadOnlySpan<T> x, T y, ReadOnlySpan<T> amount, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void Log<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : ILogarithmicFunctions<T>;
+   public static void Log<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : ILogarithmicFunctions<T>;
+   public static void Log10<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ILogarithmicFunctions<T>;
+   public static void LogP1<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ILogarithmicFunctions<T>;
+   public static void Log2P1<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ILogarithmicFunctions<T>;
+   public static void Log10P1<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ILogarithmicFunctions<T>;
+   public static void OnesComplement<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IBitwiseOperators<T, T, T>;
+   public static void PopCount<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void Pow<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IPowerFunctions<T>;
+   public static void Pow<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IPowerFunctions<T>;
+   public static void Pow<T>(T x, ReadOnlySpan<T> y, Span<T> destination) where T : IPowerFunctions<T>;
+   public static void RadiansToDegrees<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void Reciprocal<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void ReciprocalEstimate<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void ReciprocalSqrt<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void ReciprocalSqrtEstimate<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void RootN<T>(ReadOnlySpan<T> x, int n, Span<T> destination) where T : IRootFunctions<T>;
+   public static void RotateLeft<T>(ReadOnlySpan<T> x, int rotateAmount, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void RotateRight<T>(ReadOnlySpan<T> x, int rotateAmount, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void Round<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void Round<T>(ReadOnlySpan<T> x, MidpointRounding mode, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void Round<T>(ReadOnlySpan<T> x, int digits, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void Round<T>(ReadOnlySpan<T> x, int digits, MidpointRounding mode, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void ScaleB<T>(ReadOnlySpan<T> x, int n, Span<T> destination) where T : IFloatingPointIeee754<T>;
+   public static void ShiftLeft<T>(ReadOnlySpan<T> x, int shiftAmount, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void ShiftRightArithmetic<T>(ReadOnlySpan<T> x, int shiftAmount, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void ShiftRightLogical<T>(ReadOnlySpan<T> x, int shiftAmount, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void Sin<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void SinCos<T>(ReadOnlySpan<T> x, Span<T> sinDestination, Span<T> cosDestination) where T : ITrigonometricFunctions<T>;
+   public static void SinCosPi<T>(ReadOnlySpan<T> x, Span<T> sinPiDestination, Span<T> cosPiDestination) where T : ITrigonometricFunctions<T>;
+   public static void SinPi<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void Sqrt<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IRootFunctions<T>;
+   public static void Tan<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void TanPi<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ITrigonometricFunctions<T>;
+   public static void TrailingZeroCount<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IBinaryInteger<T>;
+   public static void Truncate<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IFloatingPoint<T>;
+   public static void Xor<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IBitwiseOperators<T, T, T>;
+   public static void Xor<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IBitwiseOperators<T, T, T>;

    // Overloads of existing methods
+   public static void Divide<T>(T x, ReadOnlySpan<T> y, Span<T> destination) where T : IDivisionOperators<T, T, T>;
+   public static void Max<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumber<T>;
+   public static void MaxMagnitude<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumber<T>;
+   public static void Min<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumber<T>;
+   public static void MinMagnitude<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumber<T>;
+   public static void Subtract<T>(T x, ReadOnlySpan<T> y, Span<T> destination) where T : ISubtractionOperators<T, T, T>;
}
```
## Cannot set ReadMode after creating a NamedPipeClientStream instance

**Rejected** | [#runtime/83072](https://github.com/dotnet/runtime/issues/83072#issuecomment-1930642704)

The desired functionality should already be possible with NamedPipeServerStreamAcl.Create, but apparently there's a bug.

Rather than adding this constructor, we should either fix the bug or bring back all of the constructors that were cut for being Windows-only (merging the Pipes and Pipes.AccessControl assembly implementations).
## MultiplyAddEstimate

**Approved** | [#runtime/98053](https://github.com/dotnet/runtime/issues/98053#issuecomment-1930666381)

Looks good as proposed.  For anyone who wants to hear Jeremy be way too nerdy about linguistics, you need to listen to the API Review video, because Estimate and Estimate are different words.

```C#
namespace System
{
    public partial struct Double
    {
        public static double MultiplyAddEstimate(double x, double y, double addend);
    }

    public partial struct Half
    {
        public static Half MultiplyAddEstimate(Half x, Half y, Half addend);
    }

    public partial struct Single
    {
        public static float MultiplyAddEstimate(float x, float y, float addend);
    }
}

namespace System.Numerics
{
    public partial interface IFloatingPointIeee754<TSelf>
    {
        static virtual TSelf MultiplyAddEstimate(TSelf x, TSelf y, TSelf addend);
    }

    public partial class Vector
    {
        static Vector<double> MultiplyAddEstimate(Vector<double> x, Vector<double> y, Vector<double> addend);
        static Vector<float> MultiplyAddEstimate(Vector<float> x, Vector<float> y, Vector<float> addend);
    }
}

namespace System.Numerics.Tensors
{
    public partial class TensorPrimitives
    {
        public static void MultiplyAddEstimate<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, ReadOnlySpan<T> addend, Span<T> destination) where T : IFloatingPointIeee754<T>;
    }
}

namespace System.Runtime.InteropServices
{
    public partial struct NFloat
    {
        public static NFloat MultiplyAddEstimate(NFloat x, NFloat y, NFloat addend);
    }
}

namespace System.Runtime.Intrinsics
{
    public partial class Vector64
    {
        static Vector64<double> MultiplyAddEstimate(Vector64<double> x, Vector64<double> y, Vector64<double> addend);
        static Vector64<float> MultiplyAddEstimate(Vector64<float> x, Vector64<float> y, Vector64<float> addend);
    }

    public partial class Vector128
    {
        static Vector128<double> MultiplyAddEstimate(Vector128<double> x, Vector128<double> y, Vector128<double> addend);
        static Vector128<float> MultiplyAddEstimate(Vector128<float> x, Vector128<float> y, Vector128<float> addend);
    }

    public partial class Vector256
    {
        static Vector256<double> MultiplyAddEstimate(Vector256<double> x, Vector256<double> y, Vector256<double> addend);
        static Vector256<float> MultiplyAddEstimate(Vector256<float> x, Vector256<float> y, Vector256<float> addend);
    }

    public partial class Vector512
    {
        static Vector512<double> MultiplyAddEstimate(Vector512<double> x, Vector512<double> y, Vector512<double> addend);
        static Vector512<float> MultiplyAddEstimate(Vector512<float> x, Vector512<float> y, Vector512<float> addend);
    }
}
```
