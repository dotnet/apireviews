# HttpClient extension methods for JSON 3/5/2020

**Approved** | [#runtime/33566](https://github.com/dotnet/runtime/issues/33566#issuecomment-598845161) | [Video](https://www.youtube.com/watch?v=_AHbjIS8_0I)

* `HttpClientJsonExtensions`
  * `JsonSerializerOptions` should be nullable
  * `Uri` and `string` should be nullable
  * We should add overloads that take `CancellationToken` without `JsonSerializerOptions`
  * Should we define the extension methods over `HttpMessageInvoker` instead (the base type of `HttpClient`)?
    * We concluded no, because `HttpClient` is the mainline API and except for low-level code users don't program against `HttpMessageInvoker`
  * We should remove overloads that take `object` and `Type`
    * If the generic overload is instantiated with `object` it will use the runtime type
    * ~~We should name `Type` parameter `inputType`~~
    * ~~We should swap `object value` and `Type input`~~
  * Rename `T` to `TValue`
  * We like the method names as proposed
* `JsonContent`
  * We should make the constructors internal and only have the factory methods. This avoids confusion where people never know that there are generic versions.
  * We should expose non-generic factory methods
  * Swap parameters `Type type` and `object value` to avoid `value` changing positions between overloads
  * Rename parameter `Type type` to `Type inputType`
  * Don't expose `string mediaType` and only `MediaTypeHeaderValue`. If we need the string based one, we can add it later but it should parse the media type instead of just passing it to the constructor of `MediaTypeHeaderValue`.

```C#
#nullable enable

namespace System.Net.Http.Json {
    public static class HttpClientJsonExtensions {
        public static Task<object> GetFromJsonAsync(
            this HttpClient client,
            string? requestUri,
            Type type,  
            CancellationToken cancellationToken);
        public static Task<object> GetFromJsonAsync(
            this HttpClient client,
            string? requestUri,
            Type type,  
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<object> GetFromJsonAsync(
            this HttpClient client,
            Uri? requestUri,
            Type type,
            CancellationToken cancellationToken);
        public static Task<object> GetFromJsonAsync(
            this HttpClient client,
            Uri? requestUri,
            Type type,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<TValue> GetFromJsonAsync<TValue>(
            this HttpClient client,
            string? requestUri,
            CancellationToken cancellationToken);
        public static Task<TValue> GetFromJsonAsync<TValue>(
            this HttpClient client,
            string? requestUri,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<TValue> GetFromJsonAsync<TValue>(
            this HttpClient client,
            Uri? requestUri,
            CancellationToken cancellationToken);
        public static Task<TValue> GetFromJsonAsync<TValue>(
            this HttpClient client,
            Uri? requestUri,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<HttpResponseMessage> PostAsJsonAsync<TValue>(
            this HttpClient client,
            string? requestUri,
            TValue value,
            CancellationToken cancellationToken);
        public static Task<HttpResponseMessage> PostAsJsonAsync<TValue>(
            this HttpClient client,
            string? requestUri,
            TValue value,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<HttpResponseMessage> PostAsJsonAsync<TValue>(
            this HttpClient client,
            Uri? requestUri,
            TValue value,
            CancellationToken cancellationToken);
        public static Task<HttpResponseMessage> PostAsJsonAsync<TValue>(
            this HttpClient client,
            Uri? requestUri,
            TValue value,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<HttpResponseMessage> PutAsJsonAsync<TValue>(
            this HttpClient client,
            string? requestUri,
            TValue value,
            CancellationToken cancellationToken);
        public static Task<HttpResponseMessage> PutAsJsonAsync<TValue>(
            this HttpClient client,
            string? requestUri,
            TValue value,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<HttpResponseMessage> PutAsJsonAsync<TValue>(
            this HttpClient client,
            Uri? requestUri,
            TValue value,
            CancellationToken cancellationToken);
        public static Task<HttpResponseMessage> PutAsJsonAsync<TValue>(
            this HttpClient client,
            Uri? requestUri,
            TValue value,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        }
    public static class HttpContentJsonExtensions {
        public static Task<object> ReadFromJsonAsync(
            this HttpContent content,
            Type type,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
        public static Task<T> ReadFromJsonAsync<T>(
            this HttpContent content,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
    }
    public class JsonContent : HttpContent {
        public static JsonContent Create<T>(
            T value,
            MediaTypeHeaderValue mediaType,
            JsonSerializerOptions? options = null);
        public static JsonContent Create(
            object? inputValue,
            Type type,
            MediaTypeHeaderValue mediaType,
            JsonSerializerOptions? options = null);
        public Type ObjectType { get; }
        public object? Value { get; }
    }
}
```
