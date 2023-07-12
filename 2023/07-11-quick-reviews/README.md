# API Review 07/11/2023

## ConfigureHttpClientDefaults for HttpClientFactory

**Approved** | [#runtime/87914](https://github.com/dotnet/runtime/issues/87914#issuecomment-1631218580) | [Video](https://www.youtube.com/watch?v=XrJyFBDKi-4&t=0h0m0s)

* Looks good as proposed

```C#
namespace Microsoft.Extensions.DependencyInjection;

public static partial class HttpClientFactoryServiceCollectionExtensions
{
    public static IServiceCollection ConfigureHttpClientDefaults(
        this IServiceCollection services,
        Action<IHttpClientBuilder> configure);
}

public static partial class HttpClientBuilderExtensions
{
    // Existing:
    //
    // public static IHttpClientBuilder ConfigurePrimaryHttpMessageHandler(
    //    this IHttpClientBuilder builder,
    //    Func<HttpMessageHandler> configureHandler);
    //
    // public static IHttpClientBuilder AddHttpMessageHandler(
    //    this IHttpClientBuilder builder,
    //    Func<DelegatingHandler> configureHandler);

    // new
    public static IHttpClientBuilder ConfigurePrimaryHttpMessageHandler(
        this IHttpClientBuilder builder,
        Action<HttpMessageHandler, IServiceProvider> configureHandler);

    public static IHttpClientBuilder ConfigureAdditionalHttpMessageHandlers(
        this IHttpClientBuilder builder,
        Action<IList<DelegatingHandler>, IServiceProvider> configureAdditionalHandlers);
    
    // Existing API which we should obsolete

    [Obsolete(...)] // Should point to ConfigureAdditionalHttpMessageHandlers 
    public static IHttpClientBuilder ConfigureHttpMessageHandlerBuilder(
        this IHttpClientBuilder builder,
        Action<HttpMessageHandlerBuilder> configureBuilder);
}
```
## HttpClientFactory logging configuration

**Approved** | [#runtime/77312](https://github.com/dotnet/runtime/issues/77312#issuecomment-1631264572) | [Video](https://www.youtube.com/watch?v=XrJyFBDKi-4&t=0h25m51s)

* Let's merge the logging builder with the client builder
* We should offer sync versions

```C#
namespace Microsoft.Extensions.Http.Logging
{
    public interface IHttpClientLogger
    {
        object? LogRequestStart(
            HttpRequestMessage request);

        void LogRequestStop(
            object? context,
            HttpRequestMessage request,
            HttpResponseMessage response,
            TimeSpan elapsed);

        void LogRequestFailed(
            object? context,
            HttpRequestMessage request,
            HttpResponseMessage? response,
            Exception exception,
            TimeSpan elapsed);
    }

    public interface IHttpClientAsyncLogger : IHttpClientLogger
    {
        ValueTask<object?> LogRequestStartAsync(
            HttpRequestMessage request,
            CancellationToken cancellationToken = default);

        ValueTask LogRequestStopAsync(
            object? context,
            HttpRequestMessage request,
            HttpResponseMessage response,
            TimeSpan elapsed,
            CancellationToken cancellationToken = default);

        ValueTask LogRequestFailedAsync(
            object? context,
            HttpRequestMessage request,
            HttpResponseMessage? response,
            Exception exception,
            TimeSpan elapsed,
            CancellationToken cancellationToken = default);
    }
}

namespace Microsoft.Extensions.DependencyInjection
{
    public partial static class HttpClientBuilderExtensions
    {
        public static IHttpClientBuilder AddLogger(
            this IHttpClientBuilder builder,
            Func<IServiceProvider, IHttpClientLogger> httpClientLoggerFactory,
            bool wrapHandlersPipeline = false);

        public static IHttpClientBuilder RemoveAllLoggers(
            this IHttpClientBuilder builder
        );

        public static IHttpClientBuilder AddDefaultLogger(
            this IHttpClientBuilder builder);

        public static IHttpClientBuilder AddLogger<TLogger>(
            this IHttpClientBuilder builder,
            bool wrapHandlersPipeline = false)
            where TLogger : IHttpClientLogger;
    }
}
```
