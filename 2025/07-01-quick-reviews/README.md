# API Review 07/01/2025

## Add CreateNewProcessGroup to ProcessStartInfo

**Approved** | [#runtime/44944](https://github.com/dotnet/runtime/issues/44944#issuecomment-3024894147) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h0m0s)

Looks good as proposed

```c#
namespace System.Diagnostics
{
    public sealed partial class ProcessStartInfo
    {
        [SupportedOSPlatform("windows")]
        public bool CreateNewProcessGroup { get; set; }
    }
}
```

## MLKemCng and CNG identifiers

**Approved** | [#runtime/117091](https://github.com/dotnet/runtime/issues/117091#issuecomment-3024935834) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h7m30s)

* Based on the fact that kem.Key.Dispose() causes kem to behave as disposed, we moved the property to a method that creates distinct instances.

```c#
namespace System.Security.Cryptography
{
    // New class in both Microsoft.Bcl.Cryptography and System.Security.Cryptography
    // No .NET Standard availability in M.B.C. Only netcoreapp and netfx.
    [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public sealed class MLKemCng : MLKem
    {
        [SupportedOSPlatformAttribute("windows")]
        public MLKemCng(CngKey key);
        
        public CngKey GetKey();
    }

    // New properties on existing type; System.Security.Cryptography only
    public sealed partial class CngAlgorithm
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngAlgorithm MLKem { get; }
    }

    // New properties on existing type; System.Security.Cryptography only
    public sealed partial class CngAlgorithmGroup
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngAlgorithmGroup MLKem { get; }
    }

    // New properties on existing type; System.Security.Cryptography only
    public sealed partial class CngKeyBlobFormat
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngKeyBlobFormat MLKemPrivateBlob { get; }

        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngKeyBlobFormat MLKemPrivateSeedBlob { get; }

        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngKeyBlobFormat MLKemPublicBlob { get; }
    }
}
```

## Add MediaTypeNames.Application.Yaml

**Approved** | [#runtime/105809](https://github.com/dotnet/runtime/issues/105809#issuecomment-3024943998) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h23m11s)

Looks good as proposed

```c#
namespace System.Net.Mime;

public static partial class MediaTypeNames
{
    public static partial class Application
    {
        /// <summary>Specifies that the <see cref="MediaTypeNames.Application"/> data is in YAML format.</summary>
        public const string Yaml = "application/yaml";
    }
}
```

## Flexible OpenSSL provider support

**Approved** | [#runtime/113118](https://github.com/dotnet/runtime/issues/113118#issuecomment-3024974159) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h25m57s)

* We collapsed the overload with the propertyQuery and the one without into one with default parameters.
* providerList => providerNames, to better match the current overload.
* We like the providerNames parameter as an IEnumerable.  We discussed array and ReadOnlySpan, left it as IEnumerable.

```c#
namespace System.Security.Cryptography
{
    public partial class SafeEvpPKeyHandle
    {
        // more flexible overload which supports loading multiple providers and a provider query string
        public static SafeEvpPKeyHandle OpenKeyFromProvider(IEnumerable<string> providerNames, string keyUri, string? propertyQuery = null) => throw null;
    }
}
```

## Volatile read/write for new supported types of Interlocked exchange

**Approved** | [#runtime/107449](https://github.com/dotnet/runtime/issues/107449#issuecomment-3024989151) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h36m12s)

Looks good as proposed, consistent with the change on Interlocked.

```diff
public static class Volatile
{
    // Remove constraints from existing methods, making them work for primitives and throw for unsupported types,
    // as was done with Interlocked.{Compare}Exchange
    public static T Read<T>([NotNullIfNotNull(nameof(location))] ref readonly T location)
-        where T : class?
    public static void Write<T>([NotNullIfNotNull(nameof(value))] ref T location, T value) 
-        where T : class?
}
```
## SequenceReader<T>.TryAdvanceTo(ReadOnlySpan<T> delimiter)

**Approved** | [#runtime/71471](https://github.com/dotnet/runtime/issues/71471#issuecomment-3025003825) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h41m41s)

We added the `scoped` keyword to the `delimiter` parameter; otherwise, looks good as proposed

```C#
namespace System.Buffers;

public ref struct SequenceReader<T>
{
    public bool TryAdvanceTo(scoped ReadOnlySpan<T> delimiter, bool advancePastDelimiter = true);
}
```
## Arm64: FEAT_SVE2: counting

**Approved** | [#runtime/94017](https://github.com/dotnet/runtime/issues/94017#issuecomment-3025041247) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=0h47m17s)

Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve2 : Sve /// Feature: FEAT_SVE2  Category: counting
{

  /// T: [uint, int], [ulong, long]
  public static unsafe Vector<T> CountMatchingElements(Vector<T2> mask, Vector<T2> left, Vector<T2> right); // HISTCNT

  /// T: uint, ulong
  public static unsafe Vector<T> CountMatchingElements(Vector<T> mask, Vector<T> left, Vector<T> right); // HISTCNT

  public static unsafe Vector<byte> CountMatchingElementsIn128BitSegments(Vector<sbyte> left, Vector<sbyte> right); // HISTSEG

  public static unsafe Vector<byte> CountMatchingElementsIn128BitSegments(Vector<byte> left, Vector<byte> right); // HISTSEG

  /// total method signatures: 4

}
```
## fp

**Approved** | [#runtime/94018](https://github.com/dotnet/runtime/issues/94018#issuecomment-3025106850) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=1h0m42s)

* "DownConvert" => "ConvertToSingle", to match AdvSimd
* "UpConvert" => "ConvertToDouble", to match AdvSimd
* And a bunch of other edits for name alignment with SVE(1)
* ConvertToSingleRoundToOddEven => ConvertToSingleEvenRoundToOdd to avoid "OddEven" and "OddOdd"

```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve2 : AdvSimd /// Feature: FEAT_SVE2  Category: fp
{

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AddRotateComplex(Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation); // CADD // MOVPRFX

  public static unsafe Vector<float>  ConvertToSingleOdd(Vector<double> value); // FCVTNT // predicated

  public static unsafe Vector<float>  ConvertToSingleEvenRoundToOdd(Vector<double> value); // FCVTX // predicated, MOVPRFX

  public static unsafe Vector<float>  ConvertToSingleOddRoundToOdd(Vector<double> value); // FCVTXNT // predicated

  /// T: [int, float], [long, double]
  public static unsafe Vector<T> Log2(Vector<T2> value); // FLOGB // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MultiplyAddRotateComplex(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation); // CMLA // MOVPRFX

  /// T: short, int, ushort, uint
  public static unsafe Vector<T> MultiplyAddRotateComplexBySelectedScalar(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex, [ConstantExpected] byte rotation); // CMLA // MOVPRFX

  public static unsafe Vector<uint> ReciprocalEstimate(Vector<uint> value); // URECPE // predicated, MOVPRFX

  public static unsafe Vector<uint> ReciprocalSqrtEstimate(Vector<uint> value); // URSQRTE // predicated, MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> AddSaturateRotateComplex(Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation); // SQCADD // MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> MultiplyAddRoundedDoublingSaturateHighRotateComplex(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation); // SQRDCMLAH // MOVPRFX

  /// T: short, int
  public static unsafe Vector<T> MultiplyAddRoundedDoublingSaturateHighRotateComplexBySelectedScalar(Vector<T> addend, Vector<T> left, Vector<T> right, ulong rightIndex, [ConstantExpected] byte rotation); // SQRDCMLAH // MOVPRFX

  public static unsafe Vector<double>  ConvertToDoubleOdd(Vector<float> value); // FCVTLT // predicated

  /// total method signatures: 13

}
```

## gather loads

**Approved** | [#runtime/94019](https://github.com/dotnet/runtime/issues/94019#issuecomment-3025135738) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=1h21m2s)

Updated `indices` to `indexes`; otherwise, looks good as proposed


```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve2 : AdvSimd /// Feature: FEAT_SVE2  Category: gatherloads
{

  /// T: [int, uint], [uint, uint], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorByteZeroExtendNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1B

  /// T: [int, uint], [uint, uint], [long, long], [ulong, long], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorByteZeroExtendNonTemporal(Vector<T> mask, byte* address, Vector<T2> offsets); // LDNT1B

  /// T: [int, uint], [uint, uint], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtendNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1SH

  /// T: [long, long], [ulong, long], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorInt16SignExtendNonTemporal(Vector<T> mask, short* address, Vector<T2> indexes); // LDNT1SH

  /// T: [int, uint], [uint, uint], [long, long], [ulong, long], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorInt16WithByteOffsetsSignExtendNonTemporal(Vector<T> mask, short* address, Vector<T2> offsets); // LDNT1SH

  /// T: [long, ulong], [int, uint], [ulong, ulong], [uint, uint]
  public static unsafe Vector<T> GatherVectorInt32SignExtendNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1SW

  /// T: [long, long], [int, int], [ulong, long], [uint, int], [long, ulong], [int, uint], [ulong, ulong], [uint, uint]
  public static unsafe Vector<T> GatherVectorInt32SignExtendNonTemporal(Vector<T> mask, int* address, Vector<T2> indexes); // LDNT1SW

  /// T: [long, long], [int, int], [ulong, long], [uint, int], [long, ulong], [int, uint], [ulong, ulong], [uint, uint]
  public static unsafe Vector<T> GatherVectorInt32WithByteOffsetsSignExtendNonTemporal(Vector<T> mask, int* address, Vector<T2> offsets); // LDNT1SW

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVectorNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1W or LDNT1D

  /// T: uint, ulong
  public static unsafe Vector<T> GatherVectorNonTemporal(Vector<T> mask, Vector<T> addresses); // LDNT1W or LDNT1D

  /// T: [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVectorNonTemporal(Vector<T> mask, T* address, Vector<T2> offsets); // LDNT1W or LDNT1D

  /// T: uint, long, ulong
  public static unsafe Vector<T> GatherVectorNonTemporal(Vector<T> mask, T* address, Vector<T> offsets); // LDNT1W or LDNT1D

  /// T: [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe Vector<T> GatherVectorNonTemporal(Vector<T> mask, T* address, Vector<T2> indexes); // LDNT1D

  /// T: long, ulong
  public static unsafe Vector<T> GatherVectorNonTemporal(Vector<T> mask, T* address, Vector<T> indexes); // LDNT1D

  /// T: [int, uint], [uint, uint], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtendNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1SB

  /// T: [int, uint], [uint, uint], [long, long], [ulong, long], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorSByteSignExtendNonTemporal(Vector<T> mask, sbyte* address, Vector<T2> offsets); // LDNT1SB

  /// T: [int, uint], [uint, uint], [long, long], [ulong, long], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorUInt16WithByteOffsetsZeroExtendNonTemporal(Vector<T> mask, ushort* address, Vector<T2> offsets); // LDNT1H

  /// T: [int, uint], [uint, uint], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorUInt16ZeroExtendNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1H

  /// T: [long, long], [ulong, long], [long, ulong], [ulong, ulong]
  public static unsafe Vector<T> GatherVectorUInt16ZeroExtendNonTemporal(Vector<T> mask, ushort* address, Vector<T2> indexes); // LDNT1H

  /// T: [long, long], [int, int], [ulong, long], [uint, int], [long, ulong], [int, uint], [ulong, ulong], [uint, uint]
  public static unsafe Vector<T> GatherVectorUInt32WithByteOffsetsZeroExtendNonTemporal(Vector<T> mask, uint* address, Vector<T2> offsets); // LDNT1W

  /// T: [long, ulong], [int, uint], [ulong, ulong], [uint, uint]
  public static unsafe Vector<T> GatherVectorUInt32ZeroExtendNonTemporal(Vector<T> mask, Vector<T2> addresses); // LDNT1W

  /// T: [long, long], [int, int], [ulong, long], [uint, int], [long, ulong], [int, uint], [ulong, ulong], [uint, uint]
  public static unsafe Vector<T> GatherVectorUInt32ZeroExtendNonTemporal(Vector<T> mask, uint* address, Vector<T2> indexes); // LDNT1W

  /// total method signatures: 22

}
```
## Arm64: FEAT_SVE2: scatter stores

**Approved** | [#runtime/94023](https://github.com/dotnet/runtime/issues/94023#issuecomment-3025150044) | [Video](https://www.youtube.com/watch?v=52f8Bz8db04&t=1h33m21s)

* `indices` => `indexes`
* `base` => `address`

```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve2 : AdvSimd /// Feature: FEAT_SVE2  Category: scatterstores
{

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter16BitNarrowing(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // STNT1H

  /// T: uint, ulong
  public static unsafe void Scatter16BitNarrowing(Vector<T> mask, Vector<T> addresses, Vector<T> data); // STNT1H

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<T> mask, short* address, Vector<T2> offsets, Vector<T> data); // STNT1H

  /// T: uint, ulong
  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<T> mask, ushort* address, Vector<T> offsets, Vector<T> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<long> mask, short* address, Vector<long> offsets, Vector<long> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<ulong> mask, ushort* address, Vector<long> offsets, Vector<ulong> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<long> mask, short* address, Vector<long> indexes, Vector<long> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<ulong> mask, ushort* address, Vector<long> indexes, Vector<ulong> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<long> mask, short* address, Vector<ulong> indexes, Vector<long> data); // STNT1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<ulong> mask, ushort* address, Vector<ulong> indexes, Vector<ulong> data); // STNT1H

  public static unsafe void Scatter32BitNarrowing(Vector<long> mask, Vector<ulong> addresses, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitNarrowing(Vector<ulong> mask, Vector<ulong> addresses, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<long> mask, int* address, Vector<long> offsets, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<ulong> mask, uint* address, Vector<long> offsets, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<long> mask, int* address, Vector<ulong> offsets, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<ulong> mask, uint* address, Vector<ulong> offsets, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<long> mask, int* address, Vector<long> indexes, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<ulong> mask, uint* address, Vector<long> indexes, Vector<ulong> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<long> mask, int* address, Vector<ulong> indexes, Vector<long> data); // STNT1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<ulong> mask, uint* address, Vector<ulong> indexes, Vector<ulong> data); // STNT1W

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter8BitNarrowing(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // STNT1B

  /// T: uint, ulong
  public static unsafe void Scatter8BitNarrowing(Vector<T> mask, Vector<T> addresses, Vector<T> data); // STNT1B

  /// T: [int, uint], [long, ulong]
  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<T> mask, sbyte* address, Vector<T2> offsets, Vector<T> data); // STNT1B

  /// T: uint, ulong
  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<T> mask, byte* address, Vector<T> offsets, Vector<T> data); // STNT1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<long> mask, sbyte* address, Vector<long> offsets, Vector<long> data); // STNT1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<ulong> mask, byte* address, Vector<long> offsets, Vector<ulong> data); // STNT1B

  /// T: [float, uint], [int, uint], [double, ulong], [long, ulong]
  public static unsafe void ScatterNonTemporal(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // STNT1W or STNT1D

  /// T: uint, ulong
  public static unsafe void ScatterNonTemporal(Vector<T> mask, Vector<T> addresses, Vector<T> data); // STNT1W or STNT1D

  /// T: [float, uint], [int, uint], [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe void ScatterNonTemporal(Vector<T> mask, T* address, Vector<T2> offsets, Vector<T> data); // STNT1W or STNT1D

  /// T: uint, long, ulong
  public static unsafe void ScatterNonTemporal(Vector<T> mask, T* address, Vector<T> offsets, Vector<T> data); // STNT1W or STNT1D

  /// T: [double, long], [ulong, long], [double, ulong], [long, ulong]
  public static unsafe void ScatterNonTemporal(Vector<T> mask, T* address, Vector<T2> indexes, Vector<T> data); // STNT1D

  /// T: long, ulong
  public static unsafe void ScatterNonTemporal(Vector<T> mask, T* address, Vector<T> indexes, Vector<T> data); // STNT1D

  /// total method signatures: 32

}
```
