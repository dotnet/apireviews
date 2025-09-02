# API Review 09/02/2025

## Add attribute that instructs Reflection to hide target member

**Approved** | [#runtime/118903](https://github.com/dotnet/runtime/issues/118903#issuecomment-3246164534) | [Video](https://www.youtube.com/watch?v=4DeB1qSvdfA&t=0h0m0s)


* Looks good as proposed.
* Since we don't think a general "hide this from reflection" attribute is a good idea, we are hoping that the compilers all reject this when specified by source code.

```c#
namespace System.Runtime.CompilerServices;

[AttributeUsage(AttributeTargets.All, AllowMultiple = false, Inherited = false)]
public sealed class MetadataUpdateDeletedAttribute : Attribute
{
}
```
## Kubernetes specialized Resource Monitoring

**NeedsWork** | [#extensions/6496](https://github.com/dotnet/extensions/issues/6496#issuecomment-3246215213) | [Video](https://www.youtube.com/watch?v=4DeB1qSvdfA&t=0h10m10s)

This looks like it's building with the wrong layering notion.  Specifically:

```csharp

public class ResourceMonitoringOptions
{
    (...)
    public bool UseKubernetesLimitsAndRequests { get; set; }
}
```

doesn't feel properly scalable for Kubernetes vs other container-orchestrators vs other compute hosting models, etc (and would raise questions as to what happens when multiple of them are `true`).

It seems like what's needed at this layer is a way to inject some sort of control or configuration that comes from a provider-specific package, rather than a knob that uses a provider name.


## create hardlinks

**Approved** | [#runtime/69030](https://github.com/dotnet/runtime/issues/69030#issuecomment-3246252775) | [Video](https://www.youtube.com/watch?v=4DeB1qSvdfA&t=0h25m22s)

* Looks good as proposed
* In case any OS supports hard-links to a directory, File.CreateHardLink should use the same validation that File.CreateSymbolicLink does (which might be "none").
* We can put CreateAsHardLink on FileInfo for now, and if we find any reason to move it down to FilSystemInfo later, we can.

```csharp
namespace System.IO;

public static partial class File
{
    public static FileInfo CreateHardLink(string path, string pathToTarget);
}

public partial class FileInfo
{
    public void CreateAsHardLink(string pathToTarget);
}
```
## Obsolete RSACryptoServiceProvider.Decrypt,Encrypt with fOAEP

**Approved** | [#runtime/113616](https://github.com/dotnet/runtime/issues/113616#issuecomment-3246287033) | [Video](https://www.youtube.com/watch?v=4DeB1qSvdfA&t=0h39m47s)

* Looks good as proposed, use the next available diagnostic ID.
* Let's make sure the docs say a) these methods are bad, b) what to do as equivalent calls, and c) that those calls are themselves probably not good.

```diff
namespace System.Security.Cryptography;

public partial sealed class RSACryptoServiceProvider {
+   [Obsolete("Encrypt and Decrypt on RSACryptoServiceProvider that accept a boolean value for OAEP are obsolete. Use an overload that accepts RSAEncryptionPadding.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
    public byte[] Decrypt(byte[] rgb, bool fOAEP);
+   [Obsolete("Encrypt and Decrypt on RSACryptoServiceProvider that accept a boolean value for OAEP are obsolete. Use an overload that accepts RSAEncryptionPadding.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
    public byte[] Encrypt(byte[] rgb, bool fOAEP);
}
```
## Generic Interlocked.Or/And for enums and small numbers

**Approved** | [#runtime/114568](https://github.com/dotnet/runtime/issues/114568#issuecomment-3246303538) | [Video](https://www.youtube.com/watch?v=4DeB1qSvdfA&t=0h50m1s)

* Looks good as proposed

```csharp
namespace System.Threading;

public static class Interlocked
{
    // Existing naming:
    // public static long Or(ref long location1, long value); // and int, uint, ulong
    // public static T Exchange<T>(ref T location1, T value);

    public static T Or<T>(ref T location1, T value) where T : struct;
    public static T And<T>(ref T location1, T value) where T : struct;
}
```
