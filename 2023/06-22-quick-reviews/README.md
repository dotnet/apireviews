# API Review 06/22/2023

## udate QuicError values

**Approved** | [#runtime/87259](https://github.com/dotnet/runtime/issues/87259#issuecomment-1603037411) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h0m0s)

* The "User" prefix on UserCallbackError doesn't seem significant, so changed to just "CallbackError"
* The removed items should be documented as a preview breaking change.

```diff
namespace System.Net.Quic
{
    public enum QuicError
    {
        // existing members omitted

+       /// <summary>
+       /// An error occurred in user provided callback.
+       /// </summary>
+       CallbackError,

-       AddressInUse,
-       InvalidAddress,
-       HostUnreachable, // ICMP error
    }
}
```

## Add transport error code to QuicException

**Approved** | [#runtime/87262](https://github.com/dotnet/runtime/issues/87262#issuecomment-1603044309) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h8m20s)

Looks good as proposed.  The rename needs to be documented as a preview breaking change.

```diff
namespace System.Net.Quic
{
    public enum QuicError
    {
        // existing members omitted

-       /// <summary>
-       /// A QUIC protocol error was encountered.
-       /// </summary>
-       ProtocolError,
+       /// <summary>
+       /// Operation failed because peer transport error occurred.  
+       /// </summary>
+       TransportError,
  }
  public class QuicException
  {
        public System.Net.Quic.QuicError QuicError { get { throw null; } }
        public long? ApplicationErrorCode { get { throw null; } }
+.      public long? TransportErrorCode { get { throw null; }}
  }
}
```
## Read/Write Big/LittleEndian support for Guid

**Approved** | [#runtime/86798](https://github.com/dotnet/runtime/issues/86798#issuecomment-1603081570) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h13m42s)

* The BinaryPrimitives alternatives don't seem to be necessary given the new members on Guid, so they were cut.
* We removed the `is` prefixes from the `isBigEndian` parameters
* We discussed adding `Guid..ctor(byte[], bool)` and think it's OK to leave it off.

```C#
namespace System
{
    public partial struct Guid
    {
        // public Guid(byte[] value);
        // public Guid(ReadOnlySpan<byte> value);
        public Guid(ReadOnlySpan<byte> value, bool bigEndian);

        // public byte[] ToByteArray();
        public byte[] ToByteArray(bool bigEndian);

        // public bool TryWriteBytes(Span<byte> destination);
        // public bool TryWriteBytes(Span<byte> destination, out int bytesWritten); -- new in .NET 8
        public bool TryWriteBytes(Span<byte> destination, bool bigEndian, out int bytesWritten);
    }
}
```

## Utf8.IsValid(bytes)

**Approved** | [#runtime/502](https://github.com/dotnet/runtime/issues/502#issuecomment-1603088425) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h42m25s)

Looks good as proposed

```C#
namespace System.Text.Unicode
{
    // existing static type that shipped in Core 3.0
    public static class Utf8
    {
        // proposed new API
        public static bool IsValid(ReadOnlySpan<byte> value);
    }
}
```
## Happy Eyeballs support in Socket.ConnectAsync

**Approved** | [#runtime/87932](https://github.com/dotnet/runtime/issues/87932) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h47m57s)

## Uri : ISpanFormattable

**Approved** | [#runtime/87151](https://github.com/dotnet/runtime/issues/87151#issuecomment-1603103666) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h59m10s)

Looks good as proposed.  We additionally feel that it's worthwhile to approve the IUtf8SpanFormattable addition (even if it gets implemented later)

```diff
namespace System;

public class Uri : ISerializable,
+    ISpanFormattable
+    , IUtf8SpanFormattable
{
+    public bool TryFormat(Span<char> destination, out int charsWritten);
+    public bool TryFormat(Span<byte> utf8Destination, out int bytesWritten);
+    // Explicitly implement the formattable interfaces to hide unsupported parameters.
}
```
## BCL additions to support C# 12's "Collection Literals" feature.

**NeedsWork** | [#runtime/87569](https://github.com/dotnet/runtime/issues/87569#issuecomment-1603177269) | [Video](https://www.youtube.com/watch?v=60l-cZDhRT4&t=0h59m36s)

We think that we can leave out the "Literal" token in CollectionLiteralBuilderAttribute, so just "CollectionBuilderAttribute".

There's still work to be done on the List creation pattern (exposing storage, ownership transferrence, etc).

The ImmutableCollections(ReadOnlySpan) are all good things to add, even without the language feature.


```diff
public static class ImmutableHashSet
{
+    public static System.Collections.Immutable.ImmutableHashSet<T> Create<T>(System.ReadOnlySpan<T> items);
}

public static class ImmutableList
{
+    public static System.Collections.Immutable.ImmutableList<T> Create<T>(System.ReadOnlySpan<T> items);
}

public static class ImmutableQueue
{
+    public static System.Collections.Immutable.ImmutableQueue<T> Create<T>(System.ReadOnlySpan<T> items);
}

public static class ImmutableSortedSet
{
+    public static System.Collections.Immutable.ImmutableQueue<T> Create<T>(System.ReadOnlySpan<T> items);
}

public static class ImmutableStack
{
+    public static System.Collections.Immutable.ImmutableQueue<T> Create<T>(System.ReadOnlySpan<T> items);
}
```
