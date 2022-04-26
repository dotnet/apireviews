# API Review 04/26/2022

## Developers can customize the JSON serialization contracts of their types

**NeedsWork** | [#runtime/63686](https://github.com/dotnet/runtime/issues/63686#issuecomment-1110143405) | [Video](https://www.youtube.com/watch?v=zXzxA5Ygzlo&t=0h0m0s)


* Should JsonTypeInfo.Properties be declared as IList or a specific list implementation?
  * It returns an instance of a non-public type, so using the interface is correct.
* Removed `JsonTypenInfo.CreateJsonPropertyInfo<TProperty>` since it was described as withdrawn.
* Added JsonTypeInfo.CompleteObject to pair with CreateObject and enable scenarios like ImmutableArrayBuilder->ImmutableArray
* There was a lot of unresolved discussion about how type resolution works across libraries,
  such as two different libraries expose their JsonSerializerContext types, and an application wants to include both of those
  as well as the SystemRuntimeSerializationAttributeResolver.
  * This partially comes down to "is the pattern to return null, or to make individual resolvers handle their cascade?"
  * It has implications on whether JsonSerializerOptions should take a single IJsonTypeInfoResolver, or have a list of them
    * Or keep it as a single instance and expose composing resolvers as part of the proposal.
* It was suggested that `DefaultJsonTypeInfoResolver` should be sealed instead of recommending resolvers derive from it.

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
        JsonTypeInfo? System.Text.Json.Serialization.Metadata.IJsonTypeInfoResolver.GetTypeInfo(Type type, JsonSerializerOptions options);
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

        // If provided, is called when the object is complete, and allows for changing the type of the object,
        // e.g. CreateObject returns something like ImmutableArrayBuilder, CompleteObject returns the built ImmutableArray.
        public Func<object,object>? CompleteObject { get; set; }

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

// also look at the existing APIs:
// [System.ComponentModel.EditorBrowsableAttribute(System.ComponentModel.EditorBrowsableState.Never)]
// public static partial class JsonMetadataServices
// {
//    public static JsonTypeInfo<T> CreateObjectInfo<T>(JsonSerializerOptions options, JsonObjectInfoValues<T> objectInfo) where T : notnull { throw null; }
//    public static JsonPropertyInfo CreatePropertyInfo<T>(JsonSerializerOptions options, JsonPropertyInfoValues<T> propertyInfo) { throw null; }
//    and many other members
// }
// they are currently very specific to what code gen is doing and more complicated than they should be, at least in GRPC scenario only what is proposed above is sufficient
// but will let you decide :-)
```
