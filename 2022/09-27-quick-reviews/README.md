# API Review 09/27/2022

## API for inserting a Span<T> into a List<T> efficiently

**Approved** | [#runtime/1530](https://github.com/dotnet/runtime/issues/1530#issuecomment-1259846917) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=0h0m0s)

* We should talk to @jaredpar / @dotnet/ldm about whether his proposal for "binary compatibility only" could be used here to avoid having to add new method group. We could also make them extension methods in which case they wouldn't cause ambiguities but only be used for spans. That being said, given that F# and VB wouldn't have the hypothetical feature so we probably would do the extension method for a core type anyway.
* There is another concern about generic instantiations and lack of pay-for-play for methods only used for some `T`s.

This would be the desired API:

```C#
namespace System.Collections.Generic;

public partial class List<T>
{
    public void AddRange(ReadOnlySpan<T> span);
    public void InsertRange(int index, ReadOnlySpan<T> span);
    public void CopyTo(Span<T> span);
}
```

If we go down the extension method route, it would look like this:

```C#
namespace System.Collections.Generic;

public static class CollectionExtensions
{
    public static void AddRange<T>(this List<T> list, ReadOnlySpan<T> source);
    public static void InsertRange<T>(this List<T> list, int index, ReadOnlySpan<T> source);
    public static void CopyTo<T>(this List<T> list, Span<T> destination);
    // We discussed this but decided against this until there is a need.
    // public static bool TryCopyTo<T>(this List<T> list, Span<T> destination);
}
```

We're inclined to approve the extension method shape and simultaneously work with the compiler team to see whether we can use a language feature to avoid using the extension methods.
## BinaryPrimitives.ReverseEndianness

**Approved** | [#runtime/75901](https://github.com/dotnet/runtime/issues/75901#issuecomment-1259877207) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=0h43m20s)

* Looks good as proposed
    - We discussed offering a single argument one. One argument against it that it might make code harder to read because ignoring the return value for scalars would be wrong. Another argument against it is that the two argument version is more versatile (it can handle passing the same span).

```C#
namespace System.Buffers.Binary;

public static class BinaryPrimitives
{
    public static void ReverseEndianness(ReadOnlySpan<ushort> source, Span<ushort> destination);
    public static void ReverseEndianness(ReadOnlySpan<short> source, Span<short> destination);
    public static void ReverseEndianness(ReadOnlySpan<uint> source, Span<uint> destination);
    public static void ReverseEndianness(ReadOnlySpan<int> source, Span<int> destination);
    public static void ReverseEndianness(ReadOnlySpan<ulong> source, Span<ulong> destination);
    public static void ReverseEndianness(ReadOnlySpan<long> source, Span<long> destination);
    public static void ReverseEndianness(ReadOnlySpan<nuint> source, Span<nuint> destination);
    public static void ReverseEndianness(ReadOnlySpan<nint> source, Span<nint> destination);
    public static void ReverseEndianness(ReadOnlySpan<UInt128> source, Span<UInt128> destination);
    public static void ReverseEndianness(ReadOnlySpan<Int128> source, Span<Int128> destination);
}
```

## Expose missing BinaryPrimitives APIs

**Approved** | [#runtime/72107](https://github.com/dotnet/runtime/issues/72107#issuecomment-1259896437) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=1h9m26s)

* Looks good as proposed

```C#
namespace System.Buffers.Binary;

public static partial class BinaryPrimitives
{
    // Read*BigEndian and Read*LittleEndian

    public static nint ReadIntPtrBigEndian(ReadOnlySpan<byte> source);
    public static nint ReadIntPtrLittleEndian(ReadOnlySpan<byte> source);

    public static Int128 ReadInt128BigEndian(ReadOnlySpan<byte> source);
    public static Int128 ReadInt128LittleEndian(ReadOnlySpan<byte> source);

    public static nuint ReadUIntPtrBigEndian(ReadOnlySpan<byte> source);
    public static nuint ReadUIntPtrLittleEndian(ReadOnlySpan<byte> source);

    public static UInt128 ReadUInt128BigEndian(ReadOnlySpan<byte> source);
    public static UInt128 ReadUInt128LittleEndian(ReadOnlySpan<byte> source);

    // ReverseEndianness

    public static nint ReverseEndianness(nint value);
    public static Int128 ReverseEndianness(Int128 value);
    public static nuint ReverseEndianness(nuint value);
    public static UInt128 ReverseEndianness(UInt128 value);

    // TryRead*BigEndian and TryRead*LittleEndian

    public static bool TryReadIntPtrBigEndian(ReadOnlySpan<byte> source, out nint value);
    public static bool TryReadIntPtrLittleEndian(ReadOnlySpan<byte> source, out nint value);

    public static bool TryReadInt128BigEndian(ReadOnlySpan<byte> source, out Int128 value);
    public static bool TryReadInt128LittleEndian(ReadOnlySpan<byte> source, out Int128 value);

    public static bool TryReadUIntPtrBigEndian(ReadOnlySpan<byte> source, out nuint value);
    public static bool TryReadUIntPtrLittleEndian(ReadOnlySpan<byte> source, out nuint value);

    public static bool TryReadUInt128BigEndian(ReadOnlySpan<byte> source, out UInt128 value);
    public static bool TryReadUInt128LittleEndian(ReadOnlySpan<byte> source, out UInt128 value);

    // TryWrite*BigEndian and TryWrite*LittleEndian

    public static bool TryWriteIntPtrBigEndian(ReadOnlySpan<byte> destination, nint value);
    public static bool TryWriteIntPtrLittleEndian(ReadOnlySpan<byte> destination, nint value);

    public static bool TryWriteInt128BigEndian(ReadOnlySpan<byte> destination, Int128 value);
    public static bool TryWriteInt128LittleEndian(ReadOnlySpan<byte> destination, Int128 value);

    public static bool TryWriteUIntPtrBigEndian(ReadOnlySpan<byte> destination, nuint value);
    public static bool TryWriteUIntPtrLittleEndian(ReadOnlySpan<byte> destination, nuint value);

    public static bool TryWriteUInt128BigEndian(ReadOnlySpan<byte> destination, UInt128 value);
    public static bool TryWriteUInt128LittleEndian(ReadOnlySpan<byte> destination, UInt128 value);

    // Write*BigEndian and Write*LittleEndian

    public static void WriteIntPtrBigEndian(ReadOnlySpan<byte> destination, nint value);
    public static void WriteIntPtrLittleEndian(ReadOnlySpan<byte> destination, nint value);

    public static void WriteInt128BigEndian(ReadOnlySpan<byte> destination, Int128 value);
    public static void WriteInt128LittleEndian(ReadOnlySpan<byte> destination, Int128 value);

    public static void WriteUIntPtrBigEndian(ReadOnlySpan<byte> destination, nuint value);
    public static void WriteUIntPtrLittleEndian(ReadOnlySpan<byte> destination, nuint value);

    public static void WriteUInt128BigEndian(ReadOnlySpan<byte> destination, UInt128 value);
    public static void WriteUInt128LittleEndian(ReadOnlySpan<byte> destination, UInt128 value);
}
```

## [Analyzer] Prefer .Length/Count/IsEmpty over Any()

**Approved** | [#runtime/75933](https://github.com/dotnet/runtime/issues/75933#issuecomment-1259900134) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=1h27m28s)

* Looks good as proposed

Severity: Info
Category: Performance
## Dictionary<TKey, TValue>.KeysCollection.Contains

**Approved** | [#runtime/75936](https://github.com/dotnet/runtime/issues/75936#issuecomment-1259905482) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=1h30m49s)

* Looks good as proposed
    - We should also apply that to `ReadOnlyDictionary<TKey, TValue>`
    - We should ensure the existing analyzer does the right thing with these changes.

```diff
namespace System.Collections.Generic;

public class Dictionary<TKey, TValue>
{
    public sealed class KeyCollection
    {
-        bool ICollection<TKey>.Contains(TKey item)
+        public bool Contains(TKey item);
    }
}

public class SortedDictionary<TKey, TValue>
{
    public sealed class KeyCollection
    {
-        bool ICollection<TKey>.Contains(TKey item)
+        public bool Contains(TKey item);
    }
}
```
## XXH3

**Approved** | [#runtime/75948](https://github.com/dotnet/runtime/issues/75948#issuecomment-1259920069) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=1h36m2s)

* Looks good as proposed
    - We considered also adding `XxHash128` but decided to wait until there is a need.

```C#
namespace System.IO.Hashing;

public sealed partial class XxHash3 : NonCryptographicHashAlgorithm
{
    public XxHash3();
    public XxHash3(int seed);

    public static byte[] Hash(byte[] source);
    public static byte[] Hash(byte[] source, int seed);
    public static byte[] Hash(ReadOnlySpan<byte> source, int seed = 0);
    public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination, int seed = 0);
    public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, int seed = 0);
}
```
## Base64.IsValid

**Approved** | [#runtime/76020](https://github.com/dotnet/runtime/issues/76020#issuecomment-1259936196) | [Video](https://www.youtube.com/watch?v=mRH5hFQ--v0&t=1h50m28s)

* Looks good as proposed
    - We should consider answering the question whether `Decode` will work, rather than just checking whether it's syntactically valid Base64, which would be more useful.

```C#
namespace System.Buffers.Text;

public static class Base64
{
    public static bool IsValid(ReadOnlySpan<char> base64Text);
    public static bool IsValid(ReadOnlySpan<char> base64Text, out int decodedLength);
    public static bool IsValid(ReadOnlySpan<byte> base64Text);
    public static bool IsValid(ReadOnlySpan<byte> base64Text, out int decodedLength);
}
```
