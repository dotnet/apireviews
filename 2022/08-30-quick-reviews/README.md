# API Review 08/30/2022

## Expose Vector512<T> to support the x86/x64 AVX-512 instruction set

**Approved** | [#runtime/73262](https://github.com/dotnet/runtime/issues/73262#issuecomment-1231949960) | [Video](https://www.youtube.com/watch?v=O-1AzEUXPTo&t=0h0m0s)

* We corrected `IsHardwareAccelerated` in the proposal to be static, as intended.
* Corrected a couple of typos
* Otherwise, looks good as proposed.

```C#
namespace System.Runtime.Intrinsics;

public static partial class Vector512
{
    public static bool IsHardwareAccelerated { get; }

    public static Vector512<T> Abs<T>(Vector512<T> value) where T : struct;

    public static Vector512<T> Add<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> AndNot<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<TTo> As<TFrom, TTo>(this Vector512<TFrom> vector) where TFrom : struct where TTo : struct;

    public static Vector512<byte> AsByte<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<double> AsDouble<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<short> AsInt16<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<int> AsInt32<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<long> AsInt64<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<nint> AsNInt<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<nuint> AsNUInt<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<sbyte> AsSByte<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<float> AsSingle<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<ushort> AsUInt16<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<uint> AsUInt32<T>(this Vector512<T> vector) where T : struct;
    public static Vector512<ulong> AsUInt64<T>(this Vector512<T> vector) where T : struct;

    public static Vector512<T> AsVector512<T>(this Vector<T> value) where T : struct;
    public static Vector<T> AsVector<T>(this Vector512<T> value) where T : struct;

    public static Vector512<T> BitwiseAnd<T>(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> BitwiseOr<T>(Vector512<T> left, Vector512<T> right);

    public static Vector512<double> Ceiling(Vector512<double> value);
    public static Vector512<float> Ceiling(Vector512<float> value);

    public static Vector512<T> ConditionalSelect<T>(Vector512<T> condition, Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<double> ConvertToDouble(Vector512<long> value);
    public static Vector512<double> ConvertToDouble(Vector512<ulong> value);

    public static Vector512<int> ConvertToInt32(Vector512<float> value);
    public static Vector512<long> ConvertToInt64(Vector512<double> value);

    public static Vector512<float> ConvertToSingle(Vector512<int> value);
    public static Vector512<float> ConvertToSingle(Vector512<uint> value);

    public static Vector512<uint> ConvertToUInt32(Vector512<float> value);
    public static Vector512<ulong> ConvertToUInt64(Vector512<double> value);

    // Vector64/128/256 also expose non-generic overloads
    public static Vector512<T> Create<T>(T value) where T : struct;

    // Vector64/128/256 only expose non-generic overloads
    public static Vector512<T> Create<T>(Vector256<T> lower, Vector256<T> upper) where T : struct;

    public static Vector512<T> Create<T>(T[] values) where T : struct;
    public static Vector512<T> Create<T>(T[] values, int index) where T : struct;

    public static Vector512<T> Create<T>(ReadOnlySpan<T> values) where T : struct;

    public static Vector512<byte> Create(byte e0, byte e1, byte e2, byte e3, byte e4, byte e5, byte e6, byte e7, byte e8, byte e9, byte e10, byte e11, byte e12, byte e13, byte e14, byte e15, byte e16, byte e17, byte e18, byte e19, byte e20, byte e21, byte e22, byte e23, byte e24, byte e25, byte e26, byte e27, byte e28, byte e29, byte e30, byte e31, byte e32, byte e33, byte e34, byte e35, byte e36, byte e37, byte e38, byte e39, byte e40, byte e41, byte e42, byte e43, byte e44, byte e45, byte e46, byte e47, byte e48, byte e49, byte e50, byte e51, byte e52, byte e53, byte e54, byte e55, byte e56, byte e57, byte e58, byte e59, byte e60, byte e61, byte e62, byte e63);
    public static Vector512<double> Create(double e0, double e1, double e2, double e3, double e4, double e5, double e6, double e7);
    public static Vector512<short> Create(short e0, short e1, short e2, short e3, short e4, short e5, short e6, short e7, short e8, short e9, short e10, short e11, short e12, short e13, short e14, short e15, short e16, short e17, short e18, short e19, short e20, short e21, short e22, short e23, short e24, short e25, short e26, short e27, short e28, short e29, short e30, short e31);
    public static Vector512<int> Create(int e0, int e1, int e2, int e3, int e4, int e5, int e6, int e7, int e8, int e9, int e10, int e11, int e12, int e13, int e14, int e15);
    public static Vector512<long> Create(long e0, long e1, long e2, long e3, long e4, long e5, long e6, long e7);
    public static Vector512<sbyte> Create(sbyte e0, sbyte e1, sbyte e2, sbyte e3, sbyte e4, sbyte e5, sbyte e6, sbyte e7, sbyte e8, sbyte e9, sbyte e10, sbyte e11, sbyte e12, sbyte e13, sbyte e14, sbyte e15, sbyte e16, sbyte e17, sbyte e18, sbyte e19, sbyte e20, sbyte e21, sbyte e22, sbyte e23, sbyte e24, sbyte e25, sbyte e26, sbyte e27, sbyte e28, sbyte e29, sbyte e30, sbyte e31, sbyte e32, sbyte e33, sbyte e34, sbyte e35, sbyte e36, sbyte e37, sbyte e38, sbyte e39, sbyte e40, sbyte e41, sbyte e42, sbyte e43, sbyte e44, sbyte e45, sbyte e46, sbyte e47, sbyte e48, sbyte e49, sbyte e50, sbyte e51, sbyte e52, sbyte e53, sbyte e54, sbyte e55, sbyte e56, sbyte e57, sbyte e58, sbyte e59, sbyte e60, sbyte e61, sbyte e62, sbyte e63);
    public static Vector512<float> Create(float e0, float e1, float e2, float e3, float e4, float e5, float e6, float e7, float e8, float e9, float e10, float e11, float e12, float e13, float e14, float e15);
    public static Vector512<ushort> Create(ushort e0, ushort e1, ushort e2, ushort e3, ushort e4, ushort e5, ushort e6, ushort e7, ushort e8, ushort e9, ushort e10, ushort e11, ushort e12, ushort e13, ushort e14, ushort e15, ushort e16, ushort e17, ushort e18, ushort e19, ushort e20, ushort e21, ushort e22, ushort e23, ushort e24, ushort e25, ushort e26, ushort e27, ushort e28, ushort e29, ushort e30, ushort e31);
    public static Vector512<uint> Create(uint e0, uint e1, uint e2, uint e3, uint e4, uint e5, uint e6, uint e7, uint e8, uint e9, uint e10, uint e11, uint e12, uint e13, uint e14, uint e15);
    public static Vector512<ulong> Create(ulong e0, ulong e1, ulong e2, ulong e3, ulong e4, ulong e5, ulong e6, ulong e7);
    
    // Vector64/128/256 only expose non-generic overloads
    public static Vector512<T> CreateScalar<T>(T value) where T : struct;
    public static Vector512<T> CreateScalarUnsafe<T>(T value) where T : struct;

    public static void CopyTo<T>(this Vector512<T> vector, Span<T> destination) where T : struct;
    public static void CopyTo<T>(this Vector512<T> vector, T[] destination) where T : struct;
    public static void CopyTo<T>(this Vector512<T> vector, T[] destination, int index) where T : struct;

    public static Vector512<T> Divide<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> Dot<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> Equals<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool EqualsAll<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool EqualsAny<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    
    public static ulong ExtractMostSignificantBits<T>(this Vector512<T> vector) where T : struct;

    public static Vector512<double> Floor(Vector512<double> value);
    public static Vector512<float> Floor(Vector512<float> value);

    public static T GetElement<T>(this Vector512<T> vector, int index) where T : struct;
    
    public static Vector256<T> GetLower<T>(this Vector512<T> vector) where T : struct;
    public static Vector256<T> GetUpper<T>(this Vector512<T> vector) where T : struct;

    public static Vector512<T> GreaterThan<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool GreaterThanAll<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool GreaterThanAny<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> GreaterThanOrEqual<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool GreaterThanOrEqualAll<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool GreaterThanOrEqualAny<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> LessThan<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool LessThanAll<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool LessThanAny<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> LessThanOrEqual<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool LessThanOrEqualAll<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static bool LessThanOrEqualAny<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> Load<T>(T* source) where T : unmanaged;
    public static Vector512<T> LoadAligned<T>(T* source) where T : unmanaged;
    public static Vector512<T> LoadAlignedNonTemporal<T>(T* source) where T : unmanaged;
    public static Vector512<T> LoadUnsafe<T>(ref T source) where T : struct;
    public static Vector512<T> LoadUnsafe<T>(ref T source, nuint elementOffset) where T : struct;

    public static Vector512<T> Max<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static Vector512<T> Min<T>(Vector512<T> left, Vector512<T> right) where T : struct;

    public static Vector512<T> Multiply<T>(Vector512<T> left, Vector512<T> right) where T : struct;
    public static Vector512<T> Multiply<T>(Vector512<T> left, T right) where T : struct;
    public static Vector512<T> Multiply<T>(T left, Vector512<T> right) where T : struct;

    public static Vector512<float> Narrow(Vector512<double> lower, Vector512<double> upper);
    public static Vector512<sbyte> Narrow(Vector512<short> lower, Vector512<short> upper);
    public static Vector512<short> Narrow(Vector512<int> lower, Vector512<int> upper);
    public static Vector512<int> Narrow(Vector512<long> lower, Vector512<long> upper);
    public static Vector512<byte> Narrow(Vector512<ushort> lower, Vector512<ushort> upper);
    public static Vector512<ushort> Narrow(Vector512<uint> lower, Vector512<uint> upper);
    public static Vector512<uint> Narrow(Vector512<ulong> lower, Vector512<ulong> upper);

    public static Vector512<T> Negate<T>(Vector512<T> value);
    public static Vector512<T> OnesComplement<T>(Vector512<T> value);

    public static Vector512<byte> ShiftLeft(Vector512<byte> vector, int shiftCount);
    public static Vector512<short> ShiftLeft(Vector512<short> vector, int shiftCount);
    public static Vector512<int> ShiftLeft(Vector512<int> vector, int shiftCount);
    public static Vector512<long> ShiftLeft(Vector512<long> vector, int shiftCount);
    public static Vector512<nint> ShiftLeft(Vector512<nint> vector, int shiftCount);
    public static Vector512<nuint> ShiftLeft(Vector512<nuint> vector, int shiftCount);
    public static Vector512<sbyte> ShiftLeft(Vector512<sbyte> vector, int shiftCount);
    public static Vector512<ushort> ShiftLeft(Vector512<ushort> vector, int shiftCount);
    public static Vector512<uint> ShiftLeft(Vector512<uint> vector, int shiftCount);
    public static Vector512<ulong> ShiftLeft(Vector512<ulong> vector, int shiftCount);
    
    public static Vector512<byte> ShiftRightArithmetic(Vector512<byte> vector, int shiftCount);
    public static Vector512<short> ShiftRightArithmetic(Vector512<short> vector, int shiftCount);
    public static Vector512<int> ShiftRightArithmetic(Vector512<int> vector, int shiftCount);
    public static Vector512<long> ShiftRightArithmetic(Vector512<long> vector, int shiftCount);
    public static Vector512<nint> ShiftRightArithmetic(Vector512<nint> vector, int shiftCount);
    public static Vector512<nuint> ShiftRightArithmetic(Vector512<nuint> vector, int shiftCount);
    public static Vector512<sbyte> ShiftRightArithmetic(Vector512<sbyte> vector, int shiftCount);
    public static Vector512<ushort> ShiftRightArithmetic(Vector512<ushort> vector, int shiftCount);
    public static Vector512<uint> ShiftRightArithmetic(Vector512<uint> vector, int shiftCount);
    public static Vector512<ulong> ShiftRightArithmetic(Vector512<ulong> vector, int shiftCount);
    
    public static Vector512<byte> ShiftRightLogical(Vector512<byte> vector, int shiftCount);
    public static Vector512<short> ShiftRightLogical(Vector512<short> vector, int shiftCount);
    public static Vector512<int> ShiftRightLogical(Vector512<int> vector, int shiftCount);
    public static Vector512<long> ShiftRightLogical(Vector512<long> vector, int shiftCount);
    public static Vector512<nint> ShiftRightLogical(Vector512<nint> vector, int shiftCount);
    public static Vector512<nuint> ShiftRightLogical(Vector512<nuint> vector, int shiftCount);
    public static Vector512<sbyte> ShiftRightLogical(Vector512<sbyte> vector, int shiftCount);
    public static Vector512<ushort> ShiftRightLogical(Vector512<ushort> vector, int shiftCount);
    public static Vector512<uint> ShiftRightLogical(Vector512<uint> vector, int shiftCount);
    public static Vector512<ulong> ShiftRightLogical(Vector512<ulong> vector, int shiftCount);

    public static Vector512<byte> Shuffle(Vector512<byte> vector, Vector512<byte> indices);
    public static Vector512<double> Shuffle(Vector512<double> vector, Vector512<long> indices);
    public static Vector512<short> Shuffle(Vector512<short> vector, Vector512<short> indices);
    public static Vector512<int> Shuffle(Vector512<int> vector, Vector512<int> indices);
    public static Vector512<long> Shuffle(Vector512<long> vector, Vector512<long> indices);
    public static Vector512<nint> Shuffle(Vector512<nint> vector, Vector512<nint> indices);
    public static Vector512<nuint> Shuffle(Vector512<nuint> vector, Vector512<nuint> indices);
    public static Vector512<sbyte> Shuffle(Vector512<sbyte> vector, Vector512<sbyte> indices);
    public static Vector512<float> Shuffle(Vector512<float> vector, Vector512<int> indices);
    public static Vector512<ushort> Shuffle(Vector512<ushort> vector, Vector512<ushort> indices);
    public static Vector512<uint> Shuffle(Vector512<uint> vector, Vector512<uint> indices);
    public static Vector512<ulong> Shuffle(Vector512<ulong> vector, Vector512<ulong> indices);

    public static Vector512<byte> Shuffle(Vector512<byte> lower, Vector512<byte> upper, Vector512<byte> indices);
    public static Vector512<double> Shuffle(Vector512<double> lower, Vector512<double> upper, Vector512<long> indices);
    public static Vector512<short> Shuffle(Vector512<short> lower, Vector512<short> upper, Vector512<short> indices);
    public static Vector512<int> Shuffle(Vector512<int> lower, Vector512<int> upper, Vector512<int> indices);
    public static Vector512<long> Shuffle(Vector512<long> lower, Vector512<long> upper, Vector512<long> indices);
    public static Vector512<nint> Shuffle(Vector512<nint> lower, Vector512<nint> upper, Vector512<nint> indices);
    public static Vector512<nuint> Shuffle(Vector512<nuint> lower, Vector512<nuint> upper, Vector512<nuint> indices);
    public static Vector512<sbyte> Shuffle(Vector512<sbyte> lower, Vector512<sbyte> upper, Vector512<sbyte> indices);
    public static Vector512<float> Shuffle(Vector512<float> lower, Vector512<float> upper, Vector512<int> indices);
    public static Vector512<ushort> Shuffle(Vector512<ushort> lower, Vector512<ushort> upper, Vector512<ushort> indices);
    public static Vector512<uint> Shuffle(Vector512<uint> lower, Vector512<uint> upper, Vector512<uint> indices);
    public static Vector512<ulong> Shuffle(Vector512<ulong> lower, Vector512<ulong> upper, Vector512<ulong> indices);

    public static Vector512<T> Sqrt<T>(Vector512<T> value) where T : struct;

    public static T Sum<T>(Vector512<T> vector) where T : struct;

    public static Vector512<T> Subtract(Vector512<T> left, Vector512<T> right) where T : struct;

    public static void Store<T>(this Vector512<T> source, T* destination) where T : unmanaged;
    public static void StoreAligned<T>(this Vector512<T> source, T* destination) where T : unmanaged;
    public static void StoreAlignedNonTemporal<T>(this Vector512<T> source, T* destination) where T : unmanaged;
    public static void StoreUnsafe<T>(this Vector512<T> source, ref T destination) where T : struct;
    public static void StoreUnsafe<T>(this Vector512<T> source, ref T destination, nuint elementOffset) where T : struct;

    public static T ToScalar<T>(this Vector512<T> vector) where T : struct;

    public static bool TryCopyTo<T>(this Vector512<T> vector, Span<T> destination) where T : struct;

    public static (Vector512<short> Lower, Vector512<short> Upper) Widen(Vector512<sbyte> value);
    public static (Vector512<int> Lower, Vector512<int> Upper) Widen(Vector512<short> value);
    public static (Vector512<long> Lower, Vector512<long> Upper) Widen(Vector512<int> value);
    public static (Vector512<double> Lower, Vector512<double> Upper) Widen(Vector512<float> value);
    public static (Vector512<ushort> Lower, Vector512<ushort> Upper) Widen(Vector512<byte> value);
    public static (Vector512<uint> Lower, Vector512<uint> Upper) Widen(Vector512<ushort> value);
    public static (Vector512<ulong> Lower, Vector512<ulong> Upper) Widen(Vector512<uint> value);

    public static Vector512<T> WithElement<T>(this Vector512<T> vector, int index, T value) where T : struct;
    
    public static Vector512<T> WithLower<T>(this Vector512<T> vector, Vector256<T> value) where T : struct;
    public static Vector512<T> WithUpper<T>(this Vector512<T> vector, Vector256<T> value) where T : struct;

    public static Vector512<T> Xor(Vector512<T> left, Vector512<T> right) where T : struct;
}

public partial struct Vector512<T> : IEquatable<Vector512<T>>
    where T : struct
{
    public static Vector512<T> AllBitsSet { get; }

    public static int Count { get; }

    public static bool IsSupported { get; }

    public static Vector512<T> Zero { get; }

    public T this[int index] { get; }

    public static Vector512<T> operator +(Vector512<T> value);
    public static Vector512<T> operator -(Vector512<T> value);

    public static Vector512<T> operator ~(Vector512<T> value);

    public static bool operator ==(Vector512<T> left, Vector512<T> right);
    public static bool operator !=(Vector512<T> left, Vector512<T> right);

    public static Vector512<T> operator +(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> operator -(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> operator *(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> operator *(Vector512<T> left, T right);
    public static Vector512<T> operator *(T left, Vector512<T> right);
    public static Vector512<T> operator /(Vector512<T> left, Vector512<T> right);

    public static Vector512<T> operator &(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> operator |(Vector512<T> left, Vector512<T> right);
    public static Vector512<T> operator ^(Vector512<T> left, Vector512<T> right);

    public override bool Equals([NotNullWhen(true)] object? obj);
    public bool Equals(Vector512<T> other);

    public override int GetHashCode();
    
    public override string ToString();
}
```
## Expose VectorMask<T> to support generic masking for Vector<T>

**Approved** | [#runtime/74613](https://github.com/dotnet/runtime/issues/74613#issuecomment-1232011456) | [Video](https://www.youtube.com/watch?v=O-1AzEUXPTo&t=0h12m53s)

* We corrected the `IsHardwareAccelerated` properties in the proposal to be static, as intended.
* We agreed that instead of VectorMask64 it should be Vector64Mask.
* We also would like to investigate what nested types would look like here, e.g. `Vector64<T>.Mask`
* We discussed the naming/casing of Xnor, and decided it is correct as proposed.
* After a very long discussion to try to understand the implications of the type-size conversions on vector masks, we renamed the `As-` operations to `To-` because of the complexities of "don't care" bits.
* The argument to all of the VectorNMask.Create was adjusted based on feedback (byte, ushort, uint, ulong) vs what was proposed (all ushort)

```C#
namespace System.Runtime.Intrinsics
{
    public static partial class Vector64
    {
        public Vector64Mask<T> ExtractMask<T>(this Vector64<T> vector);
    }

    public static partial class Vector128
    {
        public Vector128Mask<T> ExtractMask<T>(this Vector128<T> vector);
    }

    public static partial class Vector256
    {
        public Vector256Mask<T> ExtractMask<T>(this Vector256<T> vector);
    }

    public static partial class Vector512
    {
        public Vector512Mask<T> ExtractMask<T>(this Vector512<T> vector);
    }

    public static class Vector64Mask
    {
        public static bool IsHardwareAccelerated { get; }

        public static Vector64Mask<T> Create(byte mask);

        public static Vector64Mask<TTo> To<TFrom, TTo>(this Vector64Mask<TFrom> vector) where TFrom : struct where TTo : struct;

        public static Vector64Mask<byte>   ToByte  <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<double> ToDouble<T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<short>  ToInt16 <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<int>    ToInt32 <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<long>   ToInt64 <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<nint>   ToNInt  <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<nuint>  ToNUInt <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<sbyte>  ToSByte <T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<float>  ToSingle<T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<ushort> ToUInt16<T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<uint>   ToUInt32<T>(this Vector64Mask<T> vector) where T : struct;
        public static Vector64Mask<ulong>  ToUInt64<T>(this Vector64Mask<T> vector) where T : struct;

        public static Vector64Mask<T> BitwiseAnd<T>(Vector64Mask<T> left, Vector64Mask<T> right);
        public static Vector64Mask<T> BitwiseOr<T>(Vector64Mask<T> left, Vector64Mask<T> right);
        public static Vector64Mask<T> AndNot<T>(Vector64Mask<T> left, Vector64Mask<T> right);
        public static Vector64Mask<T> OnesComplement<T>(Vector64Mask<T> value);
        public static Vector64Mask<T> Xor<T>(Vector64Mask<T> left, Vector64Mask<T> right);
        public static Vector64Mask<T> Xnor<T>(Vector64Mask<T> left, Vector64Mask<T> right);

        public static Vector64Mask<T> ShiftLeft<T>(Vector64Mask<T> value, int count);
        public static Vector64Mask<T> ShiftRight<T>(Vector64Mask<T> value, int count);

        public static bool Equals<T>(Vector64Mask<T> left, Vector64Mask<T> right);

        public static int LeadingZeroCount(Vector64Mask<T> mask);
        public static int TrailingZeroCount(Vector64Mask<T> mask);
        public static int PopCount(Vector64Mask<T> mask);
        
        public static bool GetElement(this Vector64<T> vector, int index) where T : struct;
        public static Vector64Mask<T> WithElement(this Vector64<T> vector, int index, bool value) where T : struct;
    }

    public readonly struct Vector64Mask<T> where T : struct 
    {
        private readonly byte _value;

        public static bool IsSupported { get; }
        public static int Count { get; }

        public static Vector64Mask<T> AllBitsSet { get; }
        public static Vector64Mask<T> Zero { get; }

        public static bool this[int index] { get; }

        public static Vector64Mask<T> operator &(Vector64Mask<T> left, Vector64Mask<T> right);
        public static Vector64Mask<T> operator |(Vector64Mask<T> left, Vector64Mask<T> right);
        public static Vector64Mask<T> operator ~(Vector64Mask<T> value);
        public static Vector64Mask<T> operator ^(Vector64Mask<T> left, Vector64Mask<T> right);

        public static Vector64Mask<T> operator <<(Vector64Mask<T> value, int count);
        public static Vector64Mask<T> operator >>(Vector64Mask<T> value, int count);

        public static bool operator ==(Vector64Mask<T> left, Vector64Mask<T> right);
        public static bool operator !=(Vector64Mask<T> left, Vector64Mask<T> right);
    }

    public static class Vector128Mask
    {
        public static bool IsHardwareAccelerated { get; }

        public static Vector128Mask<T> Create(ushort mask);

        public static Vector128Mask<TTo> To<TFrom, TTo>(this Vector128Mask<TFrom> vector) where TFrom : struct where TTo : struct;

        public static Vector128Mask<byte>   ToByte  <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<double> ToDouble<T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<short>  ToInt16 <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<int>    ToInt32 <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<long>   ToInt64 <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<nint>   ToNInt  <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<nuint>  ToNUInt <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<sbyte>  ToSByte <T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<float>  ToSingle<T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<ushort> ToUInt16<T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<uint>   ToUInt32<T>(this Vector128Mask<T> vector) where T : struct;
        public static Vector128Mask<ulong>  ToUInt64<T>(this Vector128Mask<T> vector) where T : struct;

        public static Vector128Mask<T> BitwiseAnd<T>(Vector128Mask<T> left, Vector128Mask<T> right);
        public static Vector128Mask<T> BitwiseOr<T>(Vector128Mask<T> left, Vector128Mask<T> right);
        public static Vector128Mask<T> AndNot<T>(Vector128Mask<T> left, Vector128Mask<T> right);
        public static Vector128Mask<T> OnesComplement<T>(Vector128Mask<T> value);
        public static Vector128Mask<T> Xor<T>(Vector128Mask<T> left, Vector128Mask<T> right);
        public static Vector128Mask<T> Xnor<T>(Vector128Mask<T> left, Vector128Mask<T> right);

        public static Vector128Mask<T> ShiftLeft<T>(Vector128Mask<T> value, int count);
        public static Vector128Mask<T> ShiftRight<T>(Vector128Mask<T> value, int count);

        public static bool Equals<T>(Vector128Mask<T> left, Vector128Mask<T> right);

        public static int LeadingZeroCount(Vector128Mask<T> mask);
        public static int TrailingZeroCount(Vector128Mask<T> mask);
        public static int PopCount(Vector128Mask<T> mask);
        
        public static bool GetElement(this Vector128<T> vector, int index) where T : struct;
        public static Vector128Mask<T> WithElement(this Vector128<T> vector, int index, bool value) where T : struct;
    }

    public readonly struct Vector128Mask<T> where T : struct 
    {
        private readonly ushort _value;

        public static bool IsSupported { get; }
        public static int Count { get; }

        public static Vector128Mask<T> AllBitsSet { get; }
        public static Vector128Mask<T> Zero { get; }

        public static bool this[int index] { get; }

        public static Vector128Mask<T> operator &(Vector128Mask<T> left, Vector128Mask<T> right);
        public static Vector128Mask<T> operator |(Vector128Mask<T> left, Vector128Mask<T> right);
        public static Vector128Mask<T> operator ~(Vector128Mask<T> value);
        public static Vector128Mask<T> operator ^(Vector128Mask<T> left, Vector128Mask<T> right);

        public static Vector128Mask<T> operator <<(Vector128Mask<T> value, int count);
        public static Vector128Mask<T> operator >>(Vector128Mask<T> value, int count);

        public static bool operator ==(Vector128Mask<T> left, Vector128Mask<T> right);
        public static bool operator !=(Vector128Mask<T> left, Vector128Mask<T> right);
    }

    public static class Vector256Mask
    {
        public static bool IsHardwareAccelerated { get; }

        public static Vector256Mask<T> Create(uint mask);

        public static Vector256Mask<TTo> To<TFrom, TTo>(this Vector256Mask<TFrom> vector) where TFrom : struct where TTo : struct;

        public static Vector256Mask<byte>   ToByte  <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<double> ToDouble<T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<short>  ToInt16 <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<int>    ToInt32 <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<long>   ToInt64 <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<nint>   ToNInt  <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<nuint>  ToNUInt <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<sbyte>  ToSByte <T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<float>  ToSingle<T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<ushort> ToUInt16<T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<uint>   ToUInt32<T>(this Vector256Mask<T> vector) where T : struct;
        public static Vector256Mask<ulong>  ToUInt64<T>(this Vector256Mask<T> vector) where T : struct;

        public static Vector256Mask<T> BitwiseAnd<T>(Vector256Mask<T> left, Vector256Mask<T> right);
        public static Vector256Mask<T> BitwiseOr<T>(Vector256Mask<T> left, Vector256Mask<T> right);
        public static Vector256Mask<T> AndNot<T>(Vector256Mask<T> left, Vector256Mask<T> right);
        public static Vector256Mask<T> OnesComplement<T>(Vector256Mask<T> value);
        public static Vector256Mask<T> Xor<T>(Vector256Mask<T> left, Vector256Mask<T> right);
        public static Vector256Mask<T> Xnor<T>(Vector256Mask<T> left, Vector256Mask<T> right);

        public static Vector256Mask<T> ShiftLeft<T>(Vector256Mask<T> value, int count);
        public static Vector256Mask<T> ShiftRight<T>(Vector256Mask<T> value, int count);

        public static bool Equals<T>(Vector256Mask<T> left, Vector256Mask<T> right);

        public static int LeadingZeroCount(Vector256Mask<T> mask);
        public static int TrailingZeroCount(Vector256Mask<T> mask);
        public static int PopCount(Vector256Mask<T> mask);
        
        public static bool GetElement(this Vector256<T> vector, int index) where T : struct;
        public static Vector256Mask<T> WithElement(this Vector256<T> vector, int index, bool value) where T : struct;
    }

    public readonly struct Vector256Mask<T> where T : struct 
    {
        private readonly uint _value;

        public static bool IsSupported { get; }
        public static int Count { get; }

        public static Vector256Mask<T> AllBitsSet { get; }
        public static Vector256Mask<T> Zero { get; }

        public static bool this[int index] { get; }

        public static Vector256Mask<T> operator &(Vector256Mask<T> left, Vector256Mask<T> right);
        public static Vector256Mask<T> operator |(Vector256Mask<T> left, Vector256Mask<T> right);
        public static Vector256Mask<T> operator ~(Vector256Mask<T> value);
        public static Vector256Mask<T> operator ^(Vector256Mask<T> left, Vector256Mask<T> right);

        public static Vector256Mask<T> operator <<(Vector256Mask<T> value, int count);
        public static Vector256Mask<T> operator >>(Vector256Mask<T> value, int count);

        public static bool operator ==(Vector256Mask<T> left, Vector256Mask<T> right);
        public static bool operator !=(Vector256Mask<T> left, Vector256Mask<T> right);
    }

    public static class Vector512Mask
    {
        public static bool IsHardwareAccelerated { get; }

        public static Vector512Mask<T> Create(ulong mask);

        public static Vector512Mask<TTo> To<TFrom, TTo>(this Vector512Mask<TFrom> vector) where TFrom : struct where TTo : struct;

        public static Vector512Mask<byte>   ToByte  <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<double> ToDouble<T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<short>  ToInt16 <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<int>    ToInt32 <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<long>   ToInt64 <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<nint>   ToNInt  <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<nuint>  ToNUInt <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<sbyte>  ToSByte <T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<float>  ToSingle<T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<ushort> ToUInt16<T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<uint>   ToUInt32<T>(this Vector512Mask<T> vector) where T : struct;
        public static Vector512Mask<ulong>  ToUInt64<T>(this Vector512Mask<T> vector) where T : struct;

        public static Vector512Mask<T> BitwiseAnd<T>(Vector512Mask<T> left, Vector512Mask<T> right);
        public static Vector512Mask<T> BitwiseOr<T>(Vector512Mask<T> left, Vector512Mask<T> right);
        public static Vector512Mask<T> AndNot<T>(Vector512Mask<T> left, Vector512Mask<T> right);
        public static Vector512Mask<T> OnesComplement<T>(Vector512Mask<T> value);
        public static Vector512Mask<T> Xor<T>(Vector512Mask<T> left, Vector512Mask<T> right);
        public static Vector512Mask<T> Xnor<T>(Vector512Mask<T> left, Vector512Mask<T> right);

        public static Vector512Mask<T> ShiftLeft<T>(Vector512Mask<T> value, int count);
        public static Vector512Mask<T> ShiftRight<T>(Vector512Mask<T> value, int count);

        public static bool Equals<T>(Vector512Mask<T> left, Vector512Mask<T> right);

        public static int LeadingZeroCount(Vector512Mask<T> mask);
        public static int TrailingZeroCount(Vector512Mask<T> mask);
        public static int PopCount(Vector512Mask<T> mask);
        
        public static bool GetElement(this Vector512<T> vector, int index) where T : struct;
        public static Vector512Mask<T> WithElement(this Vector512<T> vector, int index, bool value) where T : struct;
    }

    public readonly struct Vector512Mask<T> where T : struct 
    {
        private readonly ulong _value;

        public static bool IsSupported { get; }
        public static int Count { get; }

        public static Vector512Mask<T> AllBitsSet { get; }
        public static Vector512Mask<T> Zero { get; }

        public static bool this[int index] { get; }

        public static Vector512Mask<T> operator &(Vector512Mask<T> left, Vector512Mask<T> right);
        public static Vector512Mask<T> operator |(Vector512Mask<T> left, Vector512Mask<T> right);
        public static Vector512Mask<T> operator ~(Vector512Mask<T> value);
        public static Vector512Mask<T> operator ^(Vector512Mask<T> left, Vector512Mask<T> right);

        public static Vector512Mask<T> operator <<(Vector512Mask<T> value, int count);
        public static Vector512Mask<T> operator >>(Vector512Mask<T> value, int count);

        public static bool operator ==(Vector512Mask<T> left, Vector512Mask<T> right);
        public static bool operator !=(Vector512Mask<T> left, Vector512Mask<T> right);
    }
}

namespace System.Numerics
{
    public static partial class Vector
    {
        public VectorMask<T> ExtractMask<T>(this Vector<T> vector);
    }

    public static class VectorMask
    {
        public static bool IsHardwareAccelerated { get; }

        public static VectorMask<T> Create(byte[] value);
        public static VectorMask<T> Create(byte[] value, int index);
        public static VectorMask<T> Create(ReadOnlySpan<byte> value);

        public static VectorMask<TTo> To<TFrom, TTo>(this VectorMask<TFrom> vector) where TFrom : struct where TTo : struct;

        public static VectorMask<byte>   ToByte  <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<double> ToDouble<T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<short>  ToInt16 <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<int>    ToInt32 <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<long>   ToInt64 <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<nint>   ToNInt  <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<nuint>  ToNUInt <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<sbyte>  ToSByte <T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<float>  ToSingle<T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<ushort> ToUInt16<T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<uint>   ToUInt32<T>(this VectorMask<T> vector) where T : struct;
        public static VectorMask<ulong>  ToUInt64<T>(this VectorMask<T> vector) where T : struct;

        public static VectorMask<T> BitwiseAnd<T>(VectorMask<T> left, VectorMask<T> right);
        public static VectorMask<T> BitwiseOr<T>(VectorMask<T> left, VectorMask<T> right);
        public static VectorMask<T> AndNot<T>(VectorMask<T> left, VectorMask<T> right);
        public static VectorMask<T> OnesComplement<T>(VectorMask<T> value);
        public static VectorMask<T> Xor<T>(VectorMask<T> left, VectorMask<T> right);
        public static VectorMask<T> Xnor<T>(VectorMask<T> left, VectorMask<T> right);

        public static VectorMask<T> ShiftLeft<T>(VectorMask<T> value, int count);
        public static VectorMask<T> ShiftRight<T>(VectorMask<T> value, int count);

        public static bool Equals<T>(VectorMask<T> left, VectorMask<T> right);

        public static int LeadingZeroCount(VectorMask<T> mask);
        public static int TrailingZeroCount(VectorMask<T> mask);
        public static int PopCount(VectorMask<T> mask);
        
        public static bool GetElement(this Vector<T> vector, int index) where T : struct;
        public static VectorMask<T> WithElement(this Vector<T> vector, int index, bool value) where T : struct;
    }

    public readonly struct VectorMask<T> where T : struct 
    {
        private readonly ulong _value;

        public static bool IsSupported { get; }
        public static int Count { get; }

        public static VectorMask<T> AllBitsSet { get; }
        public static VectorMask<T> Zero { get; }

        public static bool this[int index] { get; }

        public static VectorMask<T> operator &(VectorMask<T> left, VectorMask<T> right);
        public static VectorMask<T> operator |(VectorMask<T> left, VectorMask<T> right);
        public static VectorMask<T> operator ~(VectorMask<T> value);
        public static VectorMask<T> operator ^(VectorMask<T> left, VectorMask<T> right);

        public static VectorMask<T> operator <<(VectorMask<T> value, int count);
        public static VectorMask<T> operator >>(VectorMask<T> value, int count);

        public static bool operator ==(VectorMask<T> left, VectorMask<T> right);
        public static bool operator !=(VectorMask<T> left, VectorMask<T> right);
    }
}
```
## Expose System.Runtime.Intrinsics.X86.Avx512F

**Approved** | [#runtime/73604](https://github.com/dotnet/runtime/issues/73604#issuecomment-1232057712) | [Video](https://www.youtube.com/watch?v=O-1AzEUXPTo&t=1h16m8s)

* All of the FloatRoundingMode parameters probably want the "use a constant here" attribute.
* Many of these method groups should have both signed and unsigned integer types.  The updated version here will likely miss some, but they're considered approved.
* We talked about making the FloatRoundingMode a defaulted parameter, which would require adding a FloatRoundingMode value for "do what the control register says", but that didn't feel necessary at this time.
* "Fixup" is apparently considered one word in .NET, so that is the correct casing.
* Consider an enum instead of `byte` for `TernaryLogic`

```C#
namespace System.Runtime.Intrinsics.X86;

public enum FloatRoundingMode : byte
{
    ToEven = 0x08,                // _MM_FROUND_TO_NEAREST_INT | _MM_FROUND_NO_EXC
    ToNegativeInfinity = 0x09,    // _MM_FROUND_TO_NEG_INF | _MM_FROUND_NO_EXC
    ToPositiveInfinity = 0x0A,    // _MM_FROUND_TO_POS_INF | _MM_FROUND_NO_EXC
    ToZero = 0x0B,                // _MM_FROUND_TO_ZERO | _MM_FROUND_NO_EXC
}

public abstract partial class Avx512F : Avx2
{
    public static new bool IsSupported { get; }

    // SSE-SSE4.2

    public static Vector512<int> Abs(Vector512<int> value);
    public static Vector512<long> Abs(Vector512<long> value);

    public static Vector512<double> Add(Vector512<double> left, Vector512<double> right);
    public static Vector512<double> Add(Vector512<double> left, Vector512<double> right, FloatRoundingMode mode);

    public static Vector512<int> Add(Vector512<int> left, Vector512<int> right);
    public static Vector512<uint> Add(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<long> Add(Vector512<long> left, Vector512<long> right);
    public static Vector512<ulong> Add(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<float> Add(Vector512<float> left, Vector512<float> right);
    public static Vector512<float> Add(Vector512<float> left, Vector512<float> right, FloatRoundingMode mode);

    public static Vector128<double> AddScalar(Vector128<double> left, Vector128<double> right, FloatRoundingMode mode);
    public static Vector128<float> AddScalar(Vector128<float> left, Vector128<float> right, FloatRoundingMode mode);

    public static Vector512<int> And(Vector512<int> left, Vector512<int> right);
    public static Vector512<uint> And(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<long> And(Vector512<long> left, Vector512<long> right);
    public static Vector512<ulong> And(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<int> AndNot(Vector512<int> left, Vector512<int> right);
    public static Vector512<uint> AndNot(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<long> AndNot(Vector512<long> left, Vector512<long> right);
    public static Vector512<ulong> AndNot(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, Vector128<double> value, FloatRoundingMode mode);
    public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, Vector128<float> value, FloatRoundingMode mode);

    public static int ConvertToInt32(Vector128<double> value, FloatRoundingMode mode);
    public static int ConvertToInt32(Vector128<float> value, FloatRoundingMode mode);

    public static Vector512<double> ConvertToVector256Double(Vector256<int> value);

    public static Vector256<int> ConvertToVector256Int32(Vector512<double> value);
    public static Vector256<int> ConvertToVector256Int32(Vector512<double> value, FloatRoundingMode mode);

    public static Vector512<float> ConvertToVector256Single(Vector512<int> value);
    public static Vector512<float> ConvertToVector256Single(Vector512<int> value, FloatRoundingMode mode);

    public static Vector256<float> ConvertToVector256Single(Vector512<double> value);
    public static Vector256<float> ConvertToVector256Single(Vector512<double> value, FloatRoundingMode mode);

    public static Vector512<double> ConvertToVector512Double(Vector256<float> value);

    public static Vector512<int> ConvertToVector512Int32(Vector128<byte> value);
    public static Vector512<int> ConvertToVector512Int32(Vector512<short> value);
    public static Vector512<int> ConvertToVector512Int32(Vector512<sbyte> value);
    public static Vector512<int> ConvertToVector512Int32(Vector128<ushort> value);

    public static Vector512<int> ConvertToVector512Int32(Vector512<float> value);
    public static Vector512<int> ConvertToVector512Int32(Vector512<float> value, FloatRoundingMode mode);

    public static Vector256<int> ConvertToVector256Int32WithTruncation(Vector512<double> value);
    public static Vector512<int> ConvertToVector512Int32WithTruncation(Vector512<float> value);

    public static Vector512<long> ConvertToVector512Int64(Vector128<byte> value);
    public static Vector512<long> ConvertToVector512Int64(Vector512<short> value);
    public static Vector512<long> ConvertToVector512Int64(Vector512<int> value);
    public static Vector512<long> ConvertToVector512Int64(Vector512<sbyte> value);
    public static Vector512<long> ConvertToVector512Int64(Vector256<uint> value);
    public static Vector512<long> ConvertToVector512Int64(Vector256<ushort> value);

    public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, int value, FloatRoundingMode mode);

    public static Vector512<double> Divide(Vector512<double> left, Vector512<double> right);
    public static Vector512<double> Divide(Vector512<double> left, Vector512<double> right, FloatRoundingMode mode);

    public static Vector512<float> Divide(Vector512<float> left, Vector512<float> right);
    public static Vector512<float> Divide(Vector512<float> left, Vector512<float> right, FloatRoundingMode mode);

    public static Vector128<double> DivideScalar(Vector128<double> left, Vector128<double> right, FloatRoundingMode mode);
    public static Vector128<float> DivideScalar(Vector128<float> left, Vector128<float> right, FloatRoundingMode mode);

    public static Vector512<float> DuplicateOddIndexed(Vector512<float> value);
    public static Vector512<float> DuplicateEvenIndexed(Vector512<float> value);

    public static Vector512<byte> LoadVector512(byte* address);
    public static Vector512<double> LoadVector512(double* address);
    public static Vector512<short> LoadVector512(short* address);
    public static Vector512<int> LoadVector512(int* address);
    public static Vector512<long> LoadVector512(long* address);
    public static Vector512<nint> LoadVector512(nint* address);
    public static Vector512<sbyte> LoadVector512(sbyte* address);
    public static Vector512<float> LoadVector512(float* address);
    public static Vector512<ushort> LoadVector512(ushort* address);
    public static Vector512<uint> LoadVector512(uint* address);
    public static Vector512<ulong> LoadVector512(ulong* address);
    public static Vector512<nuint> LoadVector512(nuint* address);

    public static Vector512<byte> LoadAlignedVector512(byte* address);
    public static Vector512<double> LoadAlignedVector512(double* address);
    public static Vector512<short> LoadAlignedVector512(short* address);
    public static Vector512<int> LoadAlignedVector512(int* address);
    public static Vector512<long> LoadAlignedVector512(long* address);
    public static Vector512<nint> LoadAlignedVector512(nint* address);
    public static Vector512<sbyte> LoadAlignedVector512(sbyte* address);
    public static Vector512<float> LoadAlignedVector512(float* address);
    public static Vector512<ushort> LoadAlignedVector512(ushort* address);
    public static Vector512<uint> LoadAlignedVector512(uint* address);
    public static Vector512<ulong> LoadAlignedVector512(ulong* address);
    public static Vector512<nuint> LoadAlignedVector512(nuint* address);

    public static Vector512<byte> LoadAlignedVector512NonTemporal(byte* address);
    public static Vector512<short> LoadAlignedVector512NonTemporal(short* address);
    public static Vector512<int> LoadAlignedVector512NonTemporal(int* address);
    public static Vector512<long> LoadAlignedVector512NonTemporal(long* address);
    public static Vector512<nint> LoadAlignedVector512NonTemporal(nint* address);
    public static Vector512<sbyte> LoadAlignedVector512NonTemporal(sbyte* address);
    public static Vector512<ushort> LoadAlignedVector512NonTemporal(ushort* address);
    public static Vector512<uint> LoadAlignedVector512NonTemporal(uint* address);
    public static Vector512<ulong> LoadAlignedVector512NonTemporal(ulong* address);
    public static Vector512<nuint> LoadAlignedVector512NonTemporal(nuint* address);

    public static Vector512<double> Max(Vector512<double> left, Vector512<double> right);
    public static Vector512<int> Max(Vector512<int> left, Vector512<int> right);
    public static Vector512<long> Max(Vector512<long> left, Vector512<long> right);
    public static Vector512<float> Max(Vector512<float> left, Vector512<float> right);
    public static Vector512<uint> Max(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<ulong> Max(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<double> Min(Vector512<double> left, Vector512<double> right);
    public static Vector512<int> Min(Vector512<int> left, Vector512<int> right);
    public static Vector512<long> Min(Vector512<long> left, Vector512<long> right);
    public static Vector512<float> Min(Vector512<float> left, Vector512<float> right);
    public static Vector512<uint> Min(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<ulong> Min(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<double> MoveAndDuplicate(Vector512<double> value);

    public static Vector512<double> Multiply(Vector512<double> left, Vector512<double> right);
    public static Vector512<double> Multiply(Vector512<double> left, Vector512<double> right, FloatRoundingMode mode);

    public static Vector512<int> Multiply(Vector512<int> left, Vector512<int> right);
    public static Vector512<uint> Multiply(Vector512<uint> left, Vector512<uint> right);

    public static Vector512<float> Multiply(Vector512<float> left, Vector512<float> right, FloatRoundingMode mode);
    public static Vector512<float> Multiply(Vector512<float> left, Vector512<float> right);

    public static Vector512<int> MultiplyLow(Vector512<int> left, Vector512<int> right);
    // uint?

    public static Vector128<double> MultiplyScalar(Vector128<double> left, Vector128<double> right, FloatRoundingMode mode);
    public static Vector128<float> MultiplyScalar(Vector128<float> left, Vector128<float> right, FloatRoundingMode mode);

    public static Vector512<int> Or(Vector512<int> left, Vector512<int> right);
    public static Vector512<uint> Or(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<long> Or(Vector512<long> left, Vector512<long> right);
    public static Vector512<ulong> Or(Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<int> ShiftLeftLogical(Vector512<int> value, byte count);
    public static Vector512<int> ShiftLeftLogical(Vector512<int> value, Vector128<int> count);
    // uint?
    public static Vector512<long> ShiftLeftLogical(Vector512<long> value, byte count);
    public static Vector512<long> ShiftLeftLogical(Vector512<long> value, Vector128<long> count);
    // ulong?

    public static Vector512<int> ShiftRightArithmetic(Vector512<int> value, byte count);
    public static Vector512<int> ShiftRightArithmetic(Vector512<int> value, Vector128<int> count);
    public static Vector512<long> ShiftRightArithmetic(Vector512<long> value, byte count);
    public static Vector512<long> ShiftRightArithmetic(Vector512<long> value, Vector128<long> count);
    // unsigned types?

    public static Vector512<int> ShiftRightLogical(Vector512<int> value, byte count);
    public static Vector512<int> ShiftRightLogical(Vector512<int> value, Vector128<int> count);
    public static Vector512<long> ShiftRightLogical(Vector512<long> value, byte count);
    public static Vector512<long> ShiftRightLogical(Vector512<long> value, Vector128<long> count);
    // unsigned types?

    public static Vector512<double> Shuffle(Vector512<double> left, Vector512<double> right, byte control);
    public static Vector512<float> Shuffle(Vector512<float> left, Vector512<float> right, byte control);

    public static Vector512<int> Shuffle(Vector512<int> value, byte control);
    // uint?

    public static Vector512<double> Sqrt(Vector512<double> value, FloatRoundingMode mode);
    public static Vector512<float> Sqrt(Vector512<float> value, FloatRoundingMode mode);

    public static Vector128<double> SqrtScalar(Vector128<double> upper, Vector128<double> value, FloatRoundingMode mode);
    public static Vector128<float> SqrtScalar(Vector128<float> upper, Vector128<float> value, FloatRoundingMode mode);

    public static void Store(byte* address, Vector512<byte> value);
    public static void Store(double* address, Vector512<double> value);
    public static void Store(short* address, Vector512<short> value);
    public static void Store(int* address, Vector512<int> value);
    public static void Store(long* address, Vector512<long> value);
    public static void Store(nint* address, Vector512<nint> value);
    public static void Store(sbyte* address, Vector512<sbyte> value);
    public static void Store(float* address, Vector512<float> value);
    public static void Store(ushort* address, Vector512<ushort> value);
    public static void Store(uint* address, Vector512<uint> value);
    public static void Store(ulong* address, Vector512<ulong> value);
    public static void Store(nuint* address, Vector512<nuint> value);

    public static void StoreAligned(byte* address, Vector512<byte> value);
    public static void StoreAligned(double* address, Vector512<double> value);
    public static void StoreAligned(short* address, Vector512<short> value);
    public static void StoreAligned(int* address, Vector512<int> value);
    public static void StoreAligned(long* address, Vector512<long> value);
    public static void StoreAligned(nint* address, Vector512<nint> value);
    public static void StoreAligned(sbyte* address, Vector512<sbyte> value);
    public static void StoreAligned(float* address, Vector512<float> value);
    public static void StoreAligned(ushort* address, Vector512<ushort> value);
    public static void StoreAligned(uint* address, Vector512<uint> value);
    public static void StoreAligned(ulong* address, Vector512<ulong> value);
    public static void StoreAligned(nuint* address, Vector512<nuint> value);

    public static void StoreAlignedNonTemporal(byte* address, Vector512<byte> value);
    public static void StoreAlignedNonTemporal(double* address, Vector512<double> value);
    public static void StoreAlignedNonTemporal(short* address, Vector512<short> value);
    public static void StoreAlignedNonTemporal(int* address, Vector512<int> value);
    public static void StoreAlignedNonTemporal(long* address, Vector512<long> value);
    public static void StoreAlignedNonTemporal(nint* address, Vector512<nint> value);
    public static void StoreAlignedNonTemporal(sbyte* address, Vector512<sbyte> value);
    public static void StoreAlignedNonTemporal(float* address, Vector512<float> value);
    public static void StoreAlignedNonTemporal(ushort* address, Vector512<ushort> value);
    public static void StoreAlignedNonTemporal(uint* address, Vector512<uint> value);
    public static void StoreAlignedNonTemporal(ulong* address, Vector512<ulong> value);
    public static void StoreAlignedNonTemporal(nuint* address, Vector512<nuint> value);

    public static Vector512<double> Subtract(Vector512<double> left, Vector512<double> right);
    public static Vector512<double> Subtract(Vector512<double> left, Vector512<double> right, FloatRoundingMode mode);

    public static Vector512<int> Subtract(Vector512<int> left, Vector512<int> right);
    public static Vector512<long> Subtract(Vector512<long> left, Vector512<long> right);
    // unsigned types?

    public static Vector512<float> Subtract(Vector512<float> left, Vector512<float> right);
    public static Vector512<float> Subtract(Vector512<float> left, Vector512<float> right, FloatRoundingMode mode);

    public static Vector128<double> SubtractScalar(Vector128<double> left, Vector128<double> right, FloatRoundingMode mode);
    public static Vector128<float> SubtractScalar(Vector128<float> left, Vector128<float> right, FloatRoundingMode mode);

    public static Vector512<double> UnpackHigh(Vector512<double> left, Vector512<double> right);
    public static Vector512<float> UnpackHigh(Vector512<float> left, Vector512<float> right);

    public static Vector512<double> UnpackLow(Vector512<double> left, Vector512<double> right);
    public static Vector512<float> UnpackLow(Vector512<float> left, Vector512<float> right);

    public static Vector512<int> UnpackHigh(Vector512<int> left, Vector512<int> right);
    public static Vector512<int> UnpackLow(Vector512<int> left, Vector512<int> right);
    // unsigned types?

    public static Vector512<long> UnpackHigh(Vector512<long> left, Vector512<long> right);
    public static Vector512<long> UnpackLow(Vector512<long> left, Vector512<long> right);
    // unsigned types?

    public static Vector512<int> Xor(Vector512<int> left, Vector512<int> right);
    public static Vector512<long> Xor(Vector512<long> left, Vector512<long> right);
    // unsigned types?

    // AVX-AVX2

    public static Vector512<double> BroadcastScalarToVector512(Vector128<double> value);
    public static Vector512<int> BroadcastScalarToVector512(Vector128<int> value);
    public static Vector512<float> BroadcastScalarToVector512(Vector128<float> value);
    public static Vector512<long> BroadcastScalarToVector512(Vector128<long> value);
    // unsigned types?

    public static Vector128<float> ExtractVector128(Vector512<float> value, byte index);
    public static Vector128<int> ExtractVector128(Vector512<int> value, byte index);
    // unsigned types?

    public static Vector256<double> ExtractVector256(Vector512<double> value, byte index);
    public static Vector256<long> ExtractVector256(Vector512<long> value, byte index);
    // unsigned types?

    public static Vector512<int> InsertVector128(Vector512<int> value, Vector128<int> data, byte index);
    public static Vector512<float> InsertVector128(Vector512<float> value, Vector128<float> data, byte index);
    // unsigned types?

    public static Vector512<double> InsertVector256(Vector512<double> value, Vector256<double> data, byte index);
    public static Vector512<long> InsertVector256(Vector512<long> value, Vector256<long> data, byte index);
    // unsigned types?

    public static Vector512<double> Permute2x64(Vector512<double> value, byte control);

    public static Vector512<float> Permute(Vector512<float> value, byte control);
    public static Vector512<double> Permute(Vector512<double> value, byte control);

    public static Vector512<long> Permute4x64(Vector512<long> value, byte control);
    // unsigned types?

    public static Vector512<double> PermuteVar(Vector512<double> value, Vector512<long> control);
    public static Vector512<float> PermuteVar(Vector512<float> value, Vector512<int> control);

    public static Vector512<long> PermuteVar4x64(Vector512<long> value, Vector512<long> control);
    // unsigned types?

    public static Vector512<int> PermuteVar8x32(Vector512<int> value, Vector512<int> control);
    // unsigned types?

    public static Vector512<double> PermuteVar8x64(Vector512<double> value, Vector512<long> control);

    public static Vector512<float> PermuteVar16x32(Vector512<float> value, Vector512<int> control);

    public static Vector512<int> ShiftLeftLogicalVariable(Vector512<int> value, Vector512<int> count);
    public static Vector512<long> ShiftLeftLogicalVariable(Vector512<long> value, Vector512<long> count);
    // unsigned types?

    public static Vector512<int> ShiftRightArithmeticVariable(Vector512<int> value, Vector512<int> count);
    public static Vector512<long> ShiftRightArithmeticVariable(Vector512<long> value, Vector512<long> count);
    // unsigned types?

    public static Vector512<int> ShiftRightLogicalVariable(Vector512<int> value, Vector512<int> count);
    public static Vector512<long> ShiftRightLogicalVariable(Vector512<long> value, Vector512<long> count);
    // unsigned types?

    // AVX512

    public static Vector512<int> AlignRight(Vector512<int> left, Vector512<int> right, byte mask);
    public static Vector512<long> AlignRight(Vector512<long> left, Vector512<long> right, byte mask);
    // unsigned types?

    public static Vector512<double> BroadcastToVector512(Vector256<double> value);
    public static Vector512<int> BroadcastToVector512(Vector128<int> value);
    public static Vector512<long> BroadcastToVector512(Vector256<long> value);
    public static Vector512<float> BroadcastToVector512(Vector128<float> value);
    // unsigned types?

    public static uint ConvertToUInt32(Vector128<double> value);
    public static uint ConvertToUInt32(Vector128<double> value, FloatRoundingMode mode);

    public static uint ConvertToUInt32(Vector128<float> value);
    public static uint ConvertToUInt32(Vector128<float> value, FloatRoundingMode mode);

    public static uint ConvertToUInt32WithTruncation(Vector128<double> value);
    public static uint ConvertToUInt32WithTruncation(Vector128<float> value);

    public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector512<long> value);
    public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector512<uint> value);

    public static Vector128<short> ConvertToVector128Int16(Vector512<long> value);
    public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector512<long> value);

    public static Vector128<sbyte> ConvertToVector128SByte(Vector512<int> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector512<long> value);
    public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector512<int> value);
    public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector512<long> value);

    public static Vector128<short> ConvertToVector128UInt16WithSaturation(Vector512<long> value);

    public static Vector256<short> ConvertToVector256Int16(Vector512<int> value);
    public static Vector256<short> ConvertToVector256Int16WithSaturation(Vector512<int> value);

    public static Vector256<int> ConvertToVector256Int32(Vector512<long> value);
    public static Vector256<int> ConvertToVector256Int32WithSaturation(Vector512<long> value);

    public static Vector256<ushort> ConvertToVector256UInt16WithSaturation(Vector512<uint> value);

    public static Vector256<uint> ConvertToVector256UInt32(Vector512<double> value);
    public static Vector256<uint> ConvertToVector256UInt32(Vector512<double> value, FloatRoundingMode mode);
    public static Vector256<uint> ConvertToVector256UInt32WithSaturation(Vector512<long> value);
    public static Vector256<uint> ConvertToVector256UInt32WithTruncation(Vector512<double> value);

    public static Vector512<uint> ConvertToVector512UInt32(Vector512<float> value);
    public static Vector512<uint> ConvertToVector512UInt32(Vector512<float> value, FloatRoundingMode mode);
    public static Vector512<uint> ConvertToVector512UInt32WithTruncation(Vector512<float> value);

    public static Vector512<double> ConvertToVector512Double(Vector256<uint> value);
    public static Vector512<float> ConvertToVector512Single(Vector512<uint> value);

    public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, uint value);
    public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, uint value);

    public static Vector512<double> Fixup(Vector512<double> left, Vector512<double> right, Vector512<long> table);
    public static Vector512<float> Fixup(Vector512<float> left, Vector512<float> right, Vector512<int> table);

    public static Vector128<double> FixupScalar(Vector128<double> left, Vector128<double> right, Vector128<long> table);
    public static Vector128<float> FixupScalar(Vector128<float> left, Vector128<float> right, Vector128<int> table);

    public static Vector512<double> GatherVector512(double* baseAddress, Vector256<int> index, byte scale);
    public static Vector512<double> GatherVector512(double* baseAddress, Vector512<long> index, byte scale);

    public static Vector256<int> GatherVector256(int* baseAddress, Vector512<int> index, byte scale);
    public static Vector512<int> GatherVector512(int* baseAddress, Vector512<int> index, byte scale);

    public static Vector512<long> GatherVector512(void* baseAddress, Vector256<int> index, byte scale);
    public static Vector512<long> GatherVector512(void* baseAddress, Vector512<long> index, byte scale);

    public static Vector256<float> GatherVector256(float* baseAddress, Vector512<float> index, byte scale);
    public static Vector512<float> GatherVector512(void* baseAddress, Vector512<float> index, byte scale);

    public static Vector512<double> GetExponent(Vector512<double> value);
    public static Vector512<float> GetExponent(Vector512<float> value);

    public static Vector128<double> GetExponentScalar(Vector128<double> upper, Vector128<double> value);
    public static Vector128<float> GetExponentScalar(Vector128<float> upper, Vector128<float> value);

    public static Vector512<double> GetMantissa(Vector512<double> value, byte interval, byte signControl);
    public static Vector512<double> GetMantissa(Vector512<double> value, byte interval, byte signControl, FloatRoundingMode mode);

    public static Vector512<float> GetMantissa(Vector512<float> value, byte interval, byte signControl);
    public static Vector512<float> GetMantissa(Vector512<float> value, byte interval, byte signControl, FloatRoundingMode mode);

    public static Vector128<double> GetMantissaScalar(Vector128<double> upper, Vector128<double> value, byte interval, byte signControl);
    public static Vector128<double> GetMantissaScalar(Vector128<double> upper, Vector128<double> value, byte interval, byte signControl, FloatRoundingMode mode);

    public static Vector128<float> GetMantissaScalar(Vector128<float> upper, Vector128<float> value, byte interval, byte signControl);
    public static Vector128<float> GetMantissaScalar(Vector128<float> upper, Vector128<float> value, byte interval, byte signControl, FloatRoundingMode mode);

    public static Vector512<double> PermuteVar8x64(Vector512<double> left, Vector512<double> right, Vector512<double> control);
    public static Vector512<long> PermuteVar8x64(Vector512<long> left, Vector512<long> right, Vector512<long> control);

    public static Vector512<int> PermuteVar16x32(Vector512<int> left, Vector512<int> right, Vector512<int> control);
    public static Vector512<float> PermuteVar16x32(Vector512<float> left, Vector512<float> right, Vector512<float> control);

    public static Vector512<int> RotateLeft(Vector512<int> value, byte count);
    public static Vector512<long> RotateLeft(Vector512<long> value, byte count);

    public static Vector512<int> RotateLeftVariable(Vector512<int> value, Vector512<int> count);
    public static Vector512<long> RotateLeftVariable(Vector512<long> value, Vector512<long> count);

    public static Vector512<int> RotateRight(Vector512<int> value, byte count);
    public static Vector512<long> RotateRight(Vector512<long> value, byte count);

    public static Vector512<int> RotateRightVariable(Vector512<int> value, Vector512<int> count);
    public static Vector512<long> RotateRightVariable(Vector512<long> value, Vector512<long> count);

    public static Vector512<double> Reciprocal14(Vector512<double> value);
    public static Vector512<float> Reciprocal14(Vector512<float> value);

    public static Vector128<double> Reciprocal14Scalar(Vector128<double> upper, Vector128<double> value);
    public static Vector128<float> Reciprocal14Scalar(Vector128<float> upper, Vector128<float> value);

    public static Vector512<double> RoundScale(Vector512<double> value, byte scale);
    public static Vector512<float> RoundScale(Vector512<float> value, byte scale);

    public static Vector128<double> RoundScaleScalar(Vector128<double> upper, Vector128<double> value, byte scale);
    public static Vector128<float> RoundScaleScalar(Vector128<float> upper, Vector128<float> value, byte scale);

    public static Vector512<double> ReciprocalSqrt14(Vector512<double> value);
    public static Vector512<float> ReciprocalSqrt14(Vector512<float> value);

    public static Vector128<double> ReciprocalSqrt14Scalar(Vector128<double> upper, Vector128<double> value);
    public static Vector128<float> ReciprocalSqrt14Scalar(Vector128<float> upper, Vector128<float> value);

    public static Vector512<double> Scale(Vector512<double> left, Vector512<double> right);
    public static Vector512<double> Scale(Vector512<double> left, Vector512<double> right, FloatRoundingMode mode);

    public static Vector512<float> Scale(Vector512<float> left, Vector512<float> right);
    public static Vector512<float> Scale(Vector512<float> left, Vector512<float> right, FloatRoundingMode mode);

    public static void Scatter(double* baseAddress, Vector256<int> index, byte scale, Vector512<double> value);
    public static void Scatter(double* baseAddress, Vector512<long> index, byte scale, Vector512<double> value);

    public static void Scatter(int* baseAddress, Vector512<int> index, byte scale, Vector512<int> value);
    public static void Scatter(int* baseAddress, Vector512<long> index, byte scale, Vector256<int> value);

    public static void Scatter(long* baseAddress, Vector256<int> index, byte scale, Vector512<long> value);
    public static void Scatter(long* baseAddress, Vector512<long> index, byte scale, Vector512<long> value);

    public static void Scatter(float* baseAddress, Vector512<int> index, byte scale, Vector512<float> value);
    public static void Scatter(float* baseAddress, Vector512<long> index, byte scale, Vector256<float> value);

    public static Vector512<double> Shuffle(Vector512<double> left, Vector512<double> right, byte control);
    public static Vector512<int> Shuffle(Vector512<int> left, Vector512<int> right, byte control);
    public static Vector512<long> Shuffle(Vector512<long> left, Vector512<long> right, byte control);
    public static Vector512<float> Shuffle(Vector512<float> left, Vector512<float> right, byte control);
    // unsigned types?

    public static Vector512<int> TernaryLogic(Vector512<int> left, Vector512<int> right, byte control);
    public static Vector512<long> TernaryLogic(Vector512<long> left, Vector512<long> right, byte control);
    // unsigned types?

    public new abstract partial class X64
    {
        public static new bool IsSupported { get; }

        // SSE-SSE4.2

        public static long ConvertToInt64(Vector128<double> value, FloatRoundingMode mode);
        public static long ConvertToInt64(Vector128<float> value, FloatRoundingMode mode);

        public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, long value, FloatRoundingMode mode);
        public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, long value, FloatRoundingMode mode);

        // AVX512

        public static ulong ConvertToUInt64(Vector128<double> value);
        public static ulong ConvertToUInt64(Vector128<double> value, FloatRoundingMode mode);
        public static ulong ConvertToUInt64(Vector128<float> value);
        public static ulong ConvertToUInt64(Vector128<float> value, FloatRoundingMode mode);

        public static ulong ConvertToUInt64WithTruncation(Vector128<double> value);
        public static ulong ConvertToUInt64WithTruncation(Vector128<float> value);

        public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, ulong value);
        public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, ulong value, FloatRoundingMode mode);

        public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, ulong value);
        public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, ulong value, FloatRoundingMode mode);
    }
}
```
