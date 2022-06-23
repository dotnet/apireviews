# API Review 06/23/2022

## DispatchProxy.Create<TProxy>(Type interfaceType)

**Approved** | [#runtime/67444](https://github.com/dotnet/runtime/issues/67444#issuecomment-1164676155) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=0h0m0s)

* Makes sense
    - Let's skip the second one as it seems unfortunate to reverse the order if interface type and proxy type.

```C#
namespace System.Reflection;

public class DispatchProxy
{
    // Existing
    // public static T Create<T, TProxy>() where TProxy : DispatchProxy;
    [RequiresDynamicCode("The native code for the method requested might not be available at runtime.")]
    public static object Create(Type interfaceType, Type proxyType);
}
```
## Expose method to get system message for system error code

**Approved** | [#runtime/67872](https://github.com/dotnet/runtime/issues/67872#issuecomment-1164688141) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=0h13m15s)

* Let's also add `GetLastPInvokeErrorMessage()`

```C#
namespace System.Runtime.InteropServices;

public partial class Marshal
{
    // Existing
    // public static int GetLastPInvokeError();
    public static string GetLastPInvokeErrorMessage();
    public static string GetPInvokeErrorMessage(int error);
}
```
## When stopping an IHost instance, IHostedService instances StopAsync method is called sequentially instead of asynchronously

**Approved** | [#runtime/68036](https://github.com/dotnet/runtime/issues/68036#issuecomment-1164708145) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=0h26m39s)

* We'd prefer `Concurrent` over `Asynchronous`.
* It seems start is currently also sequential
    - Would we want to configure this as well?
    - Does this imply that this can breaking because people have interdependencies between services?

```C#
namespace Microsoft.Extensions.Hosting;

public partial class HostOptions
{
    public bool ServicesStartConcurrently { get; set; } // = false; Maybe change in 8?
    public bool ServicesStopConcurrently { get; set; } // = false; Maybe change in 8?
}
```
## Analyzer: Detect the problem of using a System.Type overload instead of the generic overload

**Approved** | [#runtime/68243](https://github.com/dotnet/runtime/issues/68243#issuecomment-1164716513) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=0h48m47s)

Looks good as proposed, that is handling any method, not just `Marshal.SizeOf`. One thing we need to do is honor the generic constraints so we don't suggest code that wouldn't compile. One way might be to do a speculative binding of the new expression and see whether it produces any diagnostics.

Category: Usage
Severity: Info
## Convert argument null checks to ArgumentNullException.ThrowIfNull

**Approved** | [#runtime/68326](https://github.com/dotnet/runtime/issues/68326#issuecomment-1164719249) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=0h57m54s)

Make sense as proposed

We feel like this would also approve the other throw helpers:
- `ObjectDisposedException.ThrowIf`
- `ArgumentException.ThrowIfNullOrEmpty`


## Additional Regex.EnumerateMatches/Count overloads

**Approved** | [#runtime/68959](https://github.com/dotnet/runtime/issues/68959#issuecomment-1164728140) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=1h0m35s)

* Makes sense

```C#
namespace System.Text.RegularExpressions;

public class Regex
{
    // Existing
    // public MatchCollection Matches(string input, int startat);
    // public Match Match(string input, int startat);
    // public bool IsMatch(string input, int startat);

    // Previously added in .NET 7
    // public int Count(ReadOnlySpan<char> input);
    // public ValueMatchEnumerator EnumerateMatches(ReadOnlySpan<char> input);

    public int Count(ReadOnlySpan<char> input, int startat);
    public ValueMatchEnumerator EnumerateMatches(ReadOnlySpan<char> input, int startat);
}
```
## Simplify calls to string.Substring and Memory/Span.Slice

**Approved** | [#runtime/68946](https://github.com/dotnet/runtime/issues/68946#issuecomment-1164731844) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=1h10m0s)

* Looks good as proposed

Category: Maintainability
Severity: Info
## Expose `IsSupported` properties on the vector types

**Approved** | [#runtime/69036](https://github.com/dotnet/runtime/issues/69036#issuecomment-1164734997) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=1h14m5s)

* Looks good as proposed

```C#
namespace System.Numerics;

public partial struct Vector<T>
{
    public static bool IsSupported { get; }
}
```

```C#
namespace System.Runtime.Intrinsics;

public partial struct Vector64<T>
{
    public static bool IsSupported { get; }
}

public partial struct Vector128<T>
{
    public static bool IsSupported { get; }
}

public partial struct Vector256<T>
{
    public static bool IsSupported { get; }
}
```
## [Analyzer]: Unexpected allocation when converting arrays to spans

**Approved** | [#runtime/69577](https://github.com/dotnet/runtime/issues/69577) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=1h17m44s)

## `System.Drawing.Imaging.ImageFormat.Heif` and `Webp`.

**Approved** | [#runtime/70422](https://github.com/dotnet/runtime/issues/70422#issuecomment-1164752413) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=1h33m28s)

* Looks good as proposed

```C#
namespace System.Drawing.Imaging;

public class ImageFormat
{
    [SupportedOSPlatform("windows10.0.17763.0")]
    public static ImageFormat Heif { get; }
    [SupportedOSPlatform("windows10.0.17763.0")]
    public static ImageFormat Webp { get; }
}
```
## Obsolete crypto factory methods accepting a string

**Approved** | [#runtime/70436](https://github.com/dotnet/runtime/issues/70436#issuecomment-1164761021) | [Video](https://www.youtube.com/watch?v=72daIfgD-3E&t=1h38m18s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography;

// CryptoStringFactoryMessage = "Cryptographic factory methods accepting an algorithm name is obsolete. Use the parameterless Create factory method on the algorithm type instead.";
// CryptoStringFactoryDiagId = "SYSLIB9999";

partial class Aes {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static Aes Create(string algorithmName);
}

partial class TripleDES {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static TripleDES Create(string algorithmName);
}

partial class DES {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static DES Create(string algName);
}

partial class RC2 {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static RC2 Create(string algorithmName);
}

partial class Rijndael {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static Rijndael Create(string algorithmName);
}

partial class SymmetricAlgorithm {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static SymmetricAlgorithm Create(string algName);
}

partial class AsymmetricAlgorithm {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static AsymmetricAlgorithm Create(string algName);
}

partial class DSA {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static DSA Create(string algName);
}

partial class RSA {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static RSA Create(string algName);
}

partial class ECDiffieHellman {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static ECDiffieHellman Create(string algorithm);
}

partial class ECDsa {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static ECDsa Create(string algorithm);
}

partial class HashAlgorithm {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static HashAlgorithm Create(string algorithm);
}

partial class HMAC {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static HMAC Create(string algorithmName);
}

partial class KeyedHashAlgorithm {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static KeyedHashAlgorithm Create(string algorithmName);
}

partial class MD5 {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static MD5 Create(string algorithmName);
}

partial class SHA1 {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static SHA1 Create(string algorithmName);
}

partial class SHA256 {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static SHA256 Create(string algorithmName);
}

partial class SHA384 {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static SHA384 Create(string algorithmName);
}

partial class SHA512 {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static SHA512 Create(string algorithmName);
}

partial class RandomNumberGenerator {
    [Obsolete(Obsoletions.CryptoStringFactoryMessage, DiagnosticId = Obsoletions.CryptoStringFactoryDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
    public static RandomNumberGenerator Create(string rngName);
}
```
