# API Review 05/12/2022

## char helpers for common ASCII categories

**Approved** | [#runtime/68868](https://github.com/dotnet/runtime/issues/68868#issuecomment-1125244417) | [Video](https://www.youtube.com/watch?v=mhdY64EnxAw&t=0h0m0s)

* We inserted "Ascii" in the Is*Hex methods
* We renamed IsInRange to IsBetween
* Otherwise looks good

```C#
namespace System
{
    public partial struct Char
    {
        public static bool IsAsciiDigit(char c); // true iff the char is 0-9
        public static bool IsAsciiLetter(char c); // true iff the char is A-Z, a-z
        public static bool IsAsciiLetterLower(char c); // true iff the char is a-z
        public static bool IsAsciiLetterUpper(char c); // true iff the char is A-Z
        public static bool IsAsciiLetterOrDigit(char c); // true iff the char is A-Z, a-z, 0-9

        public static bool IsAsciiHexDigit(char c); // true iff the char is 0-9, A-F, a-f
        public static bool IsAsciiHexDigitUpper(char c); // true iff the char is 0-9, A-F
        public static bool IsAsciiHexDigitLower(char c); // true iff the char is 0-9, a-f

        public static bool IsBetween(char c, char minInclusive, char maxInclusive); // true iff the char is >= minInclusive && <= maxInclusive
    }
}
```
## Determine recommended behavior of checked/unchecked operators for DateTime and similar types

**Approved** | [#runtime/67744](https://github.com/dotnet/runtime/issues/67744#issuecomment-1125266243) | [Video](https://www.youtube.com/watch?v=mhdY64EnxAw&t=0h19m40s)

The feeling in the room is a mix of (1) and (3).

* For types that aren't naturally "math" types, like DateTime, don't add any of the math-related interfaces (the ones in System.Numerics shouldn't be added; ISpanParsable isn't mathy, so is OK.)
* For the types that are, like BigInteger and Decimal, don't change the behavior of the existing members; only in the generic math context will the checked/unchecked differentiation take place.
## Developers can customize the JSON serialization contracts of their types

**Approved** | [#runtime/63686](https://github.com/dotnet/runtime/issues/63686#issuecomment-1125339465) | [Video](https://www.youtube.com/watch?v=mhdY64EnxAw&t=0h44m56s)

* The API owners have convinced us that we can do without CompleteObject for now, and that it could be added later if justified.
* We discussed and settled on the cascade/composition/etc pattern.
  * Added DefaultJsonTypeInfoResolver.Modifiers for mutation strategies
  * Added JsonTypeInfoResolver.Combine for linking together multiple resolvers.

```C#
namespace System.Text.Json 
{
    public sealed partial class JsonSerializerOptions
    {
        public IJsonTypeInfoResolver TypeInfoResolver { [RequiresUnreferencedCode] get; set; }
    }
}

namespace System.Text.Json.Serialization 
{
    public abstract partial class JsonSerializerContext : IJsonTypeInfoResolver
    {
        // Explicit interface implementation calling into the equivalent JsonSerializerContext abstract method
        JsonTypeInfo System.Text.Json.Serialization.Metadata.IJsonTypeInfoResolver.GetTypeInfo(Type type, JsonSerializerOptions options);
    }
}

namespace System.Text.Json.Serialization.Metadata 
{
    public interface IJsonTypeInfoResolver
    {
        JsonTypeInfo? GetTypeInfo(Type type, JsonSerializerOptions options);
    }

    // Provides the default reflection-based contract metadata resolution
    public class DefaultJsonTypeInfoResolver : IJsonTypeInfoResolver
    {
        [RequiresUnreferencedCode]
        public DefaultJsonTypeInfoResolver() { }

        public virtual JsonTypeInfo GetTypeInfo(Type type, JsonSerializerOptions options);
        
        public IList<Action<JsonTypeInfo>> Modifiers { get; }
    }

    public static class JsonTypeInfoResolver
    {
        public static IJsonTypeInfoResolver Combine(params IJsonTypeInfoResolver[] resolvers);
    }

    // Determines the kind of contract metadata a given JsonTypeInfo instance is customizing
    public enum JsonTypeInfoKind
    {
        None = 0, // Type is either a primitive value or uses a custom converter -- contract metadata does not apply here.
        Object = 1, // Type is serialized as a POCO with properties
        Enumerable = 2, // Type is serialized as a collection with elements
        Dictionary = 3 // Type is serialized as a dictionary with key/value pair entries
    }

    // remove: [System.ComponentModel.EditorBrowsableAttribute(System.ComponentModel.EditorBrowsableState.Never)]
    // note: added abstract
    public abstract partial class JsonTypeInfo
    {
        public Type Type { get; }
        public JsonSerializerOptions Options { get; }

        // The converter instance associated with the type for the given options instance -- this cannot be changed.
        public JsonConverter Converter { get; }

        // The kind of contract metadata we're customizing
        public JsonTypeInfoKind Kind { get; }

        // Untyped default constructor delegate -- deserialization not supported if set to null.
        public Func<object>? CreateObject { get; set; }

        // List of property metadata for types serialized as POCOs.
        public IList<JsonPropertyInfo> Properties { get; }

        // Equivalent to JsonNumberHandlingAttribute annotations.
        public JsonNumberHandling? NumberHandling { get; set; }

        // Factory methods for JsonTypeInfo
        public static JsonTypeInfo<T> CreateJsonTypeInfo<Τ>(JsonSerializerOptions options) { }
        public static JsonTypeInfo CreateJsonTypeInfo(Type type, JsonSerializerOptions options) { }

        // Factory methods for JsonPropertyInfo
        public JsonPropertyInfo CreateJsonPropertyInfo(Type propertyType, string name) { }
    }

    // remove: [EditorBrowsable(EditorBrowsableState.Never)]
    public abstract partial class JsonTypeInfo<T> : JsonTypeInfo
    {
        // Default typed constructor delegate
        public new Func<T>? CreateObject { get; set; }
    }

    // remove: [System.ComponentModel.EditorBrowsableAttribute(System.ComponentModel.EditorBrowsableState.Never)]
    public abstract partial class JsonPropertyInfo
    {
        public JsonSerializerOptions Options { get; }
        public Type PropertyType { get; }
        public string Name { get; set; }

        // Custom converter override at the property level, equivalent to `JsonConverterAttribute` annotation.
        public JsonConverter? CustomConverter { get; set; }

        // Untyped getter delegate
        public Func<object, object?>? Get { get; set; }

        // Untyped setter delegate
        public Action<object, object?>? Set { get; set; }
    
        // Predicate determining whether a property value should be serialized
        public Func<object, object?, bool>? ShouldSerialize { get; set; }

        // Equivalent to JsonNumberHandlingAttribute overrides.
        public JsonNumberHandling? NumberHandling { get; set; }
    }
}
