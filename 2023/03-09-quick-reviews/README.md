# API Review 03/09/2023

## Enable Library-Level Preview Features

**Approved** | [#runtime/77869](https://github.com/dotnet/runtime/issues/77869#issuecomment-1462545293) | [Video](https://www.youtube.com/watch?v=-MBZGdnYPGA&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Diagnostics.CodeAnalysis;

[AttributeUsage(AttributeTargets.Assembly |
                AttributeTargets.Module |
                AttributeTargets.Class |
                AttributeTargets.Struct |
                AttributeTargets.Enum |
                AttributeTargets.Constructor |
                AttributeTargets.Method |
                AttributeTargets.Property |
                AttributeTargets.Field |
                AttributeTargets.Event |
                AttributeTargets.Interface |
                AttributeTargets.Delegate, Inherited = false)]
public sealed class ExperimentalAttribute : Attribute
{
    public ExperimentalAttribute(string diagnosticId);
    public string DiagnosticId { get; }
    public string? UrlFormat { get; set; }
}
```
## JsonSerializerOptions should provide explicit control over chained IJsonTypeInfoResolvers

**Approved** | [#runtime/83095](https://github.com/dotnet/runtime/issues/83095#issuecomment-1462577506) | [Video](https://www.youtube.com/watch?v=-MBZGdnYPGA&t=0h8m43s)

* Should be `get`-only
* Should we rename it to just `TypeInfoResolverChain`?

```C#
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public IList<IJsonTypeInfoResolver> TypeInfoResolverChain { get; }
}
```
## GC.KeepAlive doesn't keep local delegate alive in mono (Xamarin.Android)

**Approved** | [#runtime/74966](https://github.com/dotnet/runtime/issues/74966#issuecomment-1462616973) | [Video](https://www.youtube.com/watch?v=-MBZGdnYPGA&t=0h36m46s)

* Looks good as proposed
    - There is some objection to using `GCHandle` as the recommended mitigation over rooted the delegate in a static, but that feels orthogonal to the analyzer.
    - We think there is a high chance this code pattern indicates as bug, justifying it be a warning
    - When a reverse P/Invoke is called, is the delegate rooted while its executed? If it doesn't, is a problem?

**Category**: Reliability
**Severity**: Warning

## [Analyzer/fixer suggestion]: Exceptions thrown inside async methods should be wrapped by Task.FromException

**Approved** | [#runtime/70905](https://github.com/dotnet/runtime/issues/70905#issuecomment-1462664035) | [Video](https://www.youtube.com/watch?v=-MBZGdnYPGA&t=1h2m11s)

* Seems reasonable. We should have a list of exceptions for which we don't warn (exceptions derived from `ArgumentException` for instance).
* Should not apply to methods marked `async`

**Category**: Reliability
**Severity**: Warning
## Vector128.ShuffleUnsafe

**Approved** | [#runtime/81609](https://github.com/dotnet/runtime/issues/81609#issuecomment-1462683236) | [Video](https://www.youtube.com/watch?v=-MBZGdnYPGA&t=1h32m15s)

* Looks good as proposed
* Overloads for other T's are approved by this, but we don't need them yet.

```C#
namespace System.Runtime.Intrinsics;

public partial class Vector64
{
    // Existing
    // public static Vector64<byte> Shuffle(Vector64<byte> vector, Vector64<byte> indices);

    public static Vector64<byte> ShuffleUnsafe(Vector64<byte> vector, Vector64<byte> indices);
}

public partial class Vector128
{
    // Existing
    // public static Vector128<byte> Shuffle(Vector128<byte> vector, Vector128<byte> indices);

    public static Vector128<byte> ShuffleUnsafe(Vector128<byte> vector, Vector128<byte> indices);
}

public partial class Vector256
{
    // Existing
    // public static Vector256<byte> Shuffle(Vector256<byte> vector, Vector256<byte> indices);

    public static Vector256<byte> ShuffleUnsafe(Vector256<byte> vector, Vector256<byte> indices);
}

public partial class Vector512
{
    // Existing
    // public static Vector512<byte> Shuffle(Vector512<byte> vector, Vector512<byte> indices);

    public static Vector512<byte> ShuffleUnsafe(Vector512<byte> vector, Vector512<byte> indices);
}
```
## Add an easier way of opening named keys via OpenSSL

**Approved** | [#runtime/55356](https://github.com/dotnet/runtime/issues/55356#issuecomment-1462696374) | [Video](https://www.youtube.com/watch?v=-MBZGdnYPGA&t=1h49m5s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography;

public partial class SafeEvpPKeyHandle
{
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public static SafeEvpPKeyHandle OpenPrivateKeyFromEngine(string engineName, string keyId);

    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public static SafeEvpPKeyHandle OpenPublicKeyFromEngine(string engineName, string keyId);

    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public static SafeEvpPKeyHandle OpenKeyFromProvider(string providerName, string keyUri);
}
```
