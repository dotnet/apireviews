# API Review 07/13/2021

## [MDI] Add possibility to align minimized child to default location (TOP LEFT)

**Approved** | [#winforms/4720](https://github.com/dotnet/winforms/issues/4720#issuecomment-879265522)

* It seems beneficial to indicate the Windows Forms behavior instead of describing the Win32 default. This way, should Windows change the behavior we don't have an incorrect description. Hence, the property should flip the default value and describe what Windows Forms does
* Should be in the same property grid category as `MdiChildren`


```C#
namespace System.Windows.Forms
{
    public partial class Form
    {
        [SRCategory(nameof(SR.CatWindowStyle))]
        [DefaultValue(true)]
        [SRDescription(nameof(SR.FormMdiChildrenMinimizedAnchorTopDescr))]
        public bool MdiChildrenMinimizedAnchorBottom { get; set; }
    }
}
```
## API that returns framework version

**Approved** | [#runtime/55522](https://github.com/dotnet/runtime/issues/55522#issuecomment-879274336)

* We concluded we don't need a new API, we'll just use `Environment.Version` which exposes the platform version
* WinForms/WPF can decide to support an `AppContext` switch so that both that if the new string returned via the UIA's `FrameworkId` property breaks people, they can get back the old behavior.
## Unify RequiresUnreferencedCode and RequiresAssemblyFiles attributes

**Approved** | [#runtime/55274](https://github.com/dotnet/runtime/issues/55274#issuecomment-879282507)

* We're happy with the default message most of the time, so we shouldn't remove the parameterless constructor.


```C#
namespace System.Diagnostics.CodeAnalysis
{
    public sealed class RequiresAssemblyFilesAttribute : Attribute
    {
        public RequiresAssemblyFilesAttribute();
        public RequiresAssemblyFilesAttribute(string message);
        public string Message { get; }
        public string Url { get; set; }
    }
}
```
## Allow specifying a context propagator on SocketsHttpHandler

**Approved** | [#runtime/55556](https://github.com/dotnet/runtime/issues/55556#issuecomment-879287421)

* Looks good as proposed


```C#
namespace System.Net.Http
{
    public sealed class SocketsHttpHandler
    {
        public DistributedContextPropagator? ActivityHeadersPropagator { get; set; }
    }
}
```
## EventWaitHandle can be named, but has no Name property

**Approved** | [#runtime/43997](https://github.com/dotnet/runtime/issues/43997#issuecomment-879290343)

* Looks good as proposed
    - But do we need a protected constructor that takes the name, will we use an internal setter, or should we make the property virtual? Any of these are fine with us.

```C#
namespace System.Threading
{
    public partial class WaitHandle
    {
        public string? Name { get; }
    }
}
```
## Specify SizeConst when marshalling field as ByValArray

**Approved** | [#runtime/36134](https://github.com/dotnet/runtime/issues/36134#issuecomment-879294790)

* The analyzer should probably also handle `UnmanagedType.ByValTStr`
    - Should be the same diagnostic ID with a slightly different message
* Assuming code like this wouldn't work at runtime, turning the rule on by default seems fine
    - If code like this works today, then on by default might be noisy
## Introduce static methods to allocate and throw key exception types.

**Approved** | [#runtime/48573](https://github.com/dotnet/runtime/issues/48573#issuecomment-879305700)

* Let's scope this to just `ArgumentNullException`
* This API might be less useful with C#' upcoming parameter `null`-validation feature
    - Still useful for other languages or potentially useful for letting the compiler use a centralized method
    - Also useful in cases the compiler injected validation wouldn't work, e.g. due to ordering
* We can do it for .NET 6 'cause it's cheap
* We might be able to mark this API as "doesn't change between versions" so that there is no penalty for ready-to-run

```C#
namespace System
{
    public partial class ArgumentNullException
    {
        public static void ThrowIfNull([NotNull] object? argument,
                                       [CallerArgumentExpression("argument")] string? paramName = null);
    }
}
```
## Obsolete CryptoConfig.EncodeOID

**Approved** | [#runtime/55579](https://github.com/dotnet/runtime/issues/55579#issuecomment-879308889)

* Looks good as proposed
* Just dispense another diagnostic ID :-)

```C#
namespace System.Security.Cryptography
{
    public partial class CryptoConfig
    {
        [Obsolete("EncodeOID is obsolete. Use the ASN.1 functionality provided in System.Formats.Asn1.")]
        public static byte[] EncodeOID(string str);
    }
}
```
## APIs for exporting certificates and keys to PEM

**Approved** | [#runtime/51630](https://github.com/dotnet/runtime/issues/51630#issuecomment-879312802)

* Looks good as proposed

```C#
namespace System.Security.Cryptography
{
    public partial class AsymmetricAlgorithm
    {
        public string ExportPkcs8PrivateKeyPem();
        public bool TryExportPkcs8PrivateKeyPem(Span<char> buffer, out int charsWritten);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public bool TryExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<char> destination, out int charsWritten);
        public string ExportSubjectPublicKeyInfoPem();
        public bool TryExportSubjectPublicKeyInfoPem(Span<char> destination, out int charsWritten);
    }
    public partial class RSA
    {
        public string ExportRSAPrivateKeyPem();
        public string ExportRSAPublicKeyPem();
        public bool TryExportRSAPrivateKeyPem(Span<char> destination, out int charsWritten);
        public bool TryExportRSAPublicKeyPem(Span<char> destination, out int charsWritten);
    }
    public partial class ECDsa
    {
        public string ExportECPrivateKeyPem();
        public bool TryExportECPrivateKeyPem(Span<char> destination, out int charsWritten);
    }
    public partial class ECDiffieHellman
    {
        public string ExportECPrivateKeyPem();
        public bool TryExportECPrivateKeyPem(Span<char> destination, out int charsWritten);
    }
}
namespace System.Security.Cryptography.X509Certificates
{
    public partial class X509Certificate2
    {
        public string ExportCertificatePem();
        public bool TryExportCertificatePem(Span<char> buffer, out int charsWritten);
    }
    public partial class X509Certificate2Collection
    {
        public string ExportCertificatePems();
        public bool TryExportCertificatePems(Span<char> buffer, out int charsWritten);
        public string ExportPkcs7Pem();
        public bool TryExportPkcs7Pem(Span<char> buffer, out int charsWritten);
    }
}
```
## Add new ObjectDisposedException constructor overload

**Approved** | [#runtime/51700](https://github.com/dotnet/runtime/issues/51700#issuecomment-879318401)

* Let's also add an overload that takes `Type` directly

```C#
namespace System
{
    public partial class ObjectDisposedException
    {
        public ObjectDisposedException(object instance);
        public ObjectDisposedException(Type type);
    }
}
```
## Add Dispose/IDisposable to GCHandle

**Approved** | [#runtime/54792](https://github.com/dotnet/runtime/issues/54792#issuecomment-879319758)

* Looks good as proposed

```C#
namespace System.Runtime.InteropServices
{
    public struct GCHandle : IDisposable
    {
        public void Dispose();
    }
}
```
## Add IReadOnlyList<T> constructor to ReadOnlyCollection<T>

**Rejected** | [#runtime/19089](https://github.com/dotnet/runtime/issues/19089#issuecomment-879333755)

* This API has the issue that it would be ambiguous for common types, such as `List<T>`
* This would also force the implementation to grow by a field or having to type cast in every operation between `IList<T>` and `IReadOnlyList<T>`
* Instead of this, we probably should expose an API that allows adapting an `IList<T>` to an `IReadOnlyList<T>` by casting or wrapping (and vice versa)
     - @terrajobst will make separate API proposal for this.
* Hence we wouldn't do this API
