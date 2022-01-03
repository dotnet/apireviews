# API Review 01/03/2022

## Adding EntryAssembly to AddRazorPages/AddControllers*

**Approved** | [#aspnetcore/39126](https://github.com/dotnet/aspnetcore/issues/39126#issuecomment-1004284775)

* Let's make the configure non-nullable in the API (without null-checking it so users can pass in a null if they really want to). e.g.

```diff
+ public static IMvcBuilder AddControllers(this IServiceCollection services, Assembly entryAssembly) 
{}

+ public static IMvcBuilder AddControllers(this IServiceCollection services, Assembly entryAssembly, Action<MvcOptions> configure)
+      {}
```

Given that the API touches the startup code, and the plan is to update the templates we should run it by @davidfowl  / @DamianEdwards 

## Use JSON Property Name attributes when creating ModelState Validation errors

**Approved** | [#aspnetcore/39010](https://github.com/dotnet/aspnetcore/issues/39010#issuecomment-1004289017)

Let's seal these types and name them to be more explicit about what they are associated with (STJ vs NewtonsoftJson and that they have to do with validation)

```diff
- public class JsonMetadataProvider
+ public sealed class SystemTextJsonValidationMetadataProvider

-  public class NewtonsoftJsonMetadataProvider : IDisplayMetadataProvider, IValidationMetadataProvider
+ public sealed class NewtonsoftJsonValidationMetadataProvider : IDisplayMetadataProvider, IValidationMetadataProvider
```
## A better implementation to the definition of IUrlHelper.IsLocalUrl

**Rejected** | [#aspnetcore/39119](https://github.com/dotnet/aspnetcore/issues/39119#issuecomment-1004290902)

Thanks for the issue report. We don't think this API has crisp details (what happens if the app has multiple hosts etc) and consequently don't think this is a valid API to consider. 
