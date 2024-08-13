# API Review 05/21/2024

## Add "hints" in Metric API to provide things like histogram bounds

**Approved** | [#runtime/63650](https://github.com/dotnet/runtime/issues/63650#issuecomment-2123153036) | [Video](https://www.youtube.com/watch?v=kXVhOhI_Ryc&t=0h0m0s)

* We should expose the `ExplicitBucketBoundaries` as an `IReadOnlyList<T>`
* Let's use get/set rather than a constructor + property
* We should make it simpler
    - Have one method with just the required parameter (name)
    - Have one method that takes all the optional parameters
    - We can't remove methods, but we can hide them and remove all optionality to avoid ambiguity
* We discussed whether to use get/set for the buckets but concluded that we should use the constructor

```C#
namespace System.Diagnostics.Metrics;

public sealed class HistogramAdvice<T> where T : struct
{
    public HistogramAdvice(IEnumerable<T> explicitBucketBoundaries);
    public IReadOnlyList<T> ExplicitBucketBoundaries { get; }
}

public partial class Meter
{
    // NEW: Simple one
    public Histogram<T> CreateHistogram<T> (
        string name) where T : struct;

    // Existing, mark parameters as required and EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    public Histogram<T> CreateHistogram<T> (
        string name,
        string? unit,
        string? description) where T : struct;

    // Also existing, mark EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    public Histogram<T> CreateHistogram<T>(
        string name,
        string? unit,
        string? description,
        IEnumerable<KeyValuePair<string, object?>>? tags) where T : struct;

    public Histogram<T> CreateHistogram<T> (
        string name,
        string? unit = default,
        string? description = default,
        IEnumerable<KeyValuePair<string, object?>>? tags = default,
        HistogramAdvice<T>? advice = default) where T : struct;
}

public sealed class Histogram<T> : Instrument<T> where T : struct
{
    public HistogramAdvice<T>? Advice { get; }
}
```
## Support instrumentation scope attributes on ActivitySource

**Approved** | [#runtime/93795](https://github.com/dotnet/runtime/issues/93795#issuecomment-2123161895) | [Video](https://www.youtube.com/watch?v=kXVhOhI_Ryc&t=0h52m49s)

* We should make it simpler
    - Have one method with just the required parameter (name)
    - Have one method that takes all the optional parameters
    - We can't remove methods, but we can hide them and remove all optionality to avoid ambiguity

```C#
namespace System.Diagnostics;

public partial class ActivitySource
{
    // Existing, left as-is
    // public ActivitySource(string name);

    // Existing, remove optionality and mark EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    public ActivitySource(string name, string? version);

    // New
    public ActivitySource(string name, string? version = "", IEnumerable<KeyValuePair<string, object?>>? tags = default);

    public IEnumerable<KeyValuePair<string, object?>>? Tags { get; }
}
```
## Windows CNG virtualization-based security

**Approved** | [#runtime/102492](https://github.com/dotnet/runtime/issues/102492#issuecomment-2123171175) | [Video](https://www.youtube.com/watch?v=kXVhOhI_Ryc&t=0h58m22s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography;

[Flags]
public enum CngKeyCreationOptions : int
{
    // existing:
    // None = 0x00000000,
    // MachineKey = 0x00000020,            // NCRYPT_MACHINE_KEY_FLAG
    // OverwriteExistingKey = 0x00000080,  // NCRYPT_OVERWRITE_KEY_FLAG

    // new APIs:
    PreferVbs = 0x00010000,             // NCRYPT_PREFER_VBS_FLAG
    RequireVbs = 0x00020000,            // NCRYPT_REQUIRE_VBS_FLAG
    UsePerBootKey = 0x00040000,         // NCRYPT_USE_PER_BOOT_KEY_FLAG
}
```
## [System.Text.Json] Expose additional metadata in contract APIs.

**Approved** | [#runtime/102078](https://github.com/dotnet/runtime/issues/102078#issuecomment-2123204842) | [Video](https://www.youtube.com/watch?v=kXVhOhI_Ryc&t=1h4m12s)

* Looks good as proposed
* `JsonPropertyInfo`
    - Let's rename `AssociatedParameterInfo` to just `AssociatedParameter`
* `JsonParameterInfo`
    - Let's make it `sealed`

```C#
namespace System.Text.Json.Serialization.Metadata;

public partial class JsonTypeInfo
{
    // Attribute provider for the deserialization constructor (aka `JsonConstructorAttribute` ctors)   
    public ICustomAttributeProvider? ConstructorAttributeProvider { get; }
}

public partial class JsonPropertyInfo
{
    public Type DeclaringType { get; }
    // The JsonParameterInfo that has been associated with the current property.
    public JsonParameterInfo? AssociatedParameter { get; }

    // Existing
    // public ICustomAttributeProvider? AttributeProvider { get; set; }
}

// Currently internal type, exposed as a read-only façade for now.
public sealed class JsonParameterInfo
{
    public Type DeclaringType { get; }
    public int Position { get; }
    public Type ParameterType { get; }
    public bool HasDefaultValue { get; }
    public object? DefaultValue { get; }
    public bool DisallowNull { get; }
    public ICustomAttributeProvider? AttributeProvider { get; }
}

// New APIs for SG-specific APIs
public partial class JsonObjectInfoValues<T>
{
    public Func<ICustomAttributeProvider?>? ConstructorAttributeProviderFactory { get; init; }
}

public partial class JsonPropertyInfoValues<T>
{
    public Func<ICustomAttributeProvider?>? AttributeProviderFactory { get; init; }
}

public partial class JsonParameterInfoValues
{
    public bool DisallowNull { get; init; }
}
```
## Add a STJ feature flag recognizing nullable reference type annotations on properties

**Approved** | [#runtime/100144](https://github.com/dotnet/runtime/issues/100144#issuecomment-2123233306) | [Video](https://www.youtube.com/watch?v=kXVhOhI_Ryc&t=1h24m58s)

* We should use non-negated properties (i.e. remove the `Non` infix)

```C#
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    // Existing
    // public class bool RespectRequiredConstructorParameters { get; set; } = false;
    public bool RespectNullableAnnotations { get; set; } = false;
}

public partial class JsonPropertyInfo
{
    // Existing
    // public Func<object, object?> Get { get; set; }
    public bool IsGetNullable { get; set; }

    // Existing
    // public Action<object, object?> Set { get; set; }
    public bool IsSetNullable { get; set; }
}

public partial class JsonParameterInfoValues
{
    public bool IsNullable { get; init; }
}
```
