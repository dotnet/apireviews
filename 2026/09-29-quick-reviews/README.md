# API Review 09/29/2026

## Expose SessionKey on NegotiateAuthentication

**Approved** | [#runtime/111099](https://github.com/dotnet/runtime/issues/111099#issuecomment-5895124120) | [Video](https://www.youtube.com/watch?v=1-BNpNFSCxo&t=0h0m0s)


* Since this is a power scenario, let's just go with the most empowering shape, and we can add others in if needed.
* TState needs allows ref struct so a span can be passed in.
* We don't see a need for allows ref struct on TReturn at this time.

```csharp
namespace System.Net.Security;

public partial class NegotiateAuthentication
{
    public TReturn DeriveKeyFromSessionKey<TState, TReturn>(Func<ReadOnlySpan<byte>, TState, TReturn> keyDerivationFunction, TState state) where TState : allows ref struct;

}
```
## Expose HashLog and ChainLog on ZstandardCompressionOptions

**Approved** | [#runtime/129214](https://github.com/dotnet/runtime/issues/129214#issuecomment-5895218927) | [Video](https://www.youtube.com/watch?v=1-BNpNFSCxo&t=0h16m17s)


* Based on WindowLog2, all of these need a "2" suffix.

```csharp
namespace System.IO.Compression;

public sealed partial class ZstandardCompressionOptions
{
    public static int MinHashLog2 { get; }
    public static int MaxHashLog2 { get; }
    public static int MinChainLog2 { get; }
    public static int MaxChainLog2 { get; }
 
    public int HashLog2 { get; set; }
    public int ChainLog2 { get; set; }
}
```
## Export Keying Material for TLS sessions

**Approved** | [#runtime/112529](https://github.com/dotnet/runtime/issues/112529#issuecomment-5895556434) | [Video](https://www.youtube.com/watch?v=1-BNpNFSCxo&t=0h22m30s)

* `destination` is the normal name of an output span.
* Let's go with the `ReadOnlySpan<byte>` input, since the spec isn't entirely clear on what text transforms/restrictions apply.  (7-bit ASCII, 8-bit ASCII, UTF-8, etc)
* We didn't find a good precedent example for whether the longer overload should be (label, context, destination) or (label, destination, context), or even (destination, label, context).  If anyone finds a reason why (label, context, destination) is wrong, please speak up.

```csharp
namespace System.Net.Security;

public partial class SslStream {
    public void ExportKeyingMaterial(ReadOnlySpan<byte> label, Span<byte> destination);

    public void ExportKeyingMaterial(ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, Span<byte> destination);
}
```
## SmtpClient.SslOptions

**Approved** | [#runtime/120965](https://github.com/dotnet/runtime/issues/120965#issuecomment-5895862087) | [Video](https://www.youtube.com/watch?v=1-BNpNFSCxo&t=0h44m28s)


* It doesn't seem like obsoleting the existing property provides a lot of value, there's no impact on existing callers.
  * We couldn't strongly decide on EB-Never either

```csharp
namespace System.Net.Mail;

public partial class SmtpClient
{
    public SslClientAuthenticationOptions SslOptions { get; set; }

    // existing member defers to new one
    //public X509CertificatesCollection ClientCertificates { get; } // => SslOptions.ClientCertificates
}
```
## string.GetHashCodeNonRandomized

**Approved** | [#runtime/77679](https://github.com/dotnet/runtime/issues/77679#issuecomment-5896386361) | [Video](https://www.youtube.com/watch?v=1-BNpNFSCxo&t=1h5m12s)

* "NonRandomized" doesn't _say_ "stable", but almost _implies_ it.  The best alternative we came up with was "Weak".
  * To help avoid the notion that it's stable, let's give it a per-process random seed still, but using a simple operation like XOR.
* The name "Weak" doesn't pair well with the StringComparer objects, so we'll leave those out and just have the primitive.
* string seems the best location for these, given the static string.GetHashCode(ROSpan) APIs

```csharp
namespace System
{
    public sealed class String
    {
        public static int GetHashCodeWeak(ReadOnlySpan<char> value);

        // Only Ordinal and OrdinalIgnoreCase in v1, we could all others later.
        public static int GetHashCodeWeak(ReadOnlySpan<char> value, StringComparison comparisonType);
    }
}
```
## Let accessing the instance of Exception (if any) on stack.

**Approved** | [#runtime/89724](https://github.com/dotnet/runtime/issues/89724#issuecomment-5896583086) | [Video](https://www.youtube.com/watch?v=1-BNpNFSCxo&t=1h37m59s)

* A property feels like it implies stability, let's use a Get method to show it's a point-in-time answer.

```csharp
namespace System.Runtime.ExceptionServices
{
    public static class ExceptionHandling
    {
        public static Exception? GetCurrentException();
    }
}
```
