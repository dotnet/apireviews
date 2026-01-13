# API Review 01/13/2026

## Arm64: FEAT_SVE_SM4

**Approved** | [#runtime/94426](https://github.com/dotnet/runtime/issues/94426#issuecomment-3745750240)

* Gave the parameters better names than left/right.
* Removed the "Sm4" method prefixes since it's in the ISA classname.  That's consistent with x86.Aes.Decrypt and friends.

```csharp
namespace System.Runtime.Intrinsics.Arm;

public abstract class SveSm4 : AdvSimd /// Feature: FEAT_SVE_SM4
{
  public static unsafe Vector<uint> Encode(Vector<uint> value, Vector<uint> roundKeys); // SM4E

  public static unsafe Vector<uint> KeyUpdate(Vector<uint> value, Vector<uint> constant); // SM4EKEY
}
```
## Arm64: FEAT_SM4

**Approved** | [#runtime/98696](https://github.com/dotnet/runtime/issues/98696#issuecomment-3745754259)

Made consistent with the SVE ones.

```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sm4 : AdvSimd /// Feature: FEAT_SM4
{

  public static unsafe Vector128<uint> Encode(Vector128<uint> value, Vector128<uint> roundKeys); // SM4E

  public static unsafe Vector128<uint> KeyUpdate(Vector128<uint> value, Vector128<uint> constant); // SM4EKEY

  /// total method signatures: 2

}
```
## SVE2 Rename MultiplyDoublingWideningAndSubtractSaturateOdd

**Approved** | [#runtime/121980](https://github.com/dotnet/runtime/issues/121980#issuecomment-3745780980)

* Adding the "BySelectedScalar" infix seems appropriate.

```diff
-public static unsafe Vector<T> MultiplyDoublingWideningAndSubtractSaturateOdd(Vector<T> minuend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMLSLT // MOVPRFX
+public static unsafe Vector<T> MultiplyDoublingWideningBySelectedScalarAndSubtractSaturateOdd(Vector<T> minuend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMLSLT // MOVPRFX
```
## SVE2 API for UQSHRNB and UQSHRNT

**Approved** | [#runtime/122022](https://github.com/dotnet/runtime/issues/122022#issuecomment-3745792068)


* Looks good as proposed

```c#
namespace System.Runtime.Intrinsics.Arm;

 /// T: [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T>  ShiftRightLogicalNarrowingSaturateEven(Vector<T2> value, [ConstantExpected] byte count); // UQSHRNB

  /// T: [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T>  ShiftRightLogicalNarrowingSaturateOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // UQSHRNT
```
## SVE2 Add extra parameter to ConvertToSingleOdd APIs

**Approved** | [#runtime/122031](https://github.com/dotnet/runtime/issues/122031#issuecomment-3745810315)


* Looks good as proposed

```c#
namespace System.Runtime.Intrinsics.Arm;

public partial class Sve
{
  public static unsafe Vector<float> ConvertToSingleEvenRoundToOdd(Vector<double> value); // FCVTX // predicated, MOVPRFX

  public static unsafe Vector<float> ConvertToSingleOddRoundToOdd(Vector<float> even, Vector<double> value); // FCVTXNT // predicated
}
```
## SVE2 Scatterstores are non-temporal

**Approved** | [#runtime/122033](https://github.com/dotnet/runtime/issues/122033#issuecomment-3745866807)


* The updates look good as proposed.

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve2 : AdvSimd /// Feature: FEAT_SVE2  Category: scatterstores
{

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter16BitNarrowingNonTemporal(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // STNT1H

  /// T: uint, ulong
  public static unsafe void Scatter16BitNarrowingNonTemporal(Vector<T> mask, Vector<T> addresses, Vector<T> data); // STNT1H

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter16BitWithByteOffsetsNarrowingNonTemporal(Vector<T> mask, short* address, Vector<T2> offsets, Vector<T> data); // STNT1H

  /// T: uint, ulong
  public static unsafe void Scatter16BitWithByteOffsetsNarrowingNonTemporal(Vector<T> mask, ushort* address, Vector<T> offsets, Vector<T> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowingNonTemporal(Vector<long> mask, short* address, Vector<long> offsets, Vector<long> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowingNonTemporal(Vector<ulong> mask, ushort* address, Vector<long> offsets, Vector<ulong> data); // STNT1H

  public static unsafe void Scatter16BitNarrowingNonTemporal(Vector<long> mask, short* address, Vector<long> indexes, Vector<long> data); // STNT1H

  public static unsafe void Scatter16BitNarrowingNonTemporal(Vector<ulong> mask, ushort* address, Vector<long> indexes, Vector<ulong> data); // STNT1H

  public static unsafe void Scatter16BitNarrowingNonTemporal(Vector<long> mask, short* address, Vector<ulong> indexes, Vector<long> data); // STNT1H

  public static unsafe void Scatter16BitNarrowingNonTemporal(Vector<ulong> mask, ushort* address, Vector<ulong> indexes, Vector<ulong> data); // STNT1H

  public static unsafe void Scatter32BitNarrowingNonTemporal(Vector<long> mask, Vector<ulong> addresses, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitNarrowingNonTemporal(Vector<ulong> mask, Vector<ulong> addresses, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowingNonTemporal(Vector<long> mask, int* address, Vector<long> offsets, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowingNonTemporal(Vector<ulong> mask, uint* address, Vector<long> offsets, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowingNonTemporal(Vector<long> mask, int* address, Vector<ulong> offsets, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowingNonTemporal(Vector<ulong> mask, uint* address, Vector<ulong> offsets, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitNarrowingNonTemporal(Vector<long> mask, int* address, Vector<long> indexes, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitNarrowingNonTemporal(Vector<ulong> mask, uint* address, Vector<long> indexes, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitNarrowingNonTemporal(Vector<long> mask, int* address, Vector<ulong> indexes, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitNarrowingNonTemporal(Vector<ulong> mask, uint* address, Vector<ulong> indexes, Vector<ulong> data); // STNT1W

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter8BitNarrowingNonTemporal(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // STNT1B

  /// T: uint, ulong
  public static unsafe void Scatter8BitNarrowingNonTemporal(Vector<T> mask, Vector<T> addresses, Vector<T> data); // STNT1B

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter8BitWithByteOffsetsNarrowingNonTemporal(Vector<T> mask, sbyte* address, Vector<T2> offsets, Vector<T> data); // STNT1B

  /// T: uint, ulong
  public static unsafe void Scatter8BitWithByteOffsetsNarrowingNonTemporal(Vector<T> mask, byte* address, Vector<T> offsets, Vector<T> data); // STNT1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowingNonTemporal(Vector<long> mask, sbyte* address, Vector<long> offsets, Vector<long> data); // STNT1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowingNonTemporal(Vector<ulong> mask, byte* address, Vector<long> offsets, Vector<ulong> data); // STNT1B

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe void ScatterNonTemporal(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // STNT1W or STNT1D

  /// T: uint, ulong
  public static unsafe void ScatterNonTemporal(Vector<T> mask, Vector<T> addresses, Vector<T> data); // STNT1W or STNT1D

  /// T: [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe void ScatterWithByteOffsetsNonTemporal(Vector<T> mask, T* address, Vector<T2> offsets, Vector<T> data); // STNT1W or STNT1D

  /// T: uint, long, ulong
  public static unsafe void ScatterWithByteOffsetsNonTemporal(Vector<T> mask, T* address, Vector<T> offsets, Vector<T> data); // STNT1W or STNT1D

  /// T: [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe void ScatterNonTemporal(Vector<T> mask, T* address, Vector<T2> indexes, Vector<T> data); // STNT1D

  /// T: long, ulong
  public static unsafe void ScatterNonTemporal(Vector<T> mask, T* address, Vector<T> indexes, Vector<T> data); // STNT1D

  /// total method signatures: 32

}
```
## Add defaulted offset to the SVE load/store APIs

**Rejected** | [#runtime/115241](https://github.com/dotnet/runtime/issues/115241#issuecomment-3745886561)

These API changes don't seem necessary given @jkotas' comment about just ensmartening the JIT here.

See also #108320.
## ARM64 SVE: Add additional types for the mask APIs

**Approved** | [#runtime/108233](https://github.com/dotnet/runtime/issues/108233#issuecomment-3745971622)

* Changed the suffix Int8 to SByte and UInt8 to Byte.
* Otherwise, looks good as proposed.

```c#
namespace System.Runtime.Intrinsics.Arm;

public partial class Sve
{

  /// T: float, double
  public static unsafe Vector<T> CreateBreakAfterMask(Vector<T> totalMask, Vector<T> fromMask); // BRKA // predicated

  /// T: float, double
  public static unsafe Vector<T> CreateBreakAfterPropagateMask(Vector<T> mask, Vector<T> left, Vector<T> right); // BRKPA

  /// T: float, double
  public static unsafe Vector<T> CreateBreakBeforeMask(Vector<T> totalMask, Vector<T> fromMask); // BRKB // predicated

  /// T: float, double
  public static unsafe Vector<T> CreateBreakBeforePropagateMask(Vector<T> mask, Vector<T> left, Vector<T> right); // BRKPB

  /// T: float, double
  public static unsafe Vector<T> CreateBreakPropagateMask(Vector<T> totalMask, Vector<T> fromMask); // BRKN // predicated

  /// T: float, double
  public static unsafe Vector<T> CreateMaskForFirstActiveElement(Vector<T> totalMask, Vector<T> fromMask); // PFIRST

  /// T: float, double
  public static unsafe bool TestAnyTrue(Vector<T> leftMask, Vector<T> rightMask); // PTEST

  /// T: float, double
  public static unsafe bool TestFirstTrue(Vector<T> leftMask, Vector<T> rightMask); // PTEST

  /// T: float, double
  public static unsafe bool TestLastTrue(Vector<T> leftMask, Vector<T> rightMask); // PTEST

  public static Vector<float> CreateWhileLessThanMaskSingle(int left, int right);
  public static Vector<float> CreateWhileLessThanMaskSingle(long left, long right);
  public static Vector<float> CreateWhileLessThanMaskSingle(uint left, uint right);
  public static Vector<float> CreateWhileLessThanMaskSingle(ulong left, ulong right);

  public static Vector<double> CreateWhileLessThanMaskDouble(int left, int right);
  public static Vector<double> CreateWhileLessThanMaskDouble(long left, long right);
  public static Vector<double> CreateWhileLessThanMaskDouble(uint left, uint right);
  public static Vector<double> CreateWhileLessThanMaskDouble(ulong left, ulong right);

  public static unsafe Vector<short> CreateWhileLessThanMaskInt16(int left, int right); // WHILELT
  public static unsafe Vector<short> CreateWhileLessThanMaskInt16(long left, long right); // WHILELT
  public static unsafe Vector<short> CreateWhileLessThanMaskInt16(uint left, uint right); // WHILELO
  public static unsafe Vector<short> CreateWhileLessThanMaskInt16(ulong left, ulong right); // WHILELO

  public static unsafe Vector<int> CreateWhileLessThanMaskInt32(int left, int right); // WHILELT
  public static unsafe Vector<int> CreateWhileLessThanMaskInt32(long left, long right); // WHILELT
  public static unsafe Vector<int> CreateWhileLessThanMaskInt32(uint left, uint right); // WHILELO
  public static unsafe Vector<int> CreateWhileLessThanMaskInt32(ulong left, ulong right); // WHILELO

  public static unsafe Vector<long> CreateWhileLessThanMaskInt64(int left, int right); // WHILELT
  public static unsafe Vector<long> CreateWhileLessThanMaskInt64(long left, long right); // WHILELT
  public static unsafe Vector<long> CreateWhileLessThanMaskInt64(uint left, uint right); // WHILELO
  public static unsafe Vector<long> CreateWhileLessThanMaskInt64(ulong left, ulong right); // WHILELO

  public static unsafe Vector<sbyte> CreateWhileLessThanMaskSByte(int left, int right); // WHILELT
  public static unsafe Vector<sbyte> CreateWhileLessThanMaskSByte(long left, long right); // WHILELT
  public static unsafe Vector<sbyte> CreateWhileLessThanMaskSByte(uint left, uint right); // WHILELO
  public static unsafe Vector<sbyte> CreateWhileLessThanMaskSByte(ulong left, ulong right); // WHILELO

  public static unsafe Vector<ushort> CreateWhileLessThanMaskUInt16(int left, int right); // WHILELT
  public static unsafe Vector<ushort> CreateWhileLessThanMaskUInt16(long left, long right); // WHILELT
  public static unsafe Vector<ushort> CreateWhileLessThanMaskUInt16(uint left, uint right); // WHILELO
  public static unsafe Vector<ushort> CreateWhileLessThanMaskUInt16(ulong left, ulong right); // WHILELO

  public static unsafe Vector<uint> CreateWhileLessThanMaskUInt32(int left, int right); // WHILELT
  public static unsafe Vector<uint> CreateWhileLessThanMaskUInt32(long left, long right); // WHILELT
  public static unsafe Vector<uint> CreateWhileLessThanMaskUInt32(uint left, uint right); // WHILELO
  public static unsafe Vector<uint> CreateWhileLessThanMaskUInt32(ulong left, ulong right); // WHILELO

  public static unsafe Vector<ulong> CreateWhileLessThanMaskUInt64(int left, int right); // WHILELT
  public static unsafe Vector<ulong> CreateWhileLessThanMaskUInt64(long left, long right); // WHILELT
  public static unsafe Vector<ulong> CreateWhileLessThanMaskUInt64(uint left, uint right); // WHILELO
  public static unsafe Vector<ulong> CreateWhileLessThanMaskUInt64(ulong left, ulong right); // WHILELO

  public static unsafe Vector<byte> CreateWhileLessThanMaskByte(int left, int right); // WHILELT
  public static unsafe Vector<byte> CreateWhileLessThanMaskByte(long left, long right); // WHILELT
  public static unsafe Vector<byte> CreateWhileLessThanMaskByte(uint left, uint right); // WHILELO
  public static unsafe Vector<byte> CreateWhileLessThanMaskByte(ulong left, ulong right); // WHILELO

  public static Vector<float> CreateWhileLessThanOrEqualMaskSingle(int left, int right);
  public static Vector<float> CreateWhileLessThanOrEqualMaskSingle(long left, long right);
  public static Vector<float> CreateWhileLessThanOrEqualMaskSingle(uint left, uint right);
  public static Vector<float> CreateWhileLessThanOrEqualMaskSingle(ulong left, ulong right);

  public static Vector<double> CreateWhileLessThanOrEqualMaskDouble(int left, int right);
  public static Vector<double> CreateWhileLessThanOrEqualMaskDouble(long left, long right);
  public static Vector<double> CreateWhileLessThanOrEqualMaskDouble(uint left, uint right);
  public static Vector<double> CreateWhileLessThanOrEqualMaskDouble(ulong left, ulong right);

  public static unsafe Vector<ushort> CreateWhileLessThanOrEqualMaskUInt16(int left, int right); // WHILELE
  public static unsafe Vector<ushort> CreateWhileLessThanOrEqualMaskUInt16(long left, long right); // WHILELE
  public static unsafe Vector<ushort> CreateWhileLessThanOrEqualMaskUInt16(uint left, uint right); // WHILELS
  public static unsafe Vector<ushort> CreateWhileLessThanOrEqualMaskUInt16(ulong left, ulong right); // WHILELS

  public static unsafe Vector<short> CreateWhileLessThanOrEqualMaskInt16(int left, int right); // WHILELE
  public static unsafe Vector<short> CreateWhileLessThanOrEqualMaskInt16(long left, long right); // WHILELE
  public static unsafe Vector<short> CreateWhileLessThanOrEqualMaskInt16(uint left, uint right); // WHILELS
  public static unsafe Vector<short> CreateWhileLessThanOrEqualMaskInt16(ulong left, ulong right); // WHILELS

  public static unsafe Vector<int> CreateWhileLessThanOrEqualMaskInt32(int left, int right); // WHILELE
  public static unsafe Vector<int> CreateWhileLessThanOrEqualMaskInt32(long left, long right); // WHILELE
  public static unsafe Vector<int> CreateWhileLessThanOrEqualMaskInt32(uint left, uint right); // WHILELS
  public static unsafe Vector<int> CreateWhileLessThanOrEqualMaskInt32(ulong left, ulong right); // WHILELS

  public static unsafe Vector<long> CreateWhileLessThanOrEqualMaskInt64(int left, int right); // WHILELE
  public static unsafe Vector<long> CreateWhileLessThanOrEqualMaskInt64(long left, long right); // WHILELE
  public static unsafe Vector<long> CreateWhileLessThanOrEqualMaskInt64(uint left, uint right); // WHILELS
  public static unsafe Vector<long> CreateWhileLessThanOrEqualMaskInt64(ulong left, ulong right); // WHILELS

  public static unsafe Vector<sbyte> CreateWhileLessThanOrEqualMaskSByte(int left, int right); // WHILELE
  public static unsafe Vector<sbyte> CreateWhileLessThanOrEqualMaskSByte(long left, long right); // WHILELE
  public static unsafe Vector<sbyte> CreateWhileLessThanOrEqualMaskSByte(uint left, uint right); // WHILELS
  public static unsafe Vector<sbyte> CreateWhileLessThanOrEqualMaskSByte(ulong left, ulong right); // WHILELS

  public static unsafe Vector<uint> CreateWhileLessThanOrEqualMaskUInt32(int left, int right); // WHILELE
  public static unsafe Vector<uint> CreateWhileLessThanOrEqualMaskUInt32(long left, long right); // WHILELE
  public static unsafe Vector<uint> CreateWhileLessThanOrEqualMaskUInt32(uint left, uint right); // WHILELS
  public static unsafe Vector<uint> CreateWhileLessThanOrEqualMaskUInt32(ulong left, ulong right); // WHILELS

  public static unsafe Vector<ulong> CreateWhileLessThanOrEqualMaskUInt64(int left, int right); // WHILELE
  public static unsafe Vector<ulong> CreateWhileLessThanOrEqualMaskUInt64(long left, long right); // WHILELE
  public static unsafe Vector<ulong> CreateWhileLessThanOrEqualMaskUInt64(uint left, uint right); // WHILELS
  public static unsafe Vector<ulong> CreateWhileLessThanOrEqualMaskUInt64(ulong left, ulong right); // WHILELS

  public static unsafe Vector<byte> CreateWhileLessThanOrEqualMaskByte(int left, int right); // WHILELE
  public static unsafe Vector<byte> CreateWhileLessThanOrEqualMaskByte(long left, long right); // WHILELE
  public static unsafe Vector<byte> CreateWhileLessThanOrEqualMaskByte(uint left, uint right); // WHILELS
  public static unsafe Vector<byte> CreateWhileLessThanOrEqualMaskByte(ulong left, ulong right); // WHILELS


  public static unsafe Vector<T> GetFfrSingle(); // RDFFR // predicated
  public static unsafe Vector<T> GetFfrDouble(); // RDFFR // predicated

  /// T: float, double
  public static unsafe void SetFfr(Vector<T> value); // WRFFR
}
```
## Consider adding `Uri.UriSchemeData`

**Approved** | [#runtime/122020](https://github.com/dotnet/runtime/issues/122020#issuecomment-3746003908)

* Looks good as proposed

```c#
namespace System
{
    public partial class Uri
    {
        public static readonly string UriSchemeData = "data";
    }
}
```
## Base64 parity with Base64Url

**Approved** | [#runtime/121454](https://github.com/dotnet/runtime/issues/121454#issuecomment-3746039491)


* Looks good as proposed.

```c#
namespace System.Buffers.Text;

public static partial class Base64
{
    public static byte[] DecodeFromChars(System.ReadOnlySpan<char> source);
    public static int DecodeFromChars(System.ReadOnlySpan<char> source, System.Span<byte> destination);
    public static OperationStatus DecodeFromChars(System.ReadOnlySpan<char> source, System.Span<byte> destination, out int charsConsumed, out int bytesWritten, bool isFinalBlock = true);

    public static byte[] DecodeFromUtf8(System.ReadOnlySpan<byte> source);
    public static int DecodeFromUtf8(System.ReadOnlySpan<byte> source, System.Span<byte> destination);

    public static char[] EncodeToChars(System.ReadOnlySpan<byte> source);
    public static int EncodeToChars(System.ReadOnlySpan<byte> source, System.Span<char> destination);
    public static OperationStatus EncodeToChars(System.ReadOnlySpan<byte> source, System.Span<char> destination, out int bytesConsumed, out int charsWritten, bool isFinalBlock = true);

    public static string EncodeToString(System.ReadOnlySpan<byte> source);
    public static byte[] EncodeToUtf8(System.ReadOnlySpan<byte> source);
    public static int EncodeToUtf8(System.ReadOnlySpan<byte> source, System.Span<byte> destination);

    public static bool TryDecodeFromChars(System.ReadOnlySpan<char> source, System.Span<byte> destination, out int bytesWritten);
    public static bool TryDecodeFromUtf8(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
    public static bool TryEncodeToChars(System.ReadOnlySpan<byte> source, System.Span<char> destination, out int charsWritten);
    public static bool TryEncodeToUtf8(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
    public static bool TryEncodeToUtf8InPlace(System.Span<byte> buffer, int dataLength, out int bytesWritten);

    public static int GetEncodedLength(int bytesLength);
    public static int GetMaxDecodedLength(int base64Length);
}
```
## adding C# option to the [StringSyntaxAttribute]

**Approved** | [#runtime/122604](https://github.com/dotnet/runtime/issues/122604#issuecomment-3746093451)

* Remember that `const` is forever, so make sure "C#" is the better value over "CSharp" or "c#" or "csharp"
* We've gone ahead and approved F# and VB in this same approval, but they can be omitted if there's no support.

```c#
namespace System.Diagnostics.CodeAnalysis;

public sealed class StringSyntaxAttribute : Attribute
{
    public const string CSharp = "C#";
    public const string FSharp = "F#";
    public const string VisualBasic = "VB.NET";
}
```
## Suggest `String.Concat` instead of `String.Join` with an empty separator

**Rejected** | [#runtime/122889](https://github.com/dotnet/runtime/issues/122889#issuecomment-3746140700)

In API Review we don't believe that this is actually valueable as an analyzer (is there significant enough performance difference?).  The JIT could pattern match the call over into Concat, and/or we could change Join to detect a zero-length separator and call Concat... both of those are better than an analyzer.

If making callers change their code is actually the better answer, this could be reopened with more justification.
