# API Review 06/29/2021

## Add ImmutableArray Span-based APIs

**Approved** | [#runtime/22928](https://github.com/dotnet/runtime/issues/22928#issuecomment-870794413)

* **Source Breaking Change**. We discussed several options, such as adding as a suffix like `AddRangeFromSpan` as well as extension methods that could help to have tie breakers, but it seems the break would be rare in practice:
    - It's only ambiguous for types that have an implicit conversion to `IEnumerable<T>` and `ReadOnlySpan<T>`. We only ship two types, arrays and strings. For arrays, we can add a tie breaker `AddRange(T[])` to avoid ambiguity issues. The string one in practice feels super rare that asking the consumer to add a cast or adding `.AsSpan()` seems acceptable and preferable over adding a new method group.
* We should align the API surface of the builder with immutable, as per #822.

```C#
namespace System.Collections.Immutable
{
    public static partial class ImmutableArray
    {
        public static ImmutableArray<T> Create<T>(ReadOnlySpan<T> items);
        public static ImmutableArray<T> Create<T>(Span<T> items);
        public static ImmutableArray<T> ToImmutableArray<T>(this ReadOnlySpan<T> items);
        public static ImmutableArray<T> ToImmutableArray<T>(this Span<T> items);
    }

    public partial struct ImmutableArray<T>
    {
        // Existing:
        // public ImmutableArray<T> AddRange(IEnumerable<T> items);
        // public ImmutableArray<T> AddRange(ImmutableArray<T> items);      
        public ImmutableArray<T> AddRange(ReadOnlySpan<T> items);
        public ImmutableArray<T> AddRange(params T[] items);

        public ReadOnlySpan<T> AsSpan(int start, int length);
        public ReadOnlySpan<T> AsSpan(Range range);
        public void CopyTo(Span<T> destination);

        // Existing:
        // public ImmutableArray<T> InsertRange(int index, IEnumerable<T> items);
        // public ImmutableArray<T> InsertRange(int index, ImmutableArray<T> items);
        public ImmutableArray<T> InsertRange(int index, T[] items);
        public ImmutableArray<T> InsertRange(int index, ReadOnlySpan<T> items);
        
        // Existing:
        // public ImmutableArray<T> RemoveRange(int index, int length);
        // public ImmutableArray<T> RemoveRange(IEnumerable<T> items);
        // public ImmutableArray<T> RemoveRange(ImmutableArray<T> items);
        // public ImmutableArray<T> RemoveRange(IEnumerable<T> items, IEqualityComparer<T>? equalityComparer);
        // public ImmutableArray<T> RemoveRange(ImmutableArray<T> items, IEqualityComparer<T>? equalityComparer);
        public ImmutableArray<T> RemoveRange(ReadOnlySpan<T> items, IEqualityComparer<T>? equalityComparer = null);
        public ImmutableArray<T> RemoveRange(T[] items, IEqualityComparer<T>? equalityComparer = null);

        public ImmutableArray<T> Slice(int start, int length);
        public sealed partial class Builder
        {
            // Existing:
            // public void AddRange(T[] items);
            // public void AddRange(T[] items, int length);
            // public void AddRange(IEnumerable<T> items);
            // public void AddRange(ImmutableArray<T> items);
            // public void AddRange(ImmutableArray<T> items, int length);
            public void AddRange(ReadOnlySpan<T> items);

            // Existing:
            // public void AddRange<TDerived>(Builder! items) where TDerived: T;
            // public void AddRange<TDerived>(ImmutableArray<TDerived> items) where TDerived: T;
            // public void AddRange<TDerived>(TDerived[] items) where TDerived: T;
            public void AddRange<TDerived>(ReadOnlySpan<TDerived> items) where TDerived : T;

            // TODO: As per https://github.com/dotnet/runtime/issues/822, we should add the corresponding
            //       methods on the builder as well.

            public void CopyTo<T>(Span<T> destination);
        }
    }
}
```
## Add AttributeTargets.Interface to JsonConverterAttribute

**Approved** | [#runtime/33112](https://github.com/dotnet/runtime/issues/33112#issuecomment-870805991)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization
{
    // Adding AttributeTargets.Interface
    [AttributeUsage(AttributeTargets.Class |
                    AttributeTargets.Struct |
                    AttributeTargets.Interface |
                    AttributeTargets.Enum |
                    AttributeTargets.Property |
                    AttributeTargets.Field, AllowMultiple = false)]
    public class JsonConverterAttribute : JsonAttribute
    {        
    }
}
```

## Unseal JsonStringEnumConverter

**Approved** | [#runtime/30486](https://github.com/dotnet/runtime/issues/30486#issuecomment-870804137)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization
{
    // Unsealed
    public class JsonStringEnumConverter : JsonConverterFactory
    {
        // Existing:
        // public JsonStringEnumConverter();
        // public JsonStringEnumConverter(JsonNamingPolicy? namingPolicy = null, bool allowIntegerValues = true);

        // Existing methods, now sealed
        public sealed override bool CanConvert(Type typeToConvert);
        public sealed override JsonConverter CreateConverter(Type typeToConvert, JsonSerializerOptions options);
    }
}
```

## Consider Reloadable Attribute for Hot Reload 

**Approved** | [#runtime/54633](https://github.com/dotnet/runtime/issues/54633#issuecomment-870814731)

* We should avoid "good names", that is single word names like `[Reloadable]`.
* Assuming you don't like `EditMetadataByCreatingNewTypeAttribute` come back with a few proposals and we can pick one over email

```C#
namespace System.Runtime.CompilerServices
{  
    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct, AllowMultiple = false)]  
    public class EditMetadataByCreatingNewTypeAttribute : Attribute  
    {  
    }
}
```

## Support property ordering on POCOs

**Approved** | [#runtime/54748](https://github.com/dotnet/runtime/issues/54748#issuecomment-870827031)

* It seems the default value shouldn't be zero, but a high enough number (e.g. `int.MaxValue / 2`) so that people can force fields to be at the beginning or the end without having to apply the attribute to all properties
* We should consider having an analyzer that checks that no duplicates exist
    - However, properties/fields without the attribute should be ignored
* The sort order should be across the hierarchy, e.g. a derived type should be able to inject a member between members coming from the base

```C#
namespace System.Text.Json.Serialization
{
    [AttributeUsage(AttributeTargets.Property | AttributeTargets.Field, AllowMultiple = false)]
    public sealed class JsonPropertyOrderAttribute : JsonAttribute
    {
        public JsonPropertyNameAttribute(int order);
        public int Order { get; }
    }
}
```

## FORMATEC should define cfFormat as a ushort

**Rejected** | [#runtime/40555](https://github.com/dotnet/runtime/issues/40555#issuecomment-870829690)

It's a breaking change and it seems this isn't the only place where we don't match the signedness of the native implementation. Considering this type was shipped in 2005 it seems the downsides of the breaking change outweigh the potential benefits.
## LoggerMessageAttribute should have a constructor that takes EventId, LogLevel, and Message

**Rejected** | [#runtime/52276](https://github.com/dotnet/runtime/issues/52276#issuecomment-870849586)

* We don't want to add another overload because the design of attributes is that optional parameters are settable properties and this one is optional for some consumers
* It seems for library developers it would be beneficial to have an analyzer that flags code that doesn't initialize `EventName` to a literal string. And with that, it wouldn't matter if it's a constructor or a property.
* Aesthetically, we can see the argument for wanting a constructor argument but we feel the added complexity isn't worth it.

