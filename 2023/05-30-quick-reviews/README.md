# API Review 05/30/2023

## Consider changing return types of new throw helpers

**Rejected** | [#runtime/86341](https://github.com/dotnet/runtime/issues/86341#issuecomment-1568805495) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=0h0m0s)

* It seems a bit noisy because it would require a naming conventions for the already shipped APIs
    - The hypothetical "binary breaking change" proposal by Jared might help here
* The language is looking at a proposal to allow enable void returning expressions in switch expressions
* This seems "neat" but labor intense
* For now, we'd rather reject this

```C#
namespace System;

public partial class ArgumentException
{
    public static string ThrowIfNullOrWhiteSpace([NotNull] string? argument, [CallerArgumentExpression(nameof(argument))] string? paramName = null);
}

public partial class ArgumentOutOfRangeException
{
    public static T ThrowIfEqual<T>(T value, T other, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : IEquatable<T>?;
    public static T ThrowIfGreaterThan<T>(T value, T other, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : IComparable<T>;
    public static T ThrowIfGreaterThanOrEqual<T>(T value, T other, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : IComparable<T>;
    public static T ThrowIfLessThan<T>(T value, T other, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : IComparable<T>;
    public static T ThrowIfLessThanOrEqual<T>(T value, T other, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : IComparable<T>;
    public static T ThrowIfNegative<T>(T value, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : INumberBase<T>;
    public static T ThrowIfNegativeOrZero<T>(T value, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : INumberBase<T>;
    public static T ThrowIfNotEqual<T>(T value, T other, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : IEquatable<T>?;
    public static T ThrowIfZero<T>(T value, [CallerArgumentExpression(nameof(value))] string? paramName = null) where T : INumberBase<T>;
}
```
## Add IHostApplicationBuilder interface

**Approved** | [#runtime/85486](https://github.com/dotnet/runtime/issues/85486#issuecomment-1568858340) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=0h19m32s)

* We should remove the `Build` method
    - The people consuming the abstractions don't need it and the final result will have different types to be useful

```diff
 namespace Microsoft.Extensions.Hosting;
 
 public interface IHostApplicationBuilder
 {
     IDictionary<object, object> Properties { get; }
 
     IConfigurationManager Configuration { get; }
     IHostEnvironment Environment { get; }
     ILoggingBuilder Logging { get; }
     IServiceCollection Services { get; }
 
     void ConfigureContainer<TContainerBuilder>(IServiceProviderFactory<TContainerBuilder>factory, Action<TContainerBuilder>? configure = null) where TContainerBuilder: notnull;
 }

-public sealed class HostApplicationBuilder {}
+public sealed class HostApplicationBuilder : IHostApplicationBuilder {}
```

```diff
 namespace Microsoft.Extensions.Configuration;
 
 public interface IConfigurationManager : IConfiguration, IConfigurationBuilder
 {
 }

-public sealed class ConfigurationManager : IConfigurationBuilder, IConfigurationRoot, IDisposable {}
+public sealed class ConfigurationManager : IConfigurationBuilder, IConfigurationRoot, IDisposable, IConfigurationManager {}
```
## Analyzer and Code-Fix to enable easy adoption of `[GeneratedComInterface]`

**Approved** | [#runtime/86343](https://github.com/dotnet/runtime/issues/86343#issuecomment-1568873564) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=0h59m47s)

Looks good as proposed
## Convert.TryFromHexString

**Approved** | [#runtime/78472](https://github.com/dotnet/runtime/issues/78472#issuecomment-1568898318) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=1h12m57s)

* `FromHexString`
    - We concluded we don't want the boolean because it either throws in unexpected ways or it collapses multiples errors which makes it non-trivial to recover.
    - We should use `OperationStatus` as the return value and drop the `Try` prefix
    - We should support incremental use by adding a consumed out parameter
* `ToHexString`
    - All bytes are valid, only error is destination is too small. Try-pattern is fine.

```C#
namespace System;

public partial class Convert
{
    public static OperationStatus FromHexString(string source, Span<byte> destination, out int charsConsumed, out int bytesWritten);
    public static OperationStatus FromHexString(ReadOnlySpan<char> source, Span<byte> destination, out int charsConsumed, out int bytesWritten);

    public static bool TryToHexString(ReadOnlySpan<byte> source, Span<char> destination, out int charsWritten);
}
```
## Add a `JsonTypeInfoResolver.WithModifier` extension method

**Approved** | [#runtime/86440](https://github.com/dotnet/runtime/issues/86440#issuecomment-1568913436) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=1h31m15s)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization.Metadata;

public static partial class JsonTypeInfoResolver
{
    public static IJsonTypeInfoResolver WithAddedModifier(this IJsonTypeInfoResolver resolver, Action<JsonTypeInfo> modifier);
}
```
## Remove struct constraint from `Vector*<T>`

**Approved** | [#runtime/85893](https://github.com/dotnet/runtime/issues/85893#issuecomment-1568918466) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=1h41m4s)

* Looks good as proposed

```C#
namespace System.Numerics
{
    public readonly struct Vector<T> : IEquatable<Vector<T>>, IFormattable {} // REMOVED: where T: struct
}

namespace System.Runtime.Intrinsics
{
    public readonly struct Vector64<T> : IEquatable<Vector64<T>> {}    // REMOVED: where T: struct
    public readonly struct Vector128<T> : IEquatable<Vector128<T>> {}  // REMOVED: where T: struct  
    public readonly struct Vector256<T> : IEquatable<Vector256<T>> {}  // REMOVED: where T: struct
}
```
## Create an AsnReader from an AsnReader

**Approved** | [#runtime/86723](https://github.com/dotnet/runtime/issues/86723#issuecomment-1568922963) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=1h45m21s)

* Seems like a typed `Clone()` is easier to use

```C#
namespace System.Formats.Asn1;

public partial class AsnReader
{
    public AsnReader Clone();
}
```
## Analyzer: Report inefficient use of sets.

**Approved** | [#runtime/85490](https://github.com/dotnet/runtime/issues/85490#issuecomment-1568924849) | [Video](https://www.youtube.com/watch?v=uwltIRkiNPQ&t=1h48m29s)

* Looks good as proposed
* Should also include a fixer

**Category**: Performance
**Severity**: Info
