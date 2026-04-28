# API Review 04/28/2026

## System.Text.Json union type support

**Approved** | [#runtime/127299](https://github.com/dotnet/runtime/issues/127299#issuecomment-4338197610) | [Video](https://www.youtube.com/watch?v=0QAt6IBRagc&t=0h0m0s)


* JsonSerializerOptions.UnionClassifiers => JsonSerializerOptions.TypeClassifiers
* JsonSourceGenerationOptionsAttribute.UnionClassifiers => JsonSourceGenerationOptionsAttribute.TypeClassifiers
* There was a lot of discussion around trying to make `union Pet(Cat, Dog)` work out of the box, but no good conclusions were arrived at.

```csharp
namespace System.Text.Json.Serialization
{
    // Classifier abstraction

    public delegate Type? JsonTypeClassifier(ref Utf8JsonReader reader);

    public sealed class JsonTypeClassifierContext
    {
        public Type DeclaringType { get; }
        public IReadOnlyList<JsonUnionCaseInfo> UnionCases { get; }
        public IReadOnlyList<JsonDerivedType> DerivedTypes { get; }
        public string? TypeDiscriminatorPropertyName { get; }
    }

    public abstract class JsonTypeClassifierFactory
    {
        public abstract bool CanClassify(Type declaringType);
        public abstract JsonTypeClassifier CreateJsonClassifier(
            JsonTypeClassifierContext context, JsonSerializerOptions options);
    }

    public abstract class JsonTypeClassifierFactory<T> : JsonTypeClassifierFactory
    {
        public sealed override bool CanClassify(Type declaringType) => declaringType == typeof(T);
    }

    // New options-level surface

    public sealed partial class JsonSerializerOptions
    {
        public IList<JsonTypeClassifierFactory> Classifiers { get; }
    }

    public sealed partial class JsonSourceGenerationOptionsAttribute
    {
        public Type[]? Classifiers { get; set; }
    }

    // New attribute APIs

    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct, AllowMultiple = false, Inherited = false)]
    public sealed class JsonUnionAttribute : JsonAttribute
    {
        [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicParameterlessConstructor)]
        public Type? TypeClassifier { get; set; }
    }

    public sealed partial class JsonPolymorphicAttribute
    {
        [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicParameterlessConstructor)]
        public Type? TypeClassifier { get; set; }
    }
}

namespace System.Text.Json.Serialization.Metadata
{
    // Contract customization surface for union types

    public sealed class JsonUnionCaseInfo
    {
        public JsonUnionCaseInfo(Type caseType);
        public Type CaseType { get; }
        public bool IsNullable { get; init; }
    }

    public enum JsonTypeInfoKind
    {
        Union = 4,
    }

    public abstract partial class JsonTypeInfo
    {
        public JsonTypeClassifier? TypeClassifier { get; set; }
        public IList<JsonUnionCaseInfo>? UnionCases { get; set; }
        public Func<Type, object?, object>? UnionConstructor { get; set; }
        public Func<object, (Type? CaseType, object? CaseValue)>? UnionDeconstructor { get; set; }
    }

    public sealed partial class JsonTypeInfo<T>
    {
        public new Func<Type, object?, T>? UnionConstructor { get; set; }
        public new Func<T, (Type? CaseType, object? CaseValue)>? UnionDeconstructor { get; set; }
    }

    [EditorBrowsable(EditorBrowsableState.Never)]
    public sealed class JsonUnionInfoValues<T>
    {
        public IList<JsonUnionCaseInfo>? UnionCases { get; init; }
        public Func<Type, object?, T>? UnionConstructor { get; init; }
        public JsonTypeClassifier? TypeClassifier { get; init; }
        public Func<T, (Type? CaseType, object? CaseValue)>? UnionDeconstructor { get; init; }
    }

    [EditorBrowsable(EditorBrowsableState.Never)]
    public static partial class JsonMetadataServices
    {
        public static JsonTypeInfo<T> CreateUnionInfo<T>(JsonSerializerOptions options, JsonUnionInfoValues<T> unionInfo) where T : notnull;
    }
}
```
## Add support for JSONL

**Approved** | [#runtime/126395](https://github.com/dotnet/runtime/issues/126395#issuecomment-4338250586) | [Video](https://www.youtube.com/watch?v=0QAt6IBRagc&t=1h49m22s)

Looks good as proposed

```csharp
namespace System.Text.Json;

public static partial class JsonSerializer
{
    // Stream overloads
    public static Task SerializeAsyncEnumerable<TValue>(
        Stream utf8Json,
        IAsyncEnumerable<TValue> value,
        bool topLevelValues = false,
        JsonSerializerOptions? options = null,
        CancellationToken cancellationToken = default);

    public static Task SerializeAsyncEnumerable<TValue>(
        Stream utf8Json,
        IAsyncEnumerable<TValue> value,
        JsonTypeInfo<TValue> jsonTypeInfo,
        bool topLevelValues = false,
        CancellationToken cancellationToken = default);

    // PipeWriter overloads
    public static Task SerializeAsyncEnumerable<TValue>(
        PipeWriter utf8Json,
        IAsyncEnumerable<TValue> value,
        bool topLevelValues = false,
        JsonSerializerOptions? options = null,
        CancellationToken cancellationToken = default);

    public static Task SerializeAsyncEnumerable<TValue>(
        PipeWriter utf8Json,
        IAsyncEnumerable<TValue> value,
        JsonTypeInfo<TValue> jsonTypeInfo,
        bool topLevelValues = false,
        CancellationToken cancellationToken = default);
}
```
