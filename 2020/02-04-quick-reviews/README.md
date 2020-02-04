# Quick Reviews 2/4/2020

## Add an API to perform streaming transcoding

**Approved** | [#runtime/30260](https://github.com/dotnet/runtime/issues/30260#issuecomment-582050875) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=0h0m0s)

* We'll likely need this ourselves for the `HttpClient` extension methods for JSON, so putting it in the BCL seems more reasonable than an ASP.NET/networking specific helper
* Instead of exposing a concrete type, we'd prefer a factory because we'll likely want to have dedicated implementations for specific encodings in order to speed things up.
* Putting the factory on `Encoding` seems most sensible because that's the array based helpers are too

```C#
namespace System.Text
{
    public partial class Encoding
    {
        public static Stream CreateTranscodingStream(Stream innerStream, Encoding innerStreamEncoding, Encoding outerStreamEncoding, bool leaveOpen = false);
    }
}
```
## SetCreationTime, SetLastAccessTime, SetLastWriteTime Should not open a new stream to obtain a SafeFileHandle

**Approved** | [#runtime/20234](https://github.com/dotnet/runtime/issues/20234#issuecomment-582053400) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=0h23m34s)

* The scenario makes sense, but defining them as extension methods on `SafeFileHandle` seems odd
* It seems more logical to define them as overloads to the `string` based APIs on `File`.

```C#
namespace System.IO
{
    public static partial class File
    {
        public static DateTime GetCreationTime(SafeFileHandle fileHandle);
        public static DateTime GetCreationTimeUtc(SafeFileHandle fileHandle);
        public static DateTime GetLastAccessTime(SafeFileHandle fileHandle);
        public static DateTime GetLastAccessTimeUtc(SafeFileHandle fileHandle);
        public static DateTime GetLastWriteTime(SafeFileHandle fileHandle);
        public static DateTime GetLastWriteTimeUtc(SafeFileHandle fileHandle);

        public static void SetCreationTime(SafeFileHandle fileHandle, DateTime creationTime);
        public static void SetCreationTimeUtc(SafeFileHandle fileHandle, DateTime creatinTimeUtc);
        public static void SetLastAccessTime(SafeFileHandle fileHandle, DateTime lastAccessTime);
        public static void SetLastAccessTimeUtc(SafeFileHandle fileHandle, DateTime lastAccessTimeUtc);
        public static void SetLastWriteTime(SafeFileHandle fileHandle, DateTime lastWriteTime);
        public static void SetLastWriteTimeUtc(SafeFileHandle fileHandle, DateTime lastWriteTimeUtc);

        // Seems we should be adding support for these too.
        // If we can't make it work, dropping is fine.
        public static FileAttributes GetAttributes(SafeFileHandle fileHandle);
        public static void SetAttributes(SafeFileHandle fileHandle, FileAttributes fileAttributes);
    }
}
```
## Add bit manipulation methods to Interlocked class

**Approved** | [#runtime/23819](https://github.com/dotnet/runtime/issues/23819#issuecomment-582059179) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=0h29m38s)

* Makes sense, the naming `location1` matches the existing methods
* We'll review support for unsigned in a separate issue (#24694)
* @GrabYourPitchforks will file a separate issue for test and clear/reset

```C#
namespace System.Threading
{
    public static partial class Interlocked
    {
        public static int And(ref int location1, int value);
        public static long And(ref long location1, long value);
        public static int Or(ref int location1, int value);
        public static long Or(ref long location1, long value);
        public static int Xor(ref int location1, int value);
        public static long Xor(ref long location1, long value);
    }
}
```
## Interlocked.CompareExchange missing for uint, ulong and (if possible) general structs.

**Approved** | [#runtime/24694](https://github.com/dotnet/runtime/issues/24694#issuecomment-582063204) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=0h43m4s)

* Makes sense, we added unsigned for the APIs approved in #23819
* Let's ignore the generic struct here; this should be a separate proposal
* We should probably add `Interlocked` support for `nint` and `nuint` support on Interlocked. @tannergooding, could you file a tracking item for this?

```C#
namespace System.Threading
{
    public static partial class Interlocked
    {
        // New proposed APIs, pretty much a carbon copy of the above but unsigned
        // All of these would be marked [CLSCompliant(false)]

        public static uint Add(ref uint location1, uint value);
        public static ulong Add(ref ulong location1, ulong value);
        public static uint CompareExchange(ref uint location1, uint value, uint comparand);
        public static ulong CompareExchange(ref ulong location1, ulong value, ulong comparand);
        public static UIntPtr CompareExchange(ref UIntPtr location1, UIntPtr value, UIntPtr comparand);
        public static uint Decrement(ref uint location);
        public static ulong Decrement(ref ulong location);
        public static uint Exchange(ref uint location1, uint value);
        public static ulong Exchange(ref ulong location1, ulong value);
        public static UIntPtr Exchange(ref UIntPtr location1, UIntPtr value);
        public static uint Increment(ref uint location);
        public static ulong Increment(ref ulong location);
        public static ulong Read(ref ulong location);

        // Since these were approved in #23819 we should add them:
        // All of these would be marked [CLSCompliant(false)]

        public static int And(ref uint location1, uint value);
        public static long And(ref ulong location1, ulong value);
        public static int Or(ref uint location1, uint value);
        public static long Or(ref ulong location1, ulong value);
        public static int Xor(ref uint location1, uint value);
        public static long Xor(ref ulong location1, ulong value);
    }
}
```
## One-shot PEM reader

**Approved** | [#runtime/29588](https://github.com/dotnet/runtime/issues/29588#issuecomment-582069797) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=0h52m11s)

* The constructor on `PemFields` isn't public to avoid having to validate well-formedness
* Should `PemFields` implement IEquatable<PemFields>`?

```C#
namespace System.Security.Cryptography
{
    public static class PemEncoding
    {
        public static bool TryFind(ReadOnlySpan<char> pemData, out PemFields fields);
        public static PemFields Find(ReadOnlySpan<char> pemData);

        public static int GetEncodedSize(int labelLength, int dataLength);
        public static bool TryWrite(ReadOnlySpan<char> label, ReadOnlySpan<byte> data, Span<char> destination, out int charsWritten);
        public static char[] Write(ReadOnlySpan<char> label, ReadOnlySpan<byte> data);
    }

    public readonly struct PemFields
    {
        public Range Location { get; }
        public Range Label { get; }
        public Range Base64Data { get; }
        public int DecodedDataLength { get; }
    }
 }
 ```
## Mark Write7BitEncodedInt and Read7BitEncodedInt as public

**Approved** | [#runtime/24473](https://github.com/dotnet/runtime/issues/24473#issuecomment-582076029) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=1h7m31s)

* We should reuse the existing methods, which are protected (since the method isn't virtual, making it public isn't a breaking change).
* We don't want to add `BigInteger` support because reader/writer has no support for `BigInteger`. This should be a separate proposal.

```C#
namespace System.IO
{
    public class BinaryReader
    {
        public int Read7BitEncodedInt();
        public long Read7BitEncodedInt64();
    }
    public class BinaryWriter
    {
        public int Write7BitEncodedInt(int value);
        public int Write7BitEncodedInt64(long value);
    }
}
```
## Easy reference equality comparer API

**Approved** | [#runtime/27683](https://github.com/dotnet/runtime/issues/27683#issuecomment-582080375) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=1h22m15s)

* Let's put it in `System.Collections.Generic` (because `System.Collections` doesn't have a `EqualityComparer` anyway).
* We don't want to put it on `EqualityComparer<T>` because it wouldn't vary by `T`
* Based on Internet search, it seems most folks calls their implementation `ReferenceEqualityComparer`. We considered introducing a non-generic `EqualityComparer` with a property `ReferenceEquality` that felt odd. Also, having a dedicated sealed type allows implementations to check whether it's the special reference equality comparer. The field would have to guarantee that it's the same instance, which we might not want.

```C#
namespace System.Collections.Generic
{
    public sealed class ReferenceEqualityComparer : IEqualityComparer, IEqualityComparer<object>
    {
        private ReferenceEqualityComparer();
        public static ReferenceEqualityComparer Instance { get; }

        // Plus inherited members from IEqualityComparer, IEqualityComparer<object>
    }
}
```
