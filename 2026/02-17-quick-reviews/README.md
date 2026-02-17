# API Review 02/17/2026

## RuntimeFeature.IsSingleThreaded

**Approved** | [#runtime/77541](https://github.com/dotnet/runtime/issues/77541#issuecomment-3916487194)

* Rather than "IsSingleThreaded", we feel that "IsMultithreadingSupported" is better, as it indicates the support level, rather than possibly being interpreted as a point-in-time value

```csharp
namespace System.Runtime.CompilerServices;

public static partial class RuntimeFeature
{
    [System.Diagnostics.CodeAnalysis.FeatureSwitchDefinitionAttribute("System.Runtime.CompilerServices.RuntimeFeature.IsMultithreadingSupported")]
    public static bool IsMultithreadingSupported { get; }
}
```
## JavaScript marshalling support float[], Span<float> and ArraySegment<float>

**Approved** | [#runtime/123706](https://github.com/dotnet/runtime/issues/123706#issuecomment-3916519647)

Looks good as proposed.

```csharp
namespace System.Runtime.InteropServices.JavaScript;

public partial struct JSMarshalerArgument
{
    public void ToManaged(out float[]? value) { throw null; }
    public void ToJS(float[]? value) { throw null; }
    public void ToManaged(out Span<float> value) { throw null; }
    public void ToJS(Span<float> value) { throw null; }
    public void ToManaged(out ArraySegment<float> value) { throw null; }
    public void ToJS(ArraySegment<float> value) { throw null; }
}
```
## More Vector<T> sequence and lane APIs

**Approved** | [#runtime/122557](https://github.com/dotnet/runtime/issues/122557#issuecomment-3916644599)

* Rather than ConcatHalves with a `byte` selector, we expanded the method into ConcatLowerLower...ConcatUpperUpper
* CreateAlternatingSequence(first, second) => CreateAlternatingSequence(even, odd)

```csharp
namespace System.Numerics;

public static class Vector
{
    // The elements begin at an initial value, and each subsequent element is obtained by multiplying the previous element by the multiplier.
    [Intrinsic]
    public static Vector<T> CreateGeometricSequence<T>(T initial, [ConstantExpected] T multiplier);

    // Elements alternate between two specified values, starting with the first and then repeating the pattern.
    [Intrinsic]
    public static Vector<T> CreateAlternatingSequence<T>(T even, T odd);

    // Harmonic progression: [1/start, 1/(start+step), …]
    [Intrinsic]
    public static Vector<T> CreateHarmonicSequence<T>(T start, T step);

    // Cauchy progression: [1/(i+start), …] or representable via Sqrt(CreateSequence(...))
    [Intrinsic]
    public static Vector<T> CreateCauchySequence<T>(T start, T step);

    // Interleaves the lower halves of two vectors:
    // ZipLower([1,2,3,4,5,6,7,8], [9,10,11,12,13,14,15,16])
    // => [1,9,2,10,3,11,4,12]
    [Intrinsic]
    public static Vector<T> ZipLower<T>(Vector<T> left, Vector<T> right);

    // Interleaves the upper halves of two vectors:
    // ZipUpper([1,2,3,4,5,6,7,8], [9,10,11,12,13,14,15,16])
    // => [5,13,6,14,7,15,8,16]
    [Intrinsic]
    public static Vector<T> ZipUpper<T>(Vector<T> left, Vector<T> right);

    [Intrinsic]
    public static (Vector<T> Lower, Vector<T> Upper) Zip<T>(Vector<T> left, Vector<T> right);

    // De‑interleaves the even lanes from two vectors:
    // UnzipEven([1,9,2,10,3,11,4,12], [5,13,6,14,7,15,8,16])
    // => [1,2,3,4,5,6,7,8] (the even‑indexed elements from both inputs)
    [Intrinsic]
    public static Vector<T> UnzipEven<T>(Vector<T> left, Vector<T> right);

    // De‑interleaves the odd lanes from two vectors:
    // UnzipOdd([1,9,2,10,3,11,4,12], [5,13,6,14,7,15,8,16])
    // => [9,10,11,12,13,14,15,16] (the odd‑indexed elements from both inputs)
    [Intrinsic]
    public static Vector<T> UnzipOdd<T>(Vector<T> left, Vector<T> right);

    // Splits into both even and odd at once:
    // Unzip([1,9,2,10,3,11,4,12], [5,13,6,14,7,15,8,16])
    // => ([1,2,3,4,5,6,7,8], [9,10,11,12,13,14,15,16])
    [Intrinsic]
    public static (Vector<T> Even, Vector<T> Odd) Unzip<T>(Vector<T> left, Vector<T> right);

    public static Vector<T> ConcatLowerLower<T>(Vector<T> left, Vector<T> right);
    public static Vector<T> ConcatUpperLower<T>(Vector<T> left, Vector<T> right);
    public static Vector<T> ConcatUpperUpper<T>(Vector<T> left, Vector<T> right);
    public static Vector<T> ConcatLowerUpper<T>(Vector<T> left, Vector<T> right);

    [Intrinsic]
    public static Vector<T> Reverse<T>(Vector<T> vector);
}

public struct Vector<T>
{
    // Elements alternate between +1 and -1, starting with +1 at element 0.
    // Sometimes also referred to as a BipolarSequence.
    public static Vector<T> SignSequence { [Intrinsic] get; }
}

// same surface for Vector64/128/256/512
namespace System.Runtime.Intrinsics;

public static class Vector64
{
    [Intrinsic]
    public static Vector64<T> CreateGeometricSequence<T>(T initial, [ConstantExpected] T multiplier);

    [Intrinsic]
    public static Vector64<T> CreateAlternatingSequence<T>(T even, T odd);

    [Intrinsic]
    public static Vector64<T> CreateHarmonicSequence<T>(T start, T step);

    [Intrinsic]
    public static Vector64<T> CreateCauchySequence<T>(T start, T step);

    [Intrinsic]
    public static Vector64<T> ZipLower<T>(Vector64<T> left, Vector64<T> right);

    [Intrinsic]
    public static Vector64<T> ZipUpper<T>(Vector64<T> left, Vector64<T> right);

    [Intrinsic]
    public static (Vector64<T> Lower, Vector64<T> Upper) Zip<T>(Vector64<T> left, Vector64<T> right);

    [Intrinsic]
    public static Vector64<T> UnzipEven<T>(Vector64<T> left, Vector64<T> right);

    [Intrinsic]
    public static Vector64<T> UnzipOdd<T>(Vector64<T> left, Vector64<T> right);

    [Intrinsic]
    public static (Vector64<T> Even, Vector64<T> Odd) Unzip<T>(Vector64<T> left, Vector64<T> right);

    public static Vector64<T> ConcatLowerLower<T>(Vector64<T> left, Vector64<T> right);
    public static Vector64<T> ConcatUpperLower<T>(Vector64<T> left, Vector64<T> right);
    public static Vector64<T> ConcatUpperUpper<T>(Vector64<T> left, Vector64<T> right);
    public static Vector64<T> ConcatLowerUpper<T>(Vector64<T> left, Vector64<T> right);

    [Intrinsic]
    public static Vector64<T> Reverse<T>(Vector64<T> vector);
}

public struct Vector64<T>
{
    public static Vector64<T> SignSequence { [Intrinsic] get; }
}

public static class Vector128
{
    [Intrinsic]
    public static Vector128<T> CreateGeometricSequence<T>(T initial, [ConstantExpected] T multiplier);

    [Intrinsic]
    public static Vector128<T> CreateAlternatingSequence<T>(T even, T odd);

    [Intrinsic]
    public static Vector128<T> CreateHarmonicSequence<T>(T start, T step);

    [Intrinsic]
    public static Vector128<T> CreateCauchySequence<T>(T start, T step);

    [Intrinsic]
    public static Vector128<T> ZipLower<T>(Vector128<T> left, Vector128<T> right);

    [Intrinsic]
    public static Vector128<T> ZipUpper<T>(Vector128<T> left, Vector128<T> right);

    [Intrinsic]
    public static (Vector128<T> Lower, Vector128<T> Upper) Zip<T>(Vector128<T> left, Vector128<T> right);

    [Intrinsic]
    public static Vector128<T> UnzipEven<T>(Vector128<T> left, Vector128<T> right);

    [Intrinsic]
    public static Vector128<T> UnzipOdd<T>(Vector128<T> left, Vector128<T> right);

    [Intrinsic]
    public static (Vector128<T> Even, Vector128<T> Odd) Unzip<T>(Vector128<T> left, Vector128<T> right);

    public static Vector128<T> ConcatLowerLower<T>(Vector128<T> left, Vector128<T> right);
    public static Vector128<T> ConcatUpperLower<T>(Vector128<T> left, Vector128<T> right);
    public static Vector128<T> ConcatUpperUpper<T>(Vector128<T> left, Vector128<T> right);
    public static Vector128<T> ConcatLowerUpper<T>(Vector128<T> left, Vector128<T> right);

    [Intrinsic]
    public static Vector128<T> Reverse<T>(Vector128<T> vector);
}

public struct Vector128<T>
{
    public static Vector128<T> SignSequence { [Intrinsic] get; }
}

public static class Vector256
{
    [Intrinsic]
    public static Vector256<T> CreateGeometricSequence<T>(T initial, [ConstantExpected] T multiplier);

    [Intrinsic]
    public static Vector256<T> CreateAlternatingSequence<T>(T even, T odd);

    [Intrinsic]
    public static Vector256<T> CreateHarmonicSequence<T>(T start, T step);

    [Intrinsic]
    public static Vector256<T> CreateCauchySequence<T>(T start, T step);

    [Intrinsic]
    public static Vector256<T> ZipLower<T>(Vector256<T> left, Vector256<T> right);

    [Intrinsic]
    public static Vector256<T> ZipUpper<T>(Vector256<T> left, Vector256<T> right);

    [Intrinsic]
    public static (Vector256<T> Lower, Vector256<T> Upper) Zip<T>(Vector256<T> left, Vector256<T> right);

    [Intrinsic]
    public static Vector256<T> UnzipEven<T>(Vector256<T> left, Vector256<T> right);

    [Intrinsic]
    public static Vector256<T> UnzipOdd<T>(Vector256<T> left, Vector256<T> right);

    [Intrinsic]
    public static (Vector256<T> Even, Vector256<T> Odd) Unzip<T>(Vector256<T> left, Vector256<T> right);

    public static Vector256<T> ConcatLowerLower<T>(Vector256<T> left, Vector256<T> right);
    public static Vector256<T> ConcatUpperLower<T>(Vector256<T> left, Vector256<T> right);
    public static Vector256<T> ConcatUpperUpper<T>(Vector256<T> left, Vector256<T> right);
    public static Vector256<T> ConcatLowerUpper<T>(Vector256<T> left, Vector256<T> right);

    [Intrinsic]
    public static Vector256<T> Reverse<T>(Vector256<T> vector);
}

public struct Vector256<T>
{
    public static Vector256<T> SignSequence { [Intrinsic] get; }
}

public static class Vector512
{
    [Intrinsic]
    public static Vector512<T> CreateGeometricSequence<T>(T initial, [ConstantExpected] T multiplier);

    [Intrinsic]
    public static Vector512<T> CreateAlternatingSequence<T>(T even, T odd);

    [Intrinsic]
    public static Vector512<T> CreateHarmonicSequence<T>(T start, T step);

    [Intrinsic]
    public static Vector512<T> CreateCauchySequence<T>(T start, T step);

    [Intrinsic]
    public static Vector512<T> ZipLower<T>(Vector512<T> left, Vector512<T> right);

    [Intrinsic]
    public static Vector512<T> ZipUpper<T>(Vector512<T> left, Vector512<T> right);

    [Intrinsic]
    public static (Vector512<T> Lower, Vector512<T> Upper) Zip<T>(Vector512<T> left, Vector512<T> right);

    [Intrinsic]
    public static Vector512<T> UnzipEven<T>(Vector512<T> left, Vector512<T> right);

    [Intrinsic]
    public static Vector512<T> UnzipOdd<T>(Vector512<T> left, Vector512<T> right);

    [Intrinsic]
    public static (Vector512<T> Even, Vector512<T> Odd) Unzip<T>(Vector512<T> left, Vector512<T> right);

    public static Vector512<T> ConcatLowerLower<T>(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> ConcatUpperLower<T>(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> ConcatUpperUpper<T>(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> ConcatLowerUpper<T>(Vector512<T> left, Vector512<T> right);

    [Intrinsic]
    public static Vector512<T> Reverse<T>(Vector512<T> vector);
}

public struct Vector512<T>
{
    public static Vector512<T> SignSequence { [Intrinsic] get; }
}
```
## AVX512 BMM Intrinsics

**Approved** | [#runtime/123898](https://github.com/dotnet/runtime/issues/123898#issuecomment-3916694971)


* BitMultiply...(x, y, z) to (left, right, addend) (assuming that's the correct order, fix it if not)
* ReverseBits(x) => ReverseBits(values)

```csharp
namespace System.Runtime.Intrinsics.X86
{
    public abstract class Avx512Bmm : Avx512F
    {
        public static new bool IsSupported { get; }

        public static Vector128<byte> ReverseBits(Vector128<byte> values);
        public static Vector256<byte> ReverseBits(Vector256<byte> values);
        public static Vector512<byte> ReverseBits(Vector512<byte> values);

        public static Vector256<ushort> BitMultiplyMatrix16x16WithOrReduction(Vector256<ushort> left, Vector256<ushort> right, Vector256<ushort> addend);
        public static Vector512<ushort> BitMultiplyMatrix16x16WithOrReduction(Vector512<ushort> left, Vector512<ushort> right, Vector512<ushort> addend);
        public static Vector256<ushort> BitMultiplyMatrix16x16WithXorReduction(Vector256<ushort> left, Vector256<ushort> right, Vector256<ushort> addend);
        public static Vector512<ushort> BitMultiplyMatrix16x16WithXorReduction(Vector512<ushort> left, Vector512<ushort> right, Vector512<ushort> addend);
    }
}
```
## Add `Stream` wrappers for memory and text-based types

**NeedsWork** | [#runtime/82801](https://github.com/dotnet/runtime/issues/82801#issuecomment-3916837306)

API Review (preliminary)

There was pretty strong agreement against static extension methods.  The most favorable approach was to move ROSequence into Corelib.

People were surprised at UTF-8 being the default encoding for FromText, but it might be correct (see also WriteAllText).

People thought maybe `streamEncoding` instead of `encoding` for the parameter name in FromText.
