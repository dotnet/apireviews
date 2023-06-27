# API Review 06/27/2023

## Add `IHostedLifecycleService` to support additional callbacks

**Approved** | [#runtime/86511](https://github.com/dotnet/runtime/issues/86511#issuecomment-1610015840) | [Video](https://www.youtube.com/watch?v=PwB593iwQlk&t=0h0m0s)

* Looks good as proposed
* Should `BackgroundService` implement `IHostedLifecycleService`?

```C#
namespace Microsoft.Extensions.Hosting;

public interface IHostedLifecycleService : IHostedService
{
    Task StartingAsync(CancellationToken cancellationToken);
    Task StartedAsync(CancellationToken cancellationToken);
    Task StoppingAsync(CancellationToken cancellationToken);
    Task StoppedAsync(CancellationToken cancellationToken);
}

public partial class HostOptions
{
    public TimeSpan StartupTimeout { get; set; }
}
```
## Add Keyed Services Support to Dependency Injection

**Approved** | [#runtime/64427](https://github.com/dotnet/runtime/issues/64427#issuecomment-1610060118) | [Video](https://www.youtube.com/watch?v=PwB593iwQlk&t=0h24m42s)

* `ISupportKeyedService` should be `IKeyedServiceProvider` and derive from `IServiceProvider`
* `IServiceProviderIsServiceKeyed` should be `IKeyedServiceProviderIsService`
* `IKeyedServiceProviderIsService.IsService` should be called `IsKeyedService`

```C#
namespace Microsoft.Extensions.DependencyInjection;

public partial class ServiceDescriptor
{
    public object? ServiceKey { get; }

    public object? KeyedImplementationInstance { get; }

    public Type? KeyedImplementationType { get; }

    public Func<IServiceProvider, object, object>? KeyedImplementationFactory { get; }

    public bool IsKeyedService { get; }
}

public static partial class ServiceCollectionServiceExtensions
{
    public static IServiceCollection AddKeyedScoped(this IServiceCollection services, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type serviceType, object serviceKey);
    public static IServiceCollection AddKeyedScoped(this IServiceCollection services, Type serviceType, object serviceKey, Func<IServiceProvider, object, object> implementationFactory);
    public static IServiceCollection AddKeyedScoped(this IServiceCollection services, Type serviceType, object serviceKey, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type implementationType);
    public static IServiceCollection AddKeyedScoped<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TService>(this IServiceCollection services, object serviceKey) where TService : class;
    public static IServiceCollection AddKeyedScoped<TService>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TService> implementationFactory) where TService : class;
    public static IServiceCollection AddKeyedScoped<TService, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TImplementation>(this IServiceCollection services, object serviceKey) where TService : class where TImplementation : class, TService;
    public static IServiceCollection AddKeyedScoped<TService, TImplementation>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TImplementation> implementationFactory) where TService : class where TImplementation : class, TService;
    public static IServiceCollection AddKeyedSingleton(this IServiceCollection services, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type serviceType, object serviceKey);
    public static IServiceCollection AddKeyedSingleton(this IServiceCollection services, Type serviceType, object serviceKey, Func<IServiceProvider, object, object> implementationFactory);
    public static IServiceCollection AddKeyedSingleton(this IServiceCollection services, Type serviceType, object serviceKey, object implementationInstance);
    public static IServiceCollection AddKeyedSingleton(this IServiceCollection services, Type serviceType, object serviceKey, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type implementationType);
    public static IServiceCollection AddKeyedSingleton<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TService>(this IServiceCollection services, object serviceKey) where TService : class;
    public static IServiceCollection AddKeyedSingleton<TService>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TService> implementationFactory) where TService : class;
    public static IServiceCollection AddKeyedSingleton<TService>(this IServiceCollection services, object serviceKey, TService implementationInstance) where TService : class;
    public static IServiceCollection AddKeyedSingleton<TService, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TImplementation>(this IServiceCollection services, object serviceKey) where TService : class where TImplementation : class, TService;
    public static IServiceCollection AddKeyedSingleton<TService, TImplementation>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TImplementation> implementationFactory) where TService : class where TImplementation : class, TService;
    public static IServiceCollection AddKeyedTransient(this IServiceCollection services, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type serviceType, object serviceKey);
    public static IServiceCollection AddKeyedTransient(this IServiceCollection services, Type serviceType, object serviceKey, Func<IServiceProvider, object, object> implementationFactory);
    public static IServiceCollection AddKeyedTransient(this IServiceCollection services, Type serviceType, object serviceKey, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type implementationType);
    public static IServiceCollection AddKeyedTransient<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TService>(this IServiceCollection services, object serviceKey) where TService : class;
    public static IServiceCollection AddKeyedTransient<TService>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TService> implementationFactory) where TService : class;
    public static IServiceCollection AddKeyedTransient<TService, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TImplementation>(this IServiceCollection services, object serviceKey) where TService : class where TImplementation : class, TService;
    public static IServiceCollection AddKeyedTransient<TService, TImplementation>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TImplementation> implementationFactory) where TService : class where TImplementation : class, TService;

    public static void TryAddKeyedScoped(this IServiceCollection collection, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type service, object serviceKey);
    public static void TryAddKeyedScoped(this IServiceCollection collection, Type service, object serviceKey, Func<IServiceProvider, object, object> implementationFactory);
    public static void TryAddKeyedScoped(this IServiceCollection collection, Type service, object serviceKey, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type implementationType);
    public static void TryAddKeyedScoped<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TService>(this IServiceCollection collection, object serviceKey) where TService : class;
    public static void TryAddKeyedScoped<TService>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TService> implementationFactory) where TService : class;
    public static void TryAddKeyedScoped<TService, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TImplementation>(this IServiceCollection collection, object serviceKey) where TService : class where TImplementation : class, TService;
    public static void TryAddKeyedSingleton(this IServiceCollection collection, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type service, object serviceKey);
    public static void TryAddKeyedSingleton(this IServiceCollection collection, Type service, object serviceKey, Func<IServiceProvider, object, object> implementationFactory);
    public static void TryAddKeyedSingleton(this IServiceCollection collection, Type service, object serviceKey, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type implementationType);
    public static void TryAddKeyedSingleton<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TService>(this IServiceCollection collection, object serviceKey) where TService : class;
    public static void TryAddKeyedSingleton<TService>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TService> implementationFactory) where TService : class;
    public static void TryAddKeyedSingleton<TService>(this IServiceCollection collection, object serviceKey, TService instance) where TService : class;
    public static void TryAddKeyedSingleton<TService, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TImplementation>(this IServiceCollection collection, object serviceKey) where TService : class where TImplementation : class, TService;
    public static void TryAddKeyedTransient(this IServiceCollection collection, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type service, object serviceKey);
    public static void TryAddKeyedTransient(this IServiceCollection collection, Type service, object serviceKey, Func<IServiceProvider, object, object> implementationFactory);
    public static void TryAddKeyedTransient(this IServiceCollection collection, Type service, object serviceKey, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] Type implementationType);
    public static void TryAddKeyedTransient<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TService>(this IServiceCollection collection, object serviceKey) where TService : class;
    public static void TryAddKeyedTransient<TService>(this IServiceCollection services, object serviceKey, Func<IServiceProvider, object, TService> implementationFactory) where TService : class;
    public static void TryAddKeyedTransient<TService, [DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors)] TImplementation>(this IServiceCollection collection, object serviceKey) where TService : class where TImplementation : class, TService;

    public static IServiceCollection RemoveAllKeyed(this IServiceCollection collection, Type serviceType, object serviceKey);
    public static IServiceCollection RemoveAllKeyed<T>(this IServiceCollection collection, object serviceKey);
}

public interface IKeyedServiceProvider : IServiceProvider
{
    object? GetKeyedService(Type serviceType, object serviceKey);
    object GetRequiredKeyedService(Type serviceType, object serviceKey);
}

public interface IKeyedServiceProviderIsService
{
    bool IsKeyedService(Type serviceType, object serviceKey);
}

[AttributeUsageAttribute(AttributeTargets.Parameter)]
public class ServiceKeyAttribute : Attribute
{
    public ServiceKeyAttribute();
}

[AttributeUsageAttribute(AttributeTargets.Parameter)]
public class FromKeyedServicesAttribute : Attribute
{
    public FromKeyedServicesAttribute(object key);
    public object Key { get; }
}
```

