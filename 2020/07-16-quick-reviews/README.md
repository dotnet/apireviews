# Quick Reviews 07/16/2020

## File.ReadLinesAsync

**Approved** | [#runtime/2214](https://github.com/dotnet/runtime/issues/2214#issuecomment-659551371) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=0h0m0s)

* Looks good as proposed
* The `[EnumeratorCancellation]` attribute might be needed in the implementation if we use `yield return`, but it doesn't have to be part of the ref.

```C#
namespace System.IO
{
    public static partial class File
    {
        public static IAsyncEnumerable<string> ReadLinesAsync(string path, CancellationToken cancellationToken = default);
        public static IAsyncEnumerable<string> ReadLinesAsync(string path, System.Text.Encoding encoding, CancellationToken cancellationToken = default);
    }
}
## Add `IDeviceContext` to events that hold `Graphics`

**Approved** | [#winforms/3570](https://github.com/dotnet/winforms/issues/3570#issuecomment-659555018) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=0h13m58s)

* We should implement `IDisposable` following the dispose pattern if (1) the type doesn't already implement `IDisposable` and (2) the type isn't sealed.
* `IDeviceContext` should be implemented explicitly.

```C#
namespace System.Windows.Forms
{
    public partial class PaintEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class DrawItemEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class DataGridViewRowPostPaintEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class DrawListViewItemEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class DrawToolTipEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class DrawTreeNodeEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class ToolStripArrowRenderEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class ToolStripContentPanelRenderEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class ToolStripItemRenderEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class ToolStripPanelRenderEventArgs : IDisposable, IDeviceContext
    { 
    }
    public partial class ToolStripRenderEventArgs : IDisposable, IDeviceContext
    { 
    }
}
```
## Mark Windows-specific APIs

**Approved** | [#designs/138](https://github.com/dotnet/designs/pull/138#issuecomment-659561716) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=0h20m49s)

* The [proposal](https://github.com/dotnet/designs/pull/138) looks good.
* We should consider a diagnostic to warn on typos in the OS name, e.g. `[MinimumOSPlatform("windos7.0")]`
## Expose CLong, CULong, and NFloat interchange types

**Approved** | [#runtime/13788](https://github.com/dotnet/runtime/issues/13788#issuecomment-659573076) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=0h32m37s)

* Looks good
* We should add ctors for the smaller sizes.

```C#
namespace System.Runtime.InteropServices
{
    public readonly struct CLong : IEquatable<CLong>
    {
        public CLong(int value);
        public CLong(nint value);
        public nint Value { get; }
        public override bool Equals(object o);
        public bool Equals(CLong other);
        public override int GetHashCode();
        public override string ToString();
    }
    public readonly struct CULong : IEquatable<CULong>
    {
        public CULong(uint value);
        public CULong(nuint value);
        public nuint Value { get; }
        public override bool Equals(object o);
        public bool Equals(CULong other);
        public override int GetHashCode();
        public override string ToString();
    }
    public readonly struct NFloat : IEquatable<NFloat>
    {
        public NFloat(float value);
        public NFloat(double value);
        public double Value { get; }
        public override bool Equals(object o);
        public bool Equals(NFloat other);
        public override int GetHashCode();
        public override string ToString();
    }
}
```
## Enhancement: ECDiffieHellman X509 certificate management gaps

**Approved** | [#runtime/413](https://github.com/dotnet/runtime/issues/413#issuecomment-659580892) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=0h53m25s)

* Makes sense, but we should make them proper instance methods, especially because the implementation private state of `X509Certificate2`

```C#
namespace System.Security.Cryptography.X509Certificates
{
    public partial class X509Certificate2
    {
        public X509Certificate2 CopyWithPrivateKey(ECDiffieHellman privateKey);
        public ECDiffieHellman GetECDiffieHellmanPrivateKey();
        public ECDiffieHellman GetECDiffieHellmanPublicKey();
    }
    public partial class PublicKey
    {
        public PublicKey(AsymmetricAlgorithm key);
        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
        public byte[] ExportSubjectPublicKeyInfo();
        public static PublicKey CreateFromSubjectPublicKeyInfo(ReadOnlySpan<byte> source, out int bytesRead);
    }
}
```

## export SPKI on ECDiffieHellmanPublicKey

**Approved** | [#runtime/587](https://github.com/dotnet/runtime/issues/587#issuecomment-659582856) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h7m18s)

* Makes sense. The implementation will throw `NotSupportedException`.

```C#
namespace System.Security.Cryptography
{
    public abstract partial class ECDiffieHellmanPublicKey
    {
        public virtual bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
        public virtual byte[] ExportSubjectPublicKeyInfo();
    }
}
```

## New custom attribute required to support C++ inline namespaces

**Approved** | [#runtime/783](https://github.com/dotnet/runtime/issues/783#issuecomment-659584823) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h11m10s)

* Looks good as proposed.
* @jkotas pointed out that you may no longer need it. If you don't, please close this issue. If you do, consider it approved.

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
    public sealed class CppInlineNamespaceAttribute : Attribute
    {
        public CppInlineNamespaceAttribute(string dottedName);
    }
}
```

## Add support for zlib data format (RFC 1950)

**Approved** | [#runtime/2236](https://github.com/dotnet/runtime/issues/2236#issuecomment-659590517) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h15m11s)

* Looks good as proposed.

```C#
namespace System.IO.Compression
{
    public class ZLibStream : Stream
    {
        public ZLibStream(Stream stream, CompressionLevel compressionLevel);
        public ZLibStream(Stream stream, CompressionMode mode);
        public ZLibStream(Stream stream, CompressionLevel compressionLevel, bool leaveOpen);
        public ZLibStream(Stream stream, CompressionMode mode, bool leaveOpen);
        public Stream BaseStream { get; }
    }
}
```

## IComparable for Rune

**Approved** | [#runtime/2340](https://github.com/dotnet/runtime/issues/2340#issuecomment-659592809) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h26m13s)

* Looks good as proposed.
* We should consider doing an analyzer that flags that. @terrajobst to file a request.

```C#
namespace System.Text
{
    public partial struct Rune : IComparable<Rune>, IComparable
    {
    }
}
```

## Do not use [Out] string for P/Invokes

**Approved** | [#runtime/35692](https://github.com/dotnet/runtime/issues/35692#issuecomment-659595195) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h29m43s)

The analyzer/fixer makes sense.
## Avoid StringBuilder parameters for P/Invokes

**Approved** | [#runtime/35693](https://github.com/dotnet/runtime/issues/35693#issuecomment-659597092) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h35m28s)

Shipping this as a warning and off by default makes sense. Setting the severity as suggestion would likely mean that nobody will ever see this.
## Prefer ExactSpelling=true on [DllImport] for known APIs

**Approved** | [#runtime/35695](https://github.com/dotnet/runtime/issues/35695#issuecomment-659601306) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h38m43s)

Looks good as proposed.
## CryptoStream.FlushFinalBlockAsync method missing

**Approved** | [#runtime/38076](https://github.com/dotnet/runtime/issues/38076#issuecomment-659602801) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h47m37s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography
{
    partial class CryptoStream
    {
        public ValueTask FlushFinalBlockAsync(CancellationToken cancellationToken = default);
    }
}
```
## Attribute and analyzer for "soft abstract"

**NeedsWork** | [#runtime/38238](https://github.com/dotnet/runtime/issues/38238#issuecomment-659610010) | [Video](https://www.youtube.com/watch?v=7YpDyRMaDKE&t=1h50m31s)

* We like the analzyer and we're OK with that not being handled by all languages
    - @terrajobst to fill in more details after the meeting
* The analyzer should only consider missing overrides in the direct base type. Warning on more derived base types isn't useful because the base type is most likely the one that would need to provide behavior.
* We should do a scan for all types that have virtuals that throw
* We should think about how and if we differentiate "this always throw", "this is an optional feature", "this a less performant implementation, please provide a better one" so that we can differentiate the diagnostic ID/severity accordingly.
