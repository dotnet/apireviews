# Quick Reviews 12/10/2019

## More cancellation overloads for HttpContent

**Approved** | [#runtime/576](https://github.com/dotnet/runtime/issues/576#issuecomment-564164757) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=0h0m0s)

Looks good as proposed.

```C#
partial class HttpContent
{
    protected virtual Task<Stream> CreateContentReadStreamAsync(); // Exists
    protected virtual Task<Stream> CreateContentReadStreamAsync(CancellationToken cancellationToken);

    public Task CopyToAsync(Stream stream); // Exists
    public Task CopyToAsync(Stream stream, TransportContext context); // Exists
    public Task CopyToAsync(Stream stream, CancellationToken cancellationToken);
    public Task CopyToAsync(Stream stream, TransportContext context, CancellationToken cancellationToken);
}
```
## Option to allow HttpClient to follow HTTPS -> HTTP redirects

**Needs Work** | [#corefx/33791](https://github.com/dotnet/corefx/issues/33791#issuecomment-564169859) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=0h14m5s)

This needs work:

* It's a dangerous API; the name seems OK, although we normally use the `Unsafe` prefix.
* We wonder whether instead of simple bool this should be callback that takes at least the URL or even the request message and return true whether it should be redirected or not.

```C#
namespace System.Net.Http
{
    public partial class SocketsHttpHandler
    {
        public bool DangerousAllowHttpsToHttpRedirection { get; set; }
    }
}
```
## Add ImmutableHashSet<T>.Builder.TryGetValue

**Approved** | [#corefx/34019](https://github.com/dotnet/corefx/issues/34019#issuecomment-564172216) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=0h25m52s)

Looks good as proposed.

However, we should review the API surface of all immutable types and make sure that the API surface of the builder is logically equivalent.

@GeorgeAlexandria / @safern would one of you be willing to do that?

```C#
namespace System.Collections.Immutable
{
    public partial class ImmutableHashSet<T>
    {
        public partial class Builder
        {
            public bool TryGetValue(T equalValue, out T actualValue);
        }
    }

    public partial class ImmutableSortedSet<T>
    {
        public partial class Builder
        {
            public bool TryGetValue(T equalValue, out T actualValue);
        }
    }
}
```
## Create ASCII ToUpper / ToLower APIs

**Needs Work** | [#corefx/34144](https://github.com/dotnet/corefx/issues/34144#issuecomment-564176989) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=0h31m42s)

Looks good but needs work:

* We didn't like the name `AsciiUtility`
* We felt the type should be in `System.Buffers.Text` as that's the home for low-level span-based APIs for specific encodings
* We should add other operations, such as:
    - `GetHashCode`
    - `Compare`
    - ...

```C#
namespace System.Buffers.Text
{
   public static class Ascii
   {
      public static bool EqualsOrdinalIgnoreCase(ReadOnlySpan<byte> left, ReadOnlySpan<byte> right);
      public static int ToLowerInvariant(ReadOnlySpan<byte> source, Span<byte> destination);
      public static int ToUpperInvariant(ReadOnlySpan<byte> source, Span<byte> destination);
      public static void ToLowerInvariantInPlace(Span<byte> buffer);
      public static void ToUpperInvariantInPlace(Span<byte> buffer);
   }
}
```
## Add an atomic ToArray + Clear method to ConcurrentDictionary<TKey, TValue>

**Needs Work** | [#corefx/34619](https://github.com/dotnet/corefx/issues/34619#issuecomment-564182556) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=0h43m36s)

Maybe we don't understand the scenario but as stated it doesn't make sense to us:

1. In order to be safe, dispose needs to stop any party that adds to the dictionary
2. Once that's done, you can just drain the dictionary

@stephentoub, any other thoughts?

## WaitForExitAsync for System.Diagnostics.Process

**Approved** | [#corefx/34689](https://github.com/dotnet/corefx/issues/34689#issuecomment-564185885) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=0h56m57s)

Looks good as proposed.

```C#
namespace System.Diagnostics
{
    public partial class Process
    {
        public Task WaitForExitAsync(CancellationToken cancellationToken = default);
    }
}
```
## Implement IBufferWrite<byte> on MemoryStream

**Needs Work** | [#corefx/35400](https://github.com/dotnet/corefx/issues/35400#issuecomment-564205625) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=1h5m26s)

A couple of questions:

* Why do you need it on `MemoryStream`? Why is `ArrayBufferWriter<byte>` not sufficient for your scenarios?
* Do you actually need an `IBufferWriter<byte>` that is also a stream?
* Any objection to us creating an allocation-free method on a stream type that returns an `IBufferWriter<byte>`?
## Change NetworkStream.Socket property from protected to public

**Approved** | [#corefx/35410](https://github.com/dotnet/corefx/issues/35410#issuecomment-564210981) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=1h21m54s)

Seems fine. The only question is: @scalablecory mentioned that we might want to use `NetworkStream` for non-socket based streams, e.g. QUIC. What would happen then?

> `Socket` property is protected, but not virtual, which means we should be able to just change its visibility.

That's correct and we've done this in the past.
## Add Single and Double overloads to BinaryPrimitives

**Approved** | [#corefx/35791](https://github.com/dotnet/corefx/issues/35791#issuecomment-564214440) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=1h34m0s)

* Looks good as proposed
* Should we also add these methods for parity?
    - `ReverseEndianness(float)`
    - `ReverseEndianness(double)`

```C#
namespace System.Buffers.Binary
{
    public static class BinaryPrimitives
    {
        public static float ReadSingleBigEndian(ReadOnlySpan<byte> buffer);
        public static float ReadSingleLittleEndian(ReadOnlySpan<byte> buffer);
        public static double ReadDoubleBigEndian(ReadOnlySpan<byte> buffer);
        public static double ReadDoubleLittleEndian(ReadOnlySpan<byte> buffer);

        public static bool TryReadSingleBigEndian(ReadOnlySpan<byte> buffer, out float value);
        public static bool TryReadSingleLittleEndian(ReadOnlySpan<byte> buffer, out float value);
        public static bool TryReadDoubleBigEndian(ReadOnlySpan<byte> buffer, out double value);
        public static bool TryReadDoubleLittleEndian(ReadOnlySpan<byte> buffer, out double value);

        public static bool TryWriteSingleBigEndian(System.Span<byte> destination, float value);
        public static bool TryWriteSingleLittleEndian(System.Span<byte> destination, float value);
        public static bool TryWriteDoubleBigEndian(System.Span<byte> destination, double value);
        public static bool TryWriteDoubleLittleEndian(System.Span<byte> destination, double value);

        public static void WriteSingleBigEndian(System.Span<byte> destination, float value);
        public static void WriteSingleLittleEndian(System.Span<byte> destination, float value);
        public static void WriteDoubleBigEndian(System.Span<byte> destination, double value);
        public static void WriteDoubleLittleEndian(System.Span<byte> destination, double value);
    }
}
```
## add Span overloads for Socket datagram functions

**Approved** | [#corefx/35861](https://github.com/dotnet/corefx/issues/35861#issuecomment-564217357) | [Video](https://www.youtube.com/watch?v=zTzY7iL34Hg&t=1h42m10s)

* Looks good as proposed.
* However, should we stop adding async methods as extension and just make them proper public instance methods?
* In fact we may want to hide the entire `SocketTaskExtensions` because it was only used as a bridge pack for .NET Framework.
* Remaining: we should have overloads for the method that take `IList<ArraySegment<byte>>` because they are more efficient.

```C#
namespace System.Net.Sockets
{
    // Open question: consider defaulting SocketFlags to SocketFlags.None to reduce API space? Might challenge IntelliSense.

    public partial class Socket : IDisposable
    {
        // existing: public int SendTo(byte[] buffer, EndPoint remoteEP);
        public int SendTo(ReadOnlySpan<byte> buffer, EndPoint remoteEP);

        // existing: public int SendTo(byte[] buffer, SocketFlags socketFlags, EndPoint remoteEP);
        public int SendTo(ReadOnlySpan<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP);

        // existing: public int ReceiveFrom(byte[] buffer, ref EndPoint remoteEP);
        public int ReceiveFrom(Span<byte> buffer, ref EndPoint remoteEP);

        // existing: public int ReceiveFrom(byte[] buffer, int offset, int size, SocketFlags socketFlags, ref EndPoint remoteEP);
        public int ReceiveFrom(Span<byte> buffer, SocketFlags socketFlags, ref EndPoint remoteEP);
    }

    public static class SocketTaskExtensions
    {
        // existing: public static Task<int> SendToAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP);
        public static ValueTask<int> SendToAsync(this Socket socket, ReadOnlyMemory<byte> buffer, EndPoint remoteEP, CancellationToken cancellationToken = null);
        public static ValueTask<int> SendToAsync(this Socket socket, ReadOnlyMemory<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP, CancellationToken cancellationToken = null);

        // existing: public static Task<SocketReceiveFromResult> ReceiveFromAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
        public static ValueTask<SocketReceiveFromResult> ReceiveFromAsync(this Socket socket, Memory<byte> buffer, EndPoint remoteEP, CancellationToken cancellationToken = null);
        public static ValueTask<SocketReceiveFromResult> ReceiveFromAsync(this Socket socket, Memory<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP, CancellationToken cancellationToken = null);

        // existing: public static Task<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
        public static ValueTask<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(this Socket socket, Memory<byte> buffer, EndPoint remoteEP, CancellationToken cancellationToken = null);
        public static ValueTask<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(this Socket socket, Memory<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP, CancellationToken cancellationToken = null);
    }
}
```
