# API Review 07/29/2025

## Detect legacy (w/o Durable IDs) logging APIs without using logger message generator

**Rejected** | [#runtime/117999](https://github.com/dotnet/runtime/issues/117999#issuecomment-3133437897) | [Video](https://www.youtube.com/watch?v=80u8ZNRCw-s&t=0h0m0s)

This is already covered by the existing analyzer CA1848.  The recommended fix for that analyzer may need to be updated, or we may need to register a second fixer.

Either way, such an analyzer already exists, we should not create a new one.

See https://github.com/dotnet/roslyn-analyzers/blob/d45048f958379c63acfc716136a5e2466f21c5d4/src/NetAnalyzers/Core/Microsoft.NetCore.Analyzers/Runtime/LoggerMessageDefineAnalyzer.cs
## SLH-DSA: Rename Secret Key to Private Key

**Approved** | [#runtime/117958](https://github.com/dotnet/runtime/issues/117958#issuecomment-3133444627) | [Video](https://www.youtube.com/watch?v=80u8ZNRCw-s&t=0h30m0s)

Updates look good as proposed.

```diff
namespace System.Security.Cryptogrpahy
{
    public abstract class SlhDsa : IDisposable
    {
-       public static SlhDsa ImportSlhDsaSecretKey(SlhDsaAlgorithm algorithm, byte[] source);
+       public static SlhDsa ImportSlhDsaPrivateKey(SlhDsaAlgorithm algorithm, byte[] source);
-       public static SlhDsa ImportSlhDsaSecretKey(SlhDsaAlgorithm algorithm, ReadOnlySpan<byte> source);
+       public static SlhDsa ImportSlhDsaPrivateKey(SlhDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

-       public byte[] ExportSlhDsaSecretKey();
+       public byte[] ExportSlhDsaPrivateKey();
-       public void ExportSlhDsaSecretKey(Span<byte> destination);
+       public void ExportSlhDsaPrivateKey(Span<byte> destination);
-       protected abstract void ExportSlhDsaSecretKeyCore(Span<byte> destination);
+       protected abstract void ExportSlhDsaPrivateKeyCore(Span<byte> destination);
    }

    public sealed class SlhDsaAlgorithm
    {
-       public int SecretKeySizeInBytes { get; }
+       public int PrivateKeySizeInBytes { get; }
}
```
## Add External-Mu support to ML-DSA

**Approved** | [#runtime/116995](https://github.com/dotnet/runtime/issues/116995#issuecomment-3133530796) | [Video](https://www.youtube.com/watch?v=80u8ZNRCw-s&t=0h32m45s)

* SignExternalMu(mu) -> Mu(externalMu)
* For the part 2 segment, we should defer until we have better, more clearly understood, scenarios.

```C#
namespace System.Security.Cryptography
{
    public abstract partial class MLDsa
    {
        public byte[] SignMu(byte[] externalMu);
        public byte[] SignMu(ReadOnlySpan<byte> externalMu);
        public void SignMu(ReadOnlySpan<byte> externalMu, Span<byte> destination);
        protected abstract void SignMuCore(ReadOnlySpan<byte> externalMu, Span<byte> destination);
    
        public bool VerifyMu(byte[] externalMu, byte[] signature);
        public bool VerifyMu(ReadOnlySpan<byte> externalMu, ReadOnlySpan<byte> signature);
        protected abstract bool VerifyMuCore(ReadOnlySpan<byte> externalMu, ReadOnlySpan<byte> signature);
    }
    public partial class MLDsaAlgorithm
    {
        public int MuSizeInBytes { get; }
    }
}
```
## Remove IEquatable<T>? constraint on ArgumentOutOfRangeException ThrowIfEqual/ThrowIfNotEqual

**Approved** | [#runtime/118053](https://github.com/dotnet/runtime/issues/118053#issuecomment-3133547354) | [Video](https://www.youtube.com/watch?v=80u8ZNRCw-s&t=1h2m27s)

API Review: If just dropping the constraints is valid, then we're OK with it.  But it needs to be vetted by someone like @tannergooding to know if it's a bad idea or not.
