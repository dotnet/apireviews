# Quick Reviews 04/09/2021

## API for writeable DOM and C# dynamic support

**Approved** | [#runtime/47649](https://github.com/dotnet/runtime/issues/47649#issuecomment-816912404) | [Video](https://www.youtube.com/watch?v=COnBP3RQXRE&t=0h0m0s)

Looks good as edited

```cs
namespace System.Text.Json.Node
{
    public sealed partial class JsonArray : JsonNode, ICollection<JsonNode?>, IEnumerable<JsonNode?>, IList<JsonNode?>, IEnumerable
    {
        public JsonArray(JsonNodeOptions? options = default(JsonNodeOptions?));
        public JsonArray(JsonNodeOptions options, params JsonNode?[] items);
        public JsonArray(params JsonNode?[] items);
        public int Count { get; }
        public static JsonArray? Create(JsonElement element, JsonNodeOptions? options = default(JsonNodeOptions?));
        bool ICollection<JsonNode?>.IsReadOnly { get; }
        public void Add(JsonNode? item);
        public void Add<T>(T value);
        public void Clear();
        public bool Contains(JsonNode? item);
        public IEnumerator<JsonNode?> GetEnumerator();
        public int IndexOf(JsonNode? item);
        public void Insert(int index, JsonNode? item);
        public bool Remove(JsonNode? item);
        public void RemoveAt(int index);
        void ICollection<JsonNode?>.CopyTo(JsonNode?[]? array, int arrayIndex);
        IEnumerator IEnumerable.GetEnumerator();
        public override void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = null);
    }
    public abstract partial class JsonNode
    {
        internal JsonNode();
        public JsonNode? this[int index] { get; set; }
        public JsonNode? this[string propertyName] { get; set; }
        public JsonNodeOptions? Options { get; }
        public JsonNode? Parent { get; }
        public JsonNode Root { get; }
        public JsonArray AsArray();
        public JsonObject AsObject();
        public JsonValue AsValue();
        public string GetPath();
        public virtual TValue GetValue<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors | DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.PublicProperties)]TValue>();
        public static explicit operator bool(JsonNode value);
        public static explicit operator byte(JsonNode value);
        public static explicit operator char(JsonNode value);
        public static explicit operator DateTime(JsonNode value);
        public static explicit operator DateTimeOffset(JsonNode value);
        public static explicit operator decimal(JsonNode value);
        public static explicit operator double(JsonNode value);
        public static explicit operator Guid(JsonNode value);
        public static explicit operator short(JsonNode value);
        public static explicit operator int(JsonNode value);
        public static explicit operator long(JsonNode value);
        public static explicit operator bool?(JsonNode? value);
        public static explicit operator byte?(JsonNode? value);
        public static explicit operator char?(JsonNode? value);
        public static explicit operator DateTimeOffset?(JsonNode? value);
        public static explicit operator DateTime?(JsonNode? value);
        public static explicit operator decimal?(JsonNode? value);
        public static explicit operator double?(JsonNode? value);
        public static explicit operator Guid?(JsonNode? value);
        public static explicit operator short?(JsonNode? value);
        public static explicit operator int?(JsonNode? value);
        public static explicit operator long?(JsonNode? value);
        [CLSCompliantAttribute(false)]
        public static explicit operator sbyte?(JsonNode? value);
        public static explicit operator float?(JsonNode? value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ushort?(JsonNode? value);
        [CLSCompliantAttribute(false)]
        public static explicit operator uint?(JsonNode? value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ulong?(JsonNode? value);
        [CLSCompliantAttribute(false)]
        public static explicit operator sbyte(JsonNode value);
        public static explicit operator float(JsonNode value);
        public static explicit operator string?(JsonNode? value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ushort(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator uint(JsonNode value);
        [CLSCompliantAttribute(false)]
        public static explicit operator ulong(JsonNode value);
        public static implicit operator JsonNode(bool value);
        public static implicit operator JsonNode(byte value);
        public static implicit operator JsonNode(char value);
        public static implicit operator JsonNode(DateTime value);
        public static implicit operator JsonNode(DateTimeOffset value);
        public static implicit operator JsonNode(decimal value);
        public static implicit operator JsonNode(double value);
        public static implicit operator JsonNode(Guid value);
        public static implicit operator JsonNode(short value);
        public static implicit operator JsonNode(int value);
        public static implicit operator JsonNode(long value);
        public static implicit operator JsonNode?(bool? value);
        public static implicit operator JsonNode?(byte? value);
        public static implicit operator JsonNode?(char? value);
        public static implicit operator JsonNode?(DateTimeOffset? value);
        public static implicit operator JsonNode?(DateTime? value);
        public static implicit operator JsonNode?(decimal? value);
        public static implicit operator JsonNode?(double? value);
        public static implicit operator JsonNode?(Guid? value);
        public static implicit operator JsonNode?(short? value);
        public static implicit operator JsonNode?(int? value);
        public static implicit operator JsonNode?(long? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(sbyte? value);
        public static implicit operator JsonNode?(float? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(ushort? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(uint? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode?(ulong? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode(sbyte value);
        public static implicit operator JsonNode(float value);
        public static implicit operator JsonNode?(string? value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode(ushort value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode(uint value);
        [CLSCompliantAttribute(false)]
        public static implicit operator JsonNode(ulong value);
        public static JsonNode? Parse(string json, JsonNodeOptions? nodeOptions = default(JsonNodeOptions?), JsonDocumentOptions documentOptions = default(JsonDocumentOptions));
        public static JsonNode? Parse(ref Utf8JsonReader reader, JsonNodeOptions? nodeOptions = default(JsonNodeOptions?));
        public static JsonNode? Parse(IO.Stream utf8Json, JsonNodeOptions? nodeOptions = default(JsonNodeOptions?), JsonDocumentOptions documentOptions = default(JsonDocumentOptions));
        public static JsonNode? Parse(ReadOnlySpan<byte> utf8Json, JsonNodeOptions? nodeOptions = default(JsonNodeOptions?), JsonDocumentOptions documentOptions = default(JsonDocumentOptions));
        public string ToJsonString(JsonSerializerOptions? options = null);
        public override string ToString();
        public abstract void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = null);
    }
    public partial struct JsonNodeOptions
    {
        private int _dummyPrimitive;
        public bool PropertyNameCaseInsensitive { readonly get; set; }
    }
    public sealed partial class JsonObject : JsonNode, ICollection<KeyValuePair<string, JsonNode?>>, IDictionary<string, JsonNode?>, IEnumerable<KeyValuePair<string, JsonNode?>>, IEnumerable
    {
        public JsonObject(JsonNodeOptions? options = default(JsonNodeOptions?));
        public JsonObject(IEnumerable<KeyValuePair<string, JsonNode?>> properties, JsonNodeOptions? options = default(JsonNodeOptions?));
        public int Count { get; }
        public static JsonObject? Create(JsonElement element, JsonNodeOptions? options = default(JsonNodeOptions?));
        bool ICollection<KeyValuePair<string, JsonNode?>>.IsReadOnly { get; }
        ICollection<string> IDictionary<string, JsonNode?>.Keys { get; }
        ICollection<JsonNode?> IDictionary<string, JsonNode?>.Values { get; }
        public void Add(KeyValuePair<string, JsonNode?> property);
        public void Add(string propertyName, JsonNode? value);
        public void Clear();
        public bool ContainsKey(string propertyName);
        public IEnumerator<KeyValuePair<string, JsonNode?>> GetEnumerator();
        public bool Remove(string propertyName);
        bool ICollection<KeyValuePair<string, JsonNode?>>.Contains(KeyValuePair<string, JsonNode> item);
        void ICollection<KeyValuePair<string, JsonNode?>>.CopyTo(KeyValuePair<string, JsonNode>[] array, int index);
        bool ICollection<KeyValuePair<string, JsonNode?>>.Remove(KeyValuePair<string, JsonNode> item);
        bool IDictionary<string, JsonNode?>.TryGetValue(string propertyName, [System.Diagnostics.CodeAnalysis.NotNullWhen(true)]out JsonNode jsonNode);
        IEnumerator IEnumerable.GetEnumerator();
        public bool TryGetPropertyValue(string propertyName, out JsonNode? jsonNode);
        public override void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = null);
    }
    public abstract partial class JsonValue : JsonNode
    {
        private protected JsonValue(JsonNodeOptions? options = null);
        public static JsonValue? Create<T>(T value, JsonNodeOptions? options = default(JsonNodeOptions?));
        public abstract bool TryGetValue<[DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberTypes.PublicConstructors | DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.PublicProperties)]T>([System.Diagnostics.CodeAnalysis.NotNullWhen(true)]out T? value);
    }
}
```
## Enhance user-facing API for strongly-typed ILogger messages

**Approved** | [#runtime/36022](https://github.com/dotnet/runtime/issues/36022#issuecomment-816960233) | [Video](https://www.youtube.com/watch?v=COnBP3RQXRE&t=0h8m51s)

We got rid of all attribute ctor parameters, because it's complicated validation logic done by the generator.

```C#
namespace Microsoft.Extensions.Logging
{
    [AttributeUsage(AttributeTargets.Method)]
    public sealed partial class LoggerMessageAttribute : Attribute
    {
        public LoggerMessageAttribute();
        public int? EventId { get; set; }
        public string? EventName { get; set; }
        public LogLevel? Level { get; set; }
        public string Message { get; set; }
    }
}
```

We reviewed the current generator diagnostics and had the following feedback (using the current numbers, which need to be adjusted to account for IDs already being used):

* Error 10: Either generics should work, or it also should check that it's not in a generic type
* Error 12: Too opinionated, too noisy, don't check.
* Error 17: A related error, the log level was specified as both a parameter and as an attribute value, should also be expressed.
* Error 18: Should be a warning, not an error.

## Add LoggerMessage.Define overloads accepting up to 16 arguments.

**Rejected** | [#runtime/50913](https://github.com/dotnet/runtime/issues/50913#issuecomment-816966012) | [Video](https://www.youtube.com/watch?v=COnBP3RQXRE&t=1h29m48s)

We think there's not a scenario for this, and we shouldn't add it until there is one.
