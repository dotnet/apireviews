# API Review 04/15/2025

## [Security] Add ExcludeFromScreenCapture API to Forms/Controls

**Approved** | [#winforms/13258](https://github.com/dotnet/winforms/issues/13258#issuecomment-2806968610) | [Video](https://www.youtube.com/watch?v=tnziv6OtzZg&t=0h0m0s)

* Putting the property on Control seems to make sense, even though HideWindow is only valid on top level forms.
  * If research says this is a bad idea, moving it to Form is also fine.

```C#
namespace System.Windows.Forms;

public partial class Control
{
   public ScreenCaptureMode ScreenCaptureMode { get; set; }
}

public enum ScreenCaptureMode
{
    Allow = 0,
    HideContent = 1,
    HideWindow = 2,
}
```
## Expose API to return the Legacy propagator

**Approved** | [#runtime/114584](https://github.com/dotnet/runtime/issues/114584#issuecomment-2807030021) | [Video](https://www.youtube.com/watch?v=tnziv6OtzZg&t=0h26m12s)

* "Legacy" is a difficult name, since changing the default a second time makes "legacy" ambiguous
  * CreatePreW3CPropagator was the chosen name
* We should preemptively name the new default as CreateW3CPropagator();
  * CreateDefaultPropagator will defer to this... until the default changes again in the future.

```C#
namespace System.Diagnostics
{
    public abstract partial class DistributedContextPropagator
    {
        public static DistributedContextPropagator CreateW3CPropagator();
        public static DistributedContextPropagator CreatePreW3CPropagator();
    }
}
```
## Simplify Grid Column and Row Definitions

**Approved** | [#wpf/9802](https://github.com/dotnet/wpf/issues/9802#issuecomment-2807051651) | [Video](https://www.youtube.com/watch?v=tnziv6OtzZg&t=0h49m23s)

* Looks good as proposed

```diff
namespace System.Windows.Controls
{
    public partial class Grid
    {
+       [TypeConverter(typeof(ColumnDefinitionCollectionConverter))]
        [DesignerSerializationVisibility(DesignerSerializationVisibility.Content)]
        public ColumnDefinitionCollection ColumnDefinitions
        {
            get { ... }
+           set { ... }
        }

+       [TypeConverter(typeof(RowDefinitionCollectionConverter))]
        [DesignerSerializationVisibility(DesignerSerializationVisibility.Content)]
        public RowDefinitionCollection RowDefinitions
        {
            get { ... }
+           set { ... }
        }
    }
}
```
## Mark DynamicallyAccessedMemberTypes.All as EditorBrowsable(Never)

**Approved** | [#runtime/114197](https://github.com/dotnet/runtime/issues/114197#issuecomment-2807065025) | [Video](https://www.youtube.com/watch?v=tnziv6OtzZg&t=0h58m24s)

* Marking it as hidden from IntelliSense seems fine.
  * If it turns out to be a bad idea, we can unhide it later.
* But there should also be an analyzer or other diagnostic that complains at people who have already used it.

```C#
namespace System.Diagnostics.CodeAnalysis;

[Flags]
public enum DynamicallyAccessedMemberTypes
{
    [EditorBrowsable(EditorBrowsableState.Never)]
    All, // existing member
}
```
## Add minimal QUERY verb support to HttpClient

**Approved** | [#runtime/114489](https://github.com/dotnet/runtime/issues/114489#issuecomment-2807076031) | [Video](https://www.youtube.com/watch?v=tnziv6OtzZg&t=1h3m40s)

* Looks good as proposed

```C#
namespace System.Net.Http;

public partial class HttpMethod
{
    public static HttpMethod Query { get; }
}
```
## MLKem

**NeedsWork** | [#runtime/114453](https://github.com/dotnet/runtime/issues/114453#issuecomment-2807208384) | [Video](https://www.youtube.com/watch?v=tnziv6OtzZg&t=1h7m37s)


* `Decapsulate(ReadOnlySpan<byte> ciphertext, Span<byte> sharedSecret, out int sharedSecretBytesWritten);` seems a little weird and unnecessary.  If there's demand for it we can add it in the future.
* By analogy, we cut the out-writing ones for Encapsulate
* Encapsulate got reshaped to simplify the outs vs returns vs span-writing
* ThrowIfDisposed seemes unnecessary

We ran out of time after the instance methods on MLKem, we'll resume in a future session.

```C#
namespace System.Security.Cryptogrpahy
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

// Continue from here.
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
        public MLKem GetMLKemPublicKey();

        [ExperimentalAttribute("SYSLIB5006")]
        public MLKem GetMLKemPrivateKey();

        [ExperimentalAttribute("SYSLIB5006")]
        public X509Certificate2 CopyWithPrivateKey(MLKem privateKey);
    }

    // New class (will also be used for future algorithms)
    // Present in Microsoft.Bcl.Cryptography only
    public static class X509CertificateKeyAccessors
    {
        [ExperimentalAttribute("SYSLIB5006")]
        public static MLKem GetMLKemPublicKey(this X509Certificate2 certificate);

        [ExperimentalAttribute("SYSLIB5006")]
        public static MLKem GetMLKemPrivateKey(this X509Certificate2 certificate);

        [ExperimentalAttribute("SYSLIB5006")]
        public static X509Certificate2 CopyWithPrivateKey(this X509Certificate2 certificate, MLKem key);
    }
}
```
