# API Review 06/13/2023

## Add `IHostedServiceStartup.StartupAsync()` to support pre-startup scenarios

**NeedsWork** | [#runtime/86511](https://github.com/dotnet/runtime/issues/86511#issuecomment-1589800335) | [Video](https://www.youtube.com/watch?v=gbXInVX3dDI&t=0h0m0s)

* There concerns about the number of 'stages'. This adds one stage, do we end up in world where we need multiple stages.
    - The `IOrderedHostedService` is another option
* What about a shutdown?
* This needs more discussion
    - @halter73, @eerhardt

```C#
namespace Microsoft.Extensions.Hosting
{
    public interface IHostedServiceStartup
    {
        Task StartupAsync(CancellationToken cancellationToken = default);
    }
}

// For consistency with other IServiceCollection extensions, use the DependencyInjection namespace
namespace Microsoft.Extensions.DependencyInjection
{
    public static partial class ServiceCollectionHostedServiceExtensions
    {
       public static ServiceCollection AddServiceStartup
           <[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TServiceStartup>(
           this IServiceCollection services)
           where TServiceStartup : class, IHostedServiceStartup;

       public static IServiceCollection AddServiceStartup(
           this IServiceCollection services,
           Func<IServiceProvider, CancellationToken, Task> startupFunc);
    }
}
```
## Add additional LoggerMessageAttribute constructors

**NeedsWork** | [#runtime/87254](https://github.com/dotnet/runtime/issues/87254#issuecomment-1589843966) | [Video](https://www.youtube.com/watch?v=gbXInVX3dDI&t=1h5m36s)

* They can already do this today because the properties are settable
    - We generally try to avoid explosion of constructor overloads
* The bullet points, especially that we consider event id a mistake, is somewhat surprising
    - @noahfalk @geeknoid
    - Adding those constructors is going against our existing Roslyn analyzers
* What does the console logger do for cases where there is no message?
* It would help to see code samples that highlight the reduction of ceremony

```C#
namespace Microsoft.Extensions.Logging;

public sealed partial class LoggerMessageAttribute : Attribute
{
    // Existing:
    // public LoggerMessageAttribute();
    // public LoggerMessageAttribute(int eventId, LogLevel level, string message);

    public LoggerMessageAttribute(LogLevel level, string message);
    public LoggerMessageAttribute(LogLevel level);
    public LoggerMessageAttribute(string message);
}
```
## Fix AddMetrics extension method namespace

**Approved** | [#runtime/86885](https://github.com/dotnet/runtime/issues/86885#issuecomment-1589845482) | [Video](https://www.youtube.com/watch?v=gbXInVX3dDI&t=1h38m42s)

* Looks good as proposed

```diff
-namespace Microsoft.Extensions.Diagnostics.Metrics;
+namespace Microsoft.Extensions.DependencyInjection;

 public static class MetricsServiceExtensions
 {
     public static IServiceCollection AddMetrics(this IServiceCollection services)
 }
```
## Bring back extensions API to ease adoption of extensions libraries from older frameworks

**Approved** | [#runtime/87480](https://github.com/dotnet/runtime/issues/87480#issuecomment-1589871028) | [Video](https://www.youtube.com/watch?v=gbXInVX3dDI&t=1h40m14s)

This plan makes sense as proposed. I don't think it makes sense for us to walk the entire list of APIs so we'll just approve this plan and the stated principles.
