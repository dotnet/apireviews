# Quick Reviews 5/30/2019

## We should base64 encode byte[] when writing Json

**Approved** | [#corefx/36974](https://github.com/dotnet/corefx/issues/36974#issuecomment-497411886) | [Video](https://www.youtube.com/watch?v=EVUtzNK3tdA&t=-1h-21m-41s)

* `Utf8JsonWriter.WriteAsBase64String()` -> `WriteBase64String()`
* `Utf8JsonReader.GetBytes()` -> `GetBytesFromBase64()`
* `Utf8JsonReader.TryGetBytes()` -> `TryGetBytesFromBase64()`
* `JsonElement.GetBytes()` -> `GetBytesFromBase64()`
* `JsonElement.TryGetBytes()` -> `TryGetBytesFromBase64()`
## JsonSerializer writing into Utf8JsonWriter

**Approved** | [#corefx/36714](https://github.com/dotnet/corefx/issues/36714#issuecomment-497417558) | [Video](https://www.youtube.com/watch?v=EVUtzNK3tdA&t=0h21m44s)

* We should make the `stream, reader, writer`-thingie the first argument as they are logically extension methods
* `ReadValue` makes sense
* The `As` infix notation feels odd. We should just call it `WriteValue` and `WriteProperty`. We should also change them in `JsonElement`.
* Adding `WriteProperty(JsonEncodedText propertyName, Utf8JsonWriter writer)` makes sense.
## JsonSerializer reading from Utf8JsonReader

**Approved** | [#corefx/36717](https://github.com/dotnet/corefx/issues/36717#issuecomment-497418459) | [Video](https://www.youtube.com/watch?v=EVUtzNK3tdA&t=0h39m15s)

Approved because we approved the combined one #36714.
## Utf8JsonReader\Writer should support all primitive types

**Needs Work** | [#corefx/36125](https://github.com/dotnet/corefx/issues/36125#issuecomment-497424987) | [Video](https://www.youtube.com/watch?v=EVUtzNK3tdA&t=0h40m55s)

The `char` one might be harder to define. We should get @JamesNK input on those. Basically two options:

* **We can treat it as a number**. If the primary goal is to define serialization behavior, that might be OK. And given that we have to write UTF8 that might be more honest.

* **Treat it as a single character string**. This would mean that we either throw for chars that we can't represent in UTF8 or write garbage.

The other ones seem fine.
