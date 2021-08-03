# API Review 08/03/2021

## Add description to RequiresPreviewFeaturesAttribute

**Approved** | [#runtime/56498](https://github.com/dotnet/runtime/issues/56498#issuecomment-892025270) | [Video](https://www.youtube.com/watch?v=9nVhHuAqblA&t=0h0m0s)

* We discussed whether or not the analyzer should also offer a diagnostic ID, akin to `ObsoleteAttribute`
    - We're leaning towards no, because that seems to re-introduce the feature names that we considered in the design and decided against due to complexity and test matrix concerns
    - That means we wouldn't have `UrlFormat` but just `Url`

```C#
namespace System.Runtime.Versioning
{
    public sealed class RequiresPreviewFeaturesAttribute : Attribute
    {
        // Existing
        // public RequiresPreviewFeaturesAttribute();
        public RequiresPreviewFeaturesAttribute(string? message);
        public string? Message { get; }
        public string? Url { get; set; }
    }
}
```
## Allow custom converters for dictionary keys

**NeedsWork** | [#runtime/46520](https://github.com/dotnet/runtime/issues/46520#issuecomment-892058319) | [Video](https://www.youtube.com/watch?v=9nVhHuAqblA&t=0h16m6s)

* This is a regression from .NET 5
    - Should we just fix the regression or at least fix a subset of the regression (e.g. have string-based keys just work)?
* This needs more thought
    - We should consider more JSON like names, such `XxxAsPropertyName`
    - We should consider a generic "to string" conversion so that the serializer can do The Right Thing ™️

```C#
namespace System.Text.Json.Serialization
{
    public partial class JsonConverter<T>
    {
        protected virtual T ReadAsDictionaryKey(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options);
        protected virtual void WriteAsDictionaryKey(Utf8JsonWriter writer, [DisallowNull] T value, JsonSerializerOptions options);
    }
}
```
## Compile-time source generation for System.Text.Json

**NeedsWork** | [#runtime/45448](https://github.com/dotnet/runtime/issues/45448#issuecomment-892099426) | [Video](https://www.youtube.com/watch?v=9nVhHuAqblA&t=1h6m12s)

* `JsonTypeInfo<T>`
    - The `Serialize` property can be null if we can't support the fast-path serialization, but that seems subtle. We may want to use a different name or throw an exception with more details.
* `JsonObjectInfoValues<T>`
    - We should replace the `Func` suffix with `Handler`, `Callback`, or `Factory`. Some names, such as `Getter` or `Setter` are fine too.
    - We should expand name such as `Ctor`, `Prop`, `Param`, `Init` (in fact, do we need that in the name at all?)
* `JsonSerializerContext`
    - `DesignTimeOptions` should be renamed to something like `SourceGeneratedOptions` or `GeneratedSerializerOptions`
* `JsonMetadataServices`
    - `CreateStackOrQueueInfo` should be split into two methods so that it matches the generic versions
* We should decide whether we should use classes or structs; there are performance trade offs for either choice (e.g. should it be passed by `ref`)?

```C#
namespace System.Text.Json.Serialization.Metadata
{
    // Provides serialization info about a type
    public partial class JsonTypeInfo
    {
        internal JsonTypeInfo();
    }

    // Provides serialization info about a type
    public abstract partial class JsonTypeInfo<T> : JsonTypeInfo
    {
        internal JsonTypeInfo();
        public Action<Utf8JsonWriter, T>? Serialize { get; }
    }

    // Provide serialization info about a property or field of a POCO
    public abstract partial class JsonPropertyInfo
    {
        internal JsonPropertyInfo();
    }

+   // The list of parameters in `CreatePropertyInfo<T>` below is too long, so we move the data to a struct. This should help with forward-compat since we can add members representing new serializer features.
+   public readonly struct JsonPropertyInfoValues<T>
+   {
+       public bool IsProperty { get; init; }
+       public bool IsPublic { get; init; }
+       public bool IsVirtual { get; init; }
+       public Type DeclaringType { get; init; }
+       public JsonTypeInfo PropertyTypeInfo { get; init; }
+       public JsonConverter<T>? Converter { get; init; }
+       public Func<object, T>? Getter { get; init; }
+       public Action<object, T>? Setter { get; init; }
+       public JsonIgnoreCondition? IgnoreCondition { get; init; }
+       public bool IsExtensionDataProperty { get; init; }
+       public JsonNumberHandling? NumberHandling { get; init; }
+       // The property's statically-declared name.
+       public string PropertyName { get; init; }
+       // The name to use when processing the property, specified by [JsonPropertyName(string)].
+       public string? JsonPropertyName { get; init; }
+   }

+   // For the same reason as `JsonPropertyInfoValues<T>` above, move object info to a struct.
+   // Configures information about an object with members that is deserialized using a parameterless ctor.
+   public readonly struct JsonObjectInfoValues<T>
+   {
+       // A method to create an instance of the type, using a parameterless ctor
+       public Func<T>? CreateObjectFunc { get; init; }

+       // A method to create an instance of the type, using a parameterized ctor
+       public Func<object[], T>? CreateObjectFunc { get; init; }

+       // Provides information about the type's properties and fields.
+       public Func<JsonSerializerContext, JsonPropertyInfo[]>? PropInitFunc { get; init; }

+       // Provides information about the type's ctor params.
+       public Func<JsonParameterInfo[]>? CtorParamInitFunc { get; init; }

+       // The number-handling setting for the type's properties and fields.
+       public JsonNumberHandling NumberHandling { get; init; }

+       // An optimized method to serialize instances of the type, given pre-defined serialization settings.
+       public Action<Utf8JsonWriter, T>? SerializeFunc { get; init; }
+   }

+   // Configures information about a collection
+   public readonly struct JsonCollectionInfo<T>
+   {
+       // A method to create an instance of the collection  
+       public Func<T>? CreateObjectFunc { get; init; }

+       // Serialization metadata about the collection key type, if a dictionary.
+       public JsonTypeInfo? KeyInfo { get; init; }

+       // Serialization metadata about the collection element/value type.
+       public JsonTypeInfo ElementInfo { get; init; }

+       // The number-handling setting for the collection's elements.
+       public JsonNumberHandling NumberHandling { get; init; }

+       // An optimized method to serialize instances of the collection, given pre-defined serialization settings.
+       public Action<Utf8JsonWriter, T>? SerializeFunc { get; init; }
+   }

+   // Provides serialization info about a constructor parameter
+   public readonly struct JsonParameterInfo
+   {
+       public object? DefaultValue { readonly get; init; }
+       public bool HasDefaultValue { readonly get; init; }
+       public string Name { readonly get; init; }
+       public Type ParameterType { readonly get; init; }
+       public int Position { readonly get; init; }
+   }

    // Object type and property info creators
    public static partial class JsonMetadataServices
    {
        // Creator for an object type
-       public static JsonTypeInfo<T> CreateObjectInfo<T>(JsonSerializerOptions options, Func<T>? createObjectFunc, Func<JsonSerializerContext, JsonPropertyInfo[]>? propInitFunc, JsonNumberHandling numberHandling, Action<Utf8JsonWriter, T>? serializeFunc) where T : notnull;
+       public static JsonTypeInfo<T> CreateObjectInfo<T>(JsonSerializerOptions options, JsonObjectInfoValues<T> objectInfo) where T : notnull;
        
        // Creator for an object property
-       public static JsonPropertyInfo CreatePropertyInfo<T>(JsonSerializerOptions options, bool isProperty, bool isPublic, bool isVirtual, Type declaringType, JsonTypeInfo propertyTypeInfo, JsonConverter<T>? converter, Func<object, T>? getter, Action<object, T>? setter, JsonIgnoreCondition? ignoreCondition, bool hasJsonInclude, JsonNumberHandling? numberHandling, string propertyName, string? jsonPropertyName);
+       public static JsonPropertyInfo CreatePropertyInfo<T>(JsonSerializerOptions options, JsonPropertyInfoValues<T> propertyInfo);
    }

    // Collection type info creators
    public static partial class JsonMetadataServices
    {
-       public static JsonTypeInfo<TElement[]> CreateArrayInfo<TElement>(JsonSerializerOptions options, JsonTypeInfo elementInfo, JsonNumberHandling numberHandling, Action<Utf8JsonWriter, TElement[]>? serializeFunc);
+       public static JsonTypeInfo<TElement[]> CreateArrayInfo<TElement>(JsonSerializerOptions options, JsonCollectionInfo<TElement[]> info);

-       public static JsonTypeInfo<TCollection> CreateDictionaryInfo<TCollection, TKey, TValue>(JsonSerializerOptions options, Func<TCollection> createObjectFunc, JsonTypeInfo keyInfo, JsonTypeInfo valueInfo, JsonNumberHandling numberHandling, Action<Utf8JsonWriter, TCollection>? serializeFunc) where TCollection : Dictionary<TKey, TValue> where TKey : notnull;
+       public static JsonTypeInfo<TCollection> CreateDictionaryInfo<TCollection, TKey, TValue>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : Dictionary<TKey, TValue> where TKey : notnull;

-       public static JsonTypeInfo<TCollection> CreateListInfo<TCollection, TElement>(JsonSerializerOptions options, Func<TCollection>? createObjectFunc, JsonTypeInfo elementInfo, JsonNumberHandling numberHandling, Action<Utf8JsonWriter, TCollection>? serializeFunc) where TCollection : List<TElement>;
+       public static JsonTypeInfo<TCollection> CreateListInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : List<TElement>;

+       public static JsonTypeInfo<TCollection> CreateConcurrentQueueInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : ConcurrentQueue<TElement>;
+       public static JsonTypeInfo<TCollection> CreateConcurrentStackInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : ConcurrentStack<TElement>;
+       public static JsonTypeInfo<TCollection> CreateICollectionInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : ICollection<TElement>;
+       public static JsonTypeInfo<TCollection> CreateIDictionaryInfo<TCollection>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IDictionary;
+       public static JsonTypeInfo<TCollection> CreateIDictionaryInfo<TCollection, TKey, TValue>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IDictionary<TKey, TValue> where TKey : notnull;
+       public static JsonTypeInfo<TCollection> CreateIEnumerableInfo<TCollection>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IEnumerable;
+       public static JsonTypeInfo<TCollection> CreateIEnumerableInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IEnumerable<TElement>;
+       public static JsonTypeInfo<TCollection> CreateIListInfo<TCollection>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IList;
+       public static JsonTypeInfo<TCollection> CreateIListInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IList<TElement>;
+       public static JsonTypeInfo<TCollection> CreateImmutableDictionaryInfo<TCollection, TKey, TValue>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info, Func<IEnumerable<KeyValuePair<TKey, TValue>>, TCollection> createRangeFunc) where TCollection : IReadOnlyDictionary<TKey, TValue> where TKey : notnull;
+       public static JsonTypeInfo<TCollection> CreateImmutableEnumerableInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info, Func<IEnumerable<TElement>, TCollection> createRangeFunc) where TCollection : IEnumerable<TElement>;
+       public static JsonTypeInfo<TCollection> CreateIReadOnlyDictionaryInfo<TCollection, TKey, TValue>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : IReadOnlyDictionary<TKey, TValue> where TKey : notnull;
+       public static JsonTypeInfo<TCollection> CreateISetInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : ISet<TElement>;
+       public static JsonTypeInfo<TCollection> CreateQueueInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : Queue<TElement>;
+       public static JsonTypeInfo<TCollection> CreateStackInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info) where TCollection : Stack<TElement>;
+       public static JsonTypeInfo<TCollection> CreateStackOrQueueInfo<TCollection>(JsonSerializerOptions options, JsonCollectionInfo<TCollection> info, Action<TCollection, object?> addFunc) where TCollection : IEnumerable;
    }
}
namespace System.Text.Json.Serialization
{
    public enum JsonKnownNamingPolicy
    {
        Unspecified = 0,
        CamelCase = 1,
    }

    public abstract partial class JsonSerializerContext
    {
-       protected JsonSerializerContext(JsonSerializerOptions? instanceOptions, JsonSerializerOptions? defaultOptions);
+       protected JsonSerializerContext(JsonSerializerOptions? options);
        public JsonSerializerOptions Options { get; }
        public abstract JsonTypeInfo? GetTypeInfo(Type type);
+       // The set of options that are compatible with generated serialization and (future) deserialization logic for types in the context.
+       protected abstract JsonSerializerOptions? DesignTimeOptions { get; }
    }

    [AttributeUsage(AttributeTargets.Class, AllowMultiple=false)]
    public partial sealed class JsonSourceGenerationOptionsAttribute : JsonAttribute
    {
        public JsonSourceGenerationOptionsAttribute();
        public JsonIgnoreCondition DefaultIgnoreCondition { get; set; }
        public bool IgnoreReadOnlyFields { get; set; }
        public bool IgnoreReadOnlyProperties { get; set; }
-       // Whether the generated source code should ignore converters added at runtime.
-       public bool IgnoreRuntimeCustomConverters { get; set; }
        public bool IncludeFields { get; set; }
        public JsonKnownNamingPolicy PropertyNamingPolicy { get; set; }
        public bool WriteIndented { get; set; }
        public JsonSourceGenerationMode GenerationMode { get; set; }
    }

    [Flags]
    public enum JsonSourceGenerationMode
    {
        Default = 0,
        Metadata = 1,
        Serialization = 2,
        // Future
        // Deserialization = 4
    }
}
```
