# API Review 04/03/2023

## Make Http Logging Middleware Endpoint aware

**Approved** | [#aspnetcore/43222](https://github.com/dotnet/aspnetcore/issues/43222#issuecomment-1494740637)

API review notes:
* DisableLoggingAttribute and DisableLogging method aren't actually needed, you can use HttpLoggingFields.None for the same effect
* Rename EnableLoggingAttribute to HttpLoggingAttribute
* Rename EnableLogging method to ...?
  - SetHttpLogging
  - WithHttpLogging
  - LogHttpFields
  - Log
  - LogHttp
* In the future if we want more settings in `WithHttpLogging` we could add an overload that takes `HttpLoggingOptions`.
  - FYI, if we did this, FieldSelector would be settable again 😆 
* Precedence:
  - FieldSelector
  - Attribute/Endpoint extension method
  - Options

API Approved!

```diff
namespace Microsoft.AspNetCore.HttpLogging;

+ public class HttpLoggingAttribute : Attribute
+{
+    public HttpLoggingAttribute(HttpLoggingFields loggingFields, int? requestBodyLogLimit = null, int? responseBodyLogLimit = null) { }
+    public HttpLoggingFields LoggingFields { get; }
+    public int? RequestBodyLogLimit { get; }
+    public int? ResponseBodyLogLimit { get; }
+}

namespace Microsoft.AspNetCore.Builder;

+public static class HttpLoggingEndpointConventionBuilderExtensions
+{
+    public static TBuilder WithHttpLogging<TBuilder>(this TBuilder builder, HttpLoggingFields loggingFields, int? requestBodyLogLimit = null, int? responseBodyLogLimit = null) where TBuilder : IEndpointConventionBuilder;
+}
```
## HttpLoggingMiddleware could allow custom code to decide whether to log or not

**NeedsWork** | [#aspnetcore/39200](https://github.com/dotnet/aspnetcore/issues/39200#issuecomment-1494780017)

API Review Notes:
* We would like feature parity with the attributes in https://github.com/dotnet/aspnetcore/issues/43222, meaning request/response size limits should be settable
* Adding a context object makes this more future proof
* Media type options would be a pain to pass in and allow setting per endpoint
* Similar with request/response headers, would need to copy every time
* Just keep it simple with Fields and body size limits for now, it's a context so we can add things easily in the future!
* **Calling into question why we even need this** if you can fairly easily write a middleware to do the same thing (now that the attribute apis are approved)
  - We aren't doing anything fancy on the users' behalf that they couldn't do themselves
  - caveat, if this was called after response, it would be more useful (hard to do in middleware) since we would be buffering request/response and not logging until you made a decision

API needs feedback still

```diff
namespace Microsoft.AspNetCore.HttpLogging;

+public sealed class HttpLoggingContext
+{
+    public HttpLoggingContext();

+    public HttpContext HttpContext { get; set; }

+    public HttpLoggingFields LoggingFields { get; set; }

+    public int? RequestBodyLogLimit { get; }

+    public int? ResponseBodyLogLimit { get; }
+}

namespace Microsoft.AspNetCore.HttpLogging;

public sealed class HttpLoggingOptions
{
+    public Action<HttpLoggingContext>? HttpLoggingSelector { get; set; }
}
```



