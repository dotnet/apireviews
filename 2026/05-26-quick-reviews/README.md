# API Review 05/26/2026

## ClosedAttribute

**Approved** | [#runtime/128161](https://github.com/dotnet/runtime/issues/128161#issuecomment-4546835005) | [Video](https://www.youtube.com/watch?v=n61wBO24O_E&t=0h0m0s)

* Let's not waste a single-word attribute name on something no one is ever supposed to type.
  * `IsClosedTypeAttribute`

```csharp
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Class, AllowMultiple = false, Inherited = false)]
    public sealed class IsClosedTypeAttribute : Attribute { }
}
```
## Async Validation Support for System.ComponentModel.DataAnnotations

**Approved** | [#runtime/128096](https://github.com/dotnet/runtime/issues/128096#issuecomment-4547451853) | [Video](https://www.youtube.com/watch?v=n61wBO24O_E&t=0h17m52s)


* Rather than have IsValid throw, make it `abstract override` so the user is forced to decide how to be handled by sync callers.
* Changed AsyncValidationAttribute async members to regular Task instead of ValueTask
* Removed IAsyncValidatableObject.IValidatableObject.Validate DIM for parity with `abstract` IsValid
* Discussed whether the Validator methods needed a concurrent collection or if ICollection was good enough, decided ICollection was good enough
* Changed all Validator ValueTask to Task

```csharp
namespace System.ComponentModel.DataAnnotations;

// New abstract class deriving from ValidationAttribute
public abstract class AsyncValidationAttribute : ValidationAttribute
{
    protected AsyncValidationAttribute();
    protected AsyncValidationAttribute(Func<string> errorMessageAccessor);
    protected AsyncValidationAttribute(string errorMessage);

    protected abstract override ValidationResult? IsValid(object? value, ValidationContext validationContext);

    protected abstract Task<ValidationResult?> IsValidAsync(
        object? value,
        ValidationContext validationContext,
        CancellationToken cancellationToken);

    public Task<ValidationResult?> GetValidationResultAsync(
        object? value,
        ValidationContext validationContext,
        CancellationToken cancellationToken = default);
}

public interface IAsyncValidatableObject : IValidatableObject
{
    IAsyncEnumerable<ValidationResult> ValidateAsync(
        ValidationContext validationContext,
        CancellationToken cancellationToken = default);
}

public static partial class Validator
{
    public static Task<bool> TryValidateObjectAsync(
        object instance,
        ValidationContext validationContext,
        ICollection<ValidationResult>? validationResults,
        CancellationToken cancellationToken = default);

    public static Task<bool> TryValidateObjectAsync(
        object instance,
        ValidationContext validationContext,
        ICollection<ValidationResult>? validationResults,
        bool validateAllProperties,
        CancellationToken cancellationToken = default);

    public static Task<bool> TryValidatePropertyAsync(
        object? value,
        ValidationContext validationContext,
        ICollection<ValidationResult>? validationResults,
        CancellationToken cancellationToken = default);

    public static Task<bool> TryValidateValueAsync(
        object? value,
        ValidationContext validationContext,
        ICollection<ValidationResult>? validationResults,
        IEnumerable<ValidationAttribute> validationAttributes,
        CancellationToken cancellationToken = default);

    public static Task ValidateObjectAsync(
        object instance,
        ValidationContext validationContext,
        CancellationToken cancellationToken = default);

    public static Task ValidateObjectAsync(
        object instance,
        ValidationContext validationContext,
        bool validateAllProperties,
        CancellationToken cancellationToken = default);

    public static Task ValidatePropertyAsync(
        object? value,
        ValidationContext validationContext,
        CancellationToken cancellationToken = default);

    public static Task ValidateValueAsync(
        object? value,
        ValidationContext validationContext,
        IEnumerable<ValidationAttribute> validationAttributes,
        CancellationToken cancellationToken = default);
}
```
## Async Options Validation for Microsoft.Extensions.Options

**Approved** | [#runtime/128100](https://github.com/dotnet/runtime/issues/128100#issuecomment-4547778865) | [Video](https://www.youtube.com/watch?v=n61wBO24O_E&t=1h31m25s)


* Changed ValueTask to Task to match what we did for the related issue
* Decided it's correct that IAsyncValidateOptions does not extend IValidateOptions
* OptionsBuilderExtensions, we cut ValidateOnStartAsync.  ValidateOnStart will just prefer async.  If we need a "prefer sync" mode, we can add a bool-overload later.
* Removed the EB-Never AsyncValidationState type and related machinery
* Moved the OptionsBuilder async validation registration methods to just be instance methods on OptionsBuilder.

```csharp
namespace Microsoft.Extensions.Options;

// New interface: async counterpart to IValidateOptions<T>
public interface IAsyncValidateOptions<in TOptions> where TOptions : class
{
    Task<ValidateOptionsResult> ValidateAsync(
        string? name,
        TOptions options,
        CancellationToken cancellationToken = default);
}

// New interface: async counterpart to IStartupValidator
public interface IAsyncStartupValidator
{
    Task ValidateAsync(CancellationToken cancellationToken = default);
}

// New class: async lambda-based validator (0 dependencies)
public class AsyncValidateOptions<TOptions> : IAsyncValidateOptions<TOptions>
    where TOptions : class
{
    public AsyncValidateOptions(string? name,
        Func<TOptions, CancellationToken, Task<bool>> validation,
        string failureMessage);
    public string? Name { get; }
    public Func<TOptions, CancellationToken, Task<bool>> Validation { get; }
    public string FailureMessage { get; }
    public Task<ValidateOptionsResult> ValidateAsync(
        string? name, TOptions options, CancellationToken cancellationToken = default);
}

// New class: async lambda-based validator (1 dependency)
public class AsyncValidateOptions<TOptions, TDep> : IAsyncValidateOptions<TOptions>
    where TOptions : class
{
    public AsyncValidateOptions(string? name, TDep dependency,
        Func<TOptions, TDep, CancellationToken, Task<bool>> validation,
        string failureMessage);
    public string? Name { get; }
    public TDep Dependency { get; }
    public Func<TOptions, TDep, CancellationToken, Task<bool>> Validation { get; }
    public string FailureMessage { get; }
    public Task<ValidateOptionsResult> ValidateAsync(
        string? name, TOptions options, CancellationToken cancellationToken = default);
}

// ... AsyncValidateOptions<TOptions, TDep1, TDep2> through <TOptions, TDep1..TDep5>
// (same pattern as existing sync ValidateOptions<T, TDep1..TDep5>)

public partial class OptionsBuilder<TOptions>
{
    public virtual OptionsBuilder<TOptions> Validate(
        Func<TOptions, CancellationToken, Task<bool>> validation,
        string failureMessage);

    // 1 dependency
    public virtual OptionsBuilder<TOptions> Validate<TDep>(
        Func<TOptions, TDep, CancellationToken, Task<bool>> validation,
        string failureMessage) where TDep : notnull;

    // ... up to 5 dependencies (same pattern as sync Validate<T, TDep1..TDep5>)
}
```
