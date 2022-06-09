# API Review 06/09/2022

## Add overloads to string trimming

**NeedsWork** | [#runtime/14386](https://github.com/dotnet/runtime/issues/14386#issuecomment-1151407144)

* The name "Trim" currently implies (potentially) multiple removal, these new ones are single, so should not use "Trim"
* We should use consistent wording between CompareInfo and MemoryExtensions
* Together, the previous two bullet suggest "RemovePrefix" instead of "TrimIfStartsWith".
* Is returning a Span-like type the right shape here? If so, we need 4 of each method, not 2 (Span, ROSpan, Memory, ROMemory)
  * One way to reduce the overload explosion is to return System.Range
* Consider consistency between when the prefix/suffix/etc has to be a span-like, vs a single item
## Do not discard results of methods that shouldn't be ignored

**Approved** | [#runtime/34098](https://github.com/dotnet/runtime/issues/34098#issuecomment-1151440990)

* We changed Inherited true to Inherited false
* We think that the analyzer should only look at the specific method declaration, and not try to infer the behavior by walking the virtual hierarchy
* For return values, discard assignment (_ = obj.Method()) should count as "not ignoring"
* For out values, discard assignment (out _) should count as "not ignoring".
* The proposed members seem like a good place to start, and once the attribute is merged in send an announcement to have area owners apply it to their areas as they see fit.

```C#
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.ReturnValue | AttributeTargets.Parameter, AllowMultiple = false, Inherited = false)]
    public sealed class DoNotIgnoreAttribute : Attribute
    {
        public DoNotIgnoreAttribute() {}

        public string? Message { get; set; }
    }
}
```
## Add `DeleteFromJsonAsync` for `HttpClient`

**Approved** | [#runtime/65617](https://github.com/dotnet/runtime/issues/65617#issuecomment-1151447897)

* Looking at the HttpMethod enum, we now have everything except HEAD, OPTIONS, and TRACE, but it is believed those don't apply.
* Looks good as proposed, on the basis that it is a copy/paste of the Get methods with Get replaced with Delete.
* Make the RequiresUnreferencedCode, and other such attributes, match the equivalents from the Get methods.

```C#
namespace System.Net.Http.Json
{
    partial static class HttpClientJsonExtensions
    {
        public static System.Threading.Tasks.Task<object?> DeleteFromJsonAsync(this System.Net.Http.HttpClient client, string? requestUri, System.Type type, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<object?> DeleteFromJsonAsync(this System.Net.Http.HttpClient client, System.Uri? requestUri, System.Type type, System.Text.Json.JsonSerializerOptions? options, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<object?> DeleteFromJsonAsync(this System.Net.Http.HttpClient client, System.Uri? requestUri, System.Type type, System.Text.Json.Serialization.JsonSerializerContext context, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<object?> DeleteFromJsonAsync(this System.Net.Http.HttpClient client, System.Uri? requestUri, System.Type type, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<TValue?> DeleteFromJsonAsync<TValue>(this System.Net.Http.HttpClient client, string? requestUri, System.Text.Json.JsonSerializerOptions? options, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<TValue?> DeleteFromJsonAsync<TValue>(this System.Net.Http.HttpClient client, string? requestUri, System.Text.Json.Serialization.Metadata.JsonTypeInfo<TValue> jsonTypeInfo, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<TValue?> DeleteFromJsonAsync<TValue>(this System.Net.Http.HttpClient client, string? requestUri, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<TValue?> DeleteFromJsonAsync<TValue>(this System.Net.Http.HttpClient client, System.Uri? requestUri, System.Text.Json.JsonSerializerOptions? options, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<TValue?> DeleteFromJsonAsync<TValue>(this System.Net.Http.HttpClient client, System.Uri? requestUri, System.Text.Json.Serialization.Metadata.JsonTypeInfo<TValue> jsonTypeInfo, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public static System.Threading.Tasks.Task<TValue?> DeleteFromJsonAsync<TValue>(this System.Net.Http.HttpClient client, System.Uri? requestUri, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
    }
}
```
## Enumerable.Order and OrderDescending

**Approved** | [#runtime/67194](https://github.com/dotnet/runtime/issues/67194#issuecomment-1151480488)

* Since this keeps coming up, we recommend first doing an analysis of Enumerable to see if we can do something that is consistent and not explosive, like making selector functions nullable (and defaulted to null).
  * We did the analysis on the call.  Losing generic inference makes that a worse approach.
* We removed the generic constraint both for consistency, and `IEnumerable<int?>`
* Should also apply to Queryable

```C#
namespace System.Linq
{
    public partial class Enumerable
    {
        public static IOrderedEnumerable<T> Order<T>(this IEnumerable<T> source);
        public static IOrderedEnumerable<T> Order<T>(this IEnumerable<T> source, IComparer<T> comparer);

        public static IOrderedEnumerable<T> OrderDescending<T>(this IEnumerable<T> source);
        public static IOrderedEnumerable<T> OrderDescending<T>(this IEnumerable<T> source, IComparer<T> comparer);
    }
    public partial class Queryable
    {
        // Or whatever's correct.
        [DynamicDependency("Order`1", typeof(Enumerable))]
        public static IOrderedQueryable<T> Order<T>(this IQueryable<T> source);
        [DynamicDependency("Order`1", typeof(Enumerable))]
        public static IOrderedQueryable<T> Order<T>(this IQueryable<T> source, IComparer<T> comparer);

        [DynamicDependency("OrderDescending`1", typeof(Enumerable))]
        public static IOrderedQueryable<T> OrderDescending<T>(this IQueryable<T> source);
        [DynamicDependency("OrderDescending`1", typeof(Enumerable))]
        public static IOrderedQueryable<T> OrderDescending<T>(this IQueryable<T> source, IComparer<T> comparer);
    }
}
```
## Add architecture PPC64le to Architecture enum

**Approved** | [#runtime/67428](https://github.com/dotnet/runtime/issues/67428#issuecomment-1151500969)

* We had a bit of a question of "is the -LE suffix necessary?" and it seems like most systems make a distinction between a ppc64 and a ppc64le, so "yes".
* For naming consistency and .NET's naming guidelines, we changed the casing to only the initial P being capital (`Ppc64le`).

```diff
namespace System.Runtime.InteropServices
{
    public enum Architecture
    {
        X86 = 0,
        X64 = 1,
        Arm = 2,
        Arm64 = 3,
        Wasm = 4,
        S390x = 5,
        LoongArch64 = 6,
        Armv6 = 7,
+       Ppc64le = 8,
    }
}
```
