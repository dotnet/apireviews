# API Review 01/04/2022

## Add public Architecture enum value for LoongArch64

**Approved** | [#runtime/63042](https://github.com/dotnet/runtime/issues/63042#issuecomment-1005058539) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=0h0m0s)

* Looks good as proposed
* We talked about adding an `Unknown` member to `Architecture` but decided against it; the implementation can still return a placeholder (e.g. `-1`).

```C#
namespace System.Runtime.InteropServices
{
    public enum Architecture
    {
        // Existing:
        // X86
        // X64
        // Arm
        // Arm64
        // Wasm
        // S390x
        LoongArch64,
    }
}
namespace System.Reflection.PortableExecutable
{
    public enum Machine : ushort
    {
        // Existing:
        // Alpha
        // Alpha64
        // AM33
        // Amd64
        // Arm
        // Arm64
        // ArmThumb2
        // Ebc
        // I386
        // IA64
        // M32R
        // MIPS16
        // MipsFpu
        // MipsFpu16
        // PowerPC
        // PowerPCFP
        // SH3
        // SH3Dsp
        // SH3E
        // SH4
        // SH5
        // Thumb
        // Tricore
        // Unknown
        // WceMipsV2
        LoongArch32 = 0x6232,
        LoongArch64 = 0x6264
    }
}
```
## Add architecture ARMv6 to Architecture enum

**Approved** | [#runtime/63310](https://github.com/dotnet/runtime/issues/63310#issuecomment-1005060501) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=0h14m55s)

* Looks good as proposed

```C#
namespace System.Runtime.InteropServices
{
    public enum Architecture
    {
        // Existing:
        // X86
        // X64
        // Arm
        // Arm64
        // Wasm
        // S390x
        Armv6,
    }
}
```
## Additional cross-platform hardware intrinsic APIs for loading/storing, reordering, and extracting a per-element "mask"

**Approved** | [#runtime/63331](https://github.com/dotnet/runtime/issues/63331#issuecomment-1005073176) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=0h17m28s)

* Looks good as proposed but let's make `ExtractMostSignificantBits` and `StoreXxx` extension methods
* We decided to keep the `Shuffle` APIs as non-extensions because the two-argument shuffle would be potentially confusing and we didn't want a mix of extensions/non-extensions.

```C#
namespace System.Runtime.Intrinsics
{
    public static partial class Vector64
    {
        public static uint ExtractMostSignificantBits<T>(this Vector64<T> vector);

        public static Vector64<T> Load<T>(T* source);
        public static Vector64<T> LoadAligned<T>(T* source);
        public static Vector64<T> LoadAlignedNonTemporal<T>(T* source);
        public static Vector64<T> LoadUnsafe<T>(ref T source);
        public static Vector64<T> LoadUnsafe<T>(ref T source, nuint index);

        public static void Store<T>(this Vector64<T> source, T* destination);
        public static void StoreAligned<T>(this Vector64<T> source, T* destination);
        public static void StoreAlignedNonTemporal<T>(this Vector64<T> source, T* destination);
        public static void StoreUnsafe<T>(this Vector64<T> source, ref T destination);
        public static void StoreUnsafe<T>(this Vector64<T> source, ref T destination, nuint index);

        public static Vector64<byte>   Shuffle(Vector64<byte>   vector, Vector64<byte>   indices);
        public static Vector64<sbyte>  Shuffle(Vector64<sbyte>  vector, Vector64<sbyte>  indices);

        public static Vector64<short>  Shuffle(Vector64<short>  vector, Vector64<short>  indices);
        public static Vector64<ushort> Shuffle(Vector64<ushort> vector, Vector64<ushort> indices);

        public static Vector64<int>    Shuffle(Vector64<int>    vector, Vector64<int>    indices);
        public static Vector64<uint>   Shuffle(Vector64<uint>   vector, Vector64<uint>   indices);
        public static Vector64<float>  Shuffle(Vector64<float>  vector, Vector64<int>    indices);

        public static Vector64<byte>   Shuffle(Vector64<byte>   lower, Vector64<byte>   upper, Vector64<byte>   indices);
        public static Vector64<sbyte>  Shuffle(Vector64<sbyte>  lower, Vector64<sbyte>  upper, Vector64<sbyte>  indices);

        public static Vector64<short>  Shuffle(Vector64<short>  lower, Vector64<short>  upper, Vector64<short>  indices);
        public static Vector64<ushort> Shuffle(Vector64<ushort> lower, Vector64<ushort> upper, Vector64<ushort> indices);

        public static Vector64<int>    Shuffle(Vector64<int>    lower, Vector64<int>    upper, Vector64<int>    indices);
        public static Vector64<uint>   Shuffle(Vector64<uint>   lower, Vector64<uint>   upper, Vector64<uint>   indices);
        public static Vector64<float>  Shuffle(Vector64<float>  lower, Vector64<float>  upper, Vector64<int>    indices);
    }

    public static partial class Vector128
    {
        public static uint ExtractMostSignificantBits<T>(this Vector128<T> vector);

        public static Vector128<T> Load<T>(T* source);
        public static Vector128<T> LoadAligned<T>(T* source);
        public static Vector128<T> LoadAlignedNonTemporal<T>(T* source);
        public static Vector128<T> LoadUnsafe<T>(ref T source);
        public static Vector128<T> LoadUnsafe<T>(ref T source, nuint index);

        public static void Store<T>(this Vector128<T> source, T* destination);
        public static void StoreAligned<T>(this Vector128<T> source, T* destination);
        public static void StoreAlignedNonTemporal<T>(this Vector128<T> source, T* destination);
        public static void StoreUnsafe<T>(this Vector128<T> source, ref T destination);
        public static void StoreUnsafe<T>(this Vector128<T> source, ref T destination, nuint index);

        public static Vector128<byte>   Shuffle(Vector128<byte>   vector, Vector128<byte>   indices);
        public static Vector128<sbyte>  Shuffle(Vector128<sbyte>  vector, Vector128<sbyte>  indices);

        public static Vector128<short>  Shuffle(Vector128<short>  vector, Vector128<short>  indices);
        public static Vector128<ushort> Shuffle(Vector128<ushort> vector, Vector128<ushort> indices);

        public static Vector128<int>    Shuffle(Vector128<int>    vector, Vector128<int>    indices);
        public static Vector128<uint>   Shuffle(Vector128<uint>   vector, Vector128<uint>   indices);
        public static Vector128<float>  Shuffle(Vector128<float>  vector, Vector128<int>    indices);

        public static Vector128<long>   Shuffle(Vector128<long>   vector, Vector128<long>   indices);
        public static Vector128<ulong>  Shuffle(Vector128<ulong>  vector, Vector128<ulong>  indices);
        public static Vector128<double> Shuffle(Vector128<double> vector, Vector128<long>   indices);

        public static Vector128<byte>   Shuffle(Vector128<byte>   lower, Vector128<byte>   upper, Vector128<byte>   indices);
        public static Vector128<sbyte>  Shuffle(Vector128<sbyte>  lower, Vector128<sbyte>  upper, Vector128<sbyte>  indices);

        public static Vector128<short>  Shuffle(Vector128<short>  lower, Vector128<short>  upper, Vector128<short>  indices);
        public static Vector128<ushort> Shuffle(Vector128<ushort> lower, Vector128<ushort> upper, Vector128<ushort> indices);

        public static Vector128<int>    Shuffle(Vector128<int>    lower, Vector128<int>    upper, Vector128<int>    indices);
        public static Vector128<uint>   Shuffle(Vector128<uint>   lower, Vector128<uint>   upper, Vector128<uint>   indices);
        public static Vector128<float>  Shuffle(Vector128<float>  lower, Vector128<float>  upper, Vector128<int>    indices);

        public static Vector128<long>   Shuffle(Vector128<long>   lower, Vector128<long>   upper, Vector128<long>   indices);
        public static Vector128<ulong>  Shuffle(Vector128<ulong>  lower, Vector128<ulong>  upper, Vector128<ulong>  indices);
        public static Vector128<double> Shuffle(Vector128<double> lower, Vector128<double> upper, Vector128<long>   indices);
    }

    public static partial class Vector256
    {
        public static uint ExtractMostSignificantBits<T>(this Vector256<T> vector);

        public static Vector256<T> Load<T>(T* source);
        public static Vector256<T> LoadAligned<T>(T* source);
        public static Vector256<T> LoadAlignedNonTemporal<T>(T* source);
        public static Vector256<T> LoadUnsafe<T>(ref T source);
        public static Vector256<T> LoadUnsafe<T>(ref T source, nuint index);

        public static void Store<T>(this Vector256<T> source, T* destination);
        public static void StoreAligned<T>(this Vector256<T> source, T* destination);
        public static void StoreAlignedNonTemporal<T>(this Vector256<T> source, T* destination);
        public static void StoreUnsafe<T>(this Vector256<T> source, ref T destination);
        public static void StoreUnsafe<T>(this Vector256<T> source, ref T destination, nuint index);

        public static Vector256<byte>   Shuffle(Vector256<byte>   vector, Vector256<byte>   indices);
        public static Vector256<sbyte>  Shuffle(Vector256<sbyte>  vector, Vector256<sbyte>  indices);

        public static Vector256<short>  Shuffle(Vector256<short>  vector, Vector256<short>  indices);
        public static Vector256<ushort> Shuffle(Vector256<ushort> vector, Vector256<ushort> indices);

        public static Vector256<int>    Shuffle(Vector256<int>    vector, Vector256<int>    indices);
        public static Vector256<uint>   Shuffle(Vector256<uint>   vector, Vector256<uint>   indices);
        public static Vector256<float>  Shuffle(Vector256<float>  vector, Vector256<int>    indices);

        public static Vector256<long>   Shuffle(Vector256<long>   vector, Vector256<long>   indices);
        public static Vector256<ulong>  Shuffle(Vector256<ulong>  vector, Vector256<ulong>  indices);
        public static Vector256<double> Shuffle(Vector256<double> vector, Vector256<long>   indices);

        public static Vector256<byte>   Shuffle(Vector256<byte>   lower, Vector256<byte>   upper, Vector256<byte>   indices);
        public static Vector256<sbyte>  Shuffle(Vector256<sbyte>  lower, Vector256<sbyte>  upper, Vector256<sbyte>  indices);

        public static Vector256<short>  Shuffle(Vector256<short>  lower, Vector256<short>  upper, Vector256<short>  indices);
        public static Vector256<ushort> Shuffle(Vector256<ushort> lower, Vector256<ushort> upper, Vector256<ushort> indices);

        public static Vector256<int>    Shuffle(Vector256<int>    lower, Vector256<int>    upper, Vector256<int>    indices);
        public static Vector256<uint>   Shuffle(Vector256<uint>   lower, Vector256<uint>   upper, Vector256<uint>   indices);
        public static Vector256<float>  Shuffle(Vector256<float>  lower, Vector256<float>  upper, Vector256<int>    indices);

        public static Vector256<long>   Shuffle(Vector256<long>   lower, Vector256<long>   upper, Vector256<long>   indices);
        public static Vector256<ulong>  Shuffle(Vector256<ulong>  lower, Vector256<ulong>  upper, Vector256<ulong>  indices);
        public static Vector256<double> Shuffle(Vector256<double> lower, Vector256<double> upper, Vector256<long>   indices);
    }
}
```
## ThreadStatic on a non-static field

**Approved** | [#runtime/46626](https://github.com/dotnet/runtime/issues/46626#issuecomment-1005078190) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=0h35m32s)

* Looks good as proposed by @stephentoub
    - We should have a Warning level diagnostic for applying `[ThreadStatic]` to instance fields. A fixer should offer to make it static, and potentially an option to switch it to be a `ThreadLocal<T>`, though that's more involved.
    - We should have an Info level diagnostic for providing an initial value for a `[ThreadStatic]` field.
* Should be in the Reliability category

## One shot hashing of `Stream`

**Approved** | [#runtime/62489](https://github.com/dotnet/runtime/issues/62489#issuecomment-1005091716) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=0h42m46s)

* Looks good as proposed
    - The APIs returning `byte[]` should have overloads that also accept `byte[]`

```C#
namespace System.Security.Cryptography
{
    public abstract partial class MD5
    {
        public static byte[] HashData(Stream source);
        public static int HashData(Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class SHA1
    {
        public static byte[] HashData(Stream source);
        public static int HashData(Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class SHA256
    {
        public static byte[] HashData(Stream source);
        public static int HashData(Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class SHA384
    {
        public static byte[] HashData(Stream source);
        public static int HashData(Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class SHA512
    {
        public static byte[] HashData(Stream source);
        public static int HashData(Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class HMACMD5
    {
        public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
        public static byte[] HashData(byte[] key, Stream source);
        public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class HMACSHA1
    {
        public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
        public static byte[] HashData(byte[] key, Stream source);
        public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class HMACSHA256
    {
        public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
        public static byte[] HashData(byte[] key, Stream source);
        public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class HMACSHA384
    {
        public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
        public static byte[] HashData(byte[] key, Stream source);
        public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
    public abstract partial class HMACSHA512
    {
        public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
        public static byte[] HashData(byte[] key, Stream source);
        public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

        public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    }
}
```
## ArgumentException.ThrowIfNullOrEmpty(string)

**Approved** | [#runtime/62628](https://github.com/dotnet/runtime/issues/62628#issuecomment-1005102641) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=1h7m52s)

* Looks good as proposed
    - Might be good to do a pass what `ArgumentOutOfRangeException` cases we use internally. We could base it off the unique resource strings we use in corlib.
* The API should
    - Throw `ArgumentNullException` if `argument` is `null`, otherwise `ArgumentException`
* We don't believe we need an overload of `ThrowIfNullOrEmpty` for collections as this feels rare.

```C#
namespace System
{
    public partial class ArgumentException
    {
        public static void ThrowIfNullOrEmpty([NotNull] string? argument,
                                              [CallerArgumentExpression("argument")] string? paramName = null);
    }
}
```
## RuntimeHandles should implement IEquatable

**Approved** | [#runtime/62944](https://github.com/dotnet/runtime/issues/62944#issuecomment-1005108471) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=1h17m43s)

* Looks good as proposed
* We should do a pass and find all structs that provide a custom `Equals` method without implementing `IEquatable<T>`
* This should probably be an analyzer

```C#
namespace System
{
    public struct RuntimeTypeHandle : IEquatable<RuntimeTypeHandle>
    {
        // bool Equals(RuntimeTypeHandle) already exists
    }

    public struct RuntimeMethodHandle : IEquatable<RuntimeMethodHandle>
    {
        // bool Equals(RuntimeMethodHandle) already exists
    }

    public struct RuntimeFieldHandle : IEquatable<RuntimeFieldHandle>
    {
        // bool Equals(RuntimeFieldHandle) already exists
    }
}
```
## Make `Lookup<TKey,TElement>` publicly constructable

**NeedsWork** | [#runtime/62949](https://github.com/dotnet/runtime/issues/62949#issuecomment-1005120322) | [Video](https://www.youtube.com/watch?v=yz-tRVxVo-g&t=1h25m31s)

* Looks good as proposed
    - We want to avoid adding conflicts with RX/IX so we don't to create extension methods like `ToLookupAsync()`
    - At the same time we don't want to make `Lookup<TKey, TElement>` mutable
    - This means we'd need a single entry point that takes an `IAsyncEnumerable<TSource>` and produces a `Lookup<TKey, TElement>`
    - Easiest seems to be factory method directly on `Lookup<TKey, TElement>`
* Alternatively, EF could return a custom implementation of `ILookup<TKey, TElement>`, similar to PLinq
    - Sounds like we want to explore this first

```C#
namespace System.Linq
{
    public partial class Lookup<TKey, TElement>
    {
        public static Task<Lookup<TKey, TElement>> CreateAsync<TSource>(
            IAsyncEnumerable<TSource> source,
            Func<TSource, TKey> keySelector,
            Func<TSource, TElement> elementSelector,
            IEqualityComparer<TKey> comparer);
    }
}
```
