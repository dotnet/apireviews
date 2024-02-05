# API Review 02/01/2024

## Arm64: FEAT_SVE: gather loads

**Approved** | [#runtime/94007](https://github.com/dotnet/runtime/issues/94007#issuecomment-1922024205)

* `SvePrefetchType` should be a top-level type so it can be shared with later ISAs
  - The names should match .NET naming, for example, dropping the `SV_` prefix and the names should be expanded, e.g. instead `SV_PLDL1KEEP` it would `LoadL1Temporal` and `SV_PLDL1STRM` would be `LoadL1NonTemporal`
* `GatherPrefetchInt64` and `GatherPrefetch64Bit`
  - We should remove all overloads that take `long index` because there is already a vector based index and we can use `Vector.Create()`
  - Some of the generic type arguments listed in the comments are wrong. @tannergooding will know which one ;-)
* `GatherVectorXxx`
  - `T* base` should be `T* address`
  - `Vector<T> bases` should be `Vector<T> addresses`
  - If the last parameter is `offsets` should be named with a suffix like `WithByteOffsets`.  For complex names, like `GatherVectorByteZeroExtend` that becomes an infix (`GatherVectorByteWithByteOffsetsZeroExtend`)
  - We should remove the `long offset` ones in favor of `Vector.Create()`
  - Consider if `GatherVectorInt16SignExtend` should be `GatherVector16BitSignExtend` (it seems like "no", but it came up)
  - `GatherVectorInt16ZeroExtend` and other ZeroExtend methods should say `UInt16` (et al) not `Int16`
  

```C#
namespace System.Runtime.Intrinsics.Arm;

public enum SvePrefetchType
{
    SV_PLDL1KEEP = 0,  
    SV_PLDL1STRM = 1,  
    SV_PLDL2KEEP = 2,  
    SV_PLDL2STRM = 3,  
    SV_PLDL3KEEP = 4,  
    SV_PLDL3STRM = 5,  
    SV_PSTL1KEEP = 8,  
    SV_PSTL1STRM = 9,  
    SV_PSTL2KEEP = 10, 
    SV_PSTL2STRM = 11, 
    SV_PSTL3KEEP = 12, 
    SV_PSTL3STRM = 13  
};

public abstract partial class Sve : AdvSimd
{
  /// T: [byte, uint], [byte, ulong]
  public static unsafe void GatherPrefetchBytes(Vector<T> mask, Vector<T2> bases, [ConstantExpected] SvePrefetchType op);

  /// T: [byte, int], [byte, uint], [byte, long], [byte, ulong]
  public static unsafe void GatherPrefetchBytes(Vector<T> mask, void* base, Vector<T2> offsets, [ConstantExpected] SvePrefetchType op);

  /// T: [byte, uint], [byte, ulong]
  public static unsafe void GatherPrefetchBytes(Vector<T> mask, Vector<T2> bases, long offset, [ConstantExpected] SvePrefetchType op);

  /// T: [ushort, uint], [ushort, ulong]
  public static unsafe void GatherPrefetchInt16(Vector<T> mask, Vector<T2> bases, [ConstantExpected] SvePrefetchType op);

  /// T: [ushort, int], [ushort, uint], [ushort, long], [ushort, ulong]
  public static unsafe void GatherPrefetchInt16(Vector<T> mask, void* base, Vector<T2> indices, [ConstantExpected] SvePrefetchType op);

  /// T: [ushort, uint], [ushort, ulong]
  public static unsafe void GatherPrefetchInt16(Vector<T> mask, Vector<T2> bases, long index, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt32(Vector<uint> mask, Vector<uint> bases, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt32(Vector<uint> mask, Vector<ulong> bases, [ConstantExpected] SvePrefetchType op);

  /// T: [uint, int], [uint, long], [uint, ulong]
  public static unsafe void GatherPrefetchInt32(Vector<T> mask, void* base, Vector<T2> indices, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt32(Vector<uint> mask, void* base, Vector<uint> indices, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt32(Vector<uint> mask, Vector<uint> bases, long index, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt32(Vector<uint> mask, Vector<ulong> bases, long index, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt64(Vector<ulong> mask, Vector<uint> bases, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt64(Vector<ulong> mask, Vector<ulong> bases, [ConstantExpected] SvePrefetchType op);

  /// T: [ulong, int], [ulong, uint], [ulong, long]
  public static unsafe void GatherPrefetchInt64(Vector<T> mask, void* base, Vector<T2> indices, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt64(Vector<ulong> mask, void* base, Vector<ulong> indices, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt64(Vector<ulong> mask, Vector<uint> bases, long index, [ConstantExpected] SvePrefetchType op);

  public static unsafe void GatherPrefetchInt64(Vector<ulong> mask, Vector<ulong> bases, long index, [ConstantExpected] SvePrefetchType op);

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVector(Vector<T> mask, Vector<T2> bases);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVector(Vector<T> mask, Vector<T> bases);

  /// T: [float, int], [uint, int], [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVector(Vector<T> mask, T* base, Vector<T2> offsets);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVector(Vector<T> mask, T* base, Vector<T> offsets);

  /// T: [float, int], [uint, int], [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVector(Vector<T> mask, T* base, Vector<T2> indices);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVector(Vector<T> mask, T* base, Vector<T> indices);

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVector(Vector<T> mask, Vector<T2> bases, long offset);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVector(Vector<T> mask, Vector<T> bases, long offset);

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVector(Vector<T> mask, Vector<T2> bases, long index);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVector(Vector<T> mask, Vector<T> bases, long index);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorByteZeroExtend(Vector<T> mask, Vector<T2> bases);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorByteZeroExtend(Vector<T> mask, Vector<T> bases);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorByteZeroExtend(Vector<T> mask, byte* base, Vector<T> offsets);

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorByteZeroExtend(Vector<T> mask, byte* base, Vector<T2> offsets);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorByteZeroExtend(Vector<T> mask, Vector<T2> bases, long offset);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorByteZeroExtend(Vector<T> mask, Vector<T> bases, long offset);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, Vector<T2> bases);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, Vector<T> bases);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, short* base, Vector<T> offsets);

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, short* base, Vector<T2> offsets);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, short* base, Vector<T> indices);

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, short* base, Vector<T2> indices);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, Vector<T2> bases, long offset);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, Vector<T> bases, long offset);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, Vector<T2> bases, long index);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtend(Vector<T> mask, Vector<T> bases, long index);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, Vector<T2> bases);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, Vector<T> bases);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, ushort* base, Vector<T> offsets);

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, ushort* base, Vector<T2> offsets);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, ushort* base, Vector<T> indices);

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, ushort* base, Vector<T2> indices);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, Vector<T2> bases, long offset);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, Vector<T> bases, long offset);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, Vector<T2> bases, long index);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16ZeroExtend(Vector<T> mask, Vector<T> bases, long index);

  public static unsafe Vector<long> GatherVectorInt32SignExtend(Vector<long> mask, Vector<ulong> bases);

  public static unsafe Vector<ulong> GatherVectorInt32SignExtend(Vector<ulong> mask, Vector<ulong> bases);

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorInt32SignExtend(Vector<T> mask, int* base, Vector<T> offsets);

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt32SignExtend(Vector<T> mask, int* base, Vector<T2> offsets);

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorInt32SignExtend(Vector<T> mask, int* base, Vector<T> indices);

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt32SignExtend(Vector<T> mask, int* base, Vector<T2> indices);

  public static unsafe Vector<long> GatherVectorInt32SignExtend(Vector<long> mask, Vector<ulong> bases, long offset);

  public static unsafe Vector<ulong> GatherVectorInt32SignExtend(Vector<ulong> mask, Vector<ulong> bases, long offset);

  public static unsafe Vector<long> GatherVectorInt32SignExtend(Vector<long> mask, Vector<ulong> bases, long index);

  public static unsafe Vector<ulong> GatherVectorInt32SignExtend(Vector<ulong> mask, Vector<ulong> bases, long index);

  public static unsafe Vector<long> GatherVectorInt32ZeroExtend(Vector<long> mask, Vector<ulong> bases);

  public static unsafe Vector<ulong> GatherVectorInt32ZeroExtend(Vector<ulong> mask, Vector<ulong> bases);

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorInt32ZeroExtend(Vector<T> mask, uint* base, Vector<T> offsets);

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt32ZeroExtend(Vector<T> mask, uint* base, Vector<T2> offsets);

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorInt32ZeroExtend(Vector<T> mask, uint* base, Vector<T> indices);

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt32ZeroExtend(Vector<T> mask, uint* base, Vector<T2> indices);

  public static unsafe Vector<long> GatherVectorInt32ZeroExtend(Vector<long> mask, Vector<ulong> bases, long offset);

  public static unsafe Vector<ulong> GatherVectorInt32ZeroExtend(Vector<ulong> mask, Vector<ulong> bases, long offset);

  public static unsafe Vector<long> GatherVectorInt32ZeroExtend(Vector<long> mask, Vector<ulong> bases, long index);

  public static unsafe Vector<ulong> GatherVectorInt32ZeroExtend(Vector<ulong> mask, Vector<ulong> bases, long index);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtend(Vector<T> mask, Vector<T2> bases);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorSByteSignExtend(Vector<T> mask, Vector<T> bases);

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorSByteSignExtend(Vector<T> mask, sbyte* base, Vector<T> offsets);

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtend(Vector<T> mask, sbyte* base, Vector<T2> offsets);

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtend(Vector<T> mask, Vector<T2> bases, long offset);

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorSByteSignExtend(Vector<T> mask, Vector<T> bases, long offset);
}
```
## Arm64: FEAT_SVE: bit manipulation

**Approved** | [#runtime/94008](https://github.com/dotnet/runtime/issues/94008#issuecomment-1922063618)

* CreateSeries should be added to the Vector types, not the ISA.
  * Though we renamed it to CreateSequence
* And now that CreateSeries is on the Vector types, the `Vector<T>` types can gain a static get-only property called `Indices` equivalent to `CreateSequence(0, 1)`
* `DuplicateSelectedScalarToVector(T value)` has been removed in favor of Vector.Create
* `DuplicateSelectedScalarToVector(Vector<T> data, uint index);` and friends are squished down to `DuplicateSelectedScalarToVector(Vector<T> data, [ConstantExpected] byte index);`
* `ReverseInt16WithinElements` => `ReverseElement16` to match the same idea in other architectures/ISAs.
  * And for 32 and 8 as well. 

```C#
namespace System.Numerics;

partial class Vector
{
  public static Vector<T> CreateSequence<T>(T start, T step);
}

partial class Vector64
{
  public static Vector64 CreateSequence<T>(T start, T step);
}

partial class Vector128
{
  public static Vector128 CreateSequence<T>(T start, T step);
}

partial class Vector256
{
  public static Vector256 CreateSequence<T>(T start, T step);
}

partial class Vector256
{
  public static Vector256 CreateSequence<T>(T start, T step);
}

partial class Vector<T>
{
  public static Vector<T> Indices { get; }
}

partial class Vector64<T>
{
  public static Vector64<T> Indices { get; }
}

partial class Vector128<T>
{
  public static Vector64<T> Indices { get; }
}

partial class Vector256<T>
{
  public static Vector64<T> Indices { get; }
}

partial class Vector256<T>
{
  public static Vector64<T> Indices { get; }
}

namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: bitmanipulate
{
  /// T: double, long, ulong, float, sbyte, short, int, byte, ushort, uint
  public static unsafe Vector<T> DuplicateSelectedScalarToVector(Vector<T> data, [ConstantExpected] byte index); // DUP or TBL

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ReverseBits(Vector<T> value); // RBIT // predicated, MOVPRFX

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> ReverseElement8(Vector<T> value); // REVB // predicated, MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ReverseElement(Vector<T> value); // REV

  /// T: int, long, uint, ulong
  public static unsafe Vector<T> ReverseElement16(Vector<T> value); // REVH // predicated, MOVPRFX

  /// T: long, ulong
  public static unsafe Vector<T> ReverseElement32(Vector<T> value); // REVW // predicated, MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Splice(Vector<T> mask, Vector<T> left, Vector<T> right); // SPLICE // MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> TransposeEven(Vector<T> left, Vector<T> right); // TRN1

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> TransposeOdd(Vector<T> left, Vector<T> right); // TRN2

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> UnzipEven(Vector<T> left, Vector<T> right); // UZP1

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> UnzipOdd(Vector<T> left, Vector<T> right); // UZP2

  /// T: [float, uint], [double, ulong], [sbyte, byte], [short, ushort], [int, uint], [long, ulong]
  public static unsafe Vector<T> VectorTableLookup(Vector<T> data, Vector<T2> indices); // TBL

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> VectorTableLookup(Vector<T> data, Vector<T> indices); // TBL

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ZipHigh(Vector<T> left, Vector<T> right); // ZIP2

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ZipLow(Vector<T> left, Vector<T> right); // ZIP1

  /// total method signatures: 20

}
```
## Arm64: FEAT_SVE: firstfaulting

**Approved** | [#runtime/94004](https://github.com/dotnet/runtime/issues/94004#issuecomment-1922102200)

* `T* base` => `T* address`
* `Vector<T> bases` => `Vector<T> addresses`
* Instructions taking `offset(s)` need `WithByteOffset`
* The instructions with `long offset` are removed in favor of existing API
* Int16ZeroExtend (et al) become UInt16ZeroExtend (et al)
* GatherVectorSByteSignExtendFirstFaulting with an indices/offsets parameter: make sure these are consistently named (either indices or offsets as they're the same for 8-bit) across all 8-bit loading functions.
* GetFFR =>  GetFfr.  The initialism is preserved because in the domain where someone will use SVE directly, they will know the register as "FFR", not "First Fault Register"
* WriteFFR(value) =>  SetFfr(value).
* SetFFR() would be SetFfr(), but it was erased in favor of JIT/AoT pattern matching.


```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: firstfaulting
{

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorByteZeroExtendFirstFaulting(Vector<T> mask, Vector<T2> addresses); // LDFF1B

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorByteZeroExtendFirstFaulting(Vector<T> mask, Vector<T> addresses); // LDFF1B

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorByteWithByteOffsetZeroExtendFirstFaulting(Vector<T> mask, byte* address, Vector<T> offsets); // LDFF1B

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorByteWithByteOffsetZeroExtendFirstFaulting(Vector<T> mask, byte* address, Vector<T2> offsets); // LDFF1B

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVectorFirstFaulting(Vector<T> mask, Vector<T2> addresses); // LDFF1W or LDFF1D

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorFirstFaulting(Vector<T> mask, Vector<T> addresses); // LDFF1W or LDFF1D

  /// T: [float, int], [uint, int], [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVectorWithByteOffsetFirstFaulting(Vector<T> mask, T* address, Vector<T2> offsets); // LDFF1W or LDFF1D

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorWithByteOffsetFirstFaulting(Vector<T> mask, T* address, Vector<T> offsets); // LDFF1W or LDFF1D

  /// T: [float, int], [uint, int], [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVectorFirstFaulting(Vector<T> mask, T* address, Vector<T2> indices); // LDFF1W or LDFF1D

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorFirstFaulting(Vector<T> mask, T* address, Vector<T> indices); // LDFF1W or LDFF1D

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtendFirstFaulting(Vector<T> mask, Vector<T2> addresses); // LDFF1SH

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtendFirstFaulting(Vector<T> mask, Vector<T> addresses); // LDFF1SH

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorInt16WithByteOffsetSignExtendFirstFaulting(Vector<T> mask, short* address, Vector<T> offsets); // LDFF1SH

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16WithByteOffsetSignExtendFirstFaulting(Vector<T> mask, short* address, Vector<T2> offsets); // LDFF1SH

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorInt16SignExtendFirstFaulting(Vector<T> mask, short* address, Vector<T> indices); // LDFF1SH

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtendFirstFaulting(Vector<T> mask, short* address, Vector<T2> indices); // LDFF1SH

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<T> mask, Vector<T2> addresses); // LDFF1H

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<T> mask, Vector<T> addresses); // LDFF1H

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorUInt16WithByteOffsetZeroExtendFirstFaulting(Vector<T> mask, ushort* address, Vector<T> offsets); // LDFF1H

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorUInt16WithByteOffsetZeroExtendFirstFaulting(Vector<T> mask, ushort* address, Vector<T2> offsets); // LDFF1H

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<T> mask, ushort* address, Vector<T> indices); // LDFF1H

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<T> mask, ushort* address, Vector<T2> indices); // LDFF1H

  public static unsafe Vector<long> GatherVectorInt32SignExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses); // LDFF1SW

  public static unsafe Vector<ulong> GatherVectorInt32SignExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses); // LDFF1SW

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorInt32WithByteOffsetSignExtendFirstFaulting(Vector<T> mask, int* address, Vector<T> offsets); // LDFF1SW

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt32WithByteOffsetSignExtendFirstFaulting(Vector<T> mask, int* address, Vector<T2> offsets); // LDFF1SW

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorInt32SignExtendFirstFaulting(Vector<T> mask, int* address, Vector<T> indices); // LDFF1SW

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorInt32SignExtendFirstFaulting(Vector<T> mask, int* address, Vector<T2> indices); // LDFF1SW

  public static unsafe Vector<long> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses); // LDFF1W

  public static unsafe Vector<ulong> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses); // LDFF1W

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorUInt32WithByteOffsetZeroExtendFirstFaulting(Vector<T> mask, uint* address, Vector<T> offsets); // LDFF1W

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorUInt32WithByteOffsetZeroExtendFirstFaulting(Vector<T> mask, uint* address, Vector<T2> offsets); // LDFF1W

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<T> mask, uint* address, Vector<T> indices); // LDFF1W

  /// T: [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<T> mask, uint* address, Vector<T2> indices); // LDFF1W

  /// T: [int, uint], [long, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtendFirstFaulting(Vector<T> mask, Vector<T2> addresses); // LDFF1SB

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorSByteSignExtendFirstFaulting(Vector<T> mask, Vector<T> addresses); // LDFF1SB

  /// T: int, uint, long, ulong
  public static unsafe Vector<T> GatherVectorSByteSignExtendFirstFaulting(Vector<T> mask, sbyte* address, Vector<T> offsets); // LDFF1SB

  /// T: [uint, int], [int, uint], [ulong, long], [long, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtendFirstFaulting(Vector<T> mask, sbyte* address, Vector<T2> offsets); // LDFF1SB

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> GetFfr(); // RDFFR // predicated

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> LoadVectorByteZeroExtendFirstFaulting(byte* address); // LDFF1B // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> LoadVectorFirstFaulting(T* address); // LDFF1W or LDFF1D or LDFF1B or LDFF1H // predicated

  /// T: int, long, uint, ulong
  public static unsafe Vector<T> LoadVectorInt16SignExtendFirstFaulting(short* address); // LDFF1SH // predicated

  /// T: int, long, uint, ulong
  public static unsafe Vector<T> LoadVectorInt16ZeroExtendFirstFaulting(ushort* address); // LDFF1H // predicated

  /// T: long, ulong
  public static unsafe Vector<T> LoadVectorInt32SignExtendFirstFaulting(int* address); // LDFF1SW // predicated

  /// T: long, ulong
  public static unsafe Vector<T> LoadVectorUInt32ZeroExtendFirstFaulting(uint* address); // LDFF1W // predicated

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> LoadVectorSByteSignExtendFirstFaulting(sbyte* address); // LDFF1SB // predicated

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void SetFfr(Vector<T> value); // WRFFR

  /// total method signatures: 72

}
```
## Arm64: FEAT_SVE: maths

**Approved** | [#runtime/94009](https://github.com/dotnet/runtime/issues/94009#issuecomment-1922142208)

* `DotProduct(op1, op2, op3)` => `DotProduct(addend, left, right)`
* `DotProduct(op1, op2, op3, ulong imm_index)` => `DotProductBySelectedScalar(addend, left, right, [ConstantExpected] byte rightIndex)`
* `FusedMultiplyAdd` also has a `BySelectedScalar` and renamed `index` parameter
* `FusedMultiplyAddNegate(op1, op2, op3)` => `FusedMultiplyAddNegated(addend, left, right)`
* `MultiplyReturningHighHalf` needs a better name
* Remove `SubtractReversed`
* We excluded the methods listed under optional

```C#
namespace System.Runtime.Intrinsics.Arm;

public abstract partial class Sve : AdvSimd
{
  /// T: float, double, sbyte, short, int, long
  public static unsafe Vector<T> Abs(Vector<T> value);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AbsoluteDifference(Vector<T> left, Vector<T> right);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Add(Vector<T> left, Vector<T> right);

  /// T: float, double, long, ulong
  public static unsafe Vector<T> AddAcross(Vector<T> value);

  /// T: [long, sbyte], [long, short], [long, int], [ulong, byte], [ulong, ushort], [ulong, uint]
  public static unsafe Vector<T> AddAcross(Vector<T2> value);

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AddSaturate(Vector<T> left, Vector<T> right);

  /// T: float, double, int, long, uint, ulong
  public static unsafe Vector<T> Divide(Vector<T> left, Vector<T> right);

  /// T: [int, sbyte], [long, short], [uint, byte], [ulong, ushort]
  public static unsafe Vector<T> DotProduct(Vector<T> addend, Vector<T2> left, Vector<T2> right);

  /// T: [int, sbyte], [long, short], [uint, byte], [ulong, ushort]
  public static unsafe Vector<T> DotProductBySelectedScalar(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex);

  /// T: float, double
  public static unsafe Vector<T> FusedMultiplyAdd(Vector<T> addend, Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> FusedMultiplyAddBySelectedScalar(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex);

  /// T: float, double
  public static unsafe Vector<T> FusedMultiplyAddNegated(Vector<T> addend, Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> FusedMultiplySubtract(Vector<T> minuend, Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> FusedMultiplySubtract(Vector<T> minuend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex);

  /// T: float, double
  public static unsafe Vector<T> FusedMultiplySubtractNegated(Vector<T> minuend, Vector<T> left, Vector<T> right);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Max(Vector<T> left, Vector<T> right);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MaxAcross(Vector<T> value);

  /// T: float, double
  public static unsafe Vector<T> MaxNumber(Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> MaxNumberAcross(Vector<T> value);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Min(Vector<T> left, Vector<T> right);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MinAcross(Vector<T> value);

  /// T: float, double
  public static unsafe Vector<T> MinNumber(Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> MinNumberAcross(Vector<T> value);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Multiply(Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> MultiplyBySelectedScalar(Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex);

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MultiplyAdd(Vector<T> addend, Vector<T> left, Vector<T> right);

  /// T: float, double
  public static unsafe Vector<T> MultiplyExtended(Vector<T> left, Vector<T> right);

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MultiplySubtract(Vector<T> minuend, Vector<T> left, Vector<T> right);

  /// T: float, double, sbyte, short, int, long
  public static unsafe Vector<T> Negate(Vector<T> value);

  /// T: int, long
  public static unsafe Vector<T> SignExtend16(Vector<T> value);

  public static unsafe Vector<long> SignExtend32(Vector<long> value);

  /// T: short, int, long
  public static unsafe Vector<T> SignExtend8(Vector<T> value);

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> SignExtendWideningLower(Vector<T2> value);

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> SignExtendWideningUpper(Vector<T2> value);

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Subtract(Vector<T> left, Vector<T> right);

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> SubtractSaturate(Vector<T> left, Vector<T> right);

  /// T: uint, ulong
  public static unsafe Vector<T> ZeroExtend16(Vector<T> value);

  public static unsafe Vector<ulong> ZeroExtend32(Vector<ulong> value);

  /// T: ushort, uint, ulong
  public static unsafe Vector<T> ZeroExtend8(Vector<T> value);

  /// T: [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> ZeroExtendWideningLower(Vector<T2> value);

  /// T: [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> ZeroExtendWideningUpper(Vector<T2> value);
}
```
