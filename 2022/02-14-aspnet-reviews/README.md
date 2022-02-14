# API Review 02/14/2022

## Allow customizing the cookie consent value

**Approved** | [#aspnetcore/40164](https://github.com/dotnet/aspnetcore/issues/40164)

## Consider adding support for `TryParse` as a way to bind primitives

**Approved** | [#aspnetcore/39682](https://github.com/dotnet/aspnetcore/issues/39682#issuecomment-1039424654)

API review:

Approved with notes: 

* We can start off by leaving the binder internal
* We should seal the binder provider.

```diff
+ public sealed class TryParseModelBinderProvider : IModelBinderProvider
+ {
+     public IModelBinder? GetBinder(ModelBinderProviderContext context);
+ }
```


## Allow Use of IConfigureOptions Consistently Everywhere

**Approved** | [#aspnetcore/39251](https://github.com/dotnet/aspnetcore/issues/39251#issuecomment-1039436292)

API review:
* Don't change existing overloads.
* For existing APIs that are missing the overload that does not accept an options callback, we should add one. For e.g we should do this:

```diff
+ IServiceCollection AddStackExchangeRedisCache(this IServiceCollection services)
IServiceCollection AddStackExchangeRedisCache(this IServiceCollection services, Action<RedisCacheOptions> setupAction)

```

* For new ones, we think standardize on using the pattern of having two overloads. This should allow for new options to be introduced in a future release without deviating from the pattern and also conforms to existing patterns in the framework.

```C#
public static IServerSideBlazorBuilder AddMyFeature(this IServiceCollection services);
public static IServerSideBlazorBuilder AddMyFeature(this IServiceCollection services, Action<MyFeatureOption> configureOptions) ;
```

`configureOptions` in the above example can be allowed to be soft-allow null values (i.e. not perform null checks) for the sake of brevity of implementation.

* In the event we only need the options type to configured without adding any other services, we should standardize on `Configure???` as the name for the method e.g. `ConfigureMyFeatureOptions(Action<MyFeatureOptions> configureOptions)` where `configureOptions` is non-null.

Separately we'll approve the changes proposed in this issue:

```diff
+ IServiceCollection AddStackExchangeRedisCache(this IServiceCollection services)
+ IServiceCollection AddHsts(this IServiceCollection services)
```
