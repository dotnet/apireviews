# API Review 01/28/2025

## Vector128.ShuffleNative

**Approved** | [#runtime/81609](https://github.com/dotnet/runtime/issues/81609#issuecomment-2619754460) | [Video](https://www.youtube.com/watch?v=ulYPW9T9L_0&t=0h0m0s)

* As we've already used the suffix "Native" to mean "do what the processor does" we're OK renaming ShuffleUnsafe to ShuffleNative

```c#
namespace System.Runtime.Intrinsics;

public static class Vector128
{
    public static Vector128<byte> ShuffleNative(Vector128<byte> vector, Vector128<byte> indices);
    public static Vector128<sbyte> ShuffleNative(Vector128<sbyte> vector, Vector128<sbyte> indices);
    public static Vector128<short> ShuffleNative(Vector128<short> vector, Vector128<short> indices);
    public static Vector128<ushort> ShuffleNative(Vector128<ushort> vector, Vector128<ushort> indices);
    public static Vector128<int> ShuffleNative(Vector128<int> vector, Vector128<int> indices);
    public static Vector128<uint> ShuffleNative(Vector128<uint> vector, Vector128<uint> indices);
    public static Vector128<long> ShuffleNative(Vector128<int> vector, Vector128<long> indices);
    public static Vector128<ulong> ShuffleNative(Vector128<uint> vector, Vector128<ulong> indices);
    public static Vector128<float> ShuffleNative(Vector128<float> vector, Vector128<int> indices);
    public static Vector128<double> ShuffleNative(Vector128<double> vector, Vector128<ulong> indices);
}

public static class Vector256
{
    public static Vector256<byte> ShuffleNative(Vector256<byte> vector, Vector256<byte> indices);
    public static Vector256<sbyte> ShuffleNative(Vector256<sbyte> vector, Vector256<sbyte> indices);
    public static Vector256<short> ShuffleNative(Vector256<short> vector, Vector256<short> indices);
    public static Vector256<ushort> ShuffleNative(Vector256<ushort> vector, Vector256<ushort> indices);
    public static Vector256<int> ShuffleNative(Vector256<int> vector, Vector256<int> indices);
    public static Vector256<uint> ShuffleNative(Vector256<uint> vector, Vector256<uint> indices);
    public static Vector256<long> ShuffleNative(Vector256<int> vector, Vector256<long> indices);
    public static Vector256<ulong> ShuffleNative(Vector256<uint> vector, Vector256<ulong> indices);
    public static Vector256<float> ShuffleNative(Vector256<float> vector, Vector256<int> indices);
    public static Vector256<double> ShuffleNative(Vector256<double> vector, Vector256<ulong> indices);
}

public static class Vector512
{
    public static Vector512<byte> ShuffleNative(Vector512<byte> vector, Vector512<byte> indices);
    public static Vector512<sbyte> ShuffleNative(Vector512<sbyte> vector, Vector512<sbyte> indices);
    public static Vector512<short> ShuffleNative(Vector512<short> vector, Vector512<short> indices);
    public static Vector512<ushort> ShuffleNative(Vector512<ushort> vector, Vector512<ushort> indices);
    public static Vector512<int> ShuffleNative(Vector512<int> vector, Vector512<int> indices);
    public static Vector512<uint> ShuffleNative(Vector512<uint> vector, Vector512<uint> indices);
    public static Vector512<long> ShuffleNative(Vector512<int> vector, Vector512<long> indices);
    public static Vector512<ulong> ShuffleNative(Vector512<uint> vector, Vector512<ulong> indices);
    public static Vector512<float> ShuffleNative(Vector512<float> vector, Vector512<int> indices);
    public static Vector512<double> ShuffleNative(Vector512<double> vector, Vector512<ulong> indices);
}

public static class Vector64
{
    public static Vector64<byte> ShuffleNative(Vector64<byte> vector, Vector64<byte> indices);
    public static Vector64<sbyte> ShuffleNative(Vector64<sbyte> vector, Vector64<sbyte> indices);
    public static Vector64<short> ShuffleNative(Vector64<short> vector, Vector64<short> indices);
    public static Vector64<ushort> ShuffleNative(Vector64<ushort> vector, Vector64<ushort> indices);
    public static Vector64<int> ShuffleNative(Vector64<int> vector, Vector64<int> indices);
    public static Vector64<uint> ShuffleNative(Vector64<uint> vector, Vector64<uint> indices);
    public static Vector64<long> ShuffleNative(Vector64<int> vector, Vector64<long> indices);
    public static Vector64<ulong> ShuffleNative(Vector64<uint> vector, Vector64<ulong> indices);
    public static Vector64<float> ShuffleNative(Vector64<float> vector, Vector64<int> indices);
    public static Vector64<double> ShuffleNative(Vector64<double> vector, Vector64<ulong> indices);
}
```

## Path.ContainsInvalidPathChar and Path.ContainsInvalidFileNameChar 

**Rejected** | [#runtime/110301](https://github.com/dotnet/runtime/issues/110301#issuecomment-2619851643) | 
[Video](https://www.youtube.com/watch?v=ulYPW9T9L_0&t=0h18m38s)

Through a long discussion, we're not sure there's even a valid scenario for anyone reading the existing values; therefore optimizing this doesn't seem to have inherent value.

At the end of the day, whether a particular path (or sub-path) is invalid is a decision of the target file system, and not something that can be answered with simple properties.  As with all of file creating, the only true answer is "try it, and see if it works".

## add possibility to pass `key` as `Span<byte>` in `SymmetricAlgorithm` and `KeyedHashAlgorithm` 

**Approved** | [#runtime/111154](https://github.com/dotnet/runtime/issues/111154#issuecomment-2619866315) | [Video](https://www.youtube.com/watch?v=ulYPW9T9L_0&t=1h5m56s)

* The method(s) on SymmetricAlgorithm is(are) good.  The ones on KeyedHashAlgorithm don't seem sufficiently justified.

```C#
namespace System.Security.Cryptography;

public partial class SymmetricAlgorithm
{
    public void SetKey(ReadOnlySpan<byte> key);
    protected virtual void SetKeyCore(ReadOnlySpan<byte> key);
}
```

## Consider supporting collection literals for IAsyncEnumerable<T>

**NeedsWork** | [#runtime/111715](https://github.com/dotnet/runtime/issues/111715#issuecomment-2619912753) | [Video](https://www.youtube.com/watch?v=ulYPW9T9L_0&t=1h13m50s)

This is easy to do for net10+ only, e.g.

```c#
namespace System.Collections.Generic
{
    [CollectionBuilder(typeof(AsyncIteratorMethodBuilder), nameof(AsyncIteratorMethodBuilder.CreateAsyncEnumerable))]
    public interface IAsyncEnumerable<T>
    {
    }
}

namespace System.Runtime.CompilerServices
{
    public partial struct AsyncIteratorMethodBuilder
    {
        public static IAsyncEnumerable<T> CreateAsyncEnumerable<T>(ReadOnlySpan<T> values);
    }
}
```

but Microsoft.Bcl.AsyncInterfaces typeforwards IAsyncEnumerable and AsyncIteratorMethodBuilder, which means that it's not easily possible to make it work for .NET Standard 2.0 TFM or older .NET Core TFMs.  If the downlevel TFMs are important then this needs to be special cased in the compiler (possibly in conjunction with doing this change for net10, or possibly standalone)

## LdapSessionOptions.CertificateDirectory Property

**Approved** | [#runtime/104260](https://github.com/dotnet/runtime/issues/104260#issuecomment-2619958450) | [Video](https://www.youtube.com/watch?v=ulYPW9T9L_0&t=1h38m50s)
 
* The certificate directory property should reflect that it's a directory for defining trust, not just "where to look for certificates"
  * `CertificateDirectory` => `TrustedCertificatesDirectory`
* The approval state includes the suggestion of UnsupportedOSAttributes, but if that's not consistent with the LDAP types, feel free to omit them.

```c#
namespace System.DirectoryServices.Protocols;

public partial class LdapSessionOptions
{
    [UnsupportedOS("windows")]
    [UnsupportedOS("android")]
    [UnsupportedOS("browser")]
    [UnsupportedOS("ios")]
    public string? TrustedCertificatesDirectory { get; set; }

    [UnsupportedOS("windows")]
    [UnsupportedOS("android")]
    [UnsupportedOS("browser")]
    [UnsupportedOS("ios")]
    public void StartNewTlsSessionContext();
}
```
