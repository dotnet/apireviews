# API Review 10/19/2021

## Add Enumerable.Concat overloads for variadic arguments and nested enumerables

**Approved** | [#runtime/54220](https://github.com/dotnet/runtime/issues/54220#issuecomment-946994846) | [Video](https://www.youtube.com/watch?v=etwMhIKj0tI&t=0h0m0s)

* We changed the signature of the params overload to Concat to support extension invocation and look like an easier overload.
* We renamed the `Concat(IEnumerable<IEnumerable<T>>)` to `Flatten` to better describe its use.

```C#
namespace System.Linq 
{
    public static partial class Enumerable 
    {
        public static IEnumerable<TSource> Concat<TSource>(
            this IEnumerable<TSource> first,
             IEnumerable<TSource> second,
             params IEnumerable<TSource>[] rest);


        public static IEnumerable<TSource> Flatten<TSource>(this IEnumerable<IEnumerable<TSource>> sources);
    }
}
```
## HttpClient.PatchAsJsonAsync()

**Approved** | [#runtime/60531](https://github.com/dotnet/runtime/issues/60531#issuecomment-947000766) | [Video](https://www.youtube.com/watch?v=etwMhIKj0tI&t=0h29m59s)

Looks good as proposed

```C#
namespace  System.Net.Http.Json
{
    public static partial class HttpClientJsonExtensions
    {
        public Task<HttpResponseMessage> PatchAsJsonAsync<TValue>(this HttpClient client, string? requestUri, TValue value, JsonSerializerOptions? options = null, CancellationToken cancellationToken = default);
        public Task<HttpResponseMessage> PatchAsJsonAsync<TValue>(this HttpClient client, Uri? requestUri, TValue value, JsonSerializerOptions? options = null, CancellationToken cancellationToken = default);
        public Task<HttpResponseMessage> PatchAsJsonAsync<TValue>(this HttpClient client, string? requestUri, TValue value, JsonTypeInfo<TValue> jsonTypeInfo = null, CancellationToken cancellationToken = default);
        public Task<HttpResponseMessage> PatchAsJsonAsync<TValue>(this HttpClient client, Uri? requestUri, TValue value, JsonTypeInfo<TValue> jsonTypeInfo = null, CancellationToken cancellationToken = default);
        public Task<HttpResponseMessage> PatchAsJsonAsync<TValue>(this HttpClient client, string? requestUri, TValue value, CancellationToken cancellationToken = default);
        public Task<HttpResponseMessage> PatchAsJsonAsync<TValue>(this HttpClient client, Uri? requestUri, TValue value, CancellationToken cancellationToken = default);
    }
}
```
## Support await'ing a Task without throwing

**NeedsWork** | [#runtime/22144](https://github.com/dotnet/runtime/issues/22144#issuecomment-947018377) | [Video](https://www.youtube.com/watch?v=etwMhIKj0tI&t=0h35m3s)

We discussed this today, and one of the main feelings is that the ConfigureAwait(NoThrow) feels like a pit of failure.  One piece of that is it invalidates the nullability annotations, since nulls will start appearing out of non-null-returning members.

Something like an extension method that changes from an `awaitable<T>` to an `awaitable<(T?, Exception)>`, or at least `awaitable<(bool, T?)>` might be reasonable, but that can't be described as a flags flag.  This is, in particular, important for the ValueTask versions, since the Exception cannot otherwise be recovered.

We probably want to try this out on non-public code first to get an idea of what's really needed.
