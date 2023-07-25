# API Review 07/25/2023

## BinaryData + JSON source gen produces compiler warnings

**Approved** | [#runtime/81702](https://github.com/dotnet/runtime/issues/81702#issuecomment-1650220626) | [Video](https://www.youtube.com/watch?v=ixR8JORzD0c&t=0h0m0s)

* We should add Json somewhere in the name. It felt right to put before the converter.

```C#
namespace System.Text.Json.Serialization;

public sealed class BinaryDataJsonConverter : JsonConverter<BinaryData>
{
    // Overrides omitted
}
```

## Add CreateParameter() method (and CanCreateParameter) to DbBatchCommand

**Approved** | [#runtime/82326](https://github.com/dotnet/runtime/issues/82326#issuecomment-1650231299) | [Video](https://www.youtube.com/watch?v=ixR8JORzD0c&t=0h6m6s)

* Looks good as proposed

```C#
namespace System.Data.Common;

public partial class DbBatchCommand
{
    public virtual DbParameter CreateParameter();
    public virtual bool CanCreateParameter { get; }
}
```

## Add extensions to create an OptionsBuilder with ValidateOnStart support

**Approved** | [#runtime/89263](https://github.com/dotnet/runtime/issues/89263#issuecomment-1650247058) | [Video](https://www.youtube.com/watch?v=ixR8JORzD0c&t=0h14m23s)

* Let's call it `AddOptionsWithValidateOnStart()` to make it clear that it does both `AddOptions` and calls `ValidateOnStart`.

```C#
namespace namespace Microsoft.Extensions.DependencyInjection;

public static partial class OptionsBuilderExtensions
{
    // The one existing extension method:
    // public static OptionsBuilder<TOptions> ValidateOnStart
    //     <[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicParameterlessConstructor)] TOptions>
    //     (this OptionsBuilder<TOptions> optionsBuilder)
    //     where TOptions : class;

    // New one to call ValidateOnStart() above
    public static OptionsBuilder<TOptions> AddOptionsWithValidateOnStart
        <[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.All)] TOptions>
         (this IServiceCollection services,
         string? name = null)
         where TOptions : class;

    // New one to support IValidateOptions and call ValidateOnStart() above
    public static OptionsBuilder<TOptions> AddOptionsWithValidateOnStart<
        [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.All)] TOptions,
        [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TValidateOptions>(
        this IServiceCollection services,
        string? name = null)
        where TOptions : class
        where TValidateOptions : class, IValidateOptions<TOptions>;
}
```

## Warn when comparing spans to null

**Approved** | [#runtime/84265](https://github.com/dotnet/runtime/issues/84265#issuecomment-1650277489) | [Video](https://www.youtube.com/watch?v=ixR8JORzD0c&t=0h24m45s)

We should warn for any comparison against spans and memories involving the `null` literal (`== null`, `!= null`, `is null`, `is not null`). If someone writes `== default` then they likely meant the default span, which is meaningful and should be allowed.

* Comparisons against the default span should be written as `== default`.
* Asking whether the span is empty should be written as `span.IsEmpty`

**Category**: Usage
**Severity**: Warning

