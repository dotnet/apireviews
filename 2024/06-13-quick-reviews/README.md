# API Review 06/13/2024

## Add multipart/related to `MediaTypeNames.Multipart`

**Approved** | [#runtime/95446](https://github.com/dotnet/runtime/issues/95446#issuecomment-2166357986) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=0h0m0s)


* We changed `Gzip` to `GZip` to match the casing of GZipStream
* We removed NDJson as it's not recognized by IANA.

```c#
namespace System.Net.Mime;

public static partial class MediaTypeNames
{
    public static partial class Multipart
    {
         // see https://www.ietf.org/rfc/rfc2387.txt
         public const string Related = "multipart/related";
         // see https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html
         public const string Mixed = "multipart/mixed";
    }
    public static partial class Application
    {
         // https://datatracker.ietf.org/doc/html/rfc6713
         public const string GZip = "application/gzip";
    }
    public static partial class Text
    {
        // see https://github.com/dotnet/runtime/issues/102194
        public const string EventStream = "text/event-stream";
    }
}
```
## Expose overloads of HttpContent.LoadIntoBufferAsync taking a CancellationToken

**Approved** | [#runtime/102659](https://github.com/dotnet/runtime/issues/102659#issuecomment-2166372663) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=0h6m53s)


* We asked if they should use a more complicated pattern of showing them as optional parameters and doing EditorBrowsable-Never on the existing overloads, and decided that pure overloads was more locally consistent.

```c#
 namespace System.Net.Http;
 
 public partial class HttpContent
 {
     public Task LoadIntoBufferAsync(CancellationToken cancellationToken);
     public Task LoadIntoBufferAsync(long maxBufferSize, CancellationToken cancellationToken);
 }
```
## [HTTP/3] Support multiple HTTP/3 connections

**Approved** | [#runtime/101535](https://github.com/dotnet/runtime/issues/101535#issuecomment-2166383944) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=0h10m25s)

Looks good as proposed

```c#
namespace System.Net.Http;

public sealed partial class SocketsHttpHandler : System.Net.Http.HttpMessageHandler
{
    public bool EnableMultipleHttp3Connections { get { throw null; } set { } }
}
```
## [QUIC] Support managing available streams on `QuicConnection`

**Approved** | [#runtime/101534](https://github.com/dotnet/runtime/issues/101534#issuecomment-2166451805) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=0h16m31s)


* We changed a custom callback to use Action
* We renamed... everything :)

```c#
namespace System.Net.Quic;

// Existing class.
public abstract partial class QuicConnectionOptions
{
    // New callback, raised when MAX_STREAMS frame arrives, reports new capacity for stream limits.
    // First invocation with the initial values might happen during QuicConnection.ConnectAsync or QuicListener.AcceptConnectionAsync, before the QuicConnection object is handed out.
    public Action<QuicConnection, QuicStreamCapacityChangedArgs>? StreamCapacityCallback { get; set; }
}

// Callback arguments:
public readonly struct QuicStreamCapacityChangedArgs
{
    public int BidirectionalIncrement { get; init; }
    public int UnidirectionalIncrement { get; init; }
}
```

## Obsolete ServicePointManager

**Approved** | [#runtime/71836](https://github.com/dotnet/runtime/issues/71836#issuecomment-2166471632) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=0h50m42s)

* While we generally don't like obsoleting an entire type, this type is functionally a `static class` and so the incidental signature impact concerns don't apply.
* It's unfortunate that three of the properties still have effect in a non-Obsolete type, but it's low usage.

```diff
namespace System.Net;

+ [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
  public class ServicePointManager
  {
-     [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
      public static ServicePoint FindServicePoint(Uri address);

-     [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
      public static ServicePoint FindServicePoint(string uriString, IWebProxy? proxy);

-     [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
      public static ServicePoint FindServicePoint(Uri address, IWebProxy? proxy);
  }
```
## Revert unintentional nullability changes and fix inconsistencies in StringContent ctrs

**Approved** | [#runtime/83423](https://github.com/dotnet/runtime/issues/83423#issuecomment-2166474864) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h0m39s)

The changes look consistent with the code's treatment of those parameters.

```diff
public class StringContent : ByteArrayContent
{
-   public StringContent(string content, MediaTypeHeaderValue  mediaType);
+   public StringContent(string content, MediaTypeHeaderValue? mediaType);
-   public StringContent(string content, Encoding? encoding, string  mediaType);
+   public StringContent(string content, Encoding? encoding, string? mediaType);
-   public StringContent(string content, Encoding? encoding, MediaTypeHeaderValue  mediaType);
+   public StringContent(string content, Encoding? encoding, MediaTypeHeaderValue? mediaType);
}
```
## Consider support for `IEquatable<Uri>` on `System.Uri`

**Approved** | [#runtime/97940](https://github.com/dotnet/runtime/issues/97940#issuecomment-2166478354) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h2m12s)

* Looks good as proposed
* Added a `[NotNullWhen(true)]`.

```diff
namespace System;

-public partial class Uri : ISpanFormattable, ISerializable
+public partial class Uri : ISpanFormattable, ISerializable, IEquatable<Uri>
{
    // Existing
    public override bool Equals(object? comparand);

+   public bool Equals([NotNullWhen(true)] Uri? other);
}
```
## Implement IUtf8SpanParsable on IPAddress & IPNetwork

**Approved** | [#runtime/103111](https://github.com/dotnet/runtime/issues/103111#issuecomment-2166491574) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h4m4s)


* IPAddress doesn't respect the IFormatProvider, so should use the explicit implementation pattern.
* Otherwise, looks good as proposed.

```diff
namespace System.Net;

public partial class IPAddress
    : ISpanFormattable, ISpanParsable<IPAddress>, IUtf8SpanFormattable
+   , IUtf8SpanParsable<IPAddress>
{
+   static IPAddress IUtf8SpanParsable<IPAddress>.Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);
+   public static IPAddress Parse(ReadOnlySpan<byte> utf8Text);

+   static bool IUtf8SpanParsable<IPAddress>.TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider,
+       [NotNullWhen(true)] out IPAddress? result);
+   public static bool TryParse(ReadOnlySpan<byte> utf8Text, [NotNullWhen(true)] out IPAddress? result);

}

public readonly partial struct IPNetwork
    : ISpanFormattable, ISpanParsable<IPAddress>, IUtf8SpanFormattable
+   , IUtf8SpanParsable<IPNetwork>
{
    static IPNetwork ISpanParsable<IPNetwork>.Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
    public static IPNetwork Parse(ReadOnlySpan<char> s);
+   static IPNetwork IUtf8SpanParsable<IPNetwork>.Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);
+   public static IPNetwork Parse(ReadOnlySpan<byte> utf8Text);

    static bool ISpanParsable<IPNetwork>.TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, out IPNetwork result);
    public static bool TryParse(ReadOnlySpan<char> s, out IPNetwork result);
+   static bool IUtf8SpanParsable<IPNetwork>.TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider,
+       out IPNetwork result);
+   public static bool TryParse(ReadOnlySpan<byte> utf8Text,
+       [MaybeNullWhen(false)] out IPNetwork? result);
}
```

## [QUIC] Make CompleteWritesAsync

**Approved** | [#runtime/103434](https://github.com/dotnet/runtime/issues/103434#issuecomment-2166524147) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h10m5s)

* There were questions as to why the WritesClosed property needs to still exist, but there is a legitimate scenario.

```diff
namespace System.Net.Quic;

// Existing class.
public class QuicStream : Stream
{
-    public void CompleteWrites();
+    public ValueTask CompleteWritesAsync();
}
```

## Expose the remaining const values in TimeSpan

**Approved** | [#runtime/94545](https://github.com/dotnet/runtime/issues/94545#issuecomment-2166531840) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h28m22s)


* The consts that can be `int` should be
* Otherwise, looks good as proposed.

```C#
namespace System.Collections.Generic;

public partial class TimeSpan
{
        public const int MicrosecondsPerMillisecond = TicksPerMillisecond / TicksPerMicrosecond; //           1,000
        public const int MicrosecondsPerSecond = TicksPerSecond / TicksPerMicrosecond;           //       1,000,000
        public const int MicrosecondsPerMinute = TicksPerMinute / TicksPerMicrosecond;           //      60,000,000
        public const long MicrosecondsPerHour = TicksPerHour / TicksPerMicrosecond;               //   3,600,000,000
        public const long MicrosecondsPerDay = TicksPerDay / TicksPerMicrosecond;                 //  86,400,000,000

        public const int MillisecondsPerSecond = TicksPerSecond / TicksPerMillisecond;           //           1,000
        public const int MillisecondsPerMinute = TicksPerMinute / TicksPerMillisecond;           //          60,000
        public const int MillisecondsPerHour = TicksPerHour / TicksPerMillisecond;               //       3,600,000
        public const int MillisecondsPerDay = TicksPerDay / TicksPerMillisecond;                 //      86,400,000

        public const int SecondsPerMinute = TicksPerMinute / TicksPerSecond;                     //              60
        public const int SecondsPerHour = TicksPerHour / TicksPerSecond;                         //           3,600
        public const int SecondsPerDay = TicksPerDay / TicksPerSecond;                           //          86,400

        public const int MinutesPerHour = TicksPerHour / TicksPerMinute;                         //              60
        public const int MinutesPerDay = TicksPerDay / TicksPerMinute;                           //           1,440

        public const int HoursPerDay = TicksPerDay / TicksPerHour;                               //              24
}
```

## Vector.Create

**Approved** | [#runtime/94384](https://github.com/dotnet/runtime/issues/94384#issuecomment-2166550562) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h32m57s)

Looks good as proposed

```C#
namespace System.Numerics
{
    public static partial class Vector
    {
        public static Vector<T> Create<T>(T value);
        public static Vector<T> Create<T>(ReadOnlySpan<T> values);

        public static Quaternion AsQuaternion(this Vector4 value);
        public static Plane AsPlane(this Vector4 value);

        public static Vector2 AsVector2(this Vector4 value);
        public static Vector3 AsVector3(this Vector4 value);

        public static Vector4 AsVector4(this Quaternion value);
        public static Vector4 AsVector4(this Plane value);
        public static Vector4 AsVector4(this Vector2 value);
        public static Vector4 AsVector4(this Vector3 value);
        public static Vector4 AsVector4Unsafe(this Vector2 value);
        public static Vector4 AsVector4Unsafe(this Vector3 value);
    }

    public partial struct Vector2
    {
        public static Vector2 Create(float value);
        public static Vector2 Create(float x, float y);
        public static Vector2 Create(ReadOnlySpan<float> values);
    }

    public partial struct Vector3
    {
        public static Vector3 Create(float value);
        public static Vector3 Create(float x, float y, float z);
        public static Vector3 Create(Vector2 vector, float z);
        public static Vector3 Create(ReadOnlySpan<float> values);
    }

    public partial struct Vector4
    {
        public static Vector4 Create(float value);
        public static Vector4 Create(float x, float y, float z, float w);
        public static Vector4 Create(Vector2 vector, float z, float w);
        public static Vector4 Create(Vector3 vector, float w);
        public static Vector4 Create(ReadOnlySpan<float> values);
    }
}

namespace System.Runtime.Intrinsics
{
    public static partial class Vector128
    {
        public static Vector128<float> AsVector128Unsafe(this Vector2 value);
        public static Vector128<float> AsVector128Unsafe(this Vector3 value);
        public static Vector128<float> AsVector128Unsafe(this Vector4 value);
    }
}
```
## Add missing BigMul overload for uint x uint -> ulong

**Approved** | [#runtime/98346](https://github.com/dotnet/runtime/issues/98346#issuecomment-2166565241) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h43m45s)

* The question was raised of why add these on the `Math` type, since we have previously said that the type is soft-deprecated.
  * The answer is "because they're overloading an existing member."
* There was a brief question about the applicability to generic math, but since that requires `(TSelf, TBigger)` or `(TSelf, TSmaller)` that's too complicated, so this finite set is all we get.
* We'll leave the ones on Math as `(a, b)` to match the current method, but the ones on each type will use the more globally-consistent `(left, right)` naming.
* There's no real need to do this for (s)byte -> (~u)short, or (u)short -> (u)int.

```c#
namespace System;

public static partial class Math
{
    public static Int128 BigMul(long a, long b);
    public static ulong BigMul(uint a, uint b);
    public static UInt128 BigMul(ulong a, ulong b);
}

public readonly partial struct Int32
{
    public static long BigMul(int left, int right);
}

public readonly partial struct Int64
{
    public static Int128 BigMul(long left, long right);
}

public readonly partial struct UInt32
{
    public static ulong BigMul(uint left, uint right);
}

public readonly partial struct UInt64
{
    public static UInt128 BigMul(ulong left, ulong right);
}
```
## Span.StartsWith/EndsWith(T)

**Approved** | [#runtime/87689](https://github.com/dotnet/runtime/issues/87689#issuecomment-2166580661) | [Video](https://www.youtube.com/watch?v=GRFNsuv0YRE&t=1h52m43s)

Looks good as proposed.

There was a question of whether we're missing something by not having `StartsWith(ROS<char>, char, StringComparison)`/EndsWith, e.g. the German scharfes-S compared with "ss", but we can add that later if there's a demonstrated need.

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static bool StartsWith<T>(this System.ReadOnlySpan<T> span, T value) where T : System.IEquatable<T>?;
    public static bool StartsWith<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>?;
    public static bool EndsWith<T>(this System.ReadOnlySpan<T> span, T value) where T : System.IEquatable<T>?;
    public static bool EndsWith<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>?;
}
```
