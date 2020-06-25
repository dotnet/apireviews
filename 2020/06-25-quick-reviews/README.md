# Quick Reviews 6/25/2020

## Add empty X64/Arm64 nested classes to some System.Runtime.Intrinsics

**Approved** | [#runtime/34587](https://github.com/dotnet/runtime/issues/34587#issuecomment-649714324) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=0h0m0s)

* Good point
* When inheriting, we always should also define all nested types and define `IsSupported` the appropriately, even if they don't define any other members.
* (this will probably require more nested types than listed below)

```C#
[Intrinsic]
[CLSCompliant(false)]
public partial class Sha1 : ArmBase
{
    public abstract class Arm64 : ArmBase.Arm64;
    {
        public static new bool IsSupported => Sha1.IsSupported && IntPtr.Size == 8;
    }
}
```
## Add System.Numerics.Half 16 bit floating point number conforming to IEEE 754:2008 binary16

**Approved** | [#runtime/936](https://github.com/dotnet/runtime/issues/936) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=0h28m18s)

## [Arm64] Store Pair of SIMD&FP registers

**Approved** | [#runtime/33532](https://github.com/dotnet/runtime/issues/33532#issuecomment-649724378) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=0h44m11s)

```C#
namespace System.Runtime.Intrinsics.Arm
{
    partial class AdvSimd.Arm64
    {
        public static unsafe void StorePair(byte* address, Vector64<byte> value1, Vector64<byte> value2);
        public static unsafe void StorePair(double* address, Vector64<double> value1, Vector64<double> value2);
        public static unsafe void StorePair(short* address, Vector64<short> value1, Vector64<short> value2);
        public static unsafe void StorePair(int* address, Vector64<int> value1, Vector64<int> value2);
        public static unsafe void StorePair(long* address, Vector64<long> value1, Vector64<long> value2);
        public static unsafe void StorePair(sbyte* address, Vector64<sbyte> value1, Vector64<sbyte> value2);
        public static unsafe void StorePair(float* address, Vector64<float> value1, Vector64<float> value2);
        public static unsafe void StorePair(ushort* address, Vector64<ushort> value1, Vector64<ushort> value2);
        public static unsafe void StorePair(uint* address, Vector64<uint> value1, Vector64<uint> value2);
        public static unsafe void StorePair(ulong* address, Vector64<ulong> value1, Vector64<ulong> value2);
        public static unsafe void StorePair(byte* address, Vector128<byte> value1, Vector128<byte> value2);
        public static unsafe void StorePair(double* address, Vector128<double> value1, Vector128<double> value2);
        public static unsafe void StorePair(short* address, Vector128<short> value1, Vector128<short> value2);
        public static unsafe void StorePair(int* address, Vector128<int> value1, Vector128<int> value2);
        public static unsafe void StorePair(long* address, Vector128<long> value1, Vector128<long> value2);
        public static unsafe void StorePair(sbyte* address, Vector128<sbyte> value1, Vector128<sbyte> value2);
        public static unsafe void StorePair(float* address, Vector128<float> value1, Vector128<float> value2);
        public static unsafe void StorePair(ushort* address, Vector128<ushort> value1, Vector128<ushort> value2);
        public static unsafe void StorePair(uint* address, Vector128<uint> value1, Vector128<uint> value2);
        public static unsafe void StorePair(ulong* address, Vector128<ulong> value1, Vector128<ulong> value2);

        public static unsafe void StorePairScalar(int* address, Vector64<int> value1, Vector64<int> value2);
        public static unsafe void StorePairScalar(float* address, Vector64<float> value1, Vector64<float> value2);
        public static unsafe void StorePairScalar(uint* address, Vector64<uint> value1, Vector64<uint> value2);

        public static unsafe void StorePairNonTemporal(byte* address, Vector64<byte> value1, Vector64<byte> value2);
        public static unsafe void StorePairNonTemporal(double* address, Vector64<double> value1, Vector64<double> value2);
        public static unsafe void StorePairNonTemporal(short* address, Vector64<short> value1, Vector64<short> value2);
        public static unsafe void StorePairNonTemporal(int* address, Vector64<int> value1, Vector64<int> value2);
        public static unsafe void StorePairNonTemporal(long* address, Vector64<long> value1, Vector64<long> value2);
        public static unsafe void StorePairNonTemporal(sbyte* address, Vector64<sbyte> value1, Vector64<sbyte> value2);
        public static unsafe void StorePairNonTemporal(float* address, Vector64<float> value1, Vector64<float> value2);
        public static unsafe void StorePairNonTemporal(ushort* address, Vector64<ushort> value1, Vector64<ushort> value2);
        public static unsafe void StorePairNonTemporal(uint* address, Vector64<uint> value1, Vector64<uint> value2);
        public static unsafe void StorePairNonTemporal(ulong* address, Vector64<ulong> value1, Vector64<ulong> value2);
        public static unsafe void StorePairNonTemporal(byte* address, Vector128<byte> value1, Vector128<byte> value2);
        public static unsafe void StorePairNonTemporal(double* address, Vector128<double> value1, Vector128<double> value2);
        public static unsafe void StorePairNonTemporal(short* address, Vector128<short> value1, Vector128<short> value2);
        public static unsafe void StorePairNonTemporal(int* address, Vector128<int> value1, Vector128<int> value2);
        public static unsafe void StorePairNonTemporal(long* address, Vector128<long> value1, Vector128<long> value2);
        public static unsafe void StorePairNonTemporal(sbyte* address, Vector128<sbyte> value1, Vector128<sbyte> value2);
        public static unsafe void StorePairNonTemporal(float* address, Vector128<float> value1, Vector128<float> value2);
        public static unsafe void StorePairNonTemporal(ushort* address, Vector128<ushort> value1, Vector128<ushort> value2);
        public static unsafe void StorePairNonTemporal(uint* address, Vector128<uint> value1, Vector128<uint> value2);
        public static unsafe void StorePairNonTemporal(ulong* address, Vector128<ulong> value1, Vector128<ulong> value2);

        public static unsafe void StorePairScalarNonTemporal(int* address, Vector64<int> value1, Vector64<int> value2);
        public static unsafe void StorePairScalarNonTemporal(float* address, Vector64<float> value1, Vector64<float> value2);
        public static unsafe void StorePairScalarNonTemporal(uint* address, Vector64<uint> value1, Vector64<uint> value2);
    }
}
```
## ARM Doubling Multiply intrinsics

**Approved** | [#runtime/36696](https://github.com/dotnet/runtime/issues/36696#issuecomment-649730379) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=0h47m33s)

* The TODO versions are also approved

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public abstract class AdvSimd
    {
        public static Vector64<short>  MultiplyDoublingSaturateHigh(Vector64<short>  left, Vector64<short>  right);
        public static Vector64<int>    MultiplyDoublingSaturateHigh(Vector64<int>    left, Vector64<int>    right);
        public static Vector128<short> MultiplyDoublingSaturateHigh(Vector128<short> left, Vector128<short> right);
        public static Vector128<int>   MultiplyDoublingSaturateHigh(Vector128<int>   left, Vector128<int>   right);

        public static Vector64<short>  MultiplyDoublingBySelectedScalarSaturateHigh(Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingBySelectedScalarSaturateHigh(Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyDoublingBySelectedScalarSaturateHigh(Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingBySelectedScalarSaturateHigh(Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingBySelectedScalarSaturateHigh(Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingBySelectedScalarSaturateHigh(Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingBySelectedScalarSaturateHigh(Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingBySelectedScalarSaturateHigh(Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short>  MultiplyDoublingSaturateHighScalar(Vector64<short> left, Vector64<short> right);
        public static Vector64<int>    MultiplyDoublingSaturateHighScalar(Vector64<int>   left, Vector64<int>   right);

        // TODO: Scalar BySelectedScalar variant

        public static Vector64<short>  MultiplyRoundedDoublingSaturateHigh(Vector64<short>  left, Vector64<short>  right);
        public static Vector64<int>    MultiplyRoundedDoublingSaturateHigh(Vector64<int>    left, Vector64<int>    right);
        public static Vector128<short> MultiplyRoundedDoublingSaturateHigh(Vector128<short> left, Vector128<short> right);
        public static Vector128<int>   MultiplyRoundedDoublingSaturateHigh(Vector128<int>   left, Vector128<int>   right);

        public static Vector64<short>  MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short>  MultiplyRoundedDoublingSaturateHighScalar(Vector64<short> left, Vector64<short> right);
        public static Vector64<int>    MultiplyRoundedDoublingSaturateHighScalar(Vector64<int>   left, Vector64<int>   right);

        // TODO: Scalar BySelectedScalar variant

        public static Vector64<short>  MultiplyDoublingWideningSaturateLower(Vector64<short>  left, Vector64<short>  right);
        public static Vector64<int>    MultiplyDoublingWideningSaturateLower(Vector64<int>    left, Vector64<int>    right);
        public static Vector128<short> MultiplyDoublingWideningSaturateLower(Vector128<short> left, Vector128<short> right);
        public static Vector128<int>   MultiplyDoublingWideningSaturateLower(Vector128<int>   left, Vector128<int>   right);

        public static Vector64<short>  MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingWideningLowerBySelectedScalarSaturate(Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short>  MultiplyDoublingWideningSaturateLowerScalar(Vector64<short> left, Vector64<short> right);
        public static Vector64<int>    MultiplyDoublingWideningSaturateLowerScalar(Vector64<int>   left, Vector64<int>   right);

        // TODO: Scalar BySelectedScalar variant

        public static Vector64<short>  MultiplyDoublingAddWideningLowerSaturate(Vector64<short>  addend, Vector64<short>  left, Vector64<short>  right);
        public static Vector64<int>    MultiplyDoublingAddWideningLowerSaturate(Vector64<int>    addend, Vector64<int>    left, Vector64<int>    right);
        public static Vector128<short> MultiplyDoublingAddWideningLowerSaturate(Vector128<short> addend, Vector128<short> left, Vector128<short> right);
        public static Vector128<int>   MultiplyDoublingAddWideningLowerSaturate(Vector128<int>   addend, Vector128<int>   left, Vector128<int>   right);

        public static Vector64<short>  MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector64<short>  addend, Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector64<int>    addend, Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector64<short>  addend, Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector64<int>    addend, Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector128<short> addend, Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector128<int>   addend, Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector128<short> addend, Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingAddWideningLowerBySelectedScalarSaturate(Vector128<int>   addend, Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short> MultiplyDoublingAddWideningLowerSaturateScalar(Vector64<short> addend, Vector64<short> left, Vector64<short> right);
        public static Vector64<int>   MultiplyDoublingAddWideningLowerSaturateScalar(Vector64<int>   addend, Vector64<int>   left, Vector64<int>   right);

        // TODO: Scalar BySelectedScalar variant

        public static Vector64<short>  MultiplyDoublingSubtractWideningLowerSaturate(Vector64<short>  minuend, Vector64<short>  left, Vector64<short>  right);
        public static Vector64<int>    MultiplyDoublingSubtractWideningLowerSaturate(Vector64<int>    minuend, Vector64<int>    left, Vector64<int>    right);
        public static Vector128<short> MultiplyDoublingSubtractWideningLowerSaturate(Vector128<short> minuend, Vector128<short> left, Vector128<short> right);
        public static Vector128<int>   MultiplyDoublingSubtractWideningLowerSaturate(Vector128<int>   minuend, Vector128<int>   left, Vector128<int>   right);

        public static Vector64<short>  MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector64<short>  minuend, Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector64<int>    minuend, Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector64<short>  minuend, Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector64<int>    minuend, Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector128<short> minuend, Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector128<int>   minuend, Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector128<short> minuend, Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyDoublingSubtractWideningLowerBySelectedScalarSaturate(Vector128<int>   minuend, Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short>  MultiplyDoublingSubtractWideningLowerSaturateScalar(Vector64<short> minuend, Vector64<short> left, Vector64<short> right);
        public static Vector64<int>    MultiplyDoublingSubtractWideningLowerSaturateScalar(Vector64<int>   minuend, Vector64<int>   left, Vector64<int>   right);

        // TODO: Scalar BySelectedScalar variant
    }

    public abstract class Rdm : AdvSimd
    {
        public static bool IsSupported { get; }

        // TODO: Scalar BySelectedScalar variant

        public static Vector64<short>  MultiplyRoundedDoublingAddSaturateHigh(Vector64<short>  addend, Vector64<short>  left, Vector64<short>   right);
        public static Vector64<int>    MultiplyRoundedDoublingAddSaturateHigh(Vector64<int>    addend, Vector64<int>    left, Vector64<int>     right);
        public static Vector128<short> MultiplyRoundedDoublingAddSaturateHigh(Vector128<short> addend, Vector128<short> left, Vector128<short>  right);
        public static Vector128<int>   MultiplyRoundedDoublingAddSaturateHigh(Vector128<int>   addend, Vector128<int>   left, Vector128<int>    right);

        public static Vector64<short>  MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector64<short>  addend, Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector64<int>    addend, Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector64<short>  addend, Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector64<int>    addend, Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector128<short> addend, Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector128<int>   addend, Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector128<short> addend, Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyRoundedDoublingAddBySelectedScalarSaturateHigh(Vector128<int>   addend, Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short>  MultiplyRoundedDoublingAddSaturateHighScalar(Vector64<short> addend, Vector64<short> left, Vector64<short> right);
        public static Vector64<int>    MultiplyRoundedDoublingAddSaturateHighScalar(Vector64<int>   addend, Vector64<int>   left, Vector64<int> right);

        // TODO: Scalar BySelectedScalar variant

        public static Vector64<short>  MultiplyRoundedDoublingSubtractSaturateHigh(Vector64<short>  minuend, Vector64<short>  left, Vector64<short>  right);
        public static Vector64<int>    MultiplyRoundedDoublingSubtractSaturateHigh(Vector64<int>    minuend, Vector64<int>    left, Vector64<int>    right);
        public static Vector128<short> MultiplyRoundedDoublingSubtractSaturateHigh(Vector128<short> minuend, Vector128<short> left, Vector128<short> right);
        public static Vector128<int>   MultiplyRoundedDoublingSubtractSaturateHigh(Vector128<int>   minuend, Vector128<int>   left, Vector128<int>   right);

        public static Vector64<short>  MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector64<short>  minuend, Vector64<short>  left, Vector64<short>  right, byte rightIndex);
        public static Vector64<int>    MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector64<int>    minuend, Vector64<int>    left, Vector64<int>    right, byte rightIndex);
        public static Vector64<short>  MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector64<short>  minuend, Vector64<short>  left, Vector128<short> right, byte rightIndex);
        public static Vector64<int>    MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector64<int>    minuend, Vector64<int>    left, Vector128<int>   right, byte rightIndex);
        public static Vector128<short> MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector128<short> minuend, Vector128<short> left, Vector64<short>  right, byte rightIndex);
        public static Vector128<int>   MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector128<int>   minuend, Vector128<int>   left, Vector64<int>    right, byte rightIndex);
        public static Vector128<short> MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector128<short> minuend, Vector128<short> left, Vector128<short> right, byte rightIndex);
        public static Vector128<int>   MultiplyRoundedDoublingSubtractBySelectedScalarSaturateHigh(Vector128<int>   minuend, Vector128<int>   left, Vector128<int>   right, byte rightIndex);

        public static Vector64<short>  MultiplyRoundedDoublingSubtractSaturateHighScalar(Vector64<short> minuend, Vector64<short> left, Vector64<short> right);
        public static Vector64<int>    MultiplyRoundedDoublingSubtractSaturateHighScalar(Vector64<int>   minuend, Vector64<int>   left, Vector64<int>   right);    
    }
}
```
## Remaining ARM Intrinsics

**Approved** | [#runtime/37014](https://github.com/dotnet/runtime/issues/37014#issuecomment-649746313) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=1h3m48s)

label:blocking 

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public static class AdvSimd
    {
        public static unsafe (Vector64<byte> Value1,    Vector64<byte> Value2)    LoadPairVector64(byte*   address);
        public static unsafe (Vector64<sbyte> Value1,   Vector64<sbyte> Value2)   LoadPairVector64(sbyte*  address);
        public static unsafe (Vector64<short> Value1,   Vector64<short> Value2)   LoadPairVector64(short*  address);
        public static unsafe (Vector64<ushort> Value1,  Vector64<ushort> Value2)  LoadPairVector64(ushort* address);
        public static unsafe (Vector64<int> Value1,     Vector64<int> Value2)     LoadPairVector64(int*    address);
        public static unsafe (Vector64<uint> Value1,    Vector64<uint> Value2)    LoadPairVector64(uint*   address);
        public static unsafe (Vector64<float> Value1,   Vector64<float> Value2)   LoadPairVector64(float*  address);

        public static unsafe (Vector128<byte> Value1,   Vector128<byte> Value2)   LoadPairVector128(byte*   address);
        public static unsafe (Vector128<sbyte> Value1,  Vector128<sbyte> Value2)  LoadPairVector128(sbyte*  address);
        public static unsafe (Vector128<short> Value1,  Vector128<short> Value2)  LoadPairVector128(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2) LoadPairVector128(ushort* address);
        public static unsafe (Vector128<int> Value1,    Vector128<int> Value2)    LoadPairVector128(int*    address);
        public static unsafe (Vector128<uint> Value1,   Vector128<uint> Value2)   LoadPairVector128(uint*   address);
        public static unsafe (Vector128<long> Value1,   Vector128<long> Value2)   LoadPairVector128(long*   address);
        public static unsafe (Vector128<ulong> Value1,  Vector128<ulong> Value2)  LoadPairVector128(ulong*  address);
        public static unsafe (Vector128<float> Value1,  Vector128<float> Value2)  LoadPairVector128(float*  address);

        public static unsafe (Vector64<int> Value1,     Vector64<int> Value2)     LoadPairScalarVector64(int*  address);
        public static unsafe (Vector64<uint> Value1,    Vector64<uint> Value2)    LoadPairScalarVector64(uint* address);
        public static unsafe (Vector64<long> Value1,    Vector64<long> Value2)    LoadPairScalarVector64(long*  address);
        public static unsafe (Vector64<ulong> Value1,   Vector64<ulong> Value2)   LoadPairScalarVector64(ulong* address);
        public static unsafe (Vector64<float> Value1,   Vector64<float> Value2)   LoadPairScalarVector64(float* address);
        public static unsafe (Vector64<double> Value1,  Vector64<double> Value2)  LoadPairScalarVector64(double* address);

        public static unsafe (Vector64<byte> Value1,    Vector64<byte> Value2)    LoadPairVector64NonTemporal(byte*   address);
        public static unsafe (Vector64<sbyte> Value1,   Vector64<sbyte> Value2)   LoadPairVector64NonTemporal(sbyte*  address);
        public static unsafe (Vector64<short> Value1,   Vector64<short> Value2)   LoadPairVector64NonTemporal(short*  address);
        public static unsafe (Vector64<ushort> Value1,  Vector64<ushort> Value2)  LoadPairVector64NonTemporal(ushort* address);
        public static unsafe (Vector64<int> Value1,     Vector64<int> Value2)     LoadPairVector64NonTemporal(int*    address);
        public static unsafe (Vector64<uint> Value1,    Vector64<uint> Value2)    LoadPairVector64NonTemporal(uint*   address);
        public static unsafe (Vector64<float> Value1,   Vector64<float> Value2)   LoadPairVector64NonTemporal(float*  address);

        public static unsafe (Vector128<byte> Value1,   Vector128<byte> Value2)   LoadPairVector128NonTemporal(byte*   address);
        public static unsafe (Vector128<sbyte> Value1,  Vector128<sbyte> Value2)  LoadPairVector128NonTemporal(sbyte*  address);
        public static unsafe (Vector128<short> Value1,  Vector128<short> Value2)  LoadPairVector128NonTemporal(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2) LoadPairVector128NonTemporal(ushort* address);
        public static unsafe (Vector128<int> Value1,    Vector128<int> Value2)    LoadPairVector128NonTemporal(int*    address);
        public static unsafe (Vector128<uint> Value1,   Vector128<uint> Value2)   LoadPairVector128NonTemporal(uint*   address);
        public static unsafe (Vector128<long> Value1,   Vector128<long> Value2)   LoadPairVector128NonTemporal(long*   address);
        public static unsafe (Vector128<ulong> Value1,  Vector128<ulong> Value2)  LoadPairVector128NonTemporal(ulong*  address);
        public static unsafe (Vector128<float> Value1,  Vector128<float> Value2)  LoadPairVector128NonTemporal(float*  address);

        public static unsafe (Vector64<int> Value1,     Vector64<int> Value2)     LoadPairScalarVector64NonTemporal(int*  address);
        public static unsafe (Vector64<uint> Value1,    Vector64<uint> Value2)    LoadPairScalarVector64NonTemporal(uint* address);
        public static unsafe (Vector64<long> Value1,    Vector64<long> Value2)    LoadPairScalarVector64NonTemporal(long*  address);
        public static unsafe (Vector64<ulong> Value1,   Vector64<ulong> Value2)   LoadPairScalarVector64NonTemporal(ulong* address);
        public static unsafe (Vector64<float> Value1,   Vector64<float> Value2)   LoadPairScalarVector64NonTemporal(float* address);
        public static unsafe (Vector64<double> Value1,  Vector64<double> Value2)  LoadPairScalarVector64NonTemporal(double* address);

        public static Vector64<sbyte>   ExtractNarrowingSaturateLower(Vector128<short>  value);
        public static Vector64<short>   ExtractNarrowingSaturateLower(Vector128<int>    value);
        public static Vector64<int>     ExtractNarrowingSaturateLower(Vector128<long>   value);
        public static Vector128<sbyte>  ExtractNarrowingSaturateUpper(Vector64<short>   lower, Vector128<short>  value);
        public static Vector128<short>  ExtractNarrowingSaturateUpper(Vector64<int>     lower, Vector128<int>    value);
        public static Vector128<int>    ExtractNarrowingSaturateUpper(Vector64<long>    lower, Vector128<long>   value);

        public static Vector64<byte>    ExtractNarrowingSaturateLower(Vector128<ushort> value);
        public static Vector64<ushort>  ExtractNarrowingSaturateLower(Vector128<uint>   value);
        public static Vector64<uint>    ExtractNarrowingSaturateLower(Vector128<ulong>  value);
        public static Vector128<byte>   ExtractNarrowingSaturateUpper(Vector64<ushort>  lower, Vector128<ushort> value);
        public static Vector128<ushort> ExtractNarrowingSaturateUpper(Vector64<uint>    lower, Vector128<uint>   value);
        public static Vector128<uint>   ExtractNarrowingSaturateUpper(Vector64<ulong>   lower, Vector128<ulong>  value);

        public static Vector64<byte>    ExtractNarrowingSaturateUnsignedLower(Vector128<short> value);
        public static Vector64<ushort>  ExtractNarrowingSaturateUnsignedLower(Vector128<int>   value);
        public static Vector64<uint>    ExtractNarrowingSaturateUnsignedLower(Vector128<long>  value);
        public static Vector128<byte>   ExtractNarrowingSaturateUnsignedUpper(Vector64<short>  lower, Vector128<short> value);
        public static Vector128<ushort> ExtractNarrowingSaturateUnsignedUpper(Vector64<int>    lower, Vector128<int>   value);
        public static Vector128<uint>   ExtractNarrowingSaturateUnsignedUpper(Vector64<long>   lower, Vector128<long>  value);

        public static Vector64<ushort>  ReverseElement8(Vector64<ushort>  value);
        public static Vector64<short>   ReverseElement8(Vector64<short>   value);
        public static Vector128<ushort> ReverseElement8(Vector128<ushort> value);
        public static Vector128<short>  ReverseElement8(Vector128<short>  value);

        public static Vector64<uint>    ReverseElement8(Vector64<uint>    value);
        public static Vector64<int>     ReverseElement8(Vector64<int>     value);
        public static Vector64<float>   ReverseElement8(Vector64<float>   value);
        public static Vector128<uint>   ReverseElement8(Vector128<uint>   value);
        public static Vector128<int>    ReverseElement8(Vector128<int>    value);

        public static Vector128<ulong>  ReverseElement8(Vector64<ulong>   value);
        public static Vector128<long>   ReverseElement8(Vector64<long>    value);
        public static Vector128<ulong>  ReverseElement8(Vector128<ulong>  value);
        public static Vector128<long>   ReverseElement8(Vector128<long>   value);

        public static Vector64<uint>    ReverseElement16(Vector64<uint>    value);
        public static Vector64<int>     ReverseElement16(Vector64<int>     value);
        public static Vector64<float>   ReverseElement16(Vector64<float>   value);
        public static Vector128<uint>   ReverseElement16(Vector128<uint>   value);
        public static Vector128<int>    ReverseElement16(Vector128<int>    value);

        public static Vector128<ulong>  ReverseElement16(Vector64<ulong>   value);
        public static Vector128<long>   ReverseElement16(Vector64<long>    value);
        public static Vector128<ulong>  ReverseElement16(Vector128<ulong>  value);
        public static Vector128<long>   ReverseElement16(Vector128<long>   value);

        public static Vector128<ulong>  ReverseElement32(Vector64<ulong>   value);
        public static Vector128<long>   ReverseElement32(Vector64<long>    value);
        public static Vector128<ulong>  ReverseElement32(Vector128<ulong>  value);
        public static Vector128<long>   ReverseElement32(Vector128<long>   value);
    }
}
```
## [Arm64] ASIMD InsertScalar

**Approved** | [#runtime/38137](https://github.com/dotnet/runtime/issues/38137#issuecomment-649751957) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=1h35m4s)

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public static class AdvSimd
    {
        public static Vector128<double> InsertScalar(Vector128<double> result, byte resultIndex, Vector64<double> value);
        public static Vector128<long>   InsertScalar(Vector128<long>   result, byte resultIndex, Vector64<long>   value);
        public static Vector128<ulong>  InsertScalar(Vector128<ulong>  result, byte resultIndex, Vector64<ulong>  value);
    }
}
```
## [Arm64] SIMD dot product

**Approved** | [#runtime/37169](https://github.com/dotnet/runtime/issues/37169#issuecomment-649754359) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=1h47m53s)

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public abstract class Dp : AdvSimd
    {
        public static bool IsSupported { get; }

        public static Vector64<int>   DotProduct(Vector64<int>   addend, Vector64<sbyte>  left, Vector64<sbyte>  right);
        public static Vector64<uint>  DotProduct(Vector64<uint>  addend, Vector64<byte>   left, Vector64<byte>   right);
        public static Vector128<int>  DotProduct(Vector128<int>  addend, Vector128<sbyte> left, Vector128<sbyte> right);
        public static Vector128<uint> DotProduct(Vector128<uint> addend, Vector128<byte>  left, Vector128<byte>  right);

        public static Vector64<int>   DotProductBySelectedQuadruplet(Vector64<int>   addend, Vector64<sbyte>  left, Vector64<sbyte>  right, byte rightScaledIndex);
        public static Vector64<int>   DotProductBySelectedQuadruplet(Vector64<int>   addend, Vector64<sbyte>  left, Vector128<sbyte> right, byte rightScaledIndex);
        public static Vector64<uint>  DotProductBySelectedQuadruplet(Vector64<uint>  addend, Vector64<byte>   left, Vector64<byte>   right, byte rightScaledIndex);
        public static Vector64<uint>  DotProductBySelectedQuadruplet(Vector64<uint>  addend, Vector64<byte>   left, Vector128<byte>  right, byte rightScaledIndex);
        public static Vector128<int>  DotProductBySelectedQuadruplet(Vector128<int>  addend, Vector128<sbyte> left, Vector128<sbyte> right, byte rightScaledIndex);
        public static Vector128<int>  DotProductBySelectedQuadruplet(Vector128<int>  addend, Vector128<sbyte> left, Vector64<sbyte>  right, byte rightScaledIndex);
        public static Vector128<uint> DotProductBySelectedQuadruplet(Vector128<uint> addend, Vector128<byte>  left, Vector128<byte>  right, byte rightScaledIndex);
        public static Vector128<uint> DotProductBySelectedQuadruplet(Vector128<uint> addend, Vector128<byte>  left, Vector64<byte>   right, byte rightScaledIndex);
    }
}
```
## Allow creating more cryptographic objects from ReadOnlySpan<byte>

**Approved** | [#runtime/38348](https://github.com/dotnet/runtime/issues/38348#issuecomment-649759447) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=1h52m24s)

* Looks good as proposed

```diff
namespace System.Security.Cryptography
{
    partial class AsnEncodedData
    {
        protected AsnEncodedData() { }
        public AsnEncodedData(byte[] rawData) { }
+       public AsnEncodedData(System.ReadOnlySpan<byte> rawData) { }
        public AsnEncodedData(System.Security.Cryptography.AsnEncodedData asnEncodedData) { }
        public AsnEncodedData(System.Security.Cryptography.Oid? oid, byte[] rawData) { }
+       public AsnEncodedData(System.Security.Cryptography.Oid? oid, System.ReadOnlySpan<byte> rawData) { }
        public AsnEncodedData(string oid, byte[] rawData) { }
+       public AsnEncodedData(string oid, System.ReadOnlySpan<byte> rawData) { }
    }
}

namespace System.Security.Cryptography.Pkcs
{
    partial class ContentInfo
    {
        public static System.Security.Cryptography.Oid GetContentType(byte[] encodedMessage) { throw null; }
+       public static System.Security.Cryptography.Oid GetContentType(ReadOnlySpan<byte> encodedMessage) { throw null; }
    }

    partial class EnvelopedCms
    {
        public void Decode(byte[] encodedMessage) { }
+       public void Decode(System.ReadOnlySpan<byte> encodedMessage) { }
    }

    partial class SignedCms
    {
        public void Decode(byte[] encodedMessage) { }
+       public void Decode(System.ReadOnlySpan<byte> encodedMessage) { }
     }
}

namespace System.Security.Cryptography.X509Certificates
{
    partial class CertificateRequest
    {
        public System.Security.Cryptography.X509Certificates.X509Certificate2 Create(
            System.Security.Cryptography.X509Certificates.X500DistinguishedName issuerName,
            System.Security.Cryptography.X509Certificates.X509SignatureGenerator generator,
            System.DateTimeOffset notBefore,
            System.DateTimeOffset notAfter,
            byte[] serialNumber) { throw null; }
+       public System.Security.Cryptography.X509Certificates.X509Certificate2 Create(
+           System.Security.Cryptography.X509Certificates.X500DistinguishedName issuerName,
+           System.Security.Cryptography.X509Certificates.X509SignatureGenerator generator,
+           System.DateTimeOffset notBefore,
+           System.DateTimeOffset notAfter,
+           System.ReadOnlySpan<byte> serialNumber) { throw null; }
        public System.Security.Cryptography.X509Certificates.X509Certificate2 Create(
            System.Security.Cryptography.X509Certificates.X509Certificate2 issuerCertificate,
            System.DateTimeOffset notBefore,
            System.DateTimeOffset notAfter,
            byte[] serialNumber) { throw null; }
+       public System.Security.Cryptography.X509Certificates.X509Certificate2 Create(
+           System.Security.Cryptography.X509Certificates.X509Certificate2 issuerCertificate,
+           System.DateTimeOffset notBefore,
+           System.DateTimeOffset notAfter,
+           System.ReadOnlySpan<byte> serialNumber) { throw null; }
    }

    partial class X500DistinguishedName : System.Security.Cryptography.AsnEncodedData
    {
        public X500DistinguishedName(byte[] encodedDistinguishedName) { }
+       public X500DistinguishedName(System.ReadOnlySpan<byte> encodedDistinguishedName) { }
        public X500DistinguishedName(System.Security.Cryptography.AsnEncodedData encodedDistinguishedName) { }
        public X500DistinguishedName(
            System.Security.Cryptography.X509Certificates.X500DistinguishedName distinguishedName) { }
        public X500DistinguishedName(string distinguishedName) { }
        public X500DistinguishedName(
            string distinguishedName,
            System.Security.Cryptography.X509Certificates.X500DistinguishedNameFlags flag) { }
    }

    partial class X509Certificate2 : System.Security.Cryptography.X509Certificates.X509Certificate
    {
        public X509Certificate2() { }
        public X509Certificate2(byte[] rawData) { }
        [System.CLSCompliantAttribute(false)]
        public X509Certificate2(byte[] rawData, System.Security.SecureString? password) { }
        [System.CLSCompliantAttribute(false)]
        public X509Certificate2(
            byte[] rawData,
            System.Security.SecureString? password,
            System.Security.Cryptography.X509Certificates.X509KeyStorageFlags keyStorageFlags) { }
        public X509Certificate2(byte[] rawData, string? password) { }
        public X509Certificate2(
            byte[] rawData,
            string? password,
            System.Security.Cryptography.X509Certificates.X509KeyStorageFlags keyStorageFlags) { }
        public X509Certificate2(System.IntPtr handle) { }
+       public X509Certificate2(System.ReadOnlySpan<byte> rawData) { }
+       public X509Certificate2(
+           System.ReadOnlySpan<byte> rawData,
+           System.ReadOnlySpan<char> password,
+           X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet) { }
        protected X509Certificate2(
            System.Runtime.Serialization.SerializationInfo info,
            System.Runtime.Serialization.StreamingContext context) { }
        public X509Certificate2(System.Security.Cryptography.X509Certificates.X509Certificate certificate) { }
        public X509Certificate2(string fileName) { }
+       public X509Certificate2(
+           string fileName,
+           System.ReadOnlySpan<char> password,
+           X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet) { }
        [System.CLSCompliantAttribute(false)]
        public X509Certificate2(string fileName, System.Security.SecureString? password) { }
        [System.CLSCompliantAttribute(false)]
        public X509Certificate2(
            string fileName,
            System.Security.SecureString? password,
            System.Security.Cryptography.X509Certificates.X509KeyStorageFlags keyStorageFlags) { }
        public X509Certificate2(string fileName, string? password) { }
        public X509Certificate2(
            string fileName,
            string? password,
            System.Security.Cryptography.X509Certificates.X509KeyStorageFlags keyStorageFlags) { }
        public static X509ContentType GetCertContentType(byte[] rawData) { throw null; }
+       public static X509ContentType GetCertContentType(System.ReadOnlySpan<byte> rawData) { throw null; }
        public static X509ContentType GetCertContentType(string fileName) { throw null; }
    }

    partial class X509Certificate2Collection
    {
        public void Import(byte[] rawData) { }
        public void Import(
            byte[] rawData,
            string? password,
-           X509KeyStorageFlags keyStorageFlags) { }
+           X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet) { }
+       public void Import(System.ReadOnlySpan<byte> rawData) { }
+       public void Import(
+           System.ReadOnlySpan<byte> rawData,
+           System.ReadOnlySpan<char> password,
+           X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet) { }
        public void Import(string fileName) { }
+       public void Import(
+           string fileName,
+           System.ReadOnlySpan<byte> password,
+           X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet) { }
        public void Import(
            string fileName,
            string? password,
-           X509KeyStorageFlags keyStorageFlags) { }
+           X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet) { }
    }

    partial class X509Extension : System.Security.Cryptography.AsnEncodedData
        protected X509Extension() { }
        public X509Extension(System.Security.Cryptography.AsnEncodedData encodedExtension, bool critical) { }
        public X509Extension(System.Security.Cryptography.Oid oid, byte[] rawData, bool critical) { }
+       public X509Extension(System.Security.Cryptography.Oid oid, System.ReadOnlySpan<byte> rawData, bool critical) { }
        public X509Extension(string oid, byte[] rawData, bool critical) { }
+       public X509Extension(string oid, System.ReadOnlySpan<byte> rawData, bool critical) { }
    }

    partial class X509SubjectKeyIdentifierExtension : System.Security.Cryptography.X509Certificates.X509Extension
    {
        public X509SubjectKeyIdentifierExtension() { }
        public X509SubjectKeyIdentifierExtension(byte[] subjectKeyIdentifier, bool critical) { }
+       public X509SubjectKeyIdentifierExtension(System.ReadOnlySpan<byte> subjectKeyIdentifier, bool critical) { }
        public X509SubjectKeyIdentifierExtension(System.Security.Cryptography.AsnEncodedData encodedSubjectKeyIdentifier, bool critical) { }
        public X509SubjectKeyIdentifierExtension(System.Security.Cryptography.X509Certificates.PublicKey key, bool critical) { }
        public X509SubjectKeyIdentifierExtension(System.Security.Cryptography.X509Certificates.PublicKey key, System.Security.Cryptography.X509Certificates.X509SubjectKeyIdentifierHashAlgorithm algorithm, bool critical) { }
        public X509SubjectKeyIdentifierExtension(string subjectKeyIdentifier, bool critical) { }
    }
}
```
## Implement CachedBitmap functionality in System.Drawing

**Approved** | [#runtime/36105](https://github.com/dotnet/runtime/issues/36105#issuecomment-649761495) | [Video](https://www.youtube.com/watch?v=rx_098IdZU0&t=2h3m27s)

* Looks good as proposed

```diff
namespace System.Drawing.Imaging
{
+   public sealed class CachedBitmap : MarshalByRefObject, IDisposable
+   {
+       public CachedBitmap(Bitmap bitmap, Graphics graphics);
+       public void Dispose();  
+   }
} 

namespace System.Drawing
{
    public sealed class Graphics : MarshalByRefObject, IDisposable, IDeviceContext
    {
+       public void DrawCachedBitmap(CachedBitmap cachedBitmap, int x, int y);   
    }
}
```
