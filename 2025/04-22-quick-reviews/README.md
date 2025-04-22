# API Review 04/22/2025

## FromServiceKeyAttribute(object) to support nullable (for unkeyed) and to use current service key

**Approved** | [#runtime/113585](https://github.com/dotnet/runtime/issues/113585#issuecomment-2822006787) | [Video](https://www.youtube.com/watch?v=NaAHqUa6W1A&t=0h0m0s)


* The nullable annotation changes make sense as proposed
* Adding a default constructor for "inherit current key"
  * The sentinel driving that might become public API later

```diff
namespace Microsoft.Extensions.DependencyInjection
{
    [AttributeUsage(AttributeTargets.Parameter)]
    public sealed class FromServiceKeyAttribute : Attribute
    {
-    public FromKeyedServicesAttribute(object key);
+    public FromKeyedServicesAttribute(object? key);

+   public FromKeyedServicesAttribute();

-   public object Key { get; }
+   public object? Key { get; }
    }
}
```
## resolve keyed service using the same key as current service

**Rejected** | [#runtime/99084](https://github.com/dotnet/runtime/issues/99084#issuecomment-2822008280) | [Video](https://www.youtube.com/watch?v=NaAHqUa6W1A&t=0h22m58s)

Closed in favor of https://github.com/dotnet/runtime/issues/113585
## Public API for the Runtime Async

**Approved** | [#runtime/114310](https://github.com/dotnet/runtime/issues/114310#issuecomment-2822093178) | [Video](https://www.youtube.com/watch?v=NaAHqUa6W1A&t=0h23m24s)

* Nothing in this new type should ever be called by anyone other than the compiler (and doing so may cause errors like InvalidProgramException), so the whole type should be EB-Never
* The "Helpers" suffix isn't great, but CompilerServices already has some. And we already marked it as EB-Never
* AwaitAwaiterFromRuntimeAsync => AwaitAwaiter

```c#
namespace System.Runtime.CompilerServices
{
    [Flags]
    public partial enum MethodImplOptions
    {
        Async = 0x2000,
    }
}

namespace System.Reflection
{
    public partial enum MethodImplAttributes
    {
        Async = 0x2000,
    }
}

namespace System.Runtime.CompilerServices
{
    [System.ComponentModel.EditorBrowsable(System.ComponentModel.EditorBrowsableState.Never)]
    [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5007", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
    public static partial class AsyncHelpers
    {
        public static void UnsafeAwaitAwaiter<TAwaiter>(TAwaiter awaiter) where TAwaiter : ICriticalNotifyCompletion { }
        public static void AwaitAwaiter<TAwaiter>(TAwaiter awaiter) where TAwaiter : INotifyCompletion { }

        public static void Await(System.Threading.Tasks.Task task) { }
        public static T Await<T>(System.Threading.Tasks.Task<T> task) { }
        public static void Await(System.Threading.Tasks.ValueTask task) { }
        public static T Await<T>(System.Threading.Tasks.ValueTask<T> task) { }
        public static void Await(System.Runtime.CompilerServices.ConfiguredTaskAwaitable configuredAwaitable) { }
        public static T Await<T>(System.Runtime.CompilerServices.ConfiguredTaskAwaitable<T> configuredAwaitable) { }
        public static void Await(System.Runtime.CompilerServices.ConfiguredValueTaskAwaitable configuredAwaitable) { }
        public static T Await<T>(System.Runtime.CompilerServices.ConfiguredValueTaskAwaitable<T> configuredAwaitable) { }
    }
}
```
## MLKem

**Approved** | [#runtime/114453](https://github.com/dotnet/runtime/issues/114453#issuecomment-2822224158) | [Video](https://www.youtube.com/watch?v=NaAHqUa6W1A&t=1h1m42s)

* `Decapsulate(ReadOnlySpan<byte> ciphertext, Span<byte> sharedSecret, out int sharedSecretBytesWritten);` seems a little weird and unnecessary.  If there's demand for it we can add it in the future.
* By analogy, we cut the out-writing ones for Encapsulate
* Encapsulate got reshaped to simplify the outs vs returns vs span-writing
* ThrowIfDisposed seemes unnecessary
* ImportFromEncryptedPem is password second but ImportEncryptedPkcs8PrivateKey is password first.  Since that is how we did it in AsymmetricAlgorithm, we should preserve the inconsistency here.
* X509Certificate2 has ImportFromPemFile.  AsymmetricAlgorithm does not.  We will not have it here, either, but can add it in the future if there's demonstrated need.
* X509Certificate2.GetMLKemPublicKey and friends were missing their nullable return marker.  They were added.
* X509CertificateKeyAccessors.CopyWithPrivateKey and X509Certificate2.CopyWithPrivateKey disagreed on the parameter name of the private key.  `privateKey` is the one consistent with the other algorithms.

```C#
namespace System.Security.Cryptography
{
    // Present in both System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [ExperimentalAttribute("SYSLIB5006")]
    public abstract partial class MLKem : IDisposable
    {
        protected MLKem(MLKemAlgorithm algorithm);

        public static bool IsSupported { get; }

        public MLKemAlgorithm Algorithm { get; }

        public byte[] Decapsulate(byte[] ciphertext);
        public void Decapsulate(ReadOnlySpan<byte> ciphertext, Span<byte> sharedSecret);
        protected abstract void DecapsulateCore(ReadOnlySpan<byte> ciphertext, Span<byte> sharedSecret);

        public void Encapsulate(out byte[] ciphertext, out byte[] sharedSecret);
        public void Encapsulate(Span<byte> ciphertext, Span<byte> sharedSecret);
        protected abstract void EncapsulateCore(Span<byte> ciphertext, Span<byte> sharedSecret);

        public byte[] ExportDecapsulationKey();
        public void ExportDecapsulationKey(Span<byte> destination);
        protected abstract void ExportDecapsulationKeyCore(Span<byte> destination);

        public byte[] ExportEncapsulationKey();
        public void ExportEncapsulationKey(Span<byte> destination);
        protected abstract void ExportEncapsulationKeyCore(Span<byte> destination);

        public byte[] ExportPrivateSeed();
        public void ExportPrivateSeed(Span<byte> destination);
        protected abstract void ExportPrivateSeedCore(Span<byte> destination);

        public static MLKem GenerateKey(MLKemAlgorithm algorithm);

        public static MLKem ImportDecapsulationKey(MLKemAlgorithm algorithm, byte[] source);
        public static MLKem ImportDecapsulationKey(MLKemAlgorithm algorithm, ReadOnlySpan<byte> source);
        public static MLKem ImportEncapsulationKey(MLKemAlgorithm algorithm, byte[] source);
        public static MLKem ImportEncapsulationKey(MLKemAlgorithm algorithm, ReadOnlySpan<byte> source);

        public static MLKem ImportPrivateSeed(MLKemAlgorithm algorithm, byte[] source);
        public static MLKem ImportPrivateSeed(MLKemAlgorithm algorithm, ReadOnlySpan<byte> source);

        public void Dispose();
        protected virtual void Dispose(bool disposing);

#region SPKI / PKCS8

        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(string password, PbeParameters pbeParameters);

        public byte[] ExportPkcs8PrivateKey();
        public string ExportPkcs8PrivateKeyPem();
        public bool TryExportPkcs8PrivateKey(Span<byte> destination, out int bytesWritten);
        protected abstract bool TryExportPkcs8PrivateKeyCore(Span<byte> destination, out int bytesWritten);

        public byte[] ExportSubjectPublicKeyInfo();
        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
        public string ExportSubjectPublicKeyInfoPem();

        public static MLKem ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
        public static MLKem ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
        public static MLKem ImportEncryptedPkcs8PrivateKey(string password, ReadOnlySpan<byte> source);

        public static MLKem ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
        public static MLKem ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
        public static MLKem ImportFromEncryptedPem(string source, byte[] passwordBytes);
        public static MLKem ImportFromEncryptedPem(string source, string password);

        public static MLKem ImportFromPem(ReadOnlySpan<char> source);
        public static MLKem ImportFromPem(string source);

        public static MLKem ImportPkcs8PrivateKey(byte[] source);
        public static MLKem ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);

        public static MLKem ImportSubjectPublicKeyInfo(byte[] source);
        public static MLKem ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);
#endregion
    }

    // Present in both System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [ExperimentalAttribute("SYSLIB5006")]
    public sealed partial class MLKemAlgorithm : IEquatable<MLKemAlgorithm>
    {
        internal MLKemAlgorithm();

        public static MLKemAlgorithm MLKem512 { get; }
        public static MLKemAlgorithm MLKem768 { get; }
        public static MLKemAlgorithm MLKem1024 { get; }

        public string Name { get; }

        public int CiphertextSizeInBytes { get; }
        public int DecapsulationKeySizeInBytes { get; }
        public int EncapsulationKeySizeInBytes { get; }
        public int PrivateSeedSizeInBytes { get; }
        public int SharedSecretSizeInBytes { get; }

        public bool Equals(MLKemAlgorithm? other);

        public static bool operator ==(MLKemAlgorithm? left, MLKemAlgorithm? right);
        public static bool operator !=(MLKemAlgorithm? left, MLKemAlgorithm? right);
    }

    // Present in System.Security.Cryptography only
    [ExperimentalAttribute("SYSLIB5006")]
    public sealed partial class MLKemOpenSsl : MLKem
    {
        [UnsupportedOSPlatform("android")]
        [UnsupportedOSPlatform("browser")]
        [UnsupportedOSPlatform("ios")]
        [UnsupportedOSPlatform("osx")]
        [UnsupportedOSPlatform("tvos")]
        [UnsupportedOSPlatform("windows")]
        public MLKemOpenSsl(SafeEvpPKeyHandle pkeyHandle);

        public SafeEvpPKeyHandle DuplicateKeyHandle();
    }
}

namespace System.Security.Cryptography.X509Certificates
{
    // Existing class
    // Present in System.Security.Cryptography only
    public sealed partial class PublicKey
    {
        [Experimental("SYSLIB5006")]
        public PublicKey(MLKem key);

        [Experimental("SYSLIB5006")]
        [UnsupportedOSPlatform("browser")]
        public MLKem? GetMLKemPublicKey();
    }

    // Existing class
    // Present in System.Security.Cryptography only
    public partial class X509Certificate2
    {
        [ExperimentalAttribute("SYSLIB5006")]
        public MLKem? GetMLKemPublicKey();

        [ExperimentalAttribute("SYSLIB5006")]
        public MLKem? GetMLKemPrivateKey();

        [ExperimentalAttribute("SYSLIB5006")]
        public X509Certificate2 CopyWithPrivateKey(MLKem privateKey);
    }

    // New class (will also be used for future algorithms)
    // Present in Microsoft.Bcl.Cryptography only
    public static class X509CertificateKeyAccessors
    {
        [ExperimentalAttribute("SYSLIB5006")]
        public static MLKem? GetMLKemPublicKey(this X509Certificate2 certificate);

        [ExperimentalAttribute("SYSLIB5006")]
        public static MLKem? GetMLKemPrivateKey(this X509Certificate2 certificate);

        [ExperimentalAttribute("SYSLIB5006")]
        public static X509Certificate2 CopyWithPrivateKey(this X509Certificate2 certificate, MLKem privateKey);
    }
}
```
