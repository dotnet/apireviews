# API Review 03/15/2022

## Let consumers of MemoryCache access metrics

**Approved** | [#runtime/50406](https://github.com/dotnet/runtime/issues/50406#issuecomment-1068300016) | [Video](https://www.youtube.com/watch?v=BMz1BmMASVA&t=0h0m0s)

The feature owners have agreed that they're willing to split the reference assembly to try out a Default Interface Method.

We also changed the statistics type to a class (partially from future expansion concerns).

```C#
    public static partial class MemoryCache
    {
        public MemoryCacheStatistics? GetCurrentStatistics() { throw null; }
    }

    public partial interface IMemoryCache
    {
#if NET_7_OR_GREATER
        public MemoryCacheStatistics? GetCurrentStatistics() => null;
#endif
    }

    public partial class MemoryCacheStatistics
    {
        public MemoryCacheStatistics() {}
        
        public long TotalMisses { get; init; } 
        public long TotalHits { get; init; }
        public long CurrentEntryCount { get; init; }
        public long? CurrentEstimatedSize { get; init; }
    }
```
## StopTheHostException should be made public to be dealt with gracefully

**Approved** | [#runtime/60600](https://github.com/dotnet/runtime/issues/60600#issuecomment-1068323222) | [Video](https://www.youtube.com/watch?v=BMz1BmMASVA&t=1h5m21s)

We renamed `StopTheHostException` to `HostAbortedException` now that it's a public type.  (And sealed it, and added [Serializable] for .NET Framework interop concerns)

```C#
namespace Microsoft.Extensions.Hosting
{
    [Serializable]
    public sealed partial class HostAbortedException : Exception
    {
        public HostAbortedException() { }
        private HostAbortedException(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
        public HostAbortedException(string? message) { }
        public HostAbortedException(string? message, System.Exception? innerException) { }
    }
}
```
## Add feature to bind a single element to an array on ConfigurationBinder.

**NeedsWork** | [#runtime/57325](https://github.com/dotnet/runtime/issues/57325#issuecomment-1068328386) | [Video](https://www.youtube.com/watch?v=BMz1BmMASVA&t=1h25m7s)

The feature area didn't seem convinced that this is the approach they wanted to take, marking as needs-work until they are.
## Improve HostBuilder and WebApplicationBuilder Configuration integration

**Approved** | [#runtime/61634](https://github.com/dotnet/runtime/issues/61634#issuecomment-1068335664) | [Video](https://www.youtube.com/watch?v=BMz1BmMASVA&t=1h30m13s)

Instead of making new types for the feature parity extension methods, we think it's best to put them on their (slightly inappropriately named) existing types.

```C#
namespace Microsoft.Extensions.Hosting
{
     public static partial class Host
     {
        public static HostApplicationBuilder CreateApplicationBuilder();
        public static HostApplicationBuilder CreateApplicationBuilder(string[] args);
        public static IHostBuilder CreateDefaultBuilder();
        public static IHostBuilder CreateDefaultBuilder(string[] args);
    }
    public sealed class HostApplicationBuilder
    {
        public HostApplicationBuilder();
        public HostApplicationBuilder(string[] args);
        public HostApplicationBuilder(HostApplicationBuilderSettings settings);
        public ConfigurationManager Configuration { get; }
        public IHostEnvironment Environment { get; }
        public ILoggingBuilder Logging { get; }
        public IServiceCollection Services { get; }
        public IHost Build();
        public void ConfigureContainer<TBuilder>(IServiceProviderFactory<TBuilder> factory, Action<TBuilder>? configure = null) where TBuilder : notnull;
    }
    public sealed class HostApplicationBuilderSettings
    {
        public HostApplicationBuilderSettings();
        public string ApplicationName { get; set; }
        public string[] Args { get; set; }
        public string ContentRootPath { get; set; }
        public bool DisableDefaults { get; set; }
        public string EnvironmentName { get; set; }
        public ConfigurationManager Configuration { get; set; }
    }
    public partial static class SystemdHostBuilderExtensions
    {
        public static IServiceCollection UseSystemd(this IServiceCollection services);
    }
    public partial static class WindowsServiceLifetimeHostBuilderExtensions
    {
        public static IServiceCollection UseWindowsService(this IServiceCollection services);
        public static IServiceCollection UseWindowsService(this IServiceCollection services, Action<WindowsServiceLifetimeOptions> configure);
    }
}
```
## RateLimiter should implement IDisposable and IAsyncDisposable

**Approved** | [#runtime/62370](https://github.com/dotnet/runtime/issues/62370#issuecomment-1068357166) | [Video](https://www.youtube.com/watch?v=BMz1BmMASVA&t=1h38m52s)

Looks good as proposed

```C#
namespace System.Threading.RateLimiting
{
    public partial class RateLimiter : IAsyncDisposable, IDisposable
    {
        protected virtual void Dispose(bool disposing) { }

        public void Dispose() { }

        protected virtual ValueTask DisposeAsyncCore() { }

        public ValueTask DisposeAsync() { }
    }
}
```
