# API Review 05/17/2022

## Developers can use System.Text.Json to serialize type hierarchies securely

**NeedsWork** | [#runtime/63747](https://github.com/dotnet/runtime/issues/63747#issuecomment-1129196752) | [Video](https://www.youtube.com/watch?v=bGUqmnFbFJ0&t=0h0m0s)

* `JsonPolymorphicAttribute`
    - `CustomTypeDiscriminatorPropertyName` -> `TypeDiscriminatorPropertyName`
* `JsonPolymorphicTypeInfo` sounds like it should be derived from `JsonTypeInfo`
    - `CustomTypeDiscriminatorPropertyName` -> `TypeDiscriminatorPropertyName`
    - The class itself should probably not implement `ICollection`
    - It should, however, have a property or method to enumerate the mapping
    - We'd prefer a design without the fluent `WithXxx()` methods and instead use a void returning `Add` method.
* Looks like we don't need `JsonPolymorphismOptions<TBaseType>`


```C#
namespace System.Text.Json.Serialization;

[AttributeUsage(AttributeTargets.Class |
                AttributeTargets.Interface,
                AllowMultiple = false,
                Inherited = false)]
public sealed class JsonPolymorphicAttribute : JsonAttribute
{
    public string? CustomTypeDiscriminatorPropertyName { get; set; }
    public bool IgnoreUnrecognizedTypeDiscriminators { get; set; }
    public JsonUnknownDerivedTypeHandling UnknownDerivedTypeHandling { get; set; }
}

[AttributeUsage(AttributeTargets.Class |
                AttributeTargets.Interface,
                AllowMultiple = true,
                Inherited = false)]
public class JsonDerivedTypeAttribute : JsonAttribute
{
    public JsonDerivedTypeAttribute(Type derivedType);
    public JsonDerivedTypeAttribute(Type derivedType, string typeDiscriminatorId);
    public JsonDerivedTypeAttribute(Type derivedType, int typeDiscriminatorId);

    public Type DerivedType { get; }
    public object? TypeDiscriminatorId { get; }
}

public enum JsonUnknownDerivedTypeHandling
{
    FailSerialization = 0,
    FallBackToBaseType = 1,
    FallBackToNearestAncestor = 2
}
```

```C#
namespace System.Text.Json.Metadata;

public partial class JsonTypeInfo
{
    public JsonPolymorphismOptions? PolymorphicTypeInfo { get; set; } = null;
}

public class JsonPolymorphismOptions : ICollection<(Type DerivedType, object? TypeDiscriminatorId)>
{
    public JsonPolymorphismOptions(Type baseType);
    public Type BaseType { get; }
    public string? TypeDiscriminatorPropertyName { get; set; }
    public bool IgnoreUnrecognizedTypeDiscriminators { get; set; }
    public JsonUnknownDerivedTypeHandling UnknownDerivedTypeHandling { get; set; }
    public JsonPolymorphismOptions WithDerivedType(Type derivedType);
    public JsonPolymorphismOptions WithDerivedType(Type derivedType, int typeDiscriminatorId);
    public JsonPolymorphismOptions WithDerivedType(Type derivedType, string typeDiscriminatorId);
}
```

> 
## Support non-allocating view on string properties values of JSON in System.Text.Json.Utf8JsonReader

**Approved** | [#runtime/54410](https://github.com/dotnet/runtime/issues/54410#issuecomment-1129233555) | [Video](https://www.youtube.com/watch?v=bGUqmnFbFJ0&t=1h34m41s)

* `GetString`
    - We don't need `scoped` -- marking the methods `readonly` is sufficient
    - Should return the units written instead of providing them via `out`
    - Should be named `CopyString`
* It seems we don't need `JsonEncodedText.Unescape`
    - `CopyString` already unescapes

```C#
namespace System.Text.Json;

public ref partial struct Utf8JsonReader
{
    // Existing APIs
    // public ReadOnlySpan<byte> ValueSpan { get; }
    // public ReadOnlySequence<byte> ValueSequence { get; }
    // 
    // public bool ValueIsEscaped { get; } // Whether the JSON string contains escaped characters
    // public bool HasValueSequence { get; } // The string can either be stored in a span or a ReadOnlySequence
    //
    // public string? GetString(); // How we currently decode JSON strings

    public readonly int CopyString(Span<byte> utf8Destination);
    public readonly int CopyString(Span<char> destination);
}
```
