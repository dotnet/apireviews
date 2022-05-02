# API Review 05/02/2022

## Update `RouteHandlerFilterContext.Parameters` API

**Approved** | [#aspnetcore/40514](https://github.com/dotnet/aspnetcore/issues/40514#issuecomment-1115195334)

API Review Notes:

- Should we make the generic implementations public?
  - This seems noisy and low value.
- Abstract?
  - Yes. Let's save some fields
-  `Parameter` -> `ParameterValues`? `Arguments`?
  - Let's go with `Arguments` and `GetArguments` since "Parameter variables are used to import [arguments](https://developer.mozilla.org/en-US/docs/Glossary/Argument) into functions" https://developer.mozilla.org/en-US/docs/Glossary/Parameter

```diff
- public sealed class RouteHandlerInvocationContext
+ public abstract class RouteHandlerInvocationContext
{
-     public RouteHandlerInvocationContext(HttpContext httpContext, params object[] parameters)

-    public HttpContext HttpContext { get; }
+    public abstract HttpContext HttpContext { get; }

-     public IList<object?> Parameters { get; }
+     public abstract IList<object?> Arguments { get; }

+     public abstract T GetArgument<T>(int index) { }
}

+ public class DefaultRouteHandlerInvocationContext : RouteHandlerInvocationContext
+ {
+     public RouteHandlerInvocationContext(HttpContext httpContext, params object[] parameters)
+
+     public override HttpContext HttpContext { get; }
+     public override IList<object?> Arguments { get; }
+     public override T GetArgument<T>(int index) { }
+ }
```
## Add method to Results/TypedResults to allow creating a ContentHttpResult with a custom status code

**Approved** | [#aspnetcore/41444](https://github.com/dotnet/aspnetcore/issues/41444#issuecomment-1115203945)

API Review Notes:

- Do we need the two overloads?
  - Yes.
  - > For this reason, removing parameter default values is acceptable in the specific case of "moving" those default values to a new method overload to eliminate ambiguity. For example, consider an existing method MyMethod(int a = 1). If you introduce an overload of MyMethod with two optional parameters a and b, you can preserve compatibility by moving the default value of a to the new overload. Now the two overloads are MyMethod(int a) and MyMethod(int a = 1, int b = 2). This pattern allows MyMethod() to compile. 
  - https://docs.microsoft.com/en-us/dotnet/core/compatibility/#properties-fields-parameters-and-return-values
- Are we going to add a `statusCode` parameter to any other `Results` method?
  - No. Dynamic status codes hurt statically-analyzable OpenAPI metadata.
  - Text is used a lot in error cases

Approved as proposed.

``` diff
public static class Results
{
-    public static IResult Content(string content, string? contentType = null, Encoding? contentEncoding = null);
+    public static IResult Content(string content, string? contentType, Encoding? contentEncoding);
+    public static IResult Content(string content, string? contentType = null, Encoding? contentEncoding = null, int? statusCode = null);

+    public static IResult Text(string? content, string? contentType, Encoding? contentEncoding)
+    public static IResult Text(string? content, string? contentType = null, Encoding? contentEncoding = null, int? statusCode = null);
}

public static class TypedResults
{
-    public static ContentHttpResult Content(string? content, string? contentType = null, Encoding? contentEncoding = null)
+    public static ContentHttpResult Content(string content, string? contentType, Encoding? contentEncoding);
+    public static ContentHttpResult Content(string? content, string? contentType = null, Encoding? contentEncoding = null, int? statusCode = null)

-    public static ContentHttpResult Text(string? content, string? contentType = null, Encoding? contentEncoding = null)
+    public static ContentHttpResult Text(string? content, string? contentType, Encoding? contentEncoding)
+    public static ContentHttpResult Text(string? content, string? contentType = null, Encoding? contentEncoding = null, int? statusCode = null);
}
```
