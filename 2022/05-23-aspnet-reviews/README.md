# API Review 05/23/2022

## Add client result support to Java client

**Approved** | [#aspnetcore/41777](https://github.com/dotnet/aspnetcore/issues/41777#issuecomment-1134975965)

API review notes:

1. What about the breaking change?
	  ```Java
	  // Ambiguous between Action<String> and Function<String, Object>
	  On(param ->
	  {
	    throw new RuntimeException();
	  }, String.class);
	  ```
	  We think this is rare enough that we can document it. It's also easy to work around by specifying a type. If we wanted to remove the break, we could `Class<TResult>` argument but this seems annoying.
2.  Can we come up better name than `T`?
	`TResult`
3. Can we get rid of `FunctionSingle` and check after the callback returns if the returned object is a `Single`?
	Maybe. It wouldn't be obvious Single is supported though, so we shouldn't do it.
4. Is `Function` too generic of a name?
        Maybe, but we've already shipped `Action`, so this shouldn't be any worse.


```diff
package com.microsoft.signalr;

+ public interface ActionCompletable {
+     Completable invoke();
+ }
+ public interface Action1Completable<T1> {
+     Completable invoke(T1 param1);
+ }

// Up to 8
+ Action8Completable<T1, ..., T8>
```

```diff
package com.microsoft.signalr;

+ public interface Function<TResult> {
+     T invoke();
+ }
+ public interface FunctionSingle<TResult> {
+     Single<TResult> invoke();
+ }
+ public interface Function1<T1, TResult> {
+     TResult invoke(T1 param1);
+ }
+ public interface Function1Single<T1, TResult> {
+     Single<TResult> invoke(T1 param1);
+ }
// Up to 8
+ Function8Single<T1, ..., T8, TResult>
```

```diff
package com.microsoft.signalr;

class HubConnection {
+    public Subscription on(String target, ActionCompletable callback);
+    public <TResult> Subscription on(String target, Function<TResult> callback);
+    public <TResult> Subscription on(String target, FunctionSingle<TResult> callback);

+    public <T1> Subscription on(String target, Action1Completable<T1> callback, Class<T1> param1);
+    public <T1, TResult> Subscription on(String target, Function1<T1, TResult> callback, Class<T1> param1);
+    public <T1, TResult> Subscription on(String target, Function1Single<T1, TResult> callback, Class<T1> param1);
// Up to 8
+    public <T1, ..., T8> Subscription on(String target, Action8Completable<T1, ..., T8> callback, Class<T1> param1, ..., Class<T8> param8);
+    public <T1, ..., T8, TResult> Subscription on(String target, Function8<T1, ..., T8, TResult> callback, Class<T1> param1, ..., Class<T8> param8);
+    public <T1, ..., T8, TResult> Subscription on(String target, FunctionSingle<T1, ..., T8, TResult> callback, Class<T1> param1, ..., Class<T8> param8);
}
```
## Rename GroupRouteBuilder to RouteGroupBuilder

**Approved** | [#aspnetcore/41781](https://github.com/dotnet/aspnetcore/issues/41781#issuecomment-1134978856)

API review notes:

1. Should we do this?
    This has only been out for a preview release. A lot of people use `var`. Now or never, and it seems slightly better.
## W3CLogger Custom Fields

**Approved** | [#aspnetcore/41698](https://github.com/dotnet/aspnetcore/issues/41698#issuecomment-1135001860)

API Review notes:

1. If we add response headers later, could we distinguish? 
    Yes because of the `cs`/`sc` field name prefixes.
2. Can we make this option more generic to support more field types like response headers?
    Maybe if we parsed prefixes like `cs` and `sc`, but that seems complicated. We could add a `ResponseHeaders` option in the future if we want.
3.  Settable `IReadOnlyList<string>?`?
    Even though it's an `IReadOnlyList`, we'd still have to clone the headers for safety. Let's just make it a get-only `ISet<string>`. That way we can throw after the server has started logging if someone tries to mutate the now read-only `ISet`.
4. Should we do the alternative proposal that lets you control the column name for a given field like IIS.
    It seems like the spect want you to use `(cs){lowercase request header name}` specifically. We don't let you control other column names, so we so need to offer that capability for request headers. 

```diff
namespace Microsoft.AspNetCore.HttpLogging;

public sealed class W3CLoggerOptions
{
+    /// <summary>
+    /// Barry approved summary mentioning security implications
+    /// </summary>
+    public ISet<string> AdditionalRequestHeaders { get; }
```

This isn't API, but we feel `AdditionalRequestHeaders` should be merged with any request headers specified via `W3CLoggingFields`. It should not be an error to specify "Cookies" in both places for example.
## Add generic method overloads to Microsoft.AspNetCore.Http.Results static class

**NeedsWork** | [#aspnetcore/41724](https://github.com/dotnet/aspnetcore/issues/41724#issuecomment-1135025608)

API review notes:

1. Can we return typed arguments from the new generic `Results` methods?
    No. This would be source breaking to existing apps (and annoying to new apps) with multiple returns in a lambda. And it's more consistent.
2. Risks?
    It changes the methods some existing apps call when they're recompiled. This only changes the internal runtime type we returned.
3. What about polymorphism?
    We should make sure that if I was returning `Results.Ok(myBaseTypedVariable)` that it still serializes all the same properties it would before and not just those on the declared base type.
    We think this would be breaking, so the types like `Ok<TValue>` will need a mode to continue serializing all properties even if a base type is declared. We should **ALSO CHANGE** `TypedResults` to match this behavior for consistency. If you want to exclude certain properties, you can use DTO, attribute or custom `IResult`.

```diff
namespace Microsoft.AspNetCore.Http;

public static class Results
{
+    public static IResult Accepted<TValue>(TValue? value);
+    public static IResult AcceptedAtRoute<TValue>(string? routeName, object? routeValues = null, TValue? value = null);
+    public static IResult BadRequest<TValue>(TValue? error);
+    public static IResult Conflict<TValue>(TValue? error);
+    public static IResult Created<TValue>(string uri, TValue? value);
+    public static IResult Created<TValue>(Uri uri, TValue? value);
+    public static IResult CreatedAtRoute<TValue>(string? routeName, object? routeValues = null, TValue? value = null);
+    public static IResult Json<TValue>(TValue value, JsonSerializerOptions? options = null, string? contentType = null, int? statusCode = null);
+    public static IResult NotFound<TValue>(TValue? value);
+    public static IResult Ok<TValue>(TValue? value);
+    public static IResult UnprocessableEntity<TValue>(TValue? error);
}
```

+ @JamesNK for trimmability discussion. I imagine it would be similar to the other existing `Results` or `TypedResults` methods.

Given the nuanced breaking change discussion, we'll continue this next time.
