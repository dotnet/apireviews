# Quick Reviews 03/02/2021

## Writable DOM and dynamic support for 6.0

**NeedsWork** | [#designs/163](https://github.com/dotnet/designs/pull/163#issuecomment-789181317) | [Video](https://www.youtube.com/watch?v=qnGfdfGnO2E&t=0h0m0s)

* The Azure SDK has a design (and usability studies) that uses structs. Could we lift their design?
    - It sounds like they aren't entirely happy with their design either, but it seems we might be able to merge it.
* The Azure SDK team has a set of JSON payloads that users have to pass to their APIs. We should write sample code that shows how one would do that with the DOM to make sure it's not super busy/complicated.
    - @KrzysztofCwalina will provide @SteveHarter with examples
* The proposal is lazy, but it's class-based which will cause more allocations as one navigates
* `JsonNode`
    - `GetValue<T>` with value types will throw when the value is `null`. It seems robust code should use `int?` or call `TryGetValue()`.
    - `Path` should be a method
    - The `Deserialize()` method should always take options
    - It seems `GetValue<T>` and `Deserialize<T>` are very similar. Should they be combined?
    - `ToString()` should return the JSON, with the special handling of simple string values not including the quotes, like `JsonDocument`
    - `WriteTo()` should probably have an overload that accepts a `Stream`, maybe also one that accepts a `TextWriter`
    - `WriteTo()` probably needs an async version
    - `Parse()` should probably be the inverse of `WriteTo`, in terms of types it accepts and async overloads
    - Should we remove `ValueKind`? `JsonValue<T>` will return `Unspecified` for deferred serialized objects
* `JsonArray`
    - The `Add(object?)` is a typo and should be removed, but it's useful for use with anonymous objects
    - We should consider `Add<T>(T value)` that constructs a `JsonValue<T>` automatically
    - It seems a bit unfortunate that this has deferred deserialization semantics, meaning, if you walk the DOM after calling `Add<T>()` it's a `JsonValue<T>` with no children, even if you added a compound object.
    - People may want to pass in a size that they can index into. This would be different from `List<T>` where the specified size would implicitly add `null` values which makes them valid for indexing. Alternatively, we could make arrays work like a dictionary where the key is an index.
* Action items
    - Create samples for the Azure SDK (which is creating payloads)
    - One sample for modifying an existing payload
    - One sample for pure reading

General types:

```C#
namespace System.Text.Json.Node
{
    public abstract class JsonNode : System.Dynamic.IDynamicMetaObjectProvider {}
    public sealed class JsonArray : JsonNode, IList<JsonNode?> {}
    public sealed class JsonObject : JsonNode, IDictionary<string, JsonNode?> {}
    public abstract class JsonValue : JsonNode {}
    public sealed class JsonValue<T> : JsonValue {}
}
```

Full API:

```C#
namespace System.Text.Json.Node
{
    public abstract class JsonNode : System.Dynamic.IDynamicMetaObjectProvider
    {
        internal JsonNode(); // prevent external derived classes.

        public JsonNode Clone(); // Deep clone.

        // Returns the options specified during creation\deserialization otherwise null.
        // If null, child nodes obtain value from parent nodes recursively.
        public JsonSerializerOptions? Options { get; }

        // JsonArray terse syntax support (no JsonArray cast necessary).
        public virtual JsonNode? this[int index] { get; set; }

        // JsonObject terse syntax (no JsonObject cast necessary).
        public virtual JsonNode? this[string propertyName] { get; set; }

        // JsonValue terse syntax (no JsonValue<T> cast necessary).
        // Return the internal value, a JsonElement conversion, or a custom conversion to the provided type.
        // Allows for common programming model when JsonValue<T> is based on JsonElement or a CLR value.
        // "TypeToGet" vs "T" to prevent collision on JsonValue<T>. (could just be "T" if made non-virtual and dispatch to internal virtual methods instead)
        public virtual JsonValue? GetValue<TypeToGet>();
        public virtual bool TryGetValue<TypeToGet>(out TypeToGet? value);
        // Overloads with the converter for <TypeToGet>:
        public virtual JsonValue? GetValue<TypeToGet>(JsonConverter converter);
        public virtual bool TryGetValue<TypeToGet>(JsonConverter converter, out TypeToGet? value);

        // Return the parent and root nodes; useful for LINQ.
        public JsonNode? Parent { get; }
        public JsonNode? Root { get; }

        // The JSON Path; same "JsonPath" syntax we use for JsonException information.
        public string Path { get; }

        // Serialize\deserialize wrappers. These are helpers and thus can be considered optional.
        // "TypeToDeserialize" could be "T" instead.
        public abstract TypeToDeserialize? Deserialize<TypeToDeserialize>();
        public abstract bool TryDeserialize<TypeToDeserialize>(out TypeToDeserialize? value);
        public string ToJsonString(); // serialize as a string
        // public byte[] ToUtf8Bytes(); // not proposed, but may be useful
        // WriteTo() terminology consistent with Utf8JsonWriter.
        public abstract void WriteTo(System.Text.Json.Utf8JsonWriter writer);
        // Parse() terminology consistent with Utf8JsonReader\JsonDocument.
        public static JsonNode? Parse(string? json, JsonSerializerOptions? options = null);
        public static JsonNode? ParseUtf8Bytes(ReadOnlySpan<byte> utf8Json, JsonSerializerOptions? options = null);
        public static JsonNode? ReadFrom(ref Utf8JsonReader reader, JsonSerializerOptions? options = null);

        // The ValueKind from deserialization. Not used internally but may be useful for consumers.
        public JsonValueKind ValueKind { get; }

        // JsonElement interop
        public static JsonNode GetNode(JsonElement jsonElement);
        public static bool TryGetNode(JsonElement jsonElement, [NotNullWhen(true)] out JsonNode? jsonNode);

        // Dynamic support; implemented explicitly to help hide.
        System.Dynamic.DynamicMetaObject System.Dynamic.IDynamicMetaObjectProvider.GetMetaObject(System.Linq.Expressions.Expression parameter);

        // Explicit operators (can throw) from known primitives.
        public static explicit operator bool(JsonNode value);
        public static explicit operator byte(JsonNode value);
        public static explicit operator DateTime(JsonNode value);
        public static explicit operator DateTimeOffset(JsonNode value);
        public static explicit operator decimal(JsonNode value);
        public static explicit operator double(JsonNode value);
        public static explicit operator Guid(JsonNode value);
        public static explicit operator short(JsonNode value);
        public static explicit operator int(JsonNode value);
        public static explicit operator long(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator sbyte(JsonNode value);
        public static explicit operator float(JsonNode value);
        public static explicit operator char(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ushort(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator uint(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ulong(JsonNode value);

        public static explicit operator bool?(JsonNode value);
        public static explicit operator byte?(JsonNode value);
        public static explicit operator DateTime?(JsonNode value);
        public static explicit operator DateTimeOffset?(JsonNode value);
        public static explicit operator decimal?(JsonNode value);
        public static explicit operator double?(JsonNode value);
        public static explicit operator Guid?(JsonNode value);
        public static explicit operator short?(JsonNode value);
        public static explicit operator int?(JsonNode value);
        public static explicit operator long?(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator sbyte?(JsonNode value);
        public static explicit operator float?(JsonNode value);
        public static explicit operator string?(JsonNode value);
        public static explicit operator char?(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ushort?(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator uint?(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ulong?(JsonNode value);

        // Implicit operators (won't throw) from known primitives.
        public static implicit operator JsonNode(bool value);
        public static implicit operator JsonNode(byte value);
        public static implicit operator JsonNode(DateTime value);
        public static implicit operator JsonNode(DateTimeOffset value);
        public static implicit operator JsonNode(decimal value);
        public static implicit operator JsonNode(double value);
        public static implicit operator JsonNode(Guid value);
        public static implicit operator JsonNode(short value);
        public static implicit operator JsonNode(int value);
        public static implicit operator JsonNode(long value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode(sbyte value);
        public static implicit operator JsonNode(float value);
        public static implicit operator JsonNode?(char value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(ushort value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(uint value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(ulong value);

        public static implicit operator JsonNode(bool? value);
        public static implicit operator JsonNode(byte? value);
        public static implicit operator JsonNode(DateTime? value);
        public static implicit operator JsonNode(DateTimeOffset? value);
        public static implicit operator JsonNode(decimal? value);
        public static implicit operator JsonNode(double? value);
        public static implicit operator JsonNode(Guid? value);
        public static implicit operator JsonNode(short? value);
        public static implicit operator JsonNode(string? value);
        public static implicit operator JsonNode(int? value);
        public static implicit operator JsonNode(long? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode(sbyte? value);
        public static implicit operator JsonNode(float? value);
        public static implicit operator JsonNode?(char? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(ushort? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(uint? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(ulong? value);
    }

    public sealed class JsonArray : JsonNode, IList<JsonNode?>
    {
        public JsonArray(JsonElement jsonElement, JsonSerializerOptions? options = null);

        // Param-based constructors for easy constructor initializers.
        public JsonArray(JsonSerializerOptions? options, params JsonNode[] items);
        public JsonArray(params JsonNode[] items);

        public override JsonNode Clone();

        public override void WriteTo(System.Text.Json.Utf8JsonWriter writer);

        public static JsonArray? Parse(string? json, JsonSerializerOptions? options = null);
        public static JsonArray? ParseUtf8Bytes(ReadOnlySpan<byte> utf8Json, JsonSerializerOptions? options = null);
        public static JsonArray? ReadFrom(ref Utf8JsonReader reader, JsonSerializerOptions? options = null);

        // IList<JsonNode?> (some hidden via explicit implementation):
        public int Count { get ;}
        bool ICollection<JsonNode?>.IsReadOnly { get ;}
        public void Add(object? item);
        public void Add(JsonNode? item);
        public void Clear();
        public bool Contains(JsonNode? item);
        public IEnumerator<JsonNode?> GetEnumerator();
        public int IndexOf(JsonNode? item);
        public void Insert(int index, JsonNode? item);
        public bool Remove(JsonNode? item);
        public void RemoveAt(int index);
        void ICollection<JsonNode?>.CopyTo(JsonNode?[]? array, int arrayIndex);
        IEnumerator IEnumerable.GetEnumerator();
    }

    public sealed class JsonObject : JsonNode, IDictionary<string, JsonNode?>
    {
        public JsonObject(JsonSerializerOptions? options = null);
        public JsonObject(JsonElement jsonElement, JsonSerializerOptions? options = null);

        public override JsonNode Clone();

        public bool TryGetPropertyValue(string propertyName, outJsonNode? jsonNode);

        public override void WriteTo(System.Text.Json.Utf8JsonWriter writer);

        public static JsonObject? Parse(string? json, JsonSerializerOptions? options = null);
        public static JsonObject? ParseUtf8Bytes(ReadOnlySpan<byte> utf8Json, JsonSerializerOptions? options = null);
        public static JsonObject? ReadFrom(ref Utf8JsonReader reader, JsonSerializerOptions? options = null);

        // IDictionary<string, JsonNode?> (some hidden via explicit implementation):
        public int Count { get; }
        bool ICollection<KeyValuePair<string,JsonNode?>>.IsReadOnly { get; }
        ICollection<string> IDictionary<string,JsonNode?>.Keys { get; }
        ICollection<JsonNode?> IDictionary<string,JsonNode?>.Values { get; }
        public void Add(string propertyName,JsonNode? value);
        public void Clear();
        public bool ContainsKey(string propertyName);
        public IEnumerator<KeyValuePair<string,JsonNode?>> GetEnumerator();
        public bool Remove(string propertyName);
        void ICollection<KeyValuePair<string,JsonNode?>>.Add(KeyValuePair<string,JsonNode> item);
        bool ICollection<KeyValuePair<string,JsonNode?>>.Contains(KeyValuePair<string,JsonNode> item);
        void ICollection<KeyValuePair<string,JsonNode?>>.CopyTo(KeyValuePair<string,JsonNode>[] array, int arrayIndex);
        bool ICollection<KeyValuePair<string,JsonNode?>>.Remove(KeyValuePair<string,JsonNode> item);
        IEnumerator IEnumerable.GetEnumerator();
        bool IDictionary<string,JsonNode?>.TryGetValue(string propertyName, outJsonNode? jsonNode);
    }

    // Separate class to make it easy to check type via "if (node is JsonValue)"
    // and to support passing of a value class polymorphically (without the <T>)
    public abstract class JsonValue : JsonNode
    {
        public JsonValue(JsonSerializerOptions? options = null);

        // Possible factory method that doesn't require specifying <T> due to generic type inference:
        // public static JsonValue<T> Create(T value);
    }

    public sealed class JsonValue<T> : JsonValue
    {
        public JsonValue(T value, JsonSerializerOptions? options = null);

        // Allow a custom converter and JsonValueKind to be specified.
        public JsonValue(T value, JsonConverter? converter = null, JsonValueKind valueKind, JsonSerializerOptions? options = null);

        public override JsonNode Clone();

        public override TypeToReturn GetValue<TypeToReturn>();
        public override TypeToReturn GetValue<TypeToReturn>(JsonConverter converter);
        public override bool TryGetValue<TypeToReturn>(out TypeToReturn value);
        public override bool TryGetValue<TypeToReturn>(JsonConverter converter, out TypeToReturn value);

        // The internal raw value.
        public override T Value {get; set;}

        public override void WriteTo(System.Text.Json.Utf8JsonWriter writer);

        public static JsonValue<T> Parse(string? json, JsonSerializerOptions? options = null);
        public static JsonValue<T> ParseUtf8Bytes(ReadOnlySpan<byte> utf8Json, JsonSerializerOptions? options = null);
        public static JsonValue<T> ReadFrom(ref Utf8JsonReader reader, JsonSerializerOptions? options = null);
    }
}
```
