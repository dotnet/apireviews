# API Review 01/06/2026

## Arm64: FEAT_SVE2: mask

**Approved** | [#runtime/94021](https://github.com/dotnet/runtime/issues/94021#issuecomment-3715976979) | [Video](https://www.youtube.com/watch?v=wqBZTcYfVwk&t=0h0m0s)


* The CreateWhileGreater- methods all need an {N}Bit suffix because they vary only by return type.
* For CreateWhileReadAfterWriteMask (and its inverse), consider renaming left/right to source/destination (if those terms are applicable)
* Should CreateWhileWriteAfterReadMask be "ReadOrWrite", or is "Read" sufficient?
* Changed -Lower to -Even (and -Upper to Odd)
* Changed -Odd (op) param to (value)
* Fixed the order of the parts: Saturating Extract Unsigned Narrowing => Extract Narrowing Saturate Unsigned (Saturate, not Saturating)

```csharp
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve2 : AdvSimd /// Feature: FEAT_SVE2  Category: mask
{
  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanMask{N}Bit(int left, int right); // WHILEGT

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanMask{N}Bit(long left, long right); // WHILEGT

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanMask{N}Bit(uint left, uint right); // WHILEHI

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanMask{N}Bit(ulong left, ulong right); // WHILEHI

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanOrEqualMask{N}Bit(int left, int right); // WHILEGE

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanOrEqualMask{N}Bit(long left, long right); // WHILEGE

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanOrEqualMask{N}Bit(uint left, uint right); // WHILEHS

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileGreaterThanOrEqualMask{N}Bit(ulong left, ulong right); // WHILEHS

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileReadAfterWriteMask(T* left, T* right); // WHILERW

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileWriteAfterReadMask(T* left, T* right); // WHILEWR

  /// T: sbyte, short, byte, ushort
  public static unsafe Vector<T> Match(Vector<T> mask, Vector<T> left, Vector<T> right); // MATCH
  
  /// T: sbyte, short, byte, ushort
  public static unsafe Vector<T> NoMatch(Vector<T> mask, Vector<T> left, Vector<T> right); // NMATCH

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ExtractNarrowingSaturateEven(Vector<T2> value); // SQXTNB or UQXTNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ExtractNarrowingSaturateOdd(Vector<T> even, Vector<T2> value); // SQXTNT or UQXTNT

  /// T: [byte, short], [ushort, int], [uint, long]
  public static unsafe Vector<T> ExtractNarrowingSaturateUnsignedEven(Vector<T2> value); // SQXTUNB

  /// T: [byte, short], [ushort, int], [uint, long]
  public static unsafe Vector<T> ExtractNarrowingSaturateUnsignedOdd(Vector<T> even, Vector<T2> value); // SQXTUNT
}
```
## Arm64: FEAT_SVE_AES

**Approved** | [#runtime/94423](https://github.com/dotnet/runtime/issues/94423#issuecomment-3716024455) | [Video](https://www.youtube.com/watch?v=wqBZTcYfVwk&t=1h9m59s)

* Changed -Lower to -Even (and -Upper to Odd)
* Left out the optional ones, as the Vector.Create(right) should be hoisted outside the loop, the optional calls are an anti-pattern.

```csharp
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class SveAes : AdvSimd /// Feature: FEAT_SVE_AES
{

  public static unsafe Vector<byte> InverseMixColumns(Vector<byte> value); // AESIMC

  public static unsafe Vector<byte> MixColumns(Vector<byte> value); // AESMC

  public static unsafe Vector<byte> Decrypt(Vector<byte> left, Vector<byte> right); // AESD

  public static unsafe Vector<byte> Encrypt(Vector<byte> left, Vector<byte> right); // AESE

  public static unsafe Vector<ulong> PolynomialMultiplyWideningEven(Vector<ulong> left, Vector<ulong> right); // PMULLB

  public static unsafe Vector<ulong> PolynomialMultiplyWideningOdd(Vector<ulong> left, Vector<ulong> right); // PMULLT
}
  /// total method signatures: 6

```
## Arm64: FEAT_SHA3

**Approved** | [#runtime/98692](https://github.com/dotnet/runtime/issues/98692#issuecomment-3716088520) | [Video](https://www.youtube.com/watch?v=wqBZTcYfVwk&t=1h25m1s)


* BitwiseRotateLeftBy1AndXor(a, b) => BitwiseRotateLeftBy1AndXor(xor, rol1)

```csharp
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sha3 : AdvSimd /// Feature: FEAT_SHA3
{

  /// T: byte, ushort, uint, ulong, sbyte, short, int, long
  public static unsafe Vector128<T> BitwiseClearXor(Vector128<T> xor, Vector128<T> value, Vector128<T> mask); // BCAX

  public static unsafe Vector128<ulong> BitwiseRotateLeftBy1AndXor(Vector128<ulong> xor, Vector128<ulong> rol1); // RAX1

  /// T: byte, ushort, uint, ulong, sbyte, short, int, long
  public static unsafe Vector128<T> Xor(Vector128<T> value1, Vector128<T> value2, Vector128<T> value3); // EOR3

  public static unsafe Vector128<ulong> XorRotateRight(Vector128<ulong> left, Vector128<ulong> right, [ConstantExpected] byte count); // XAR

  /// total method signatures: 4
}

```
## Arm64: FEAT_SVE_SHA3

**Approved** | [#runtime/94425](https://github.com/dotnet/runtime/issues/94425#issuecomment-3716098838) | [Video](https://www.youtube.com/watch?v=wqBZTcYfVwk&t=1h46m13s)


* Added the missing IsSupported property
* Corrected the base type (AdvSimd => Sve)
* BitwiseRotateLeftBy1AndXor(left, right) => BitwiseRotateLeftBy1AndXor(xor, rol1)

```csharp
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class SveSha3 : Sve /// Feature: FEAT_SVE_SHA3
{
  public static bool IsSupported { get; }

  /// T: long, ulong
  public static unsafe Vector<T> BitwiseRotateLeftBy1AndXor(Vector<T> xor, Vector<T> rol1); // RAX1
}
```
