# Quick Reviews 11/12/2019

## HMAC-based Extract-and-Expand Key Derivation Function (HKDF)

**Approved** | [#corefx/29660](https://github.com/dotnet/corefx/issues/29660#issuecomment-553037689) | [Video](https://www.youtube.com/watch?v=6NLJ1csBw14&t=0h0m0s)

* ~~`Expand`. We should align the optionality of the `info` parameter between the span version and the byte array version.~~
    - This might cause ambiguity. We decided that optionality is omitted from the span versions
    - However, we should align the order to `outputLength` and `output`. We should swap `output` and `info` in the span-based version.

```C#
namespace System.Security.Cryptography
{
    public static class HKDF
    {
        public static byte[] Extract(HashAlgorithmName hashAlgorithmName, byte[] ikm, byte[] salt = null);
        public static int Extract(HashAlgorithmName hashAlgorithmName, ReadOnlySpan<byte> ikm, ReadOnlySpan<byte> salt, Span<byte> prk);

        public static byte[] Expand(HashAlgorithmName hashAlgorithmName, byte[] prk, int outputLength, byte[] info = null);
        public static void Expand(HashAlgorithmName hashAlgorithmName, ReadOnlySpan<byte> prk, Span<byte> output, ReadOnlySpan<byte> info);

        public static byte[] DeriveKey(HashAlgorithmName hashAlgorithmName, byte[] ikm, int outputLength, byte[] salt = null, byte[] info = null);
        public static void DeriveKey(HashAlgorithmName hashAlgorithmName, ReadOnlySpan<byte> ikm, Span<byte> output, ReadOnlySpan<byte> salt, ReadOnlySpan<byte> info);
    }
}
```
## Add cancellation overloads on HttpClient and HttpContent

**Approved** | [#corefx/32615](https://github.com/dotnet/corefx/issues/32615#issuecomment-553050411) | [Video](https://www.youtube.com/watch?v=6NLJ1csBw14&t=0h23m44s)

* Looks good, but optionality is pointless because the overload without the token already exists
* Name of methods on `HttpContent` should be `ReadAsXxx`
* Return types needs fixing up

```C#
partial class HttpClient
{
    // New APIs

    public Task<byte[]> GetByteArrayAsync(string requestUri, CancellationToken cancellationToken);
    public Task<byte[]> GetByteArrayAsync(Uri requestUri, CancellationToken cancellationToken);

    public Task<Stream> GetStreamAsync(string requestUri, CancellationToken cancellationToken);
    public Task<Stream> GetStreamAsync(Uri requestUri, CancellationToken cancellationToken);

    public Task<string> GetStringAsync(string requestUri, CancellationToken cancellationToken);
    public Task<string> GetStringAsync(Uri requestUri, CancellationToken cancellationToken);

    // Existing APIs

    public Task<byte[]> GetByteArrayAsync(string requestUri);
    public Task<byte[]> GetByteArrayAsync(Uri requestUri);

    public Task<Stream> GetStreamAsync(string requestUri);
    public Task<Stream> GetStreamAsync(Uri requestUri);

    public Task<string> GetStringAsync(string requestUri);
    public Task<string> GetStringAsync(Uri requestUri);
}

partial class HttpContent
{
    // New APIs

    public Task<byte[]> ReadAsByteArrayAsync(CancellationToken cancellationToken);
    public Task<Stream> ReadAsStreamAsync(CancellationToken cancellationToken);
    public Task<string> ReadAsStringAsync(CancellationToken cancellationToken);

    // Existing APIs

    public Task<byte[]> ReadAsByteArrayAsync();
    public Task<Stream> ReadAsStreamAsync();
    public Task<string> ReadAsStringAsync();
}
```
## Add List<T>.CopyTo(Span<T>) overloads

**Approved** | [#corefx/33006](https://github.com/dotnet/corefx/issues/33006#issuecomment-553078596) | [Video](https://www.youtube.com/watch?v=6NLJ1csBw14&t=0h40m29s)

Wow, that took quoit a bit 😄 

* Makes sense. We should just make sure that `List<T>.CopyTo(someInt, someArray, anotherInt)` doesn't magically bind to the span version.
    - While not a breaking having the third parameter having different behavior is bad (`arrayIndex` vs. `count`)

```C#
public partial class List<T>
{
    // public void CopyTo(int index, T[] array, int arrayIndex, int count);
    // public void CopyTo(T[] array, int arrayIndex);
    // public void CopyTo(T[] array);

    public void CopyTo(Span<T> destination);
    public void CopyTo(int sourceIndex, int count, Span<T> destination);
}
```
## Expose the match length when using String.IndexOf in culture specific mode

**Needs Work** | [#corefx/33543](https://github.com/dotnet/corefx/issues/33543#issuecomment-553082487) | [Video](https://www.youtube.com/watch?v=6NLJ1csBw14&t=1h29m9s)

@GrabYourPitchforks mentioned that he's planning on addressing this issue in UTF8 string. He's thinking of exposing something like `TryFind` that outputs a `Range`. We should make sure the work aligns.
## Hide Thread.VolatileRead and Thread.VolatileWrite

**Approved** | [#corefx/33701](https://github.com/dotnet/corefx/issues/33701#issuecomment-553086736) | [Video](https://www.youtube.com/watch?v=6NLJ1csBw14&t=1h37m21s)

* This seems like a good candidate for a code fixer that replaces the call site.
* Usage of these APIs is low.
* We should consider this API in our plan for obsoletion.
* Hiding these APIs seems fine.
## ComputeHash Async required

**Approved** | [#corefx/36068](https://github.com/dotnet/corefx/issues/36068#issuecomment-553088815) | [Video](https://www.youtube.com/watch?v=6NLJ1csBw14&t=1h48m11s)

* Makes sense. We shouldn't make the API virtual though because it just does an async read and then forwards to the existing sync API
* Without `virtual`, we don't need `HashCoreAsync`


```C#
public class HashAlgorithm
{
    public Task<byte[]> ComputeHashAsync(Stream inputStream, CancellationToken cancellationToken = default);
}
```
