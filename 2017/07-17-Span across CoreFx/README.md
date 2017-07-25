# API Review: Adding `Span<T>`/`Buffer<T>`-based APIs across CoreFX

Status: **Approved with Feedback** | [Issue](https://github.com/dotnet/corefx/issues/21281) |
[Video 1](https://www.youtube.com/watch?v=VKnHYFIQpOs) |
[Video 2](https://www.youtube.com/watch?v=jKQWMCPZ08Y) |
[Video 3](https://www.youtube.com/watch?v=10w3tS03W60)

With the advent of `Span<T>` and `Buffer<T>`, there are a multitude of
improvements we'll want to make to types across CoreCLR/CoreFX. Some of these
changes include exposing `Span<T>`, `Buffer<T>`, `ReadOnlySpan<T>`, or
`ReadOnlyBuffer<T>` themselves and the associated operations on them. Other
changes involve using those types internally to improve memory usage of existing
code. This [issue](https://github.com/dotnet/corefx/issues/21281) isn't about
either of those. Rather, this issue is about what new APIs we want to expose
across CoreFX (with some implementations in CoreCLR/CoreRT).

## Notes 1 | [Video](https://www.youtube.com/watch?v=VKnHYFIQpOs)

* Principles:
    - Consistency in naming: if we add logical overloads to existing types, we
      should stick to existing naming conventions on the type
    - Keep existing behavior of the types as much as possible
    - Logically, provide overloads for byte[]
* `BitConverter`
    - Should take endianness  into account and provide overloads that allows
      consumers to specify the endianness they want
      [corefx#22405](https://github.com/dotnet/corefx/issues/22405)
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
    - We should `MemoryStream`, `UnmanagedMemoryStream`, `BufferStream`,
      and `ReadOnlyBufferStream` should all implement `IHasBuffer<T>`. So
      regardless of whether we make `BufferStream` & `ReadOnlyBufferStream`
      public.
    - Alternatively, we could make the `IHasBuffer<T>.TryGetBuffer()` to the
      `Stream` class. This would make delegation scenario where one dynamically
      delegates to an unknown instance.

## Notes 2 | [Video](https://youtu.be/jKQWMCPZ08Y?t=1355)

* We need general for principles for how `Span<T>`-based APIs should be exposed:
     - If you have a single span and a single error condition (buffer too
       small), us the `TryXxx` pattern
     - If you have multiple buffers or multiple error conditions the transform
       pattern should be used
     - When introducing `Span<T>`-base overloads to existing types, try to match
       the semantics and signature of existing overloads.
* `BitConverter`
    - Should we provide an overload that returns the `bytesWritten` as an `out`
      parameter?
* `TextReader`/`TextWriter`
    - The challenge is deciding what the new virtual methods should do and how
      we handle the overrides in the BCL to make sure we correctly deal with
      instances of types that don't override the new methods.
    - We don't believe these issues will be as bad the `XxxAsync` methods where
      we ran into deadlocks, so we don't expect a bug tail functionally, but
      there is likely going to be a performance bug tail.
* `File`
    - We decided against adding overloads for string that takes
      `ReadOnlySpan<char>`. We're not convinced it's common enough.
    - We believe `AppendAllBytes` might be a performance issue.
    - We believe the `Read` methods are neither convenient nor performant, so we
      shouldn't have it.
    - The `WriteAllBytes` seem fine, but at the same time since we scrapped all
      the other APIs, we believe we should just scrap of all of `File` for now.
* `StringBuilder`
    - We should fix `StringBuilder` to make it for efficient for encoding
      primitives, right now it called `value.ToString()` and writes the result
      to the underlying array. This would not require new APIs on
      `StringBuilder`
    - We should also have a separate meeting to discuss how we can make general
      string formatting more efficient.
* `Encoding`
    - The proposed APIs makes sense, but we should consider aligning the shape
      of the APIs with the general `Transform` pattern
        ```C#
        TransformationStatus Transform(ReadOnlySpan<T1> source, Span<T2> destination, out int bytesConsumed, out int bytesWritten)
        ```
    - We could also match the existing pointer-based overload, which would look
      like this:
        ```C#
        int GetBytes(ReadOnlySpan<char> source, Span<bytes> destination)
        ```
      It would still throw for invalid data (bad encoding).
* `BigInteger`
    - `TryCopyTo` -> `bool TryWriteBytes(Span<bytes> destination, out int bytesWritten)`
    - We should expose a method that computes the size of the needed output
      buffer

## Notes 3 | [Video](https://www.youtube.com/watch?v=10w3tS03W60)

* `IPAddress`
    - `TryWriteBytes` should have `out int bytesWritten`
    - Should we add a `TryFormat` to have symmetry, i.e. if we add a
      `Span<T>`-based parsing, we should have `Span<T>`-based formatting.
* `Socket`
    - `flags` should match the existing `Receive` method and thus be
      `socketFlags`
    - We should add the overloads that return the `SocketError` as an `out`
      parameter, looking at telemetry it seems those are popular
* `SocketTaskExtensions`
    - Fun fact: for .NET Core, these are calling internal APIs directly on
      `Socket`
    - Nothing on `Socket` supports cancellation today; we should investigate
      whether we can support cancellation in principle and add the APIs now.
* `SocketAsyncEventArgs`
    - When the CoreFxLab representation for multiple buffers is stable, we
      should probably add an alternative for `BufferList`.
* `WebSocket`
    - Currently no support for avoiding allocations when the operation completed
      synchronously. Proposal is use `ValueTask` and a struct-based alternative
      `WebSocketReceiveResult`, called `ValueWebSocketReceiveResult`.
* `HttpClient`
    - Skipped for now; it's unclear how we want to handle this given this type
      heavily on `Stream`. Maybe this is addressed by spanifying `Stream`?
* `HashAlgorithm`
    - We shouldn't spanify `TryTransformBlock` and `TryTransformBlockFinal`
      because we only brought `TransformBlock` and `TransformBlockFinal` back
      back for compat. Customers should use `IncrementalHash` instead.
* `RSA`
    - Remove `DecryptValue` and `EncryptValue` -- they always throw
    - `HashData` should be protected, not public
    - The methods `Encrypt`, `Decrypt`, `HashData`, `SignData`, `SignHash`
      should follow the `Try` pattern and return `bool` and return the numbers
      of bytes written as `out` parameter
* `DSA`
    - The methods `CreateSignature`, `HashData`, `SignData` should follow the
      `Try` pattern and return `bool` and return the numbers of bytes written as
      `out` parameter
    - `HashData` should be protected, not public
* `ECDsa`
    - The methods `HashData`, `SignData`, `SignHash` should follow the `Try`
      pattern and return `bool` and return the numbers of bytes written as `out`
      parameter
    - `HashData` should be protected, not public
* `ICryptoTransform`
    - Used by our `CryptoStream`
    - Ideally, we'd leverage default implementations of interfaces to add
      overloads for `TransformBlock` and `TransformBlockFinal` that operate on
      spans.
    - Option 1: Do nothing
    - Option 2: Add a second interface
    - Option 3: Add a second interface implement it on our types
    - Option 4: Wait for default implementations of interfaces
    - Option 5: Do some internal magic
    - Consensus go with (1) and wait for a compelling use case and then see
      whether we can go with 4. Otherwise, a combination of (3) and (4) later
      seems appealing.
* Open Issues / Other APIs
    - Collections / Immutable / Metadata
    - `ImmutableArray<T>` is probably the only one that's super interesting
        + We should add an implicit conversion to `ReadOnlySpan<T>`
    - We should do another pass to make we accept `CancellationToken` everywhere
      where we should