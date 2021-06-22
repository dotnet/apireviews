# API Review 06/22/2021

## Add API to control trust during TLS handshake

**Approved** | [#runtime/54219](https://github.com/dotnet/runtime/issues/54219#issuecomment-866187863)

* Let's make it an overload of `SslStreamCertificateContext.Create()` and apply the versioning rules of methods with optional parameters.
* We don't believe we need the `trustMode` on `SslCertificateTrust`

```C#
namespace System.Net.Security
{
    public partial class SslStreamCertificateContext
    {
        // Existing API:
        // - Hide and remove default-ness
        [EditorBrowsable(EditorBrowsableState.Never)]
        public static SslStreamCertificateContext Create(
            X509Certificate2 target, 
            X509Certificate2Collection? additionalCertificates, 
            bool offline
        );
        public static SslStreamCertificateContext Create( 
            X509Certificate2 target,  
            X509Certificate2Collection? additionalCertificates,  
            bool offline = false,
            SslCertificateTrust? trust = null
        );
    }

    public sealed class SslCertificateTrust
    {
        public static SslCertificateTrust CreateForX509Store(
            X509Store store, 
            bool sendTrustInHandshake = false
        );
 
        [UnsupportedPlatform("windows")]
        public static SslCertificateTrust CreateForX509Collection(
            X509Certificate2Collection trustList,
            bool sendTrustInHandshake = false
        );
    }
}
```
## Consider a hot reload feature switch for linking

**Approved** | [#runtime/51159](https://github.com/dotnet/runtime/issues/51159#issuecomment-866199485)

* At this point it seems better to make a dedicated type
    - Let's move the already approved `AssemblyExtensions.ApplyUpdate()`
* Let's rename the property to `IsSupported`
* We may want to leave the old APIs until preview 7 so that we keep infrastructure working

```C#
namespace System.Reflection.Metadata
{
    public static class MetadataUpdater
    {
        public static void ApplyUpdate(Assembly assembly, ReadOnlySpan<byte> metadataDelta, ReadOnlySpan<byte> ilDelta, ReadOnlySpan<byte> pdbDelta);
        public static bool IsSupported { get; }
    }
}
```

## Add ImmutableArray Span-based APIs

**NeedsWork** | [#runtime/22928](https://github.com/dotnet/runtime/issues/22928#issuecomment-866221826)

* Let's add `ImmutableArray<T>.AsSpan(int, int)` to match arrays
* We're not sure we can add `Create<T>(ReadOnlySpan<T> items)` and `ToImmutableArray<T>(this ReadOnlySpan<T> items)`
    - They might cause ambiguities with existing ones over `IEnumerable<T>`. We can, of, course address those by adding overloads for common concrete types that have implicit conversion to `ReadOnlySpan<T>` (e.g. `string` and array).
    - @333fred will investigate
* Let's not add methods to any other types because we haven't done this for the mutable collections either (mostly lack of feedback but also due to performance considerations)

```C#
namespace System.Collections.Immutable
{
    public static partial class ImmutableArray
    {
        // Open question:
        // public static ImmutableArray<T> Create<T>(ReadOnlySpan<T> items);
        // public static ImmutableArray<T> ToImmutableArray<T>(this ReadOnlySpan<T> items);
    }

    public partial struct ImmutableArray<T>
    {
        public ReadOnlySpan<T> AsSpan(int start, int length);
        public ImmutableArray<T> AddRange(ReadOnlySpan<T> items);
        public void CopyTo(Span<T> destination);
        public ImmutableArray<T> RemoveRange(ReadOnlySpan<T> items, IEqualityComparer<T>? equalityComparer);
        public ImmutableArray<T> Slice(int start, int length);
        public sealed partial class Builder
        {
            public void AddRange(ReadOnlySpan<T> items);
            public void AddRange<TDerived>(ReadOnlySpan<TDerived> items) where TDerived : T;
            public void CopyTo(Span<T> destination);
        }
    }
}
```

## Add JIT Metrics APIs

**Approved** | [#runtime/54444](https://github.com/dotnet/runtime/issues/54444#issuecomment-866244391)

* It seems we can't meaningfully return information on a per-thread basis
    - If we do, we'd add a parameter rather than a new set of methods, probably defaulted such that it would return the global state when called without specifying.
* We should probably put this in `System.Runtime` as `System.Runtime.CompilerServices` is largely for APIs we expect to be used by compilers (or by people who want write very low level code). These seem on par with GC information APIs.
* Let's expose them as static methods so if we need to expose more information that needs snapshot semantics, we can make it struct/class and add corresponding instance properties.

```C#
namespace System.Runtime
{
    public static class JitInfo
    {
        public static long GetCompiledILBytes();
        public static long GetCompiledMethodCount();
        public static TimeSpan GetCompilationTime();
    }
}
```

## Add support for POCO serialization callbacks

**Approved** | [#runtime/54528](https://github.com/dotnet/runtime/issues/54528#issuecomment-866260736)

* We should make sure this feature works well in source generation
    - We can either say we don't support private/internal at all
    - We could say we support private/internal only when used in reflection mode
    - Or we could change the shape of the feature for this problem to disappear, for example, by using interfaces instead.

```C#
namespace System.Text.Json.Serialization
{
    public interface IJsonOnDeserializing
    {
        void OnDeserializing();
    }

    public interface IJsonOnDeserialized
    {
        void OnDeserialized();
    }

    public interface IJsonOnSerializing
    {
        void OnSerializing();
    }

    public interface IJsonOnSerialized
    {
        void OnSerialized();
    }
}
```

