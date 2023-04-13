# API Review 04/13/2023

## Add exception property to the problem details context

**Approved** | [#aspnetcore/47651](https://github.com/dotnet/aspnetcore/issues/47651#issuecomment-1507691880)

API Review Notes:
* Who is setting the exception?
  - We would be in DeveloperException page etc.
* The Exception won't be written by our implementations of `IProblemDetailsWriter`
  - Would be up to the user to put the exception in Extensions in CustomizeProblemDetails if they want that

API Approved!

```diff
namespace Microsoft.AspNetCore.Http;

public sealed class ProblemDetailsContext
{
+   public Exception? Exception { get; init; }
}
```
## Avoid closure in IConnectionBuilder.Use

**Approved** | [#aspnetcore/46533](https://github.com/dotnet/aspnetcore/issues/46533#issuecomment-1507694367)

API Review Notes:
* Prior art with `Use` in IApplicationBuilder
* We like saving allocations!

API Approved!

```diff
namespace Microsoft.AspNetCore.Connections;

public static class ConnectionBuilderExtensions
{
     public static IConnectionBuilder Use(this IConnectionBuilder connectionBuilder, Func<ConnectionContext, Func<Task>, Task> middleware)
+    public static IConnectionBuilder Use(this IConnectionBuilder connectionBuilder, Func<ConnectionContext, Func<ConnectionContext, Task>, Task> middleware)
}
```
## Mapping Exception x StatusCode in ExceptionHandlerMiddleware

**Approved** | [#aspnetcore/44156](https://github.com/dotnet/aspnetcore/issues/44156#issuecomment-1507711981)

API Review notes:
* How would this interact with `AllowStatusCode404Response`?
  - We could ignore that check since we "know" the user set a 404 on purpose
* StatusCodeMapping makes me think you're mapping a status code to an Exception 😆 
  - MapExceptionToStatusCode
  - ExceptionToStatusCodeMapping
  - MapStatusCode
* Why are we doing this if it's already possible in the `ExceptionHandler` request delegate?
  - i.e. is it too complicated to implement for the average user?
  - It's hard because the ProblemDetails writing is handled *only* if the `ExceptionHandler` isn't set
* How are AggregateExceptions handled?
  - Don't handle it specially, users either map AggregateException to a status code or get todays behavior.
* `Func<Exception, int>` would allow users to look at the exception and provide the status code they want, and not inadvertently write to the response if we passed in HttpContext, this helps with the ProblemDetails handling in the middleware
  - StatusCodeSelector

API Approved!

```diff
namespace Microsoft.AspNetCore.Builder;

public class ExceptionHandlerOptions
{
+    public Func<Exception, int>? StatusCodeSelector { get; set; }
}
```
## Introduce trim-friendly APIs for controlling HTTPS Configuration support

**Approved** | [#aspnetcore/47567](https://github.com/dotnet/aspnetcore/issues/47567#issuecomment-1507722242)

API Review notes:
* UseHttpsConfiguration is on IWebHostBuilder which is odd since it's for kestrel
  - Name it UseKestrelHttpsConfiguration to make it clearer that it's for kestrel
  - If this was on KestrelOptions it would heavily complicate what user code has to do to get this behavior
  - We're in favor of a simple single line call to enable the functionality
* Still aren't a fan of Slim suffix
  - Core has been used in other APIs like AddMvcCore, AddSignalRCore, AddRoutingCore, but those are more like the "core" services needed for those areas
  - This is for an opinionated trimmed Kestrel, not the "core" services to use Kestrel
  - Core was voted on by peers
* Remove other overloads of UseKestrelCore (or whatever we name it)
  - Those APIs are for convenience and this API isn't exactly designed for that

API Approved!

```diff
namespace Microsoft.AspNetCore.Hosting;

public static class WebHostBuilderKestrelExtensions
{
+    public static IWebHostBuilder UseKestrelHttpsConfiguration(this IWebHostBuilder hostBuilder);

+    public static IWebHostBuilder UseKestrelCore(this IWebHostBuilder hostBuilder);
}
```
