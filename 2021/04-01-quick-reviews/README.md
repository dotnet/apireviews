# Quick Reviews 04/01/2021

## [DialogResult] Add TryAgain and Continue (result 10, and 11) respectively.

**Approved** | [#winforms/4712](https://github.com/dotnet/winforms/issues/4712#issuecomment-812047601)

* Looks good as proposed


```C#
namespace System.Windows.Forms
{
    public partial enum DialogResult
    {
        // ...
        TryAgain,
        Continue
    }
    public partial enum MessageBoxButtons
    {
        // ...
        CancelTryContinue
    }
    public partial enum MessageBoxDefaultButton
    {
        // ...
        Button4
    }
}
```

## Add a notification mechanism for EnC and HotReload

**Approved** | [#runtime/49361](https://github.com/dotnet/runtime/issues/49361#issuecomment-812052792)

* Looks good as proposed

```C#
namespace System.Reflection.Metadata
{
    [AttributeUsage(AttributeTargets.Assembly, AllowMultiple=True)]
    public sealed class MetadataUpdateHandlerAttribute : Attribute
    {
        public MetadataUpdateHandlerAttribute(Type type)   
    }
}
```

## Add BaseUtcOffset to TimeZoneInfo.AdjustmentRule.

**Approved** | [#runtime/50256](https://github.com/dotnet/runtime/issues/50256#issuecomment-812056450)

* Looks good as proposed

```C#
namespace System
{
   public partial class TimeZoneInfo
   {
        public partial class AdjustmentRule
        {
            public TimeSpan BaseUtcOffsetDelta { get; }

            // Existing method:
            // public static AdjustmentRule CreateAdjustmentRule(
            //     DateTime dateStart,
            //     DateTime dateEnd,
            //     TimeSpan daylightDelta,
            //     TransitionTime daylightTransitionStart,
            //     TransitionTime daylightTransitionEnd
            // );

            public static AdjustmentRule CreateAdjustmentRule(
                DateTime dateStart,
                DateTime dateEnd,
                TimeSpan daylightDelta,
                TransitionTime daylightTransitionStart,
                TransitionTime daylightTransitionEnd,
                TimeSpan baseUtcOffsetDelta
            );
         }
    }
}
```

## Developers using JsonSerializer can asynchronously (de)serialize IAsyncEnumerable<T>

**Approved** | [#runtime/1570](https://github.com/dotnet/runtime/issues/1570#issuecomment-812078537)

* We don't believe we need any opt-in or opt-out

```C#
namespace System.Text.Json
{
    public partial class JsonSerializer
    {
        public static IAsyncEnumerable<TValue?> DeserializeAsyncEnumerable<TValue>(Stream utf8Json, JsonSerializerOptions? options = null);
    }
}
```

## API for writeable DOM and C# dynamic support

**NeedsWork** | [#runtime/47649](https://github.com/dotnet/runtime/issues/47649#issuecomment-812123225)

* `JsonNode`
    - `ReadFrom` should be `Parse`
    - `WriteToUtf8` should be `WriteTo`
    - Remove `ToUtf8Bytes()` but keep `ToJsonString()`
    - The conversions seem fine, the equivalent is `JsonValue.Create()` or extracting the value
    - Consider implementing `IConvertible`
    - One downside of implicit conversions is that `jsonArray.Contains(3)` will
      compile but always return `false` because it's reference equality
* `JsonValue`
    - The non-generic class should have a `Value` property of type `object` that returns the boxed value
    - The constructor should be `private protected` so that consumers can't subclass
* `JsonValue<T>`
    - It seems when reading a document most values are going to be `JsonValue<JsonElement>` (as opposed to `JsonValue<int>` or `JsonValue<string>`)
    - Should we make this class internal and only deal with `JsonValue`? This way, callers can't get the `T` wrong. If we do this, we should remove the non-generic `Value` property from `JsonValue` as well.
* `JsonObject`
    - Should this have a property `IEnumerable<string> Properties`?
    - We should implement `ContainsKey()` explicit and add `HasProperty()`
* `JsonNode`
    - Should we have `JsonNode<T> AsValue<T>()`?

```C#
namespace System.Text.Json.Node
{
    public sealed partial class JsonArray : JsonNode, ICollection<JsonNode?>, IEnumerable<JsonNode?>, IList<JsonNode?>, IEnumerable
    {
        public JsonArray(JsonNodeOptions? options = default);
        public JsonArray(JsonElement element, JsonNodeOptions? options = default);
        public JsonArray(JsonNodeOptions options, params JsonNode[] items);
        public JsonArray(params JsonNode[] items);
        public int Count { get; }
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
        public override void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = default);
    }
    public abstract partial class JsonNode
    {
        internal JsonNode();
        public JsonNode? this[int index] { get; set; }
        public JsonNode? this[string key] { get; set; }
        public JsonNodeOptions? Options { get; }
        public JsonNode? Parent { get; }
        public JsonNode Root { get; }
        public JsonArray AsArray();
        public JsonObject AsObject();
        public JsonValue AsValue();
        public string GetPath();
        public virtual TValue GetValue<TValue>(JsonSerializerOptions? options = null);
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
        public static JsonNode? Parse(string json, JsonNodeOptions? nodeOptions = default, JsonDocumentOptions documentOptions = default);
        public static JsonNode? ParseUtf8Bytes(ReadOnlySpan<byte> utf8Json, JsonNodeOptions? nodeOptions = default, JsonDocumentOptions documentOptions = default);
        public static JsonNode? ReadFrom(ref Utf8JsonReader reader, JsonNodeOptions? nodeOptions = default);
        public static JsonNode? ReadFromUtf8Stream(Stream utf8Json, JsonNodeOptions? nodeOptions = default, JsonDocumentOptions documentOptions = default);
        public string ToJsonString(JsonSerializerOptions? options = default);
        public override string ToString();
        public byte[] ToUtf8Bytes(JsonSerializerOptions? options = default);
        public abstract void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = default);
        public void WriteToUtf8Stream(Stream utf8Json, JsonSerializerOptions? options = default);
    }
    public partial struct JsonNodeOptions
    {
        public bool PropertyNameCaseInsensitive { readonly get; set; }
    }
    public sealed partial class JsonObject : JsonNode, ICollection<KeyValuePair<string, JsonNode?>>, IDictionary<string, JsonNode?>, IEnumerable<KeyValuePair<string, JsonNode?>>, IEnumerable
    {
        public JsonObject(JsonNodeOptions? options = default);
        public JsonObject(JsonElement element, JsonNodeOptions? options = default);
        public int Count { get; }
        bool ICollection<KeyValuePair<string, JsonNode?>>.IsReadOnly { get; }
        ICollection<string> IDictionary<string, JsonNode?>.Keys { get; }
        ICollection<JsonNode?> IDictionary<string, JsonNode?>.Values { get; }
        public void Add(string propertyName, JsonNode? value);
        public void Clear();
        public bool ContainsKey(string propertyName);
        public IEnumerator<KeyValuePair<string, JsonNode?>> GetEnumerator();
        public bool Remove(string propertyName);
        void ICollection<KeyValuePair<string, JsonNode?>>.Add(KeyValuePair<string, JsonNode> item);
        bool ICollection<KeyValuePair<string, JsonNode?>>.Contains(KeyValuePair<string, JsonNode> item);
        void ICollection<KeyValuePair<string, JsonNode?>>.CopyTo(KeyValuePair<string, JsonNode>[] array, int arrayIndex);
        bool ICollection<KeyValuePair<string, JsonNode?>>.Remove(KeyValuePair<string, JsonNode> item);
        bool IDictionary<string, JsonNode?>.TryGetValue(string propertyName, out JsonNode jsonNode);
        IEnumerator IEnumerable.GetEnumerator();
        public bool TryGetPropertyValue(string propertyName, out JsonNode? jsonNode);
        public override void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = default);
    }
    public abstract partial class JsonValue : JsonNode
    {
        private protected JsonValue(JsonNodeOptions? options = default);
        public object Value { get; }
        public static JsonValue<T>? Create<T>(T value, JsonNodeOptions? options = default);
        public abstract bool TryGetValue<TValue>(out TValue? value, JsonSerializerOptions? options = default);
    }
    public sealed partial class JsonValue<T> : JsonValue
    {
        public JsonValue(T value, JsonNodeOptions? options = default);
        public new T Value { get; }
        public override TypeToReturn GetValue<TypeToReturn>(JsonSerializerOptions? options = default);
        public override bool TryGetValue<TValue>(out TValue value, JsonSerializerOptions? options = default);
        public override void WriteTo(Utf8JsonWriter writer, JsonSerializerOptions? options = default);
    }
}
```

