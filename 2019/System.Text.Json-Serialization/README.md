# System.Text.Json (Serialization)

Status: **Review not complete** | 
[API Ref](https://github.com/dotnet/corefx/issues/34372)

## Round 1

[Video](https://www.youtube.com/watch?v=I_CNwnkNDNA)

* We're shooting for an MVP in the .NET Core 3.0 timeframe, mostly due to
  time constraints but also because baking a serializer takes time
* We should compare & contrast usability
* "What's the percentage of customers we want to be happy with the built-in JSON
  serialization."
* We shouldn't return `Span<T>`; we should let the caller pass in the buffer to
  fill. This likely also requires `out` parameters to indicate how many bytes
  were written.
    - Make them regular static methods (rather than extension methods).
* The APIs returning `ValueTask` should probably return `Task`
* The writer APIs should take the value and then the writer (source -> destination)
    - IOW, this:
    ```C#
    public static ValueTask ToJsonAsync(this PipeWriter writer, object value, JsonConverterOptions options = null, CancellationToken cancellationToken = default);
    public static ValueTask ToJsonsync(this Stream stream, object value, JsonConverterOptions options = null, CancellationToken cancellationToken = default);
    ```
    - Should be this:
    ```C#
    public static ValueTask ToJsonAsync(object value, PipeWriter writer, JsonConverterOptions options = null, CancellationToken cancellationToken = default);
    public static ValueTask ToJsonsync(object value, Stream stream, JsonConverterOptions options = null, CancellationToken cancellationToken = default);
    ```
* The APIs taking `PipeWriter` might be problematic because it implies that the
  JSON component depends on pipelines. We probably want this the other way
  around.
* We should avoid the `To` and `From` prefixes, we should go with something simpler 
    - e.g. `JsonSerializer.Read`, `JsonSerializer.ReadAsync`, `JsonSerializer.Write`, `JsonSerializer.WriteAsync`
* Should we have `Utf8` in the name somewhere? How would we grow the serializer
  to support other encodings, such as UTF16?
* Should the methods like `ToJson` and `ToJsonString` that only take `object`
  also take a `Type` or be generic?
* The nullable properties on the options class probably shouldn't be nullable
  because there is only one global behavior.
    - We believe we should strive for the least confusion values, which would be
      these:
    - `SerializeNullValues`: `true`
    - `DeserializeNullValues`: `true`
    - `CaseInsensitivePropertyName`: `false`
* We should consider moving `MaxDepth` into reader/writer options

## Round 2

[Video](https://www.youtube.com/watch?v=mAg2UGrkJdA)

* `JsonSerializer`
    - The `Write` methods are weird because they produce a `byte[]`, we'll want
      a name that indicates a value is being produced (e.g. `ToBytes()`). We
      also discussed not having the API and instead take `Span<T>` but that's
      hard as the caller has no way of knowing how big the buffer would be and
      the callee can't easily be made incremental. However, we probably want an
      API like this which probably requires us to expose a state value that the
      caller has to pass in, similar to how the `JsonReader` works.
    - Let's remove these methods for now; callers can just construct a
      `MemoryStream`, and call the `Write` method that takes a stream.
    - The `Write` method (by-design). It seems that *might* not be desirable. We
      might want to redesign this part.
    - We should rename `WriteString` to `ToString`
    - We should rename `ReadString` to `Parse`
    - We should rename the names taking text `Parse`
    - We should remove the `Read` method that takes `ReadOnlySequence<T>`
    - We should remove the `Read` method that takes `Span<T>`
    - Do we ever want to make this type instantiable? If so we'll have to think
      about the name of the methods and the type because they would clash.
* `JsonSerializerOptions`
    - `SkipNullValueOnRead` -> `SkipNullPropertyOnRead`
    - `SkipNullValueOnWrite` -> `SkipNullPropertyOnWrite`
    - `DefaultBufferSize` should be nullable
    - We've talked a lot about how to customize the serialization behavior, such
      as whether attributes, reflection context, or callbacks would be better.
      This isn't trivial; we'll have to determine what caching granularity we
      need and whether it would work well for AOT scenarios.
    - Krzysztof will submit a proposal for a policy API that would be useful for
      Azure SDK.
* Open Issues:
    - We need to think about AOT generation in the context of policies more
    - We need to think about version resiliency / (overflow/underflow roundtripping)

## Round 3

[Video](https://www.youtube.com/watch?v=_CdV75tEsVk)

* We currently don't expose pipeline-based APIs but there is a
  [separate issue](https://github.com/dotnet/corefx/issues/35808) for that.
* Attribute programming model
    - Adding attributes to the options feels strange
    - It seems the current naming policies don't support handling properties vs.
      dictionary keys differently.
    - The camel casing rules are lot more complicated. We should take a look at
      what custom rules JSON.NET has to make sure we've sane default for common
      strings like ID and URL.
* `JsonSerializerOptions`
    - The constructor argument should be a property as it's more in-line with
      our guidance.
    - The challenge is that this isn't a setting that can (or even should) be
      changed after `GetClassInfo()` was called.
    - Maybe we should just throw exceptions if options are changed after objects
      have been serialized. This gives people the easiest way to set the options
      (all just properties that can be set in order) while having deterministic
      behavior that informs the developer when they are doing things that are
      most likely wrong.
    - `GetClassInfo()` we should change to `GetTypeInfo()` as it's not specific
      to classes.
* `JsonPropertyInfo`
    - We shouldn't be exposing UTF8 strings here; they are just internal caches
    - It's unclear how to ignore properties, e.g. the representation for
      `[DoNotSerializer]`.

## Round 4

[Video](https://www.youtube.com/watch?v=I8jzQ2oMf_o) |
[Reader/Writer based deserialization](https://github.com/dotnet/corefx/issues/34372) |
[Property name policy](https://github.com/dotnet/corefx/issues/36351)

* We should add an abstract base type for all JSON related attributes, such as
  `JsonSerializationAttribute` that all others derive from
    - Groups them nicely in docs
    - Avoids having to take literally any attribute or having to add a gazillion
      overloads.
* We should consider splitting the `JsonIgnorePropertyAttribute` into separate attributes
    - It's confusion that applying that attribute without setting an properties
      is a no-op, especially because the name implies that something is being
      ignored.
* We should revisit the namespace layout
    - Right now, the reader/writer are very low-level types
* There is an issue with merging attributes with multiple values across the
  hierarchy
* We should consider whether we need attributes at all or whether we can we can
  get away without having any attributes and rely on global configuration.
