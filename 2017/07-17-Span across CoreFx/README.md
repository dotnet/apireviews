# API Review: Adding `Span<T>`/`Buffer<T>`-based APIs across CoreFX

Status: **Review In Progress** | [Issue](https://github.com/dotnet/corefx/issues/21281) |
[Video](https://www.youtube.com/watch?v=VKnHYFIQpOs)

With the advent of `Span<T>` and `Buffer<T>`, there are a multitude of
improvements we'll want to make to types across CoreCLR/CoreFX. Some of these
changes include exposing `Span<T>`, `Buffer<T>`, `ReadOnlySpan<T>`, or
`ReadOnlyBuffer<T>` themselves and the associated operations on them. Other
changes involve using those types internally to improve memory usage of existing
code. This [issue](https://github.com/dotnet/corefx/issues/21281) isn't about
either of those. Rather, this issue is about what new APIs we want to expose
across CoreFX (with some implementations in CoreCLR/CoreRT).

## Notes

* Principles:
    - Consistency in naming: if we add logical overloads to existing types, we
      should stick to existing naming conventions on the type
    - Keep existing behavior of the types as much as possible
    - Logically, provide overloads for byte[]
* `BitConverter`
    - Should take endianess into account and provide overloads that allows
      consumers to specify the endianess they want
    - Should offer a method that allows arbitrary `T`s to be read/written
    - `TryCopyBytes`
        + Should be called `TryWriteBytes`
        + Then naming the first argument `value` makes sense. If we stick
          with `TryCopyBytes` we should probably name it `source`.
        + `bytes` should be called `destination`
        + `destination` should be the first argument
        + `value` should be second
        + This would also allow us to make them extension methods if wanted
    - Base-64:
        + `Try` doesn't work super well as we have two errors: not enough space
          and invalid data. Invalid data would still throw. We should probably
          follow CoreFxLab and use an enum. Ideally, we'd use the same, so we
          should move `TransformationStatus` down:
            ```C#
            public static TransformationStatus FromBase64(ReadOnlySpan<char> s, Span<byte> destination, out int charsConsumed, out int bytesWritten, Base46FormattingOptions options = ...);
            public static TransformationStatus FromBase64(string s, Span<byte> destination, out int charsConsumed, out int bytesWritten, Base46FormattingOptions options = ...);
            public static string ToBase64String(ReadOnlySpan<byte> bytes, Base64FormattingOptions options = Base64FormattingOptions.None);
            public static TransformationStatus ToBase64Chars(ReadOnlySpan<byte> bytes, Span<char> chars, out int bytesConsumed, out int charsWritten, Base64FormattingOptions options = Base64FormattingOptions.None);
            ```
* `Random`
    - The API shape is fine; the question is what the implementation does. We
      don't want to call the existing array one, because perf tanks. We'll
      call `Sample` as that's the true primitive and doesn't allocate.
* Primitive parse
    - We'll use `ReadOnlySpan<char>` as a slicable string, always in UTF16
    - We'll use `TryFormat` as span-based `ToString`
    - For cases where `ParseExact` exists, we should offer `Span` based versions
      too
    - `TryFormat`: we should rename `chars` to `destination`
        ```C#
            public struct Int16
            {
                public static short Parse(ReadOnlySpan<char> chars, NumberStyles style = NumberStyles.Integer, IFormatProvider provider = null);
                public static bool TryParse(ReadOnlySpan<char> chars, out short result, NumberStyles style = NumberStyles.Integer, IFormatProvider provider = null);
                public bool TryFormat(Span<char> destination, out int charsWritten, string format = null, IFormatProvider provider = null);
            }
        ```
    - We need to reconcile this with CoreFxLab's extension methods
* `Guid`
    - `TryCopyTo` should be `TryWriteBytes(Span<bytes> destination)`
* `String`
    - The constructor taking the delegate is fine. Ideally, we'd use
      `Action<TState, Span<char>>` but `Span<T>` cannot be used a generic
      arguments, so we have to create a custom delegate type. We should probably
      make this a nested type or put it in a different namespace.
    - We could make a set of `Func<>` and `Action<>` that already uses `Span<T>`
      in one of the signatures
    - Right now, the compiler doesn't allow lambdas and `Span<T>` in the same
      scope, regardless of whether it's captured or not.
    - Maybe we should make this a separate issue.
* `Stream`
    - Sync overloads take `Span<byte>`
    - Async overloads take `Buffer<byte>`
    - The implementation on `Stream` will call the existing ones that take
      `byte[]`, but we'll probably acquire the array from the pool
    - Our own derivatives of `Stream` will override the new method to do
      something smarter
    - We'll also have to change our code that consumes `Stream` to use `Span<T>`
      based overloads when appropriate
* `BufferStream` and `ReadOnlyBufferStream`
    - These are "type-based overloads" of `MemoryStream`
    - Instead of public types these could be internal and we can have factor
      methods that return you a `Stream`. Would probably require the
      `IHasBuffer<T>` pattern to make it as versatile as explicit types.
