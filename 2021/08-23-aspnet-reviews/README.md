# API Review 08/23/2021

## Allow external libraries to extend Results

**Approved** | [#aspnetcore/35508](https://github.com/dotnet/aspnetcore/issues/35508#issuecomment-903986460)


### API Review

We considered a few options

* Not shipping this and hoping a language feature that supersedes this.
* Using a static holder type e.g.
```
using static ResultsHolder = Microsoft.AspNetCore.Http;

public static class HttpResults
{
    public static IResults Results { get; }
}
```

This felt iffy since it's only usable within C# that has global static usings.

At this point, we feel there is a need for an API for the community to rally around, so even if it does get deprecated by a newer language feature, we think it's worthwhile.

Naming:

* `Results.Custom`
* `Results.Contributions`

Since the type name has `IResultExtensions` already has `Extensions` in it, it felt like keeping that name is ok.

There was some concern about using `Extensions` as a property name, since there's a chance that it can conflict with a namespace, or if multiple libraries add the same named extension, but we can provide guidance here.

## `{Date, Time}Only` Querystring building APIs for Blazor

**Approved** | [#aspnetcore/35567](https://github.com/dotnet/aspnetcore/issues/35567)

## Allow more failure details for AuthZ handlers

**Approved** | [#aspnetcore/35319](https://github.com/dotnet/aspnetcore/issues/35319#issuecomment-903999113)

* Remove setters from properties.

```diff
public class AuthorizationFailureReason
{
+   public AuthorizationFailureReason(IAuthorizationHandler handler, string message)
+   public string Message { get; }
+   public IAuthorizationHandler Handler { get; }
}
```

API consideration: Can this type be sealed and use a `IDictionary` to pass additional data instead e.g.

```diff
+public sealed class AuthorizationFailureReason
{
+   public AuthorizationFailureReason(IAuthorizationHandler handler, string message)
+   public string Message { get; }
+   public IAuthorizationHandler Handler { get; }
+   public IDictionary<object, object> Properties { get; } 
}
```

AuthorizationFailure

* Update property name to `FailureReasons` for consistency:

```diff
public class AuthorizationFailure
{
+ // Calls Fail and stores the failure reasons for future reference. Can be called multiple times.
+ public static void ExplicitFail(IEnumerable<AuthorizationFailureReason> reasons);
+ public IEnumerable<AuthorizationFailureReason> FailureReasons { get; }
```
    


## Minimal APIs naming cleanup

**Approved** | [#aspnetcore/35478](https://github.com/dotnet/aspnetcore/issues/35478)

