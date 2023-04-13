# API Review 04/13/2023

## IUtf8SpanFormattable and IUtf8SpanParsable

**Approved** | [#runtime/81500](https://github.com/dotnet/runtime/issues/81500#issuecomment-1507326851) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=0h0m0s)

This came up in review to discuss whether the types implementing this interface should implement the methods implicitly (public on the type) or explicitly (requiring the interface cast/coercion).

The answer was "match what we did for ISpanParsable/ISpanFormattable", which seems to be implicit everywhere except System.Char (explicit there).
## Parallel.ForAsync

**Approved** | [#runtime/59019](https://github.com/dotnet/runtime/issues/59019#issuecomment-1507355191) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=0h9m18s)

Looks good as proposed

```C#
namespace System.Threading.Tasks
{
    public static class Parallel
    {
        public static Task ForAsync(int fromInclusive, int toExclusive, Func<int, CancellationToken, ValueTask> body);
        public static Task ForAsync(int fromInclusive, int toExclusive, CancellationToken cancellationToken, Func<int, CancellationToken, ValueTask> body);
        public static Task ForAsync(int fromInclusive, int toExclusive, ParallelOptions parallelOptions, Func<int, CancellationToken, ValueTask> body);
        public static Task ForAsync(long fromInclusive, long toExclusive, Func<long, CancellationToken, ValueTask> body);
        public static Task ForAsync(long fromInclusive, long toExclusive, CancellationToken cancellationToken, Func<long, CancellationToken, ValueTask> body);
        public static Task ForAsync(long fromInclusive, long toExclusive, ParallelOptions parallelOptions, Func<long, CancellationToken, ValueTask> body);
    }
}
```
## Add TargetHostName to QuicConnection

**Approved** | [#runtime/80508](https://github.com/dotnet/runtime/issues/80508#issuecomment-1507358695) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=0h24m8s)

Looks good as proposed (assuming "..." was "get;")

```C#
namespace System.Net.Quic;

public sealed partial class QuicConnection : IAsyncDisposable
{
     public string TargetHostName { get; }
}
```
## Make some System.Net.Security functions/properties public for System.Net.Quic

**NeedsWork** | [#runtime/67552](https://github.com/dotnet/runtime/issues/67552#issuecomment-1507370783) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=0h27m0s)

We weren't feeling good about the ReadOnlyMemory<X509Certificate2> property, and want to have a discussion about it.

```C#
// Public ctors, but getters are internal.
public class SslStreamCertificateContext
{
-   internal readonly X509Certificate2 Certificate;
+   public X509Certificate2 TargetCertificate { get; }
+   public ReadOnlyMemory<X509Certificate2> IntermediateCertificates { get; }
    // public ctors
}

// It has public getters, but cannot be constructed with values outside of System.Net.Security.
public readonly struct SslClientHelloInfo
{
    public readonly string ServerName { get; }
    public readonly SslProtocols SslProtocols { get; }

-   internal SslClientHelloInfo(string serverName, SslProtocols sslProtocols);
+   public SslClientHelloInfo(string serverName, SslProtocols sslProtocols);
}
```
## Add overload for EventSource primitives

**Approved** | [#runtime/83751](https://github.com/dotnet/runtime/pull/83751#issuecomment-1507465690) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=0h35m55s)

* This type should support all of the types that EventSource considers a primitive.
* If any of the specific types are a trimming problem then the conversion operator can/will gain the trimmer warning attributes.
* `nuint` is in the list to force it to explicitly go through ToString instead of an implicit ulong conversion.
* Passing a literal `null` into `WriteEvent(int, params object[])`/`WriteEvent(int, params EventSourcePrimitive[])` will become a source breaking change, but we're OK with that.
* Tracing may want to look into adding support for additional types (e.g. TimeOnly/DateOnly/DateTimeOffSet/Half/Int128/UInt128/nuint), all of which currently go through ToString().  This proposal merely pointed out the strangeness at this boundary.


```C#
namespace System.Diagnostics.Tracing
{
    public class EventSource : IDisposable
    {
        protected void WriteEvent(int eventId, params EventSourcePrimitive[] args) { }

        public readonly struct EventSourcePrimitive
        {
            private object _reference;
            private int _value;

            public static implicit operator EventSourcePrimitive(bool value) => ...;
            public static implicit operator EventSourcePrimitive(byte value) => ...;
            public static implicit operator EventSourcePrimitive(short value) => ...;
            public static implicit operator EventSourcePrimitive(int value) => ...;
            public static implicit operator EventSourcePrimitive(long value) => ...;

            public static implicit operator EventSourcePrimitive(sbyte value) => ...;
            public static implicit operator EventSourcePrimitive(ushort value) => ...;
            public static implicit operator EventSourcePrimitive(uint value) => ...;
            public static implicit operator EventSourcePrimitive(ulong value) => ...;

            public static implicit operator EventSourcePrimitive(float value) => ...;
            public static implicit operator EventSourcePrimitive(double value) => ...;
            public static implicit operator EventSourcePrimitive(decimal value) => ...;

            public static implicit operator EventSourcePrimitive(string? value) => ...;
            public static implicit operator EventSourcePrimitive(byte[]? value) => ...;

            public static implicit operator EventSourcePrimitive(Guid value) => ...;
            public static implicit operator EventSourcePrimitive(DateTime value) => ...;
            public static implicit operator EventSourcePrimitive(nint value) => ...;
            public static implicit operator EventSourcePrimitive(nuint value) => ...;
            public static implicit operator EventSourcePrimitive(char value) => ...;

            public static implicit operator EventSourcePrimitive(Enum value) => ...;
        }
    }
}
```
## Make RegexRunner.CharInClass public instead of protected

**Approved** | [#runtime/84783](https://github.com/dotnet/runtime/issues/84783#issuecomment-1507469875) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=1h47m1s)

Looks good as proposed, though there was a recommendation to EditorBrowsable-Never the member (or the type, for that matter).

```diff
namespace System.Text.RegularExpressions;

public abstract class RegexRunner
{
-    protected static bool CharInClass(char ch, string charClass);
+    public    static bool CharInClass(char ch, string charClass);
}
```
## Obsolete some protected members of Regex{Runner}

**Approved** | [#runtime/62573](https://github.com/dotnet/runtime/issues/62573#issuecomment-1507482840) | [Video](https://www.youtube.com/watch?v=_ZWe0DXheDE&t=1h50m20s)

Because the proposal mentions removing the body of CharInSet, I've marked it as a breaking change.

Since CharInSet is having its body removed, it probably warrants obsolete-as-error, the rest being obsolete-as-warning.

Obsoletions should just use the next available SYSLIB code.  (The same code can apply to all of them, even though it's mixing warning and error)

EditorBrowsable-Never is also fine if it makes you happy.

```diff
namespace System.Text.RegularExpressions;

public class Regex
{
+   [Obsolete(...)]
    protected void InitializeReferences();

+   [Obsolete(...)]
    protected bool UseOptionC()

+   [Obsolete(...)]
    protected bool UseOptionR();
}

public abstract class RegexRunner
{
+   [Obsolete(...)]
    protected Match? Scan(Regex regex, string text, int textbeg, int textend, int textstart, int prevlen, bool quick) => Scan(regex, text, textbeg, textend, textstart, prevlen, quick, regex.MatchTimeout);

+   [Obsolete(...)]
    protected internal Match? Scan(Regex regex, string text, int textbeg, int textend, int textstart, int prevlen, bool quick, TimeSpan timeout)

+   [Obsolete(...)]
    protected static bool CharInSet(char ch, string set, string category);
}
```
