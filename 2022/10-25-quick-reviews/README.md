# API Review 10/25/2022

## Vector64/128/256/512<T> and Vector<T> Consistency/Enhancements

**Approved** | [#runtime/76593](https://github.com/dotnet/runtime/issues/76593#issuecomment-1290939323) | [Video](https://www.youtube.com/watch?v=KV85UmAY7iA&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Numerics;

public static partial class Vector
{
    public static Vector<T> Divide(Vector<T> left, T right);

    public static T GetElement<T>(this Vector<T> vector, int index) where T : struct;

    public static Vector<T> Load<T>(T* source) where T : unmanaged;
    public static Vector<T> LoadAligned<T>(T* source) where T : unmanaged;
    public static Vector<T> LoadAlignedNonTemporal<T>(T* source) where T : unmanaged;
    public static Vector<T> LoadUnsafe<T>(ref T source) where T : struct;
    public static Vector<T> LoadUnsafe<T>(ref T source, nuint index) where T : struct;

    public static unsafe void Store<T>(this Vector<T> source, T* destination) where T : unmanaged;
    public static unsafe void StoreAligned<T>(this Vector<T> source, T* destination) where T : unmanaged;
    public static unsafe void StoreAlignedNonTemporal<T>(this Vector<T> source, T* destination) where T : unmanaged;
    public static unsafe void StoreUnsafe<T>(this Vector<T> source, ref T destination) where T : struct;
    public static unsafe void StoreUnsafe<T>(this Vector<T> source, ref T destination, nuint index) where T : struct;

    public static T ToScalar<T>(this Vector<T> vector) where T : struct;

    public static Vector<T> WithElement<T> (this Vector<T> vector, int index, T value) where T : struct;
}

public partial struct Vector<T>
{
    public static Vector<T> AllBitsSet { get; }
    public static Vector<T> operator /(Vector<T> left, T right);
    public static Vector<T> operator +(Vector<T> value);
}
```

```C#
namespace System.Runtime.Intrinsics.X86;

public static partial class Vector64
{
    public static Vector64<T> CreateScalar<T>(T value) where T : struct;

    public static Vector64<T>     CreateScalarUnsafe<T>(T      value) where T : struct;
    public static Vector64<long>  CreateScalarUnsafe   (double value);
    public static Vector64<long>  CreateScalarUnsafe   (long   value);
    public static Vector64<ulong> CreateScalarUnsafe   (ulong  value);

    public static Vector64<T> Divide(Vector64<T> left, T right);

    public static Vector64<ushort> WidenLower(Vector64<byte>   value);
    public static Vector64<int>    WidenLower(Vector64<short>  value);
    public static Vector64<long>   WidenLower(Vector64<int>    value);
    public static Vector64<short>  WidenLower(Vector64<sbyte>  value);
    public static Vector64<double> WidenLower(Vector64<float>  value);
    public static Vector64<uint>   WidenLower(Vector64<ushort> value);
    public static Vector64<ulong>  WidenLower(Vector64<uint>   value);

    public static Vector64<ushort> WidenUpper(Vector64<byte>   value);
    public static Vector64<int>    WidenUpper(Vector64<short>  value);
    public static Vector64<long>   WidenUpper(Vector64<int>    value);
    public static Vector64<short>  WidenUpper(Vector64<sbyte>  value);
    public static Vector64<double> WidenUpper(Vector64<float>  value);
    public static Vector64<uint>   WidenUpper(Vector64<ushort> value);
    public static Vector64<ulong>  WidenUpper(Vector64<uint>   value);
}

public partial struct Vector64<T>
{
    public static Vector64<T> One { get; }

    public static Vector64<T> operator /(Vector64<T> left, T right);

    public static Vector64<T> operator <<(Vector64<T> left, int shiftAmount);
    public static Vector64<T> operator >>(Vector64<T> left, int shiftAmount);
    public static Vector64<T> operator >>>(Vector64<T> left, int shiftAmount);
}

public static partial class Vector128
{
    public static Vector128<T>     Create<T>(Vector64<T>     lower, Vector64<T>     upper) where T : struct;
    public static Vector128<nint>  Create   (Vector64<nint>  lower, Vector64<nint>  upper);
    public static Vector128<nuint> Create   (Vector64<nuint> lower, Vector64<nuint> upper);

    public static Vector128<T> CreateScalar<T>(T value) where T : struct;

    public static Vector128<T> CreateScalarUnsafe<T>(T value) where T : struct;

    public static Vector128<T> Divide(Vector128<T> left, T right);

    public static Vector128<ushort> WidenLower(Vector128<byte>   value);
    public static Vector128<int>    WidenLower(Vector128<short>  value);
    public static Vector128<long>   WidenLower(Vector128<int>    value);
    public static Vector128<short>  WidenLower(Vector128<sbyte>  value);
    public static Vector128<double> WidenLower(Vector128<float>  value);
    public static Vector128<uint>   WidenLower(Vector128<ushort> value);
    public static Vector128<ulong>  WidenLower(Vector128<uint>   value);

    public static Vector128<ushort> WidenUpper(Vector128<byte>   value);
    public static Vector128<int>    WidenUpper(Vector128<short>  value);
    public static Vector128<long>   WidenUpper(Vector128<int>    value);
    public static Vector128<short>  WidenUpper(Vector128<sbyte>  value);
    public static Vector128<double> WidenUpper(Vector128<float>  value);
    public static Vector128<uint>   WidenUpper(Vector128<ushort> value);
    public static Vector128<ulong>  WidenUpper(Vector128<uint>   value);
}

public partial struct Vector128<T>
{
    public static Vector128<T> One { get; }

    public static Vector128<T> operator /(Vector128<T> left, T right);

    public static Vector128<T> operator <<(Vector128<T> left, int shiftAmount);
    public static Vector128<T> operator >>(Vector128<T> left, int shiftAmount);
    public static Vector128<T> operator >>>(Vector128<T> left, int shiftAmount);
}

public static partial class Vector256
{
    public static Vector256<T>     Create<T>(Vector128<T>     lower, Vector128<T>     upper) where T : struct;
    public static Vector256<nint>  Create   (Vector128<nint>  lower, Vector128<nint>  upper);
    public static Vector256<nuint> Create   (Vector128<nuint> lower, Vector128<nuint> upper);

    public static Vector256<T> CreateScalar<T>(T value) where T : struct;

    public static Vector256<T> CreateScalarUnsafe<T>(T value) where T : struct;

    public static Vector256<T> Divide(Vector256<T> left, T right);

    public static Vector256<ushort> WidenLower(Vector256<byte>   value);
    public static Vector256<int>    WidenLower(Vector256<short>  value);
    public static Vector256<long>   WidenLower(Vector256<int>    value);
    public static Vector256<short>  WidenLower(Vector256<sbyte>  value);
    public static Vector256<double> WidenLower(Vector256<float>  value);
    public static Vector256<uint>   WidenLower(Vector256<ushort> value);
    public static Vector256<ulong>  WidenLower(Vector256<uint>   value);

    public static Vector256<ushort> WidenUpper(Vector256<byte>   value);
    public static Vector256<int>    WidenUpper(Vector256<short>  value);
    public static Vector256<long>   WidenUpper(Vector256<int>    value);
    public static Vector256<short>  WidenUpper(Vector256<sbyte>  value);
    public static Vector256<double> WidenUpper(Vector256<float>  value);
    public static Vector256<uint>   WidenUpper(Vector256<ushort> value);
    public static Vector256<ulong>  WidenUpper(Vector256<uint>   value);
}

public partial struct Vector256<T>
{
    public static Vector256<T> One { get; }

    public static Vector256<T> operator /(Vector256<T> left, T right);

    public static Vector256<T> operator <<(Vector256<T> left, int shiftAmount);
    public static Vector256<T> operator >>(Vector256<T> left, int shiftAmount);
    public static Vector256<T> operator >>>(Vector256<T> left, int shiftAmount);
}
```
## Expose System.Runtime.Intrinsics.X86.Avx512F.VL

**Approved** | [#runtime/74813](https://github.com/dotnet/runtime/issues/74813#issuecomment-1290951050) | [Video](https://www.youtube.com/watch?v=KV85UmAY7iA&t=0h48m54s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract partial class Avx512F : Avx2
{
    public abstract partial class VL
    {
        public static bool IsSupported { get; }

        // AVX512

        public static Vector256<int> BroadcastToVector256(Vector128<int> value);
        public static Vector256<uint> BroadcastToVector256(Vector128<uint> value);
        public static Vector256<float> BroadcastToVector256(Vector128<float> value);

        public static Vector128<byte> ConvertToVector128Byte(Vector256<int> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector128<int> value);

        public static Vector128<byte> ConvertToVector128Byte(Vector256<long> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector128<long> value);

        public static Vector128<byte> ConvertToVector128Byte(Vector256<uint> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector128<uint> value);

        public static Vector128<byte> ConvertToVector128Byte(Vector256<ulong> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector128<ulong> value);

        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector256<uint> value);
        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector128<uint> value);

        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector256<ulong> value);
        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector128<ulong> value);

        public static Vector128<short> ConvertToVector128Int16(Vector256<int> value);
        public static Vector128<short> ConvertToVector128Int16(Vector128<int> value);

        public static Vector128<short> ConvertToVector128Int16(Vector256<long> value);
        public static Vector128<short> ConvertToVector128Int16(Vector128<long> value);

        public static Vector128<short> ConvertToVector128Int16(Vector256<uint> value);
        public static Vector128<short> ConvertToVector128Int16(Vector128<uint> value);

        public static Vector128<short> ConvertToVector128Int16(Vector256<ulong> value);
        public static Vector128<short> ConvertToVector128Int16(Vector128<ulong> value);

        public static Vector128<int> ConvertToVector128Int32(Vector256<long> value);
        public static Vector128<int> ConvertToVector128Int32(Vector128<long> value);

        public static Vector128<int> ConvertToVector128Int32(Vector256<ulong> value);
        public static Vector128<int> ConvertToVector128Int32(Vector128<ulong> value);

        public static Vector128<int> ConvertToVector128Int32WithSaturation(Vector256<long> value);
        public static Vector128<int> ConvertToVector128Int32WithSaturation(Vector128<long> value);

        public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector256<int> value);
        public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector128<int> value);

        public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector256<long> value);
        public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector128<long> value);

        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<int> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector128<int> value);

        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<long> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector128<long> value);

        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<uint> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector128<uint> value);

        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<ulong> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector128<ulong> value);

        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector256<int> value);
        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector128<int> value);

        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector256<long> value);
        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector128<long> value);

        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<int> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector128<int> value);

        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<long> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector128<long> value);

        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<uint> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector128<uint> value);

        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<ulong> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector128<ulong> value);

        public static Vector128<ushort> ConvertToVector128UInt16WithSaturation(Vector256<uint> value);
        public static Vector128<ushort> ConvertToVector128UInt16WithSaturation(Vector128<uint> value);

        public static Vector128<short> ConvertToVector128UInt16WithSaturation(Vector256<long> value);
        public static Vector128<short> ConvertToVector128UInt16WithSaturation(Vector128<long> value);

        public static Vector128<uint> ConvertToVector128UInt32(Vector256<double> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector128<float> value);

        public static Vector128<uint> ConvertToVector128UInt32(Vector256<long> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector128<long> value);

        public static Vector128<uint> ConvertToVector128UInt32(Vector256<ulong> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector128<ulong> value);

        public static Vector128<uint> ConvertToVector128UInt32WithSaturation(Vector256<ulong> value);
        public static Vector128<uint> ConvertToVector128UInt32WithSaturation(Vector128<ulong> value);

        public static Vector256<double> ConvertToVector256Double(Vector128<uint> value);
        public static Vector128<double> ConvertToVector128Double(Vector128<uint> value);

        public static Vector256<float> ConvertToVector256Single(Vector128<uint> value);
        public static Vector128<float> ConvertToVector128Single(Vector128<uint> value);

        public static Vector256<double> Fixup(Vector256<double> left, Vector256<double> right, Vector256<long> table);
        public static Vector128<double> Fixup(Vector128<double> left, Vector128<double> right, Vector128<long> table);

        public static Vector256<float> Fixup(Vector256<float> left, Vector256<float> right, Vector256<int> table);
        public static Vector128<float> Fixup(Vector128<float> left, Vector128<float> right, Vector128<int> table);

        public static Vector256<double> GetExponent(Vector256<double> value);
        public static Vector128<double> GetExponent(Vector128<double> value);

        public static Vector256<float> GetExponent(Vector256<float> value);
        public static Vector128<float> GetExponent(Vector128<float> value);

        public static Vector256<double> GetMantissa(Vector256<double> value, byte interval, byte signControl);
        public static Vector128<double> GetMantissa(Vector128<double> value, byte interval, byte signControl);

        public static Vector256<float> GetMantissa(Vector256<float> value, byte interval, byte signControl);
        public static Vector128<float> GetMantissa(Vector128<float> value, byte interval, byte signControl);

        public static Vector128<double> PermuteVar2x64(Vector128<double> left, Vector128<double> right, Vector128<long> control);

        public static Vector128<long> PermuteVar2x64(Vector128<long> left, Vector128<long> right, Vector128<long> control);
        public static Vector128<ulong> PermuteVar2x64(Vector128<ulong> left, Vector128<ulong> right, Vector128<ulong> control);

        public static Vector128<float> PermuteVar4x32(Vector128<float> left, Vector128<float> right>, Vector128<int> control);

        public static Vector128<int> PermuteVar4x32(Vector128<int> left, Vector128<int> right, Vector128<int> control);
        public static Vector128<uint> PermuteVar4x32(Vector128<uint> left, Vector128<uint> right, Vector128<uint> control);

        public static Vector256<double> PermuteVar4x64(Vector256<double> value, Vector256<long> control);
        public static Vector256<double> PermuteVar4x64(Vector256<double> left, Vector256<double> right, Vector256<long> control);

        public static Vector256<long> PermuteVar4x64(Vector256<long> value, Vector256<long> control);
        public static Vector256<long> PermuteVar4x64(Vector256<long> left, Vector256<long> right, Vector256<long> control);

        public static Vector256<ulong> PermuteVar4x64(Vector256<ulong> value, Vector256<ulong> control);
        public static Vector256<ulong> PermuteVar4x64(Vector256<ulong> left, Vector256<ulong> right, Vector256<ulong> control);

        public static Vector256<float> PermuteVar8x32(Vector256<float> value, Vector256<int> control);
        public static Vector256<float> PermuteVar8x32(Vector256<float> left, Vector256<float> right>, Vector256<int> control);

        public static Vector256<int> PermuteVar8x32(Vector256<int> left, Vector256<int> right, Vector256<int> control);
        public static Vector256<uint> PermuteVar8x32(Vector256<uint> left, Vector256<uint> right, Vector256<uint> control);

        public static Vector256<long> ShiftRightArithmeticVariable(Vector256<long> value, Vector256<ulong> count);
        public static Vector128<long> ShiftRightArithmeticVariable(Vector256<int> value, Vector256<uint> count);

        public static Vector256<int> RotateLeft(Vector256<int> value, byte count);
        public static Vector128<int> RotateLeft(Vector128<int> value, byte count);

        public static Vector256<long> RotateLeft(Vector256<long> value, byte count);
        public static Vector128<long> RotateLeft(Vector128<long> value, byte count);

        public static Vector256<uint> RotateLeft(Vector256<uint> value, byte count);
        public static Vector128<uint> RotateLeft(Vector128<uint> value, byte count);

        public static Vector256<ulong> RotateLeft(Vector256<ulong> value, byte count);
        public static Vector128<ulong> RotateLeft(Vector128<ulong> value, byte count);

        public static Vector256<int> RotateLeftVariable(Vector256<int> value, Vector256<int> count);
        public static Vector128<int> RotateLeftVariable(Vector128<int> value, Vector128<int> count);

        public static Vector256<long> RotateLeftVariable(Vector256<long> value, Vector256<long> count);
        public static Vector128<long> RotateLeftVariable(Vector128<long> value, Vector128<long> count);

        public static Vector256<uint> RotateLeftVariable(Vector256<uint> value, Vector256<uint> count);
        public static Vector128<uint> RotateLeftVariable(Vector128<uint> value, Vector128<uint> count);

        public static Vector256<ulong> RotateLeftVariable(Vector256<ulong> value, Vector256<ulong> count);
        public static Vector128<ulong> RotateLeftVariable(Vector128<ulong> value, Vector128<ulong> count);

        public static Vector256<int> RotateRight(Vector256<int> value, byte count);
        public static Vector128<int> RotateRight(Vector128<int> value, byte count);

        public static Vector256<long> RotateRight(Vector256<long> value, byte count);
        public static Vector128<long> RotateRight(Vector128<long> value, byte count);

        public static Vector256<uint> RotateRight(Vector256<uint> value, byte count);
        public static Vector128<uint> RotateRight(Vector128<uint> value, byte count);

        public static Vector256<ulong> RotateRight(Vector256<ulong> value, byte count);
        public static Vector128<ulong> RotateRight(Vector128<ulong> value, byte count);

        public static Vector256<int> RotateRightVariable(Vector256<int> value, Vector256<int> count);
        public static Vector128<int> RotateRightVariable(Vector128<int> value, Vector128<int> count);

        public static Vector256<long> RotateRightVariable(Vector256<long> value, Vector256<long> count);
        public static Vector128<long> RotateRightVariable(Vector128<long> value, Vector128<long> count);

        public static Vector256<uint> RotateRightVariable(Vector256<uint> value, Vector256<uint> count);
        public static Vector128<uint> RotateRightVariable(Vector128<uint> value, Vector128<uint> count);

        public static Vector256<ulong> RotateRightVariable(Vector256<ulong> value, Vector256<ulong> count);
        public static Vector128<ulong> RotateRightVariable(Vector128<ulong> value, Vector128<ulong> count);

        public static void Scatter(double* baseAddress, Vector128<int> index, byte scale, Vector256<double> value);
        public static void Scatter(double* baseAddress, Vector128<int> index, byte scale, Vector128<double> value);

        public static void Scatter(double* baseAddress, Vector256<double> index, byte scale, Vector256<double> value);
        public static void Scatter(double* baseAddress, Vector128<double> index, byte scale, Vector128<double> value);

        public static void Scatter(int* baseAddress, Vector256<int> index, byte scale, Vector256<int> value);
        public static void Scatter(int* baseAddress, Vector128<int> index, byte scale, Vector128<int> value);

        public static void Scatter(int* baseAddress, Vector256<long> index, byte scale, Vector128<int> value);
        public static void Scatter(int* baseAddress, Vector128<long> index, byte scale, Vector128<int> value);

        public static void Scatter(long* baseAddress, Vector128<int> index, byte scale, Vector256<long> value);
        public static void Scatter(long* baseAddress, Vector128<int> index, byte scale, Vector128<long> value);

        public static void Scatter(long* baseAddress, Vector256<long> index, byte scale, Vector256<long> value);
        public static void Scatter(long* baseAddress, Vector128<long> index, byte scale, Vector128<long> value);

        public static void Scatter(float* baseAddress, Vector256<float> index, byte scale, Vector256<float> value);
        public static void Scatter(float* baseAddress, Vector128<float> index, byte scale, Vector128<float> value);

        public static void Scatter(float* baseAddress, Vector256<long> index, byte scale, Vector128<float> value);
        public static void Scatter(float* baseAddress, Vector128<long> index, byte scale, Vector128<float> value);

        public static Vector256<int> TernaryLogic(Vector256<int> left, Vector256<int> right, byte control);
        public static Vector128<int> TernaryLogic(Vector128<int> left, Vector128<int> right, byte control);

        public static Vector256<long> TernaryLogic(Vector256<long> left, Vector256<long> right, byte control);
        public static Vector128<long> TernaryLogic(Vector128<long> left, Vector128<long> right, byte control);

        public static Vector256<double> Reciprocal14(Vector256<double> value);
        public static Vector128<double> Reciprocal14(Vector128<double> value);

        public static Vector256<float> Reciprocal14(Vector256<float> value);
        public static Vector128<float> Reciprocal14(Vector128<float> value);

        public static Vector256<double> RoundScale(Vector256<double> value, byte scale);
        public static Vector128<double> RoundScale(Vector128<double> value, byte scale);

        public static Vector256<float> RoundScale(Vector256<float> value, byte scale);
        public static Vector128<float> RoundScale(Vector128<float> value, byte scale);

        public static Vector256<double> ReciprocalSqrt14(Vector256<double> value);
        public static Vector128<double> ReciprocalSqrt14(Vector128<double> value);

        public static Vector256<float> ReciprocalSqrt14(Vector256<float> value);
        public static Vector128<float> ReciprocalSqrt14(Vector128<float> value);

        public static Vector256<double> Scale(Vector256<double> value);
        public static Vector128<double> Scale(Vector128<double> value);

        public static Vector256<float> Scale(Vector256<float> value);
        public static Vector128<float> Scale(Vector128<float> value););

        public static Vector256<int> Shuffle(Vector256<int> left, Vector256<int> right, byte control);
    }
}
```
## Expose AVX512BW, AVX512CD, and AVX512DQ

**Approved** | [#runtime/76579](https://github.com/dotnet/runtime/issues/76579#issuecomment-1290974174) | [Video](https://www.youtube.com/watch?v=KV85UmAY7iA&t=0h59m52s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract partial class Avx512BW : Avx512F
{
    // SSE-SSE4.2

    public static Vector512<sbyte> Abs(Vector512<sbyte> value);
    public static Vector512<short> Abs(Vector512<short> value);

    public static Vector512<byte>   Add(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<short>  Add(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<sbyte>  Add(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<ushort> Add(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<sbyte>  AddSaturate(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<short>  AddSaturate(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<byte>   AddSaturate(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<ushort> AddSaturate(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<byte>  AlignRight(Vector512<byte>  left, Vector512<sbyte>  right, byte mask);
    public static Vector512<sbyte> AlignRight(Vector512<sbyte> left, Vector512<sbyte> right, byte mask);

    public static Vector512<byte>   Average(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<ushort> Average(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<short> ConvertToVector512Int16(Vector256<byte>  value);
    public static Vector512<short> ConvertToVector512Int16(Vector512<sbyte> value);

    public static Vector512<byte>   Max(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<short>  Max(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<sbyte>  Max(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<ushort> Max(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<byte>   Min(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<short>  Min(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<sbyte>  Min(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<ushort> Min(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<int>   MultiplyAddAdjacent(Vector512<short> left, Vector512<short> right);
    public static Vector512<short> MultiplyAddAdjacent(Vector512<byte>  left, Vector512<sbyte> right);

    public static Vector512<short>  MultiplyHigh(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<ushort> MultiplyHigh(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<short> MultiplyHighRoundScale(Vector512<short> left, Vector512<short> right);

    public static Vector512<short>  MultiplyLow(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<ushort> MultiplyLow(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<sbyte> PackSignedSaturate(Vector512<short> left, Vector512<short> right);
    public static Vector512<short> PackSignedSaturate(Vector512<int>   left, Vector512<int>   right);

    public static Vector512<byte>   PackUnsignedSaturate(Vector512<short> left, Vector512<short> right);
    public static Vector512<ushort> PackUnsignedSaturate(Vector512<int>   left, Vector512<int>   right);

    public static Vector512<short>  ShiftLeftLogical(Vector512<short>  value, byte count);
    public static Vector512<ushort> ShiftLeftLogical(Vector512<ushort> value, byte count);
    public static Vector512<short>  ShiftLeftLogical(Vector512<short>  value, Vector128<short>  count);
    public static Vector512<ushort> ShiftLeftLogical(Vector512<ushort> value, Vector128<ushort> count);

    public static Vector512<byte>   ShiftLeftLogical128BitLane(Vector512<byte>   value, byte numBytes);
    public static Vector512<short>  ShiftLeftLogical128BitLane(Vector512<short>  value, byte numBytes);
    public static Vector512<int>    ShiftLeftLogical128BitLane(Vector512<int>    value, byte numBytes);
    public static Vector512<long>   ShiftLeftLogical128BitLane(Vector512<long>   value, byte numBytes);
    public static Vector512<sbyte>  ShiftLeftLogical128BitLane(Vector512<sbyte>  value, byte numBytes);
    public static Vector512<ushort> ShiftLeftLogical128BitLane(Vector512<ushort> value, byte numBytes);
    public static Vector512<uint>   ShiftLeftLogical128BitLane(Vector512<uint>   value, byte numBytes);
    public static Vector512<ulong>  ShiftLeftLogical128BitLane(Vector512<ulong>  value, byte numBytes);

    public static Vector512<short>  ShiftRightArithmetic(Vector512<short>  value, byte count);
    public static Vector512<short>  ShiftRightArithmetic(Vector512<short>  value, Vector128<short>  count);

    public static Vector512<short>  ShiftRightLogical(Vector512<short>  value, byte count);
    public static Vector512<ushort> ShiftRightLogical(Vector512<ushort> value, byte count);
    public static Vector512<short>  ShiftRightLogical(Vector512<short>  value, Vector128<short>  count);
    public static Vector512<ushort> ShiftRightLogical(Vector512<ushort> value, Vector128<ushort> count);

    public static Vector512<byte>   ShiftRightLogical128BitLane(Vector512<byte>   value, byte numBytes);
    public static Vector512<short>  ShiftRightLogical128BitLane(Vector512<short>  value, byte numBytes);
    public static Vector512<int>    ShiftRightLogical128BitLane(Vector512<int>    value, byte numBytes);
    public static Vector512<long>   ShiftRightLogical128BitLane(Vector512<long>   value, byte numBytes);
    public static Vector512<sbyte>  ShiftRightLogical128BitLane(Vector512<sbyte>  value, byte numBytes);
    public static Vector512<ushort> ShiftRightLogical128BitLane(Vector512<ushort> value, byte numBytes);
    public static Vector512<uint>   ShiftRightLogical128BitLane(Vector512<uint>   value, byte numBytes);
    public static Vector512<ulong>  ShiftRightLogical128BitLane(Vector512<ulong>  value, byte numBytes);

    public static Vector512<byte>  Shuffle(Vector512<byte>  value, Vector512<byte>  mask);
    public static Vector512<sbyte> Shuffle(Vector512<sbyte> value, Vector512<sbyte> mask);

    public static Vector512<short>  ShuffleHigh(Vector512<short>  value, byte control);
    public static Vector512<ushort> ShuffleHigh(Vector512<ushort> value, byte control);

    public static Vector512<short>  ShuffleLow(Vector512<short>  value, byte control);
    public static Vector512<ushort> ShuffleLow(Vector512<ushort> value, byte control);

    public static Vector512<byte>   Subtract(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<short>  Subtract(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<sbyte>  Subtract(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<ushort> Subtract(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<byte>   SubtractSaturate(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<short>  SubtractSaturate(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<sbyte>  SubtractSaturate(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<ushort> SubtractSaturate(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<ushort> SumAbsoluteDifferences(Vector512<byte> byte, Vector512<byte> byte);

    public static Vector512<byte>   UnpackHigh(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<sbyte>  UnpackHigh(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<short>  UnpackHigh(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<ushort> UnpackHigh(Vector512<ushort> left, Vector512<ushort> right);

    public static Vector512<byte>   UnpackLow(Vector512<byte>   left, Vector512<byte>   right);
    public static Vector512<short>  UnpackLow(Vector512<short>  left, Vector512<short>  right);
    public static Vector512<sbyte>  UnpackLow(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static Vector512<ushort> UnpackLow(Vector512<ushort> left, Vector512<ushort> right);

    // AVX-AVX2

    public static Vector512<byte>   BroadcastScalarToVector512(Vector128<byte>   value);
    public static Vector512<short>  BroadcastScalarToVector512(Vector128<short>  value);
    public static Vector512<sbyte>  BroadcastScalarToVector512(Vector128<sbyte>  value);
    public static Vector512<ushort> BroadcastScalarToVector512(Vector128<ushort> value);

    public static Vector512<short>  PermuteVar32x16(Vector512<short>  value, Vector512<short>  control);
    public static Vector512<ushort> PermuteVar32x16(Vector512<ushort> value, Vector512<ushort> control);

    // AVX512

    public static Vector512<byte> SumAbsoluteDifferencesWidening(Vector512<byte> left, Vector512<byte> right, byte control);

    public static Vector512<short>  PermuteVar32x16(Vector512<short>  left, Vector512<short>  right, Vector512<short>  control);
    public static Vector512<ushort> PermuteVar32x16(Vector512<ushort> left, Vector512<ushort> right, Vector512<ushort> control);

    public static Vector256<byte>  ConvertToVector256ByteWithSaturation (Vector512<ushort> value);
    public static Vector256<sbyte> ConvertToVector256SByteWithSaturation(Vector512<short>  value);

    public static Vector512<short>  ShiftLeftLogicalVariable(Vector512<short>  value, Vector512<short>  count);
    public static Vector512<ushort> ShiftLeftLogicalVariable(Vector512<ushort> value, Vector512<ushort> count);

    public static Vector512<short>  ShiftRightArithmeticVariable(Vector512<short> value, Vector512<short> count);

    public static Vector512<short>  ShiftRightLogicalVariable(Vector512<short> value, Vector512<short>  count);
    public static Vector512<ushort> ShiftRightLogicalVariable(Vector512<short> value, Vector512<ushort> count);
}

public abstract partial class Avx512CD : Avx512F
{
    // AVX512

    public static Vector512<int>   DetectConflicts(Vector512<int>   value);
    public static Vector512<long>  DetectConflicts(Vector512<long>  value);
    public static Vector512<uint>  DetectConflicts(Vector512<uint>  value);
    public static Vector512<ulong> DetectConflicts(Vector512<ulong> value);

    public static Vector512<int>   LeadingZeroCount(Vector512<int>   value);
    public static Vector512<long>  LeadingZeroCount(Vector512<long>  value);
    public static Vector512<uint>  LeadingZeroCount(Vector512<uint>  value);
    public static Vector512<ulong> LeadingZeroCount(Vector512<ulong> value);
}


public abstract partial class Avx512DQ : Avx512F
{
    // SSE-SSE4.2

    public static Vector512<double> And(Vector512<double> left, Vector512<double> right);
    public static Vector512<float>  And(Vector512<float>  left, Vector512<float>  right);

    public static Vector512<double> AndNot(Vector512<double> left, Vector512<double> right);
    public static Vector512<float>  AndNot(Vector512<float>  left, Vector512<float>  right);

    public static Vector512<long>  MultiplyLow(Vector512<long>  left, Vector512<long>  right);
    public static Vector512<ulong> MultiplyLow(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<double> Or(Vector512<double> left, Vector512<double> right);
    public static Vector512<float>  Or(Vector512<float>  left, Vector512<float>  right);

    public static Vector512<double> Xor(Vector512<double> left, Vector512<double> right);
    public static Vector512<float>  Xor(Vector512<float>  left, Vector512<float>  right);

    // AVX-AVX2

    public static Vector128<double> ExtractVector128(Vector512<double> value, byte index);
    public static Vector128<long>   ExtractVector128(Vector512<long>   value, byte index);
    public static Vector128<ulong>  ExtractVector128(Vector512<ulong>  value, byte index);

    public static Vector256<int>   ExtractVector256(Vector512<int>   value, byte index);
    public static Vector256<float> ExtractVector256(Vector512<float> value, byte index);
    public static Vector256<uint>  ExtractVector256(Vector512<uint>  value, byte index);

    public static Vector512<double> InsertVector128(Vector512<double> value, Vector128<double> data, byte index);
    public static Vector512<long>   InsertVector128(Vector512<long>   value, Vector128<long>   data, byte index);
    public static Vector512<ulong>  InsertVector128(Vector512<ulong>  value, Vector128<ulong>  data, byte index);

    public static Vector512<int>   InsertVector256(Vector512<int>   value, Vector256<int>   data, byte index);
    public static Vector512<float> InsertVector256(Vector512<float> value, Vector256<float> data, byte index);
    public static Vector512<uint>  InsertVector256(Vector512<uint>  value, Vector256<uint>  data, byte index);

    public static Vector512<int>   BroadcastPairScalarToVector512(Vector128<int>  value);
    public static Vector512<uint>  BroadcastPairScalarToVector512(Vector128<uint> value);

    public static Vector512<long>  BroadcastToVector512(Vector128<long>  value);
    public static Vector512<ulong> BroadcastToVector512(Vector128<ulong> value);
    public static Vector512<int>   BroadcastToVector512(Vector256<int>   value);
    public static Vector512<uint>  BroadcastToVector512(Vector256<uint>  value);

    // AVX512

    public static Vector512<float> BroadcastPairScalarToVector512(Vector128<float> value);

    public static Vector512<double> BroadcastToVector512(Vector128<double> value);
    public static Vector512<float>  BroadcastToVector512(Vector256<float>  value);

    public static Vector256<float> ConvertToVector256Single(Vector512<long>  value);
    public static Vector256<float> ConvertToVector256Single(Vector512<long>  value, FloatRoundingMode mode);
    public static Vector256<float> ConvertToVector256Single(Vector512<ulong> value);
    public static Vector256<float> ConvertToVector256Single(Vector512<ulong> value, FloatRoundingMode mode);

    public static Vector512<double> ConvertToVector512Double(Vector512<long> value);
    public static Vector512<double> ConvertToVector512Double(Vector512<long> value, FloatRoundingMode mode);
    public static Vector512<double> ConvertToVector512Double(Vector512<ulong> value);
    public static Vector512<double> ConvertToVector512Double(Vector512<ulong> value, FloatRoundingMode mode);

    public static Vector512<long> ConvertToVector512Int64(Vector512<double> value);
    public static Vector512<long> ConvertToVector512Int64(Vector512<double> value, FloatRoundingMode mode);
    public static Vector512<long> ConvertToVector512Int64(Vector512<float>  value);
    public static Vector512<long> ConvertToVector512Int64(Vector512<float>  value, FloatRoundingMode mode);

    public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector256<float>  value);
    public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector256<float>  value, FloatRoundingMode mode);
    public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector512<double> value);
    public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector512<double> value, FloatRoundingMode mode);

    public static Vector512<ulong> ConvertToVector512UInt64(Vector256<float>  value);
    public static Vector512<ulong> ConvertToVector512UInt64(Vector256<float>  value, FloatRoundingMode mode);
    public static Vector512<ulong> ConvertToVector512UInt64(Vector512<double> value);
    public static Vector512<ulong> ConvertToVector512UInt64(Vector512<double> value, FloatRoundingMode mode);

    public static Vector512<double> RangeRestrict(Vector512<double> left, Vector512<double> right, byte control);
    public static Vector512<float>  RangeRestrict(Vector512<float>  left, Vector512<float>  right, byte control);

    public static Vector128<double> RangeRestrictScalar(Vector128<double> left,  Vector128<double> right, byte control);
    public static Vector128<float>  RangeRestrictScalar(Vector128<float>  left,  Vector128<float>  right, byte control);

    public static Vector128<double> RangeRestrictScalar(Vector128<double> upper, Vector128<double> left, Vector128<double> right, byte control);
    public static Vector128<float>  RangeRestrictScalar(Vector128<float>  upper, Vector128<float>  left, Vector128<float>  right, byte control);

    public static Vector512<double> ReductionTransform(Vector512<double> value, byte control);
    public static Vector512<float>  ReductionTransform(Vector512<float>  value, byte control);

    public static Vector128<double> ReductionTransformScalar(Vector128<double> upper, Vector128<double> value, byte control);
    public static Vector128<float>  ReductionTransformScalar(Vector128<float>  upper, Vector128<float>  value, byte control);
}
```
## Vector{128|256}.Narrow with saturation

**Approved** | [#runtime/75724](https://github.com/dotnet/runtime/issues/75724#issuecomment-1290988369) | [Video](https://www.youtube.com/watch?v=KV85UmAY7iA&t=1h22m5s)

* We should also add them to `Vector<T>`
    - part of the proposal
* We should differentiate between signed and unsigned narrowing
    - TBD, @tannergooding will update this comment

```C#
namespace System.Numerics;

public partial static class Vector
{
    [CLSCompliant(false)]
    public static Vector<sbyte> NarrowWithSaturation(Vector<short> lower, Vector<short> upper);
    public static Vector<short> NarrowWithSaturation(Vector<int> lower, Vector<int> upper);
    public static Vector<int> NarrowWithSaturation(Vector<long> lower, Vector<long> upper);
    [CLSCompliant(false)]
    public static Vector<byte> NarrowWithSaturation(Vector<ushort> lower, Vector<ushort> upper);
    [CLSCompliant(false)]
    public static Vector<ushort> NarrowWithSaturation(Vector<uint> lower, Vector<uint> upper);
    public static Vector<uint> NarrowWithSaturation(Vector<ulong> lower, Vector<ulong> upper);
    public static Vector<float> NarrowWithSaturation(Vector<double> lower, Vector<double> upper);
}
```

```C#
namespace System.Runtime.Intrinsics;

public partial static class Vector64
{
    [CLSCompliant(false)]
    public static Vector64<sbyte> NarrowWithSaturation(Vector64<short> lower, Vector64<short> upper);
    public static Vector64<short> NarrowWithSaturation(Vector64<int> lower, Vector64<int> upper);
    public static Vector64<int> NarrowWithSaturation(Vector64<long> lower, Vector64<long> upper);
    [CLSCompliant(false)]
    public static Vector64<byte> NarrowWithSaturation(Vector64<ushort> lower, Vector64<ushort> upper);
    [CLSCompliant(false)]
    public static Vector64<ushort> NarrowWithSaturation(Vector64<uint> lower, Vector64<uint> upper);
    public static Vector64<uint> NarrowWithSaturation(Vector64<ulong> lower, Vector64<ulong> upper);
    public static Vector64<float> NarrowWithSaturation(Vector64<double> lower, Vector64<double> upper);
}

public partial static class Vector128
{
    [CLSCompliant(false)]
    public static Vector128<sbyte> NarrowWithSaturation(Vector128<short> lower, Vector128<short> upper);
    public static Vector128<short> NarrowWithSaturation(Vector128<int> lower, Vector128<int> upper);
    public static Vector128<int> NarrowWithSaturation(Vector128<long> lower, Vector128<long> upper);
    [CLSCompliant(false)]
    public static Vector128<byte> NarrowWithSaturation(Vector128<ushort> lower, Vector128<ushort> upper);
    [CLSCompliant(false)]
    public static Vector128<ushort> NarrowWithSaturation(Vector128<uint> lower, Vector128<uint> upper);
    public static Vector128<uint> NarrowWithSaturation(Vector128<ulong> lower, Vector128<ulong> upper);
    public static Vector128<float> NarrowWithSaturation(Vector128<double> lower, Vector128<double> upper);
}

public partial static class Vector256
{
    [CLSCompliant(false)]
    public static Vector256<sbyte> NarrowWithSaturation(Vector256<short> lower, Vector256<short> upper);
    public static Vector256<short> NarrowWithSaturation(Vector256<int> lower, Vector256<int> upper);
    public static Vector256<int> NarrowWithSaturation(Vector256<long> lower, Vector256<long> upper);
    [CLSCompliant(false)]
    public static Vector256<byte> NarrowWithSaturation(Vector256<ushort> lower, Vector256<ushort> upper);
    [CLSCompliant(false)]
    public static Vector256<ushort> NarrowWithSaturation(Vector256<uint> lower, Vector256<uint> upper);
    public static Vector256<uint> NarrowWithSaturation(Vector256<ulong> lower, Vector256<ulong> upper);
    public static Vector256<float> NarrowWithSaturation(Vector256<double> lower, Vector256<double> upper);
}

public partial static class Vector512
{
    [CLSCompliant(false)]
    public static Vector512<sbyte> NarrowWithSaturation(Vector512<short> lower, Vector512<short> upper);
    public static Vector512<short> NarrowWithSaturation(Vector512<int> lower, Vector512<int> upper);
    public static Vector512<int> NarrowWithSaturation(Vector512<long> lower, Vector512<long> upper);
    [CLSCompliant(false)]
    public static Vector512<byte> NarrowWithSaturation(Vector512<ushort> lower, Vector512<ushort> upper);
    [CLSCompliant(false)]
    public static Vector512<ushort> NarrowWithSaturation(Vector512<uint> lower, Vector512<uint> upper);
    public static Vector512<uint> NarrowWithSaturation(Vector512<ulong> lower, Vector512<ulong> upper);
    public static Vector512<float> NarrowWithSaturation(Vector512<double> lower, Vector512<double> upper);
}
```
## Vectorize IndexOfAny on more than 5 chars

**NeedsWork** | [#runtime/68328](https://github.com/dotnet/runtime/issues/68328#issuecomment-1291026417) | [Video](https://www.youtube.com/watch?v=KV85UmAY7iA&t=1h35m3s)

* We don't like extending `ReadOnlySpan<byte>`
* We also don't like a nested type that is critical for use; should be a top-level type, like `Ascii`
* Should this be an iterator style API that returns something one can use to find all matches?

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static int IndexOfAny(this ReadOnlySpan<byte> span, IndexOfAnyAsciiValues asciiValues);
    public static int IndexOfAnyExcept(this ReadOnlySpan<byte> span, IndexOfAnyAsciiValues asciiValues);
    public static int LastIndexOfAny(this ReadOnlySpan<byte> span, IndexOfAnyAsciiValues asciiValues);
    public static int LastIndexOfAnyExcept(this ReadOnlySpan<byte> span, IndexOfAnyAsciiValues asciiValues);

    public static int IndexOfAny(this ReadOnlySpan<char> span, IndexOfAnyAsciiValues asciiValues);
    public static int IndexOfAnyExcept(this ReadOnlySpan<char> span, IndexOfAnyAsciiValues asciiValues);
    public static int LastIndexOfAny(this ReadOnlySpan<char> span, IndexOfAnyAsciiValues asciiValues);
    public static int LastIndexOfAnyExcept(this ReadOnlySpan<char> span, IndexOfAnyAsciiValues asciiValues);
    // ... repeat for Span overloads

    public readonly struct IndexOfAnyAsciiValues
    {
        public IndexOfAnyAsciiValues(ReadOnlySpan<char> asciiValues);
        // No other public members
    }
}
```
