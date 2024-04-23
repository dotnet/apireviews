# API Review 04/23/2024

## Reevaluate `params ReadOnlySpan<string>` overloads from #77873

**Approved** | [#runtime/101261](https://github.com/dotnet/runtime/issues/101261#issuecomment-2073078961) | [Video](https://www.youtube.com/watch?v=RjRvGZODI2k&t=0h0m0s)


* We are aware of the break, and think that it's fine going forward.  It would be nice if the language handled it.
* Adding an implicit conversion to `ReadOnlySpan<string>` will fix the problem for this pattern, and we've decided to proactively add it to `StringValues`.  It's up to the owners of the library to decide if this new conversion is available on .NET Standard 2.0, or only on .NET 9+

```c#
namespace Microsoft.Extensions.Primitives
{
    public readonly partial struct StringValues
    {
        public static implicit operator ReadOnlySpan<string>(in StringValues value);
    }
}
```

## Support adding links to Activity after it is created

**Approved** | [#runtime/97680](https://github.com/dotnet/runtime/issues/97680#issuecomment-2073087554) | [Video](https://www.youtube.com/watch?v=RjRvGZODI2k&t=1h4m5s)


* We generally don't do the fluent pattern, but that's consistent with the type already.
* So, looks good as proposed.

```c#
namespace System.Diagnostics
{
    public partial class Activity
    {
        public System.Diagnostics.Activity AddLink(System.Diagnostics.ActivityLink link);
    }
}
```

## Add a STJ feature flag recognizing nullable reference type annotations on properties

**Approved** | [#runtime/100144](https://github.com/dotnet/runtime/issues/100144#issuecomment-2073212111) | [Video](https://www.youtube.com/watch?v=RjRvGZODI2k&t=1h8m54s)


* AllowNullReads/Writes changed to DisallowNullReads/Writes to better match usage expectations and an existing [DisallowNull] attribute.
* ResolveNullableReferenceTypes => IgnoreNullableAnnotations
* Probably wants an AppContext switch to change the default.  e.g. `System.Text.Json.JsonSerializer.IgnoreNullableAnnotationsDefault`

```c#
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public bool IgnoreNullableAnnotations { get; set; } = false;
}

public partial class JsonSourceGenerationOptionsAttribute
{
    public bool IgnoreNullableAnnotations { get; set; } = false;
}

namespace System.Text.Json.Serialization.Metadata;

public partial class JsonPropertyInfo
{
    public bool DisallowNullWrites { get; set; }

    public bool DisallowNullReads { get; set; }
}
```

## Add a STJ feature flag treating non-optional constructor parameters as required

**Approved** | [#runtime/100075](https://github.com/dotnet/runtime/issues/100075#issuecomment-2073231418) | [Video](https://www.youtube.com/watch?v=RjRvGZODI2k&t=1h53m38s)


* RequireConstructorParameters => RespectRequiredConstructorParameters
* Should use an AppContext value to determine the default, but absent one it should be on (breaking change).
  * `System.Text.Json.JsonSerializer.RespectRequiredConstructorParametersDefault`

```c#
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public bool RespectRequiredConstructorParameters { get; set; } = AppContext(true);
}

namespace System.Text.Json.Serialization;

public partial class JsonSourceGenerationOptionsAttribute
{
    public bool RespectRequiredConstructorParameters { get; set; } = AppContext(true);
}
```

