# API Review 06/14/2021

## JsonSerializer support for TimeSpan in 3.0?

**Approved** | [#runtime/29932](https://github.com/dotnet/runtime/issues/29932#issuecomment-860853752) | [Video](https://www.youtube.com/watch?v=OF1RNuK0zhQ&t=0h0m0s)

* We believe it's an acceptable breaking change even though we serialized `TimeSpan` before as a POCO. If developers don't want the new behavior, they can register their own `TimeSpan` converter. We may want to provide a "generic" converter that provides POCO style-serialization for any type (e.g. `DefaultPropertyConverter<T>`). The source generator is new, so it's not a breaking change there regardless.
    - @layomia could you file a separate issue for this?
* There is no way to configure the emitted format of the the time span (we emit it as `days:hours:minute:seconds`). We could add an API to configure this later, but we don't believe it's needed.

```C#
namespace System.Text.Json.Serialization.Metadata
{
    public static class JsonMetadataServices
    {
        public static JsonConverter<TimeSpan> TimeSpanConverter { get; }
    }
}
```
## Support DateOnly and TimeOnly in JsonSerializer

**NeedsWork** | [#runtime/53539](https://github.com/dotnet/runtime/issues/53539#issuecomment-860868464) | [Video](https://www.youtube.com/watch?v=OF1RNuK0zhQ&t=0h14m39s)

* This is the first time where we split the reference assembly between .NET Core and .NET Standard so we should make sure we're happy with that added complexity.
    - Alternatively, we could make `DateOnly` and `TimeOnly` available as a .NET Standard 2.0 package.
    - Or we maybe we change the source generator to bind via method syntax (rather than a property) so that they could be provided as extension methods, that, for example, the user provides.
    - Or we could expose a generic method like `JsonConverter<T> JsonMetadataServices.GetConverter<T>()` that can return built-in generators without having to talk about the type.
    - Or we could source generator them in case `DateOnly`/`TimeOnly` exists but those properties don't.
* We need think about how we can support types that aren't in the .NET Standard 2.0 API surface

```C#
namespace System.Text.Json.Serialization.Metadata
{
    public static partial class JsonMetadataServices
    {
        public static JsonConverter<DateOnly> DateOnlyConverter { get; }
        public static JsonConverter<TimeOnly> TimeOnlyConverter { get; }
    }
}
```
## Writing raw JSON values when using Utf8JsonWriter

**Approved** | [#runtime/1784](https://github.com/dotnet/runtime/issues/1784#issuecomment-860908002) | [Video](https://www.youtube.com/watch?v=OF1RNuK0zhQ&t=0h36m26s)

* We should rename `skipValidation` to `skipInputValidation` to make it clear that this about catching issues with untrusted user input, rather than structural coding issues from the side of the caller.
* When `skipInputValidation` is set to `false` we should enforce that no comments are present (or alternatively we strip them).
* If we need to add options, we can either add an overload with an enum, a struct, or just more Booleans
* The name `RawValue` is fine, but if we ever add the overload that takes a property name the convention would be to call it `WriteRaw` which is a bit odd

```C#
namespace System.Text.Json
{
    public sealed partial class Utf8JsonWriter
    {
        public void WriteRawValue(ReadOnlySpan<byte> utf8Json, bool skipInputValidation = false);
        public void WriteRawValue(ReadOnlySpan<char> json, bool skipInputValidation = false);
        public void WriteRawValue(string json, bool skipInputValidation = false);
    }
}
```

## Minimalistic HTTP2 flow control API

**Approved** | [#runtime/53372](https://github.com/dotnet/runtime/issues/53372#issuecomment-860917481) | [Video](https://www.youtube.com/watch?v=OF1RNuK0zhQ&t=1h37m53s)

* We believe `EnableDynamicHttp2StreamWindowSizing` could be served as an app context switch, because it's just an escape hatch
    - The name should follow app context switch naming conventions but use "Disable" rather than "Enable" naming, because it's about turning it off.

```C#
namespace System.Net.Http
{
    public partial class SocketsHttpHandler
    {
        public int InitialHttp2StreamWindowSize { get; set; } // = 65535;
    }
}
```

## Add a method consuming ReadOnlySpan<byte> to System.HashCode

**Approved** | [#runtime/48702](https://github.com/dotnet/runtime/issues/48702#issuecomment-860922933) | [Video](https://www.youtube.com/watch?v=OF1RNuK0zhQ&t=1h52m15s)

* We don't believe we'll ever need to add `Add<T>(ReadOnlySpan<T> value)`
* We don't want to constrain the implementation to do the same as calling `Add<T>(T value)` in a loop

```C#
namespace System
{
    public struct HashCode
    {
         public void Add(ReadOnlySpan<byte> value);
    }
}
```

