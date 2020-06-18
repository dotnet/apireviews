# Quick Reviews 6/18/2020

## SslStream API improvements for enhanced use cases 

**Approved** | [#runtime/37933](https://github.com/dotnet/runtime/issues/37933#issuecomment-646228075) | [Video](https://www.youtube.com/watch?v=t-X09mGPvNM&t=0h0m0s)

* ServerOptionsSelectionCallback should use a context object instead of the hostname for future expansion
* Rename `sender` to `stream` in `ServerOptionsSelectionCallback`
* Why is `SslStream.HostName` `virtual`?
* Rename `HostName` to `TargetHostName`
* Rename `SslStreamCertificateContext.CreateForServer` to `SslStreamCertificateContext.Create` to unify with client usage (and drop the usage type check)
* Add an `offline` parameter to the SslStreamCertificateContext Create method
* Add a debugger proxy to show the target cert.

```C#
public readonly struct SslClientHelloInfo
{
    public string ServerName { get; }
    public SslProtocols SslProtocols { get; }
}

public delegate ValueTask<SslServerAuthenticationOptions> ServerOptionsSelectionCallback(SslStream stream, SslClientHelloInfo clientHelloInfo, object? state, CancellationToken cancellationToken);

partial class SslStream
{
    public string TargetHostName { get; }
    public Task AuthenticateAsServerAsync(
        ServerOptionsSelectionCallback optionCallback,
        object? state,
        CancellationToken cancellationToken = default);      
}

public sealed class SslStreamCertificateContext
{
    public static SslStreamCertificateContext Create(
        X509Certificate2 target,
        X509Certificate2Collection? additionalCertificates,
        bool offline = false);
}

partial class SslServerAuthenticationOptions
{
    SslStreamCertificateContext ServerCertificateContext  { get; set; };
}
```
## CompareAttribute.Validate method does not create a ValidationResult with MemberNames

**Needs Work** | [#runtime/29214](https://github.com/dotnet/runtime/issues/29214) | [Video](https://www.youtube.com/watch?v=t-X09mGPvNM&t=1h12m54s)

## Add support for validating complex or collection properties using System.ComponentModel.DataAnnotations.Validator

**Needs Work** | [#runtime/31400](https://github.com/dotnet/runtime/issues/31400#issuecomment-646249648) | [Video](https://www.youtube.com/watch?v=t-X09mGPvNM&t=1h21m41s)

* The new attribute seems fine, but be careful in how it gets consumed so as to not surprise users who use the objects in multiple contexts (Blazor, EF6, etc).
* It seems like some new data belongs on ValidationContext, instead of new overloads to (Try)ValidateObject, around memberName-path representations and other handling of [ValidateComplexType].

## Add easy way to create a certificate from a multi-PEM or cert-PEM + key-PEM

**Approved** | [#runtime/31944](https://github.com/dotnet/runtime/issues/31944#issuecomment-646253846) | [Video](https://www.youtube.com/watch?v=t-X09mGPvNM&t=1h57m38s)

Approved without the byte-based password inputs.

```C#
namespace System.Security.Cryptography.X509Certificates {
    partial class X509Certificate2 {
        public static X509Certificate2 CreateFromPemFile(string certPemFilePath, string keyPemFilePath = default);
        public static X509Certificate2 CreateFromEncryptedPemFile(string certPemFilePath, ReadOnlySpan<char> password, string keyPemFilePath = default);

        public static X509Certificate2 CreateFromPem(ReadOnlySpan<char> certPem, ReadOnlySpan<char> keyPem);
        public static X509Certificate2 CreateFromEncryptedPem(ReadOnlySpan<char> certPem, ReadOnlySpan<char> keyPem, ReadOnlySpan<char> password);
    }

    partial class X509Certificate2Collection {
        public void ImportFromPemFile(string certPemFilePath);
        public void ImportFromPem(ReadOnlySpan<char> certPem);
    }
}
```
