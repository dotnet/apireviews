# API Review 05/12/2026

## UnmanagedCallersOnlyAttribute.AssociatedType

**Approved** | [#runtime/127634](https://github.com/dotnet/runtime/issues/127634#issuecomment-4433123325) | [Video](https://www.youtube.com/watch?v=Ni04mZXEp8M&t=0h0m0s)

* There was discussion as to how to handle a callback that was based on two types, and the feeling was that it can be handled with typemap.
  * We could add a `Type[] AssociatedTypes` later if need be.
* Added infix "Source" to match the name in typemap.
* Associated{Source}Type should really be a property, but all of the other members on this attribute type are fields, so this one can be a field, too.

```csharp
namespace System.Runtime.InteropServices;

public partial class UnmanagedCallersOnlyAttribute : Attribute
{
    public Type? AssociatedSourceType;
}
```
## BitOperations.ReverseBits

**Approved** | [#runtime/125879](https://github.com/dotnet/runtime/issues/125879#issuecomment-4433166525) | [Video](https://www.youtube.com/watch?v=Ni04mZXEp8M&t=0h31m22s)

Looks good as proposed.

```csharp
namespace System.Numerics
{
    public partial interface IBinaryInteger<TSelf> : IBinaryNumber<TSelf>, IShiftOperators<TSelf, int, TSelf>
        where TSelf : IBinaryInteger<TSelf>?
    {
        static virtual TSelf ReverseBits(TSelf value);
    }
}

// Public/implicit on all types except char (explicitly special handled), and BigInteger (either explicit or via DIM)

namespace System
{
    public readonly partial struct Byte : IBinaryInteger<byte>
    {
       public static byte ReverseBits(byte value);
    }

    public readonly partial struct SByte : IBinaryInteger<sbyte>
    {
       public static sbyte ReverseBits(sbyte value);
    }

    public readonly partial struct UInt16 : IBinaryInteger<ushort>
    {
       public static ushort ReverseBits(ushort value);
    }

    public readonly partial struct Int16 : IBinaryInteger<short>
    {
       public static short ReverseBits(short value);
    }

    public readonly partial struct UInt32 : IBinaryInteger<uint>
    {
       public static uint ReverseBits(uint value);
    }

    public readonly partial struct Int32 : IBinaryInteger<int>
    {
       public static int ReverseBits(int value);
    }

    public readonly partial struct UInt64 : IBinaryInteger<ulong>
    {
       public static ulong ReverseBits(ulong value);
    }

    public readonly partial struct Int64 : IBinaryInteger<long>
    {
       public static long ReverseBits(long value);
    }

    public readonly partial struct UIntPtr : IBinaryInteger<nuint>
    {
       public static nuint ReverseBits(nuint value);
    }

    public readonly partial struct IntPtr : IBinaryInteger<nint>
    {
       public static nint ReverseBits(nint value);
    }

    public readonly partial struct UInt128 : IBinaryInteger<UInt128>
    {
       public static UInt128 ReverseBits(UInt128 value);
    }

    public readonly partial struct Int128 : IBinaryInteger<Int128>
    {
       public static Int128 ReverseBits(Int128 value);
    }

    public readonly partial struct Char : IBinaryInteger<char>
    {
       static char IBinaryInteger<char>.ReverseBits(char value);
    }
}
```
## HybridCache: Setting expiration based on value in cache entry factory

**Approved** | [#runtime/121530](https://github.com/dotnet/runtime/issues/121530#issuecomment-4433496637) | [Video](https://www.youtube.com/watch?v=Ni04mZXEp8M&t=0h37m32s)

* Rather than introduce a copy of HybridCacheEntryOptions just to change get/init to get/set, let's just change HybridCacheEntryOptions to get/set.
  * It feels like GetOrCreateAsync with a non-null HybridCacheEntryOptions can maybe not clone that options value.  Or maybe cloning is good.
  * Added Revision to HybridCacheEntryOptions so implementations can identify if the options changed in the callback.

```diff
public partial class HybridCacheEntryOptions
{
-   public TimeSpan? Expiration { get; init; }
+   public TimeSpan? Expiration { get; set; }
-   public TimeSpan? LocalCacheExpiration { get; init; }
+   public TimeSpan? LocalCacheExpiration { get; set; }
-   public HybridCacheEntryFlags? Flags { get; init; }
+   public HybridCacheEntryFlags? Flags { get; set; }
}
```

```csharp

public partial class HybridCacheEntryOptions
{
    public int Revision { get; private set; }
}

public partial abstract class HybridCache
{
    // New core overload
    public virtual ValueTask<T> GetOrCreateAsync<TState, T>(
        string key, TState state,
        Func<TState, HybridCacheEntryOptions, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    // Convenience (no TState)
    public ValueTask<T> GetOrCreateAsync<T>(
        string key,
        Func<HybridCacheEntryOptions, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

#if NET
    // ReadOnlySpan<char> key variants
    public virtual ValueTask<T> GetOrCreateAsync<TState, T>(
        ReadOnlySpan<char> key, TState state,
        Func<TState, HybridCacheEntryOptions, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    public ValueTask<T> GetOrCreateAsync<T>(
        ReadOnlySpan<char> key,
        Func<HybridCacheEntryOptions, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    // DefaultInterpolatedStringHandler key variants
    public ValueTask<T> GetOrCreateAsync<TState, T>(
        ref DefaultInterpolatedStringHandler key, TState state,
        Func<TState, HybridCacheEntryOptions, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    public ValueTask<T> GetOrCreateAsync<T>(
        ref DefaultInterpolatedStringHandler key,
        Func<HybridCacheEntryOptions, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);
#endif
}
```
## Specify L1 memory usage in HybridCache

**Approved** | [#runtime/114366](https://github.com/dotnet/runtime/issues/114366#issuecomment-4433589016) | [Video](https://www.youtube.com/watch?v=Ni04mZXEp8M&t=1h21m21s)

* We shortened `LocalCacheSize` to `LocalSize` to avoid ambiguous parsing from the common compound `CacheSize`

```csharp
namespace Microsoft.Extensions.Caching.Hybrid;

public partial class HybridCacheEntryOptions
{
    public long? LocalSize { get; set; }
}
```
## SequenceReaderExtensions.{TryPeekLittleEndian, TryPeekBigEndian} extensions

**Approved** | [#runtime/116414](https://github.com/dotnet/runtime/issues/116414#issuecomment-4433741088) | [Video](https://www.youtube.com/watch?v=Ni04mZXEp8M&t=1h30m33s)

Looks good as proposed.

If unsigned variants, or offset variants, are needed later, they can come as a separate proposal.

```csharp
namespace System.Buffers;

public partial static class SequenceReaderExtensions
{
    public static bool TryPeekLittleEndian(ref this SequenceReader<byte> reader, out short value);
    public static bool TryPeekLittleEndian(ref this SequenceReader<byte> reader, out int value);
    public static bool TryPeekLittleEndian(ref this SequenceReader<byte> reader, out long value);

    public static bool TryPeekBigEndian(ref this SequenceReader<byte> reader, out short value);
    public static bool TryPeekBigEndian(ref this SequenceReader<byte> reader, out int value);
    public static bool TryPeekBigEndian(ref this SequenceReader<byte> reader, out long value);
}
```
## Extend the vector types to have `char` as a supported element type

**Approved** | [#runtime/127611](https://github.com/dotnet/runtime/issues/127611#issuecomment-4433869101) | [Video](https://www.youtube.com/watch?v=Ni04mZXEp8M&t=1h50m59s)


* Let's add the "homogenous" method support, so Create/Shufle/Shift, but not Narrow/Widen
* Clone for Vector/Vector64/Vector256/Vector512 as appropriate.
* Flagged as breaking change because of Vector128.Char('a','a','a','a','a','a','a','a') currently compiling (as ushort?).

```csharp
using System.Runtime.Intrinsics;

public static partial class Vector128
{
    public static Vector128<char> AsChar<T>(this Vector128<T> vector);
}

public static partial class Vector128
{
    public static Vector128<char> Create(char e0, char e1, char e2, char e3, char e4, char e5, char e6, char e7);

    // We have `M<T>(...)` variants, the non-generic versions are older and so these are "consistency"
    public static Vector128<char> Create(char value);
    public static Vector128<char> Create(Vector64<char> lower, Vector64<char> upper);
    public static Vector128<char> CreateScalar(char value);
    public static Vector128<char> CreateScalarUnsafe(char value);

    // Operators exist, these are just named variants
    public static Vector128<char> ShiftLeft(Vector128<char> vector, int shiftCount);
    public static Vector128<char> ShiftRightLogical(Vector128<char> vector, int shiftCount);

    // Indices need to explicitly be integers (same for `float`/`double`)
    public static Vectore128<char> Shuffle(Vector128<char> vector, Vector128<ushort> indices);
    public static Vectore128<char> ShuffleNative(Vector128<char> vector, Vector128<ushort> indices);
}
```
