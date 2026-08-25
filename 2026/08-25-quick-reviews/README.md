# API Review 08/25/2026

## Message template formatting support for DataAnnotations validation attributes

**Approved** | [#runtime/132605](https://github.com/dotnet/runtime/issues/132605#issuecomment-5414424858) | [Video](https://www.youtube.com/watch?v=SfsHPgvtk7c&t=0h0m0s)

* Added the CompositeFormat syntax marker, renamed the first parameter to `format` to match String.Format

```csharp
namespace System.ComponentModel.DataAnnotations;

public abstract partial class ValidationAttribute
{
    public virtual string FormatMessage([StringSyntax("CompositeFormat")] string format, string name);
}
```
## Suggestion `ComWrappers.TryGetComInstance()` to improve UX

**NeedsWork** | [#runtime/106979](https://github.com/dotnet/runtime/issues/106979#issuecomment-5414842094) | [Video](https://www.youtube.com/watch?v=SfsHPgvtk7c&t=0h45m35s)

* Multiple reviewers thought that the method was caching the value AFTER the QI call.  So we recommend taking a look offline and see if a different name/layer would make things clearer.  (If not, we're probably happy with the current name)
* Consider removing the `T` parameter from the StrategyBasedComWrappers version as it seems like T : TInterface is implied by context.

```csharp
namespace System.Runtime.InteropServices;

public partial class ComWrappers
{
    public IntPtr GetOrCreateComInterfaceForObject(object instance, CreateComInterfaceFlags flags, in Guid interfaceId);
}

public partial class StrategyBasedComWrappers
{
    public IntPtr GetOrCreateComInterfaceForObject<T, TInterface>(T instance, CreateComInterfaceFlags flags);
}
```
## System.Runtime.Intrinsics.Wasm.RelaxedSimd

**Approved** | [#runtime/130223](https://github.com/dotnet/runtime/issues/130223#issuecomment-5415172902) | [Video](https://www.youtube.com/watch?v=SfsHPgvtk7c&t=1h20m31s)


* Let's go ahead and slap the "Native" suffix on everything (except the "Estimate" methods) to be defensive against future ISA unification masking the name out (as happened with AVX512/FMA)
* Let's keep "indices" with a C because it matches PackedSimd.Swizzle, even though it is wrong per FDG.
* MultiplyAddEstimate/MultiplyAddNegatedEstimate should rename parameters as left/right/addend (or addend/left/right, whichever is correct for the platform)

```csharp
namespace System.Runtime.Intrinsics.Wasm;

[CLSCompliant(false)]
public abstract class RelaxedSimd
{
    public static bool IsSupported { get; }

    // Swizzle — i8x16.relaxed_swizzle. Out-of-range indices produce an
    // implementation-defined value (PackedSimd.Swizzle zeroes them).
    public static Vector128<sbyte> SwizzleNative(Vector128<sbyte> vector, Vector128<sbyte> indices);
    public static Vector128<byte>  SwizzleNative(Vector128<byte>  vector, Vector128<byte>  indices);

    // Float-to-int truncation — i32x4.relaxed_trunc_f{32x4,64x2}_{s,u}[_zero].
    // NaN and out-of-range inputs may produce any of the results the wasm
    // relaxed-trunc semantics permit — closest existing .NET contract is
    // Vector128.ConvertToInt32Native ("platform specific behavior on overflow"),
    // with the additional caveat that on wasm the specific "engine choice" is
    // per-embedding rather than per-ISA. Callers wanting portable behavior
    // should use PackedSimd.ConvertToInt32Saturate.
    public static Vector128<int>  ConvertToInt32Native(Vector128<float>  value);
    public static Vector128<uint> ConvertToUInt32Native(Vector128<float>  value);
    public static Vector128<int>  ConvertToInt32Native(Vector128<double> value);
    public static Vector128<uint> ConvertToUInt32Native(Vector128<double> value);

    // Multiply-add estimates — f{32x4,64x2}.relaxed_{madd,nmadd}.
    // Implementation may or may not fuse; matches Vector128.MultiplyAddEstimate.
    public static Vector128<float>  MultiplyAddEstimate        (Vector128<float>  left, Vector128<float>  right, Vector128<float>  addend);
    public static Vector128<double> MultiplyAddEstimate        (Vector128<double> left, Vector128<double> right, Vector128<double> addend);
    public static Vector128<float>  MultiplyAddNegatedEstimate (Vector128<float>  left, Vector128<float>  right, Vector128<float>  addend);
    public static Vector128<double> MultiplyAddNegatedEstimate (Vector128<double> left, Vector128<double> right, Vector128<double> addend);

    // Lane-select — i{8,16,32,64}x{16,8,4,2}.relaxed_laneselect.
    // Behavior of non-high mask bits is implementation-defined
    // (PackedSimd.BitwiseSelect examines every bit).
    public static Vector128<sbyte>  LaneSelectNative(Vector128<sbyte>  left, Vector128<sbyte>  right, Vector128<sbyte>  mask);
    public static Vector128<byte>   LaneSelectNative(Vector128<byte>   left, Vector128<byte>   right, Vector128<byte>   mask);
    public static Vector128<short>  LaneSelectNative(Vector128<short>  left, Vector128<short>  right, Vector128<short>  mask);
    public static Vector128<ushort> LaneSelectNative(Vector128<ushort> left, Vector128<ushort> right, Vector128<ushort> mask);
    public static Vector128<int>    LaneSelectNative(Vector128<int>    left, Vector128<int>    right, Vector128<int>    mask);
    public static Vector128<uint>   LaneSelectNative(Vector128<uint>   left, Vector128<uint>   right, Vector128<uint>   mask);
    public static Vector128<long>   LaneSelectNative(Vector128<long>   left, Vector128<long>   right, Vector128<long>   mask);
    public static Vector128<ulong>  LaneSelectNative(Vector128<ulong>  left, Vector128<ulong>  right, Vector128<ulong>  mask);

    // Min/Max — f{32x4,64x2}.relaxed_{min,max}.
    // NaN and signed-zero handling is implementation-defined
    // (PackedSimd.Min/Max are IEEE-conformant).
    public static Vector128<float>  MinNative(Vector128<float>  left, Vector128<float>  right);
    public static Vector128<float>  MaxNative(Vector128<float>  left, Vector128<float>  right);
    public static Vector128<double> MinNative(Vector128<double> left, Vector128<double> right);
    public static Vector128<double> MaxNative(Vector128<double> left, Vector128<double> right);

    // Q15 fixed-point multiply — i16x8.relaxed_q15mulr_s.
    // Saturation on 0x8000 * 0x8000 is implementation-defined
    // (PackedSimd.MultiplyRoundedSaturateQ15 always saturates).
    public static Vector128<short> MultiplyRoundedQ15Native(Vector128<short> left, Vector128<short> right);

    // Extended integer dot products — i16x8.relaxed_dot_i8x16_i7x16_s,
    //                                 i32x4.relaxed_dot_i8x16_i7x16_add_s.
    // Per the finished spec: `a` is signed, `b` is unsigned 7-bit. When any lane
    // of `b` has the high bit set, that lane's product is implementation-defined
    // (may be interpreted as signed or unsigned). The sum reduction is also
    // implementation-defined-saturating (may or may not saturate on overflow).
    public static Vector128<short> DotProductNative   (Vector128<sbyte> left, Vector128<byte> right);
    public static Vector128<int>   DotProductAddNative(Vector128<sbyte> left, Vector128<byte> right, Vector128<int> accumulator);
}
```
## Add BigInteger.ModInverse(BigInteger value, BigInteger modulus)

**Approved** | [#runtime/130021](https://github.com/dotnet/runtime/issues/130021#issuecomment-5415264164) | [Video](https://www.youtube.com/watch?v=SfsHPgvtk7c&t=1h48m34s)

* Given we have GCD and ModPow already, and LCM was used in the example, let's also add LeastCommonMultiple

```csharp
namespace System.Numerics
{
    public readonly partial struct BigInteger
    {
        public static BigInteger ModInverse(BigInteger value, BigInteger modulus);

        public static BigInteger LeastCommonMultiple(BigInteger left, BigInteger right);
    }
}
```
