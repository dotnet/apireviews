# API Review 10/31/2023

## Provide a configurable overload of MemoryCacheEntryOptions for the commonly used method GetOrCreate in MemoryCacheExtension

**Approved** | [#runtime/92101](https://github.com/dotnet/runtime/issues/92101#issuecomment-1787669035) | [Video](https://www.youtube.com/watch?v=GmzttIbEu3w&t=0h0m0s)

* We changed the `options` parameter to `createOptions` to clue that they're only utilized on the "OrCreate" path.
 
```C#
namespace Microsoft.Extensions.Caching.Memory;

public partial class CacheExtensions
{
        public static TItem? GetOrCreate<TItem>(
            this IMemoryCache cache,
            object key,
            Func<ICacheEntry, TItem> factory,
            MemoryCacheEntryOptions? createOptions);

        public static async Task<TItem?> GetOrCreateAsync<TItem>(
            this IMemoryCache cache,
            object key,
            Func<ICacheEntry, Task<TItem>> factory,
            MemoryCacheEntryOptions? createOptions);
}
``` 
## More granular X.509 certificate loader

**Approved** | [#runtime/91763](https://github.com/dotnet/runtime/issues/91763#issuecomment-1787730116) | [Video](https://www.youtube.com/watch?v=GmzttIbEu3w&t=0h20m53s)

* Added `[UnsupportedOSPlatform("browser")]` to match the current X509Certificate2 ctors.
* Made all the `string password` parameters be `string? password` because that's more correct
* Added "FromFile" to the file-loading ones
* We cut the "Load into" collection methods, we can always add them back later.
* We should also obsolete the collection Import methods

```C#
namespace System.Security.Cryptography.X509Certificates
{
    [UnsupportedOSPlatform("browser")]
    public static partial class X509CertificateLoader
    {
        // A single X509Certificate value, PEM or DER
        // No collection variant needed.
        public static partial X509Certificate2 LoadCertificate(byte[] data);
        public static partial X509Certificate2 LoadCertificate(ReadOnlySpan<byte> data);
        public static partial X509Certificate2 LoadCertificateFromFile(string path);

        // Load "the best" certificate from a PFX: first-cert-with-privkey ?? first-cert ?? throw.
        // equivalent to the certificate from `new X509Certificate2(data, password, keyStorageFlags)`
        public static X509Certificate2 LoadPkcs12(
            byte[] data,
            string password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits? loaderLimits = null);
        public static partial X509Certificate2 LoadPkcs12(
            ReadOnlySpan<byte> data,
            ReadOnlySpan<char> password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits? loaderLimits = null);
        public static X509Certificate2 LoadPkcs12FromFile(
            string path,
            string password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits? loaderLimits = null);
        public static partial X509Certificate2 LoadPkcs12FromFile(
            string path,
            ReadOnlySpan<char> password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits? loaderLimits = null);

        // Load a PFX as a collection.
        // null loaderLimits means Pkcs12LoaderLimits.Default
        public static X509Certificate2Collection LoadPkcs12Collection(
            byte[] data,
            string? password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits loaderLimits = null);
        public static partial X509Certificate2Collection LoadPkcs12Collection(
            ReadOnlySpan<byte> data,
            ReadOnlySpan<char> password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits? loaderLimits = null);
        public static X509Certificate2Collection LoadPkcs12CollectionFromFile(
            string path,
            string? password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits loaderLimits = null);
        public static partial X509Certificate2Collection LoadPkcs12CollectionFromFile(
            string path,
            ReadOnlySpan<char> password,
            X509KeyStorageFlags keyStorageFlags = X509KeyStorageFlags.DefaultKeySet,
            Pkcs12LoaderLimits? loaderLimits = null);
    }

    public sealed class Pkcs12LoaderLimits
    {
        public static Pkcs12LoaderLimits Defaults { get; } = new Pkcs12LoaderLimits();

        public static Pkcs12LoaderLimits DangerousNoLimits { get; } = new Pkcs12LoaderLimits
        {
            MacIterationLimit = null,
            IndividualKdfIterationLimit = null,
            TotalKdfIterationLimit = null,
            MaxKeys = null,
            MaxCertificates = null,
            PreserveStorageProvider = true,
            PreserveKeyName = true,
            PreserveCertificateAlias = true,
            PreserveUnknownAttributes = true,
        };

        public Pkcs12LoaderLimits();
        public Pkcs12LoaderLimits(Pkcs12LoaderLimits copyFrom);

        public bool IsReadOnly { get; }
        public void MakeReadOnly();

        public int? MacIterationLimit { get; set; } = 300_000;
        public int? IndividualKdfIterationLimit { get; set; } = 300_000;
        public int? TotalKdfIterationLimit { get; set; } = 1_000_000;
        public int? MaxKeys { get; set; } = 200;
        public int? MaxCertificates { get; set; } = 200;

        public bool PreserveStorageProvider { get; set; } // = false;
        public bool PreserveKeyName { get; set; } // = false;
        public bool PreserveCertificateAlias { get; set; } // = false;
        public bool PreserveUnknownAttributes { get; set; } // = false;

        public bool IgnorePrivateKeys { get; set; } // = false;
        public bool IgnoreEncryptedAuthSafes { get; set; } // = false;
    }

    public sealed class Pkcs12LoadLimitExceededException : CryptographicException
    {
        public Pkcs12LoadLimitExceededException(string propertyName)
            : base($"The PKCS#12/PFX violated the '{propertyName}' limit.")
        {
        }
    }

    // .NET 9+
    public partial class X509Certificate2
    {
        // mark all byte[] and fileName ctors as [Obsolete]
    }

        // .NET 9+
    public partial class X509Certificate2Collection
    {
        // mark all byte[] and fileName Import methods as [Obsolete]
    }
}
```
