# System.Runtime.Intrinsics.Arm

```C#
namespace System.Runtime.Intrinsics.Arm
{
    // "Current" state of the world: https://apiview.azurewebsites.net/Assemblies/Review/2ddcfab08dfd46b4b1fcbd02594cb648
    //    * Discuss how Scalar for Vector64<T> works
    //    * Remove Compare...Zero: https://github.com/dotnet/corefx/issues/26180
    //    * Discuss Insert/Extract: https://github.com/dotnet/corefx/issues/26183
    //    * Discuss load/store variants: https://github.com/dotnet/corefx/issues/26527
    //    * Discuss crc32 issue: https://github.com/dotnet/corefx/issues/26220
    //    * Crypto: https://github.com/dotnet/corefx/issues/26564
    //    * Bitwise Select: Type of first argument
    // https://github.com/dotnet/corefx/issues/26581
    // https://github.com/dotnet/corefx/issues/42176

    [CLSCompliant(false)]
    public abstract partial class AdvSimd : ArmBase
    {
        internal AdvSimd() { }
        public static new bool IsSupported { get => throw null; }

        // ******************************************************************************************************************************** //

        public static Vector128<ushort> Abs(Vector128<short> value) => throw null;
        public static Vector128<uint> Abs(Vector128<int> value) => throw null;
        public static Vector128<byte> Abs(Vector128<sbyte> value) => throw null;
        public static Vector128<float> Abs(Vector128<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<ushort> Abs(Vector64<short> value) => throw null;
        public static Vector64<uint> Abs(Vector64<int> value) => throw null;
        public static Vector64<byte> Abs(Vector64<sbyte> value) => throw null;
        public static Vector64<float> Abs(Vector64<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> AbsScalar(Vector64<double> value) => throw null;
        public static Vector64<float> AbsScalar(Vector64<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Add(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> Add(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Add(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> Add(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> Add(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Add(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Add(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Add(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> Add(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Add(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<short> Add(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Add(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<sbyte> Add(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Add(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Add(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Add(Vector64<uint> left, Vector64<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> AddScalar(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<long> AddScalar(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<float> AddScalar(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ulong> AddScalar(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> And(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<double> And(Vector128<double> left, Vector128<double> right) => throw null;
        public static Vector128<short> And(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> And(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> And(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> And(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> And(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> And(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> And(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> And(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> And(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<double> And(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<short> And(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> And(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<long> And(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<sbyte> And(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> And(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> And(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> And(Vector64<uint> left, Vector64<uint> right) => throw null;
        public static Vector64<ulong> And(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> AndNot(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<double> AndNot(Vector128<double> left, Vector128<double> right) => throw null;
        public static Vector128<short> AndNot(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> AndNot(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> AndNot(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> AndNot(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> AndNot(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> AndNot(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> AndNot(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> AndNot(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> AndNot(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<double> AndNot(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<short> AndNot(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> AndNot(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<long> AndNot(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<sbyte> AndNot(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> AndNot(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> AndNot(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> AndNot(Vector64<uint> left, Vector64<uint> right) => throw null;
        public static Vector64<ulong> AndNot(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> BitwiseSelect(Vector128<byte> select, Vector128<byte> left, Vector128<byte> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<double> BitwiseSelect(Vector128<double> select, Vector128<double> left, Vector128<double> right) => throw null;
        public static Vector128<short> BitwiseSelect(Vector128<short> select, Vector128<short> left, Vector128<short> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<int> BitwiseSelect(Vector128<int> select, Vector128<int> left, Vector128<int> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<long> BitwiseSelect(Vector128<long> select, Vector128<long> left, Vector128<long> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<sbyte> BitwiseSelect(Vector128<sbyte> select, Vector128<sbyte> left, Vector128<sbyte> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<float> BitwiseSelect(Vector128<float> select, Vector128<float> left, Vector128<float> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<ushort> BitwiseSelect(Vector128<ushort> select, Vector128<ushort> left, Vector128<ushort> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<uint> BitwiseSelect(Vector128<uint> select, Vector128<uint> left, Vector128<uint> right) { throw new PlatformNotSupportedException(); }
        public static Vector128<ulong> BitwiseSelect(Vector128<ulong> select, Vector128<ulong> left, Vector128<ulong> right) { throw new PlatformNotSupportedException(); }

        // ******************************************************************************************************************************** //

        public static Vector64<byte> BitwiseSelect(Vector64<byte> select, Vector64<byte> left, Vector64<byte> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<double> BitwiseSelect(Vector64<double> select, Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<short> BitwiseSelect(Vector64<short> select, Vector64<short> left, Vector64<short> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<int> BitwiseSelect(Vector64<int> select, Vector64<int> left, Vector64<int> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<long> BitwiseSelect(Vector64<long> select, Vector64<long> left, Vector64<long> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<sbyte> BitwiseSelect(Vector64<sbyte> select, Vector64<sbyte> left, Vector64<sbyte> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<float> BitwiseSelect(Vector64<float> select, Vector64<float> left, Vector64<float> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<ushort> BitwiseSelect(Vector64<ushort> select, Vector64<ushort> left, Vector64<ushort> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<uint> BitwiseSelect(Vector64<uint> select, Vector64<uint> left, Vector64<uint> right) { throw new PlatformNotSupportedException(); }
        public static Vector64<ulong> BitwiseSelect(Vector64<ulong> select, Vector64<ulong> left, Vector64<ulong> right) { throw new PlatformNotSupportedException(); }

        // ******************************************************************************************************************************** //

        public static Vector128<byte> CompareEqual(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> CompareEqual(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> CompareEqual(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> CompareEqual(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> CompareEqual(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> CompareEqual(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> CompareEqual(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> CompareEqual(Vector64<byte> left, Vector128<byte> right) => throw null;
        public static Vector64<short> CompareEqual(Vector64<short> left, Vector128<short> right) => throw null;
        public static Vector64<int> CompareEqual(Vector64<int> left, Vector128<int> right) => throw null;
        public static Vector64<sbyte> CompareEqual(Vector64<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector64<float> CompareEqual(Vector64<float> left, Vector128<float> right) => throw null;
        public static Vector64<ushort> CompareEqual(Vector64<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector64<uint> CompareEqual(Vector64<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> CompareGreaterThanOrEqual(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> CompareGreaterThanOrEqual(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> CompareGreaterThanOrEqual(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> CompareGreaterThanOrEqual(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> CompareGreaterThanOrEqual(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> CompareGreaterThanOrEqual(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> CompareGreaterThanOrEqual(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> CompareGreaterThanOrEqual(Vector64<byte> left, Vector128<byte> right) => throw null;
        public static Vector64<short> CompareGreaterThanOrEqual(Vector64<short> left, Vector128<short> right) => throw null;
        public static Vector64<int> CompareGreaterThanOrEqual(Vector64<int> left, Vector128<int> right) => throw null;
        public static Vector64<sbyte> CompareGreaterThanOrEqual(Vector64<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector64<float> CompareGreaterThanOrEqual(Vector64<float> left, Vector128<float> right) => throw null;
        public static Vector64<ushort> CompareGreaterThanOrEqual(Vector64<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector64<uint> CompareGreaterThanOrEqual(Vector64<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> CompareGreaterThan(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> CompareGreaterThan(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> CompareGreaterThan(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> CompareGreaterThan(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> CompareGreaterThan(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> CompareGreaterThan(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> CompareGreaterThan(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> CompareGreaterThan(Vector64<byte> left, Vector128<byte> right) => throw null;
        public static Vector64<short> CompareGreaterThan(Vector64<short> left, Vector128<short> right) => throw null;
        public static Vector64<int> CompareGreaterThan(Vector64<int> left, Vector128<int> right) => throw null;
        public static Vector64<sbyte> CompareGreaterThan(Vector64<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector64<float> CompareGreaterThan(Vector64<float> left, Vector128<float> right) => throw null;
        public static Vector64<ushort> CompareGreaterThan(Vector64<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector64<uint> CompareGreaterThan(Vector64<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> CompareLessThanOrEqual(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> CompareLessThanOrEqual(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> CompareLessThanOrEqual(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> CompareLessThanOrEqual(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> CompareLessThanOrEqual(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> CompareLessThanOrEqual(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> CompareLessThanOrEqual(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> CompareLessThanOrEqual(Vector64<byte> left, Vector128<byte> right) => throw null;
        public static Vector64<short> CompareLessThanOrEqual(Vector64<short> left, Vector128<short> right) => throw null;
        public static Vector64<int> CompareLessThanOrEqual(Vector64<int> left, Vector128<int> right) => throw null;
        public static Vector64<sbyte> CompareLessThanOrEqual(Vector64<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector64<float> CompareLessThanOrEqual(Vector64<float> left, Vector128<float> right) => throw null;
        public static Vector64<ushort> CompareLessThanOrEqual(Vector64<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector64<uint> CompareLessThanOrEqual(Vector64<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> CompareLessThan(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> CompareLessThan(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> CompareLessThan(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> CompareLessThan(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> CompareLessThan(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> CompareLessThan(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> CompareLessThan(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> CompareLessThan(Vector64<byte> left, Vector128<byte> right) => throw null;
        public static Vector64<short> CompareLessThan(Vector64<short> left, Vector128<short> right) => throw null;
        public static Vector64<int> CompareLessThan(Vector64<int> left, Vector128<int> right) => throw null;
        public static Vector64<sbyte> CompareLessThan(Vector64<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector64<float> CompareLessThan(Vector64<float> left, Vector128<float> right) => throw null;
        public static Vector64<ushort> CompareLessThan(Vector64<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector64<uint> CompareLessThan(Vector64<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> DivideScalar(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<float> DivideScalar(Vector64<float> left, Vector64<float> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> LeadingSignCount(Vector128<byte> value) => throw null;
        public static Vector128<short> LeadingSignCount(Vector128<short> value) => throw null;
        public static Vector128<int> LeadingSignCount(Vector128<int> value) => throw null;
        public static Vector128<sbyte> LeadingSignCount(Vector128<sbyte> value) => throw null;
        public static Vector128<ushort> LeadingSignCount(Vector128<ushort> value) => throw null;
        public static Vector128<uint> LeadingSignCount(Vector128<uint> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> LeadingSignCount(Vector64<byte> value) => throw null;
        public static Vector64<short> LeadingSignCount(Vector64<short> value) => throw null;
        public static Vector64<int> LeadingSignCount(Vector64<int> value) => throw null;
        public static Vector64<sbyte> LeadingSignCount(Vector64<sbyte> value) => throw null;
        public static Vector64<ushort> LeadingSignCount(Vector64<ushort> value) => throw null;
        public static Vector64<uint> LeadingSignCount(Vector64<uint> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> LeadingZeroCount(Vector128<byte> value) => throw null;
        public static Vector128<short> LeadingZeroCount(Vector128<short> value) => throw null;
        public static Vector128<int> LeadingZeroCount(Vector128<int> value) => throw null;
        public static Vector128<sbyte> LeadingZeroCount(Vector128<sbyte> value) => throw null;
        public static Vector128<ushort> LeadingZeroCount(Vector128<ushort> value) => throw null;
        public static Vector128<uint> LeadingZeroCount(Vector128<uint> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> LeadingZeroCount(Vector64<byte> value) => throw null;
        public static Vector64<short> LeadingZeroCount(Vector64<short> value) => throw null;
        public static Vector64<int> LeadingZeroCount(Vector64<int> value) => throw null;
        public static Vector64<sbyte> LeadingZeroCount(Vector64<sbyte> value) => throw null;
        public static Vector64<ushort> LeadingZeroCount(Vector64<ushort> value) => throw null;
        public static Vector64<uint> LeadingZeroCount(Vector64<uint> value) => throw null;

        // ******************************************************************************************************************************** //

        public unsafe static Vector128<byte> LoadVector128(byte* address) => throw null;
        public unsafe static Vector128<double> LoadVector128(double* address) => throw null;
        public unsafe static Vector128<short> LoadVector128(short* address) => throw null;
        public unsafe static Vector128<int> LoadVector128(int* address) => throw null;
        public unsafe static Vector128<long> LoadVector128(long* address) => throw null;
        public unsafe static Vector128<sbyte> LoadVector128(sbyte* address) => throw null;
        public unsafe static Vector128<float> LoadVector128(float* address) => throw null;
        public unsafe static Vector128<ushort> LoadVector128(ushort* address) => throw null;
        public unsafe static Vector128<uint> LoadVector128(uint* address) => throw null;
        public unsafe static Vector128<ulong> LoadVector128(ulong* address) => throw null;

        // ******************************************************************************************************************************** //

        public unsafe static Vector64<byte> LoadVector64(byte* address) => throw null;
        public unsafe static Vector64<double> LoadVector64(double* address) => throw null;
        public unsafe static Vector64<short> LoadVector64(short* address) => throw null;
        public unsafe static Vector64<int> LoadVector64(int* address) => throw null;
        public unsafe static Vector64<long> LoadVector64(long* address) => throw null;
        public unsafe static Vector64<sbyte> LoadVector64(sbyte* address) => throw null;
        public unsafe static Vector64<float> LoadVector64(float* address) => throw null;
        public unsafe static Vector64<ushort> LoadVector64(ushort* address) => throw null;
        public unsafe static Vector64<uint> LoadVector64(uint* address) => throw null;
        public unsafe static Vector64<ulong> LoadVector64(ulong* address) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Max(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<short> Max(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Max(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<sbyte> Max(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Max(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Max(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Max(Vector64<uint> left, Vector64<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Max(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> Max(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Max(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> Max(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Max(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Max(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Max(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Min(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<short> Min(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Min(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<sbyte> Min(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Min(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Min(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Min(Vector64<uint> left, Vector64<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Min(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> Min(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Min(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> Min(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Min(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Min(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Min(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Multiply(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> Multiply(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Multiply(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<sbyte> Multiply(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Multiply(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Multiply(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Multiply(Vector128<uint> left, Vector128<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Multiply(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<short> Multiply(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Multiply(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<sbyte> Multiply(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Multiply(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Multiply(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Multiply(Vector64<uint> left, Vector64<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> MultiplyScalar(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<float> MultiplyScalar(Vector64<float> left, Vector64<float> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<short> Negate(Vector64<short> value) => throw null;
        public static Vector64<int> Negate(Vector64<int> value) => throw null;
        public static Vector64<sbyte> Negate(Vector64<sbyte> value) => throw null;
        public static Vector64<float> Negate(Vector64<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<short> Negate(Vector128<short> value) => throw null;
        public static Vector128<int> Negate(Vector128<int> value) => throw null;
        public static Vector128<sbyte> Negate(Vector128<sbyte> value) => throw null;
        public static Vector128<float> Negate(Vector128<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> NegateScalar(Vector64<double> value) => throw null;
        public static Vector64<float> NegateScalar(Vector64<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Not(Vector128<byte> value) => throw null;
        public static Vector128<double> Not(Vector128<double> value) => throw null;
        public static Vector128<short> Not(Vector128<short> value) => throw null;
        public static Vector128<int> Not(Vector128<int> value) => throw null;
        public static Vector128<long> Not(Vector128<long> value) => throw null;
        public static Vector128<sbyte> Not(Vector128<sbyte> value) => throw null;
        public static Vector128<float> Not(Vector128<float> value) => throw null;
        public static Vector128<ushort> Not(Vector128<ushort> value) => throw null;
        public static Vector128<uint> Not(Vector128<uint> value) => throw null;
        public static Vector128<ulong> Not(Vector128<ulong> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Not(Vector64<byte> value) => throw null;
        public static Vector64<double> Not(Vector64<double> value) => throw null;
        public static Vector64<short> Not(Vector64<short> value) => throw null;
        public static Vector64<int> Not(Vector64<int> value) => throw null;
        public static Vector64<long> Not(Vector64<long> value) => throw null;
        public static Vector64<sbyte> Not(Vector64<sbyte> value) => throw null;
        public static Vector64<float> Not(Vector64<float> value) => throw null;
        public static Vector64<ushort> Not(Vector64<ushort> value) => throw null;
        public static Vector64<uint> Not(Vector64<uint> value) => throw null;
        public static Vector64<ulong> Not(Vector64<ulong> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Or(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<double> Or(Vector128<double> left, Vector128<double> right) => throw null;
        public static Vector128<short> Or(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Or(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> Or(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> Or(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Or(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Or(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Or(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> Or(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Or(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<double> Or(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<short> Or(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Or(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<long> Or(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<sbyte> Or(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Or(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Or(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Or(Vector64<uint> left, Vector64<uint> right) => throw null;
        public static Vector64<ulong> Or(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> OrNot(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<double> OrNot(Vector128<double> left, Vector128<double> right) => throw null;
        public static Vector128<short> OrNot(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> OrNot(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> OrNot(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> OrNot(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> OrNot(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> OrNot(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> OrNot(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> OrNot(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> OrNot(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<double> OrNot(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<short> OrNot(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> OrNot(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<long> OrNot(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<sbyte> OrNot(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> OrNot(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> OrNot(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> OrNot(Vector64<uint> left, Vector64<uint> right) => throw null;
        public static Vector64<ulong> OrNot(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> PopCount(Vector128<byte> value) => throw null;
        public static Vector128<sbyte> PopCount(Vector128<sbyte> value) => throw null;
        public static Vector64<byte> PopCount(Vector64<byte> value) => throw null;
        public static Vector64<sbyte> PopCount(Vector64<sbyte> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> SqrtScalar(Vector64<double> value) => throw null;
        public static Vector64<float> SqrtScalar(Vector64<float> value) => throw null;

        // ******************************************************************************************************************************** //

        public unsafe static void Store(byte* address, Vector128<byte> value) => throw null;
        public unsafe static void Store(double* address, Vector128<double> value) => throw null;
        public unsafe static void Store(short* address, Vector128<short> value) => throw null;
        public unsafe static void Store(int* address, Vector128<int> value) => throw null;
        public unsafe static void Store(long* address, Vector128<long> value) => throw null;
        public unsafe static void Store(sbyte* address, Vector128<sbyte> value) => throw null;
        public unsafe static void Store(float* address, Vector128<float> value) => throw null;
        public unsafe static void Store(ushort* address, Vector128<ushort> value) => throw null;
        public unsafe static void Store(uint* address, Vector128<uint> value) => throw null;
        public unsafe static void Store(ulong* address, Vector128<ulong> value) => throw null;

        // ******************************************************************************************************************************** //

        public unsafe static void Store(byte* address, Vector64<byte> value) => throw null;
        public unsafe static void Store(double* address, Vector64<double> value) => throw null;
        public unsafe static void Store(short* address, Vector64<short> value) => throw null;
        public unsafe static void Store(int* address, Vector64<int> value) => throw null;
        public unsafe static void Store(long* address, Vector64<long> value) => throw null;
        public unsafe static void Store(sbyte* address, Vector64<sbyte> value) => throw null;
        public unsafe static void Store(float* address, Vector64<float> value) => throw null;
        public unsafe static void Store(ushort* address, Vector64<ushort> value) => throw null;
        public unsafe static void Store(uint* address, Vector64<uint> value) => throw null;
        public unsafe static void Store(ulong* address, Vector64<ulong> value) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Subtract(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<short> Subtract(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Subtract(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> Subtract(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> Subtract(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Subtract(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Subtract(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Subtract(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> Subtract(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Subtract(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<short> Subtract(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Subtract(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<sbyte> Subtract(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Subtract(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Subtract(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Subtract(Vector64<uint> left, Vector64<uint> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<double> SubtractScalar(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<long> SubtractScalar(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<float> SubtractScalar(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ulong> SubtractScalar(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Xor(Vector128<byte> left, Vector128<byte> right) => throw null;
        public static Vector128<double> Xor(Vector128<double> left, Vector128<double> right) => throw null;
        public static Vector128<short> Xor(Vector128<short> left, Vector128<short> right) => throw null;
        public static Vector128<int> Xor(Vector128<int> left, Vector128<int> right) => throw null;
        public static Vector128<long> Xor(Vector128<long> left, Vector128<long> right) => throw null;
        public static Vector128<sbyte> Xor(Vector128<sbyte> left, Vector128<sbyte> right) => throw null;
        public static Vector128<float> Xor(Vector128<float> left, Vector128<float> right) => throw null;
        public static Vector128<ushort> Xor(Vector128<ushort> left, Vector128<ushort> right) => throw null;
        public static Vector128<uint> Xor(Vector128<uint> left, Vector128<uint> right) => throw null;
        public static Vector128<ulong> Xor(Vector128<ulong> left, Vector128<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public static Vector64<byte> Xor(Vector64<byte> left, Vector64<byte> right) => throw null;
        public static Vector64<double> Xor(Vector64<double> left, Vector64<double> right) => throw null;
        public static Vector64<short> Xor(Vector64<short> left, Vector64<short> right) => throw null;
        public static Vector64<int> Xor(Vector64<int> left, Vector64<int> right) => throw null;
        public static Vector64<long> Xor(Vector64<long> left, Vector64<long> right) => throw null;
        public static Vector64<sbyte> Xor(Vector64<sbyte> left, Vector64<sbyte> right) => throw null;
        public static Vector64<float> Xor(Vector64<float> left, Vector64<float> right) => throw null;
        public static Vector64<ushort> Xor(Vector64<ushort> left, Vector64<ushort> right) => throw null;
        public static Vector64<uint> Xor(Vector64<uint> left, Vector64<uint> right) => throw null;
        public static Vector64<ulong> Xor(Vector64<ulong> left, Vector64<ulong> right) => throw null;

        // ******************************************************************************************************************************** //

        public new abstract partial class Arm64 : ArmBase.Arm64
        {
            internal Arm64() { }
            public static new bool IsSupported { get => throw null; }

            // ******************************************************************************************************************************** //

            public static Vector128<double> Abs(Vector128<double> value) => throw null;
            public static Vector128<ulong> Abs(Vector128<long> value) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<ulong> AbsScalar(Vector64<long> value) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Add(Vector128<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> CompareEqual(Vector128<double> left, Vector128<double> right) => throw null;
            public static Vector128<long> CompareEqual(Vector128<long> left, Vector128<long> right) => throw null;
            public static Vector128<ulong> CompareEqual(Vector128<ulong> left, Vector128<ulong> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> CompareEqual(Vector64<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> CompareGreaterThanOrEqual(Vector128<double> left, Vector128<double> right) => throw null;
            public static Vector128<long> CompareGreaterThanOrEqual(Vector128<long> left, Vector128<long> right) => throw null;
            public static Vector128<ulong> CompareGreaterThanOrEqual(Vector128<ulong> left, Vector128<ulong> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> CompareGreaterThanOrEqual(Vector64<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> CompareGreaterThan(Vector128<double> left, Vector128<double> right) => throw null;
            public static Vector128<long> CompareGreaterThan(Vector128<long> left, Vector128<long> right) => throw null;
            public static Vector128<ulong> CompareGreaterThan(Vector128<ulong> left, Vector128<ulong> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> CompareGreaterThan(Vector64<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> CompareLessThanOrEqual(Vector128<double> left, Vector128<double> right) => throw null;
            public static Vector128<long> CompareLessThanOrEqual(Vector128<long> left, Vector128<long> right) => throw null;
            public static Vector128<ulong> CompareLessThanOrEqual(Vector128<ulong> left, Vector128<ulong> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> CompareLessThanOrEqual(Vector64<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> CompareLessThan(Vector128<double> left, Vector128<double> right) => throw null;
            public static Vector128<long> CompareLessThan(Vector128<long> left, Vector128<long> right) => throw null;
            public static Vector128<ulong> CompareLessThan(Vector128<ulong> left, Vector128<ulong> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> CompareLessThan(Vector64<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Divide(Vector128<double> left, Vector128<double> right) => throw null;
            public static Vector128<float> Divide(Vector128<float> left, Vector128<float> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<float> Divide(Vector64<float> left, Vector64<float> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Max(Vector128<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> MaxScalar(Vector64<double> left, Vector64<double> right) => throw null;
            public static Vector64<float> MaxScalar(Vector64<float> left, Vector64<float> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Min(Vector128<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> MinScalar(Vector64<double> left, Vector64<double> right) => throw null;
            public static Vector64<float> MinScalar(Vector64<float> left, Vector64<float> right) => throw null; 

            // ******************************************************************************************************************************** //

            public static Vector128<double> Multiply(Vector128<double> left, Vector128<double> right) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Negate(Vector128<double> value) => throw null;
            public static Vector128<long> Negate(Vector128<long> value) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<long> NegateScalar(Vector64<long> value) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector64<double> Sqrt(Vector64<double> value) => throw null;
            public static Vector64<float> Sqrt(Vector64<float> value) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Sqrt(Vector128<double> value) => throw null;
            public static Vector128<float> Sqrt(Vector128<float> value) => throw null;

            // ******************************************************************************************************************************** //

            public static Vector128<double> Subtract(Vector128<double> left, Vector128<double> right) => throw null;
        }
    }

    [CLSCompliant(false)]
    public abstract partial class Aes : ArmBase
    {
        internal Aes() { }
        public static new bool IsSupported { get => throw null; }

        // ******************************************************************************************************************************** //

        public static Vector128<byte> Decrypt(Vector128<byte> value, Vector128<byte> roundKey) => throw null;
        public static Vector128<byte> Encrypt(Vector128<byte> value, Vector128<byte> roundKey) => throw null;
        public static Vector128<byte> InverseMixColumns(Vector128<byte> value) => throw null;
        public static Vector128<byte> MixColumns(Vector128<byte> value) => throw null;
    }

    [CLSCompliant(false)]
    public abstract partial class ArmBase
    {
        internal ArmBase() { }
        public static bool IsSupported { get => throw null; }

        // ******************************************************************************************************************************** //

        public static int LeadingZeroCount(int value) => throw null;
        public static int LeadingZeroCount(uint value) => throw null;

        // ******************************************************************************************************************************** //

        public abstract partial class Arm64
        {
            internal Arm64() { }
            public static bool IsSupported { get => throw null; }

            // ******************************************************************************************************************************** //

            public static int LeadingSignCount(int value) => throw null;
            public static int LeadingSignCount(long value) => throw null;
            public static int LeadingSignCount(uint value) => throw null;
            public static int LeadingSignCount(ulong value) => throw null;

            // ******************************************************************************************************************************** //

            public static int LeadingZeroCount(long value) => throw null;
            public static int LeadingZeroCount(ulong value) => throw null;
        }
    }

    [CLSCompliant(false)]
    public abstract partial class Crc32
    {
        internal Crc32() { }
        public static new bool IsSupported { get => throw null; }

        // ******************************************************************************************************************************** //

        public static uint _Crc32(uint crc, byte data) => throw null;
        public static uint _Crc32(uint crc, ushort data) => throw null;
        public static uint _Crc32(uint crc, uint data) => throw null;

        // ******************************************************************************************************************************** //

        public static uint Crc32C(uint crc, byte data) => throw null;
        public static uint Crc32C(uint crc, ushort data) => throw null;
        public static uint Crc32C(uint crc, uint data) => throw null;

        // ******************************************************************************************************************************** //

        public abstract partial class Arm64
        {
            internal Arm64() { }
            public static bool IsSupported { get => throw null; }

            // ******************************************************************************************************************************** //

            public static uint Crc32(uint crc, ulong data) => throw null;

            // ******************************************************************************************************************************** //

            public static uint Crc32C(uint crc, ulong data) => throw null;
        }
    }

    [CLSCompliant(false)]
    public abstract partial class Sha1 : ArmBase
    {
        internal Sha1() { }
        public static new bool IsSupported { get => throw null; }

        // ******************************************************************************************************************************** //

        public static uint FixedRotate(uint hash_e) => throw null;
        public static Vector128<uint> HashUpdateChoose(Vector128<uint> hash_abcd, uint hash_e, Vector128<uint> wk) => throw null;
        public static Vector128<uint> HashUpdateMajority(Vector128<uint> hash_abcd, uint hash_e, Vector128<uint> wk) => throw null;
        public static Vector128<uint> HashUpdateParity(Vector128<uint> hash_abcd, uint hash_e, Vector128<uint> wk) => throw null;
        public static Vector128<uint> ScheduleUpdate0(Vector128<uint> w0_3, Vector128<uint> w4_7, Vector128<uint> w8_11) => throw null;
        public static Vector128<uint> ScheduleUpdate1(Vector128<uint> tw0_3, Vector128<uint> w12_15) => throw null;
    }

    [CLSCompliant(false)]
    public abstract partial class Sha256 : ArmBase
    {
        internal Sha256() { }
        public static new bool IsSupported { get => throw null; }

        // ******************************************************************************************************************************** //

        public static Vector128<uint> HashUpdate1(Vector128<uint> hash_abcd, Vector128<uint> hash_efgh, Vector128<uint> wk) => throw null;
        public static Vector128<uint> HashUpdate2(Vector128<uint> hash_efgh, Vector128<uint> hash_abcd, Vector128<uint> wk) => throw null;
        public static Vector128<uint> ScheduleUpdate0(Vector128<uint> w0_3, Vector128<uint> w4_7) => throw null;
        public static Vector128<uint> ScheduleUpdate1(Vector128<uint> w0_3, Vector128<uint> w8_11, Vector128<uint> w12_15) => throw null;
    }
}
```