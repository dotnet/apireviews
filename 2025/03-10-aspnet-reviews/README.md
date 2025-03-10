# API Review 03/10/2025

## Support unified model for DataAnnotations-based validation via source generator

**Approved** | [#aspnetcore/46349](https://github.com/dotnet/aspnetcore/issues/46349#issuecomment-2711474254)

* The methods returning ValueTask probably want to return Task instead, as they are unlikely to use the completion source implementation.
* The constructors on the abstract types should be declared protected for consistency.
* "ValidateContext" should be "ValidationContext".
  * The incorrect name was chosen to avoid a conflict with a ValidationContext type in System.ComponentModel, which is involved in this flow.
  * This new context type is proposed to move into System.ComponentModel, so further bikeshedding on the name isn't valuable in this meeting (it'll be debated in the .NET BCL API Review meeting)
* The design of ValidateContext (abstract with no members) leads to very low usability.  DefaultValidateContext is just being merged down.
* ValidateContext.Prefix needs a better name.  e.g. "Path", "FullName", "Scope", ...
* ValidationOptions.Resolvers probably wants to be IList with a non-public type (to permit future customization)
* TODO: Find a better namespace and library

```c#
// Assembly: Microsoft.AspNetCore.Http.Abstractions

namespace Microsoft.AspNetCore.Http.Validation;

public interface IValidatableInfo
{
  ValueTask ValidateAsync(object? value, ValidateContext context, CancellationToken cancellationToken);
}

public abstract class ValidatablePropertyInfo : IValidateInfo
{
  protected ValidatablePropertyInfo(
        Type declaringType,
        Type propertyType,
        string name,
        string displayName);

  protected abstract ValidationAttribute[] GetValidationAttributes();

  public virtual ValueTask ValidateAsync(object? value, ValidateContext context, CancellationToken cancellationToken);
}

public abstract class ValidatableParameterInfo : IValidateInfo
{
  protected ValidatablePropertyInfo(
        Type parameterType,
        string name,
        string displayName);

  protected abstract ValidationAttribute[] GetValidationAttributes();

  public virtual ValueTask ValidateAsync(object? value, ValidateContext context, CancellationToken cancellationToken);
}

public abstract class ValidatableTypeInfo : IValidateInfo
{
  protected ValidatableTypeInfo(
        Type type,
        IEnumerable<ValidatablePropertyInfo> members);

  protected abstract ValidationAttribute[] GetValidationAttributes();

  public virtual ValueTask ValidateAsync(object? value, ValidateContext context, CancellationToken cancellationToken);
}

public sealed class ValidateContext
{
    public ValidationContext? ValidationContext { get; set; }
    public string Prefix { get; set; }
    public int CurrentDepth { get; set; }
    public required ValidationOptions ValidationOptions { get; set; }
    public Dictionary<string, string[]>? ValidationErrors { get; set; }
}

public interface IValidatableInfoResolver
{
    bool TryGetValidatableTypeInfo(Type type, [NotNullWhen(true)] out IValidatableInfo? validatableTypeInfo);
    bool TryGetValidatableParameterInfo(ParameterInfo parameterInfo, [NotNullWhen(true)] out IValidatableInfo? validatableParameterInfo);
}

public class ValidationOptions
{
    public IList<IValidatableInfoResolver> Resolvers { get; } = [];
    public int MaxDepth { get; set; } = 32;

    public bool TryGetValidatableTypeInfo(Type type, [NotNullWhen(true)] out IValidatableInfo? validatableTypeInfo);
    public bool TryGetValidatableParameterInfo(ParameterInfo parameterInfo, [NotNullWhen(true)] out IValidatableInfo? validatableParameterInfo);
}

[AttributeUsage(AttributeTargets.Class)]
public sealed class ValidatableTypeAttribute : Attribute { }

namespace Microsoft.Extensions.DependencyInjection;

public static class ValidationServiceCollectionExtensions
{
    public static IServiceCollection AddValidation(this IServiceCollection services,
        Action<ValidationOptions>? configureOptions);
}

// Assembly: Microsoft.AspNetCore.Routing

namespace Microsoft.AspNetCore.Builder;

public static class ValidationEndpointConventionBuilderExtensions
{
  public static TBuilder DisableValidation<TBuilder>(this TBuilder builder)
        where TBuilder : IEndpointConventionBuilder
} 

// Assembly: Microsoft.AspNetCore.Http.Abstractions

namespace Microsoft.AspNetCore.Http.Metadata;

public interface IDisableValidationMetadata { }
```
