# API Review 10/12/2021

## JsonSerializer support for JsonDocument types in source generation mode

**Approved** | [#runtime/59954](https://github.com/dotnet/runtime/issues/59954#issuecomment-941250320) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization.Metadata
{
    public static partial class JsonMetadataServices
    {
        // Existing
        // public JsonConverter<JsonElement> JsonElementConverter { get; }
        public JsonConverter<JsonDocument> JsonDocumentConverter { get; }
    }
}
```
## Add string indexers for JsonElement

**Rejected** | [#runtime/60279](https://github.com/dotnet/runtime/issues/60279#issuecomment-941256995) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h4m3s)

* We don't think this is a good idea because name lookups are a `O(n)` complexity, which will result in people accidentally creating `O(n^2)` algorithms when being used in a loop.
* The general guidance is that property access should "cheap" or "reasonably fast", which basically means `O(n)` is a bad idea...

```C#
namespace System.Text.Json
{
    public readonly partial struct JsonElement
    {
        // Existing API
        // public JsonElement this[int index] { get; }
        public JsonElement this[string propertyName] { get; }  
        public JsonElement this[ReadOnlySpan<char> propertyName] { get; }  
        public JsonElement this[ReadOnlySpan<byte> utf8PropertyName] { get; }  
    }
}
```
## Customizable IConfigurationRoot.GetDebugView() for hiding the value

**Approved** | [#runtime/60065](https://github.com/dotnet/runtime/issues/60065#issuecomment-941273491) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h11m5s)

* The scenario makes sense but we don't like the use of `Func` because it's not clear what the first three strings
* Should `Value` be nullable?

```C#
namespace Microsoft.Extensions.Configuration
{
    public readonly struct ConfigurationDebugViewContext
    {
        public ConfigurationDebugViewContext(string path, string key, string value, IConfigurationProvider configurationProvider);
        public string Path { get; }
        public string Key { get; }
        public string Value { get; }
        public IConfigurationProvider ConfigurationProvider { get; }
    }

    public static class ConfigurationRootExtensions
    {
        // Existing
        // public static string GetDebugView(this IConfigurationRoot root);
        public static string GetDebugView(this IConfigurationRoot root, Func<ConfigurationDebugViewContext, string> processValue);
    }
}
```
## Add an IAsyncEnumerable<T>.ToEnumerable extension method.

**Approved** | [#runtime/60106](https://github.com/dotnet/runtime/issues/60106#issuecomment-941288052) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h26m35s)

* We think `ToListAsync` goes down path of `System.Linq.Async`. It seems once you want this functionality, you're better off adding a reference to that.
* `System.Linq.Async` already has `ToEnumerable` method (even though it doesn't take a cancellation token), so we'd prefer adding `Blocking` to avoid any ambiguity errors in case they ever add an overload with a cancellation token. It also puts more attention of the potentially problematic blocking behavior.

```C#
namespace System.Threading.Tasks
{
    public static class TaskAsyncEnumerableExtensions
    {
        public static IEnumerable<T> ToBlockingEnumerable<T>(this IAsyncEnumerable<T> source, CancellationToken cancellationToken = default);
    }
}
```
## SignedCms:  does not enable RSASSA-PSS signing

**Approved** | [#runtime/60125](https://github.com/dotnet/runtime/issues/60125#issuecomment-941291692) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h42m55s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography.Pkcs
{
    public partial class CmsSigner
    {
        // Existing ctors:
        // public CmsSigner();
        // public CmsSigner(SubjectIdentifierType signerIdentifierType);
        // public CmsSigner(X509Certificate2? certificate);
        // public CmsSigner(SubjectIdentifierType signerIdentifierType, X509Certificate2? certificate);
        //
        // public CmsSigner(
        //     SubjectIdentifierType signerIdentifierType,
        //     X509Certificate2? certificate,
        //     AsymmetricAlgorithm? privateKey);

        public CmsSigner(
            SubjectIdentifierType signerIdentifierType,
            X509Certificate2? certificate,
            RSA? privateKey,
            RSASignaturePadding? signaturePadding);

        public RSASignaturePadding? SignaturePadding { get; set; }
    }
}
```
## Do not double-return arrays to ArrayPool

**Rejected** | [#runtime/33767](https://github.com/dotnet/runtime/issues/33767#issuecomment-941297075) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h46m49s)

* We believe this is going to be difficult, given that the compiler/analyzer doesn't expose flow in a way where we could easily tell whether something is called more than once.
* Another way of solving this problem is returning a disposable value from a method that helps manage the lifetime
* Hence we don't believe it's worth investing in as of this moment

```text
Analzyer: Do not double-return arrays to ArrayPool 
```
## System.Collections.Generic.SortedList<TKey, TValue> should have SetByIndex(Int32,TValue)

**Approved** | [#runtime/58962](https://github.com/dotnet/runtime/issues/58962#issuecomment-941332116) | [Video](https://www.youtube.com/watch?v=sh6LRNVf8NA&t=0h52m7s)

* It seems `GetByIndex()` can already be achieved by indexing into the `Values` and `Keys` properties.
    - So we don't need to expose `GetByIndex`, but feels good for symmetry
* The `Values` throws NotSupportedException on writes
    - We could make this writable but this would be odd
* We should add `SetValueByIndex`

```C#
namespace System.Collections.Generic
{
    public partial class SortedList<TKey, TValue>
    {
        public TKey GetKeyAtIndex(int index);
        public TValue GetValueAtIndex(int index);
        public void SetValueAtIndex(int index, TValue value);
    }
}
```
