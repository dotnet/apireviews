# Quick Reviews 2/5/2019

## Expose line number and byte position in line as public properties on Utf8JsonReader

**Needs Work** | [#corefx/34768](https://github.com/dotnet/corefx/issues/34768#issuecomment-460749514) | [Video](https://www.youtube.com/watch?v=jfJkaggdGws&t=0h0m0s)

This looks useful. We discussed whether we should expose this on the state as well so that consumers can use that information when invalid schema information was found (rather than when the reader detects invalid JSON).

The current design has the unfortunate behavior that the line/byte position represents where the next token will start reading. Note that this isn't even the start of next token (due to skipped whitespace). Since schema validation requires the token to be read, it's always at the end of the token, rather than at the beginning.

@ahsonkhan please explore what the documentation for these APIs would say to explaint their behavior. We can then review wether that's something developers will be able to reason about.
## Option to support trailing commas with JsonReader

**Approved** | [#corefx/34794](https://github.com/dotnet/corefx/issues/34794) | [Video](https://www.youtube.com/watch?v=jfJkaggdGws&t=0h33m21s)

## Add (Try)GetDateTime(Offset) to Utf8JsonReader and JsonDocument

**Approved** | [#corefx/34690](https://github.com/dotnet/corefx/issues/34690#issuecomment-460761972) | [Video](https://www.youtube.com/watch?v=jfJkaggdGws&t=0h44m38s)

Looks good.

We should consider applying the same principle from the writer to the reader: the writer differentiates between the underlying primitive and the convenience conversion (e.g. `WriteString` that takes a `DateTime`). For example, instead of `GetDateTime()` we'd have `GetStringAsDateTime`. That also makes it clear where we expect the value to come from, e.g. `GetNumberAsInt32` makes it clear we won't parse the `int` from a `string`. And it makes the reader and the writer more consistent.
## System.Text.Utf8Char data type to represent UTF-8 text data

**Needs Work** | [#corefx/34094](https://github.com/dotnet/corefx/issues/34094#issuecomment-460774440) | [Video](https://www.youtube.com/watch?v=jfJkaggdGws&t=1h7m45s)

* We need to decide if we want -- or need -- language support, including built-in conversions. If so, we can't ship user defined operators.
* Should the API be in `System.Text`? We probably want to align this choice wherever we'll think we'll want to ship the UTF8 string type.
* How should we name the type? We probably want to align this choice with the UTF8 string support and the language support.
* Does this type need consideration for marshaling? We'll want to makes sure that the behavior we have in 3.0 we don't need to change later.

It seems as a next step we should have a meeting with the language folks.
## Rune.DecodeFirstRune and friends

**Approved** | [#corefx/34822](https://github.com/dotnet/corefx/issues/34822#issuecomment-460780898) | [Video](https://www.youtube.com/watch?v=jfJkaggdGws&t=1h42m43s)

Looks good.

* We'll have to make sure that the `out` parameter `charsConsumed` makes sense with the final name of `Utf8Char`. If it doesn't have `char` in the name. Alternatively, we could decide to use something like `consumedCount` that implies that it's number of elements, regardless of the type.

```C#
public static OperationStatus Decode(ReadOnlySpan<char> source, out Rune result, out int charsConsumed);
public static OperationStatus Decode(ReadOnlySpan<Utf8Char> source, out Rune result, out int charsConsumed);
public static OperationStatus DecodeFromEnd(ReadOnlySpan<char> source, out Rune result, out int charsConsumed);
public static OperationStatus DecodeFromEnd(ReadOnlySpan<Utf8Char> source, out Rune result, out int charsConsumed);
```

@KrzysztofCwalina, do you have an opinion on the name of the `out` parameter?
