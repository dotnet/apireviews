# API Review 09/15/2026

## AddHttpLatencyTelemetry for incoming HTTP request logs

**Approved** | [#extensions/7729](https://github.com/dotnet/extensions/issues/7729#issuecomment-5685739334) | [Video](https://www.youtube.com/watch?v=ykgQkeaSsQ4&t=0h0m0s)

* The new registration method seems like it fits in with HttpLoggingServiceCollectionExtensions without us needing a new class.
* Since there is already an AddHttpClientLatencyTelemetry registration method we felt that including the word "Server" helps provide the role clarity.

```csharp
namespace Microsoft.Extensions.DependencyInjection;

public static partial class HttpLoggingServiceCollectionExtensions
{
    public static IServiceCollection AddHttpServerLatencyTelemetry(this IServiceCollection services);
}
```
## Add RequireNamedArgumentAttribute

**Approved** | [#runtime/132924](https://github.com/dotnet/runtime/issues/132924#issuecomment-5686007524) | [Video](https://www.youtube.com/watch?v=ykgQkeaSsQ4&t=0h22m47s)


* Since it's supposed to be typed by a user, it shouldn't be in CompilerServices.  System.Diagnostics.CodeAnalysis seems better.
* The name of the feature is "Named Arguments" (plural), and when used on a method/etc it looks weird as a singular.  It's slightly odd for a plural on a single parameter, but it's just the name of the feature, so it sort of works.  Lets add the "s" on the end.
* Let's go ahead and add a C# analyzer that respects it.  Category: Usage, Severity: Info

```csharp
namespace System.Diagnostics.CodeAnalysis;

[AttributeUsage(
    AttributeTargets.Method |
    AttributeTargets.Constructor |
    AttributeTargets.Property |
    AttributeTargets.Delegate |
    AttributeTargets.Parameter,
    AllowMultiple = false,
    Inherited = false)]
public sealed class RequireNamedArgumentsAttribute : Attribute
{
    public RequireNamedArgumentsAttribute();
}
```
## Don't include non-`IEquatable` structs in records

**Approved** | [#runtime/102593](https://github.com/dotnet/runtime/issues/102593#issuecomment-5686396601) | [Video](https://www.youtube.com/watch?v=ykgQkeaSsQ4&t=0h49m34s)

Other than a potential for noise complaints, there doesn't seem to be a downside, so let's do it.

* Category: Performance
* Severity: Info
