# API Review 02/08/2022

## Improve HostBuilder and WebApplicationBuilder Configuration integration

**Approved** | [#runtime/61634](https://github.com/dotnet/runtime/issues/61634#issuecomment-1033038101) | [Video](https://www.youtube.com/watch?v=r4wqWX0kVgA&t=0h0m0s)

* Consider making the constructor-based flow for `HostApplicationBuilder` the "canonical" way moving forward
    - That doesn't mean we should not have the factory methods on `Host` to aid discoverability for existing code, but it would mean that we think about the constructors not as an advanced thing, but as something that should be usable by itself; e.g. maybe we should have one that takes `string[] args`.
* We'd prefer to drop `Host` from `HostApplicationBuilder` but ASP.NET Core already has `IApplicationBuilder` that wouldn't be implemented by this type, which would be confusing.
* `HostApplicationBuilder` doesn't implement `IHostBuilder` which means non of the existing methods will work.
    - Based on what @halter73 said, we don't believe this to be a problem and in fact wouldn't want this due to mismatches in capabilities.
    - @davidfowl suggested to look at GitHub and see what `UseXxx` extensions people actually have
* We haven't really used the host builder pattern outside of web; MAUI has just started. We should validate that the concepts hold water outside of web.
* @davidfowl has concerns that `Build()` taking the factory might not work
    - Suggestion was to replace that with the other existing pattern `ConfigureContainer<TBuilder>()`
* It was suggested that `HostApplicationBuilderSettings.Configuration` should be `IConfiguration` but the idea is that `HostApplicationBuilder` will be set to that instance if it's provided, hence it makes more sense to be typed as `ConfigurationManager`.
* We need to figure out how ASP.NET Core will use `HostApplicationBuilder` will it derive from it or will it wrap it? Based on that we should either make this type sealed or inheritable.

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
}
```
