# Quick Reviews 05/19/2021

##  Add public Architecture enum value for s390x

**Approved** | [#runtime/52909](https://github.com/dotnet/runtime/issues/52909#issuecomment-844306365) | [Video](https://www.youtube.com/watch?v=IHRPmHuDQvU&t=0h0m0s)

Looks good as proposed

```diff
namespace System.Runtime.InteropServices
{
    public enum Architecture
    {
        X86,
        X64,
        Arm,
        Arm64,
        Wasm,
+       S390x,
    }
}
```
## HMAC one-shot methods

**Approved** | [#runtime/40012](https://github.com/dotnet/runtime/issues/40012#issuecomment-844312500) | [Video](https://www.youtube.com/watch?v=IHRPmHuDQvU&t=0h12m38s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography {
    public partial class HMACMD5 {
        public static byte[] HashData(byte[] key, byte[] source) => throw null;
        public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source) => throw null;
        public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
        public static bool TryHashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
    }

    public partial class HMACSHA1 {
        public static byte[] HashData(byte[] key, byte[] source) => throw null;
        public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source) => throw null;
        public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
        public static bool TryHashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
    }

    public partial class HMACSHA256 {
        public static byte[] HashData(byte[] key, byte[] source) => throw null;
        public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source) => throw null;
        public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
        public static bool TryHashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
    }

    public partial class HMACSHA384 {
        public static byte[] HashData(byte[] key, byte[] source) => throw null;
        public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source) => throw null;
        public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
        public static bool TryHashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
    }

    public partial class HMACSHA512 {
        public static byte[] HashData(byte[] key, byte[] source) => throw null;
        public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source) => throw null;
        public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
        public static bool TryHashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
    }
}
```
## SecureString obsoletions and shrouded buffer proposal

**Approved** | [#designs/147](https://github.com/dotnet/designs/pull/147#issuecomment-844392771) | [Video](https://www.youtube.com/watch?v=IHRPmHuDQvU&t=0h20m37s)

* The System.Security namespace is largely CAS, and may have the same bad connotations as the "Secure" in "SecureString"
  * The type being "System.Secret" seems like we're going to set up for too much conflict.
  * "System.Buffers.Secret"
  * If we wanted System, we need a suffix, like "System.SecretBuffer", "System.SecretData"
* Currently there's only one property (Length).  Changing it to a method means this will generally be considered an empty object to serializers.
* We renamed SecretExtensions to Secret and added Secret.Create<T> to improve generic inference.
* "Unshroud" feels a little weird, but it does have value as being easy to search for.
  * We seem to like "Reveal", which still provides a search target.  "Expose" was the runner up.
* We added a "RevealAnd" prefix to "Use" so all of the data-exposing methods start with the same word.
* We discussed the SecureString/Secret layering and decided to added the conversion methods on SecureString, even though they're .NET 6.0 only and the Secret type is .NET Standard 2.0
* Discussing overloads/etc (impact of the new type) is pushed to a later date, because we hit the end of the meeting.
* The ToString() method should just be the same answer as object.ToString() (nothing useful), but we should override it to Obsolete it (to point to RevealToString()).
* A DebuggerDisplay attribute seems good.  "Length={length}, Text={...}" for char, "Length={length}" for other.  Or something.
* The issue title talks about obsoleting SecureString.  We're not applying the attribute at this time, just starting down the road for doing it later.

netstandard2.0:
```C#
namespace System.Buffers
{
    public sealed partial class Secret<T> : System.IDisposable where T : unmanaged
    {
        public Secret(System.ReadOnlySpan<T> buffer) { }
        public int GetLength() => throw null;
        public System.Security.Secret<T> Clone() { throw null; }
        public void Dispose() { }
        public int RevealInto(System.Span<T> destination) { }
        public T[] RevealToArray() { throw null; }
        public void RevealAndUse<TArg>(TArg arg, System.Buffers.ReadOnlySpanAction<T, TArg> spanAction) { }

        [Obsolete("Use RevealToString")]
        [EditorBrowsable(Never)]
        public override string ToString() => base.ToString();
    }
    public static partial class Secret
    {
        // ArgumentNullException
        public static Secret<char> Create(string value) => throw null;
        // ArgumentNullException
        public static Secret<T> Create<T>(T[] buffer) where T : unmanaged => throw null;

        public static Secret<T> Create<T>(ReadOnlySpan<T> buffer) where T : unmanaged => throw null;
        public static string RevealToString(this System.Security.Secret<char> secret) { throw null; }
    }
}
```

net60:
```C#
namespace System.Security
{
     partial class SecureString : System.IDisposable
     {
        public SecureString(System.Security.Secret<char> secret) { }
        public System.Security.Secret<char> ToSecret() { throw null; }
     }
 }
```
