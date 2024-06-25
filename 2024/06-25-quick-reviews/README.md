# API Review 06/25/2024

## Expose the remaining const values in TimeSpan

**Approved** | [#runtime/94545](https://github.com/dotnet/runtime/issues/94545#issuecomment-2189535305) | [Video](https://www.youtube.com/watch?v=dD4Xj_sIZQY&t=0h0m0s)

Based on the domain (the `TimeSpan` type itself), the belief is that `long` is correct for everything except `HoursPerDay` (`int`).

While that's not so nice for `seconds * TimeSpan.MillisecondsPerSecond` for passing to (e.g.) `new CancellationTokenSource(int)`, it is more appropriate for working with the TimeSpan type itself.

```C#
namespace System.Collections.Generic;

public class TimeSpan: // elided details
{
        // more elision

        public const long MicrosecondsPerMillisecond = TicksPerMillisecond / TicksPerMicrosecond; //           1,000
        public const long MicrosecondsPerSecond = TicksPerSecond / TicksPerMicrosecond;           //       1,000,000
        public const long MicrosecondsPerMinute = TicksPerMinute / TicksPerMicrosecond;           //      60,000,000
        public const long MicrosecondsPerHour = TicksPerHour / TicksPerMicrosecond;               //   3,600,000,000
        public const long MicrosecondsPerDay = TicksPerDay / TicksPerMicrosecond;                 //  86,400,000,000

        public const long MillisecondsPerSecond = TicksPerSecond / TicksPerMillisecond;           //           1,000
        public const long MillisecondsPerMinute = TicksPerMinute / TicksPerMillisecond;           //          60,000
        public const long MillisecondsPerHour = TicksPerHour / TicksPerMillisecond;               //       3,600,000
        public const long MillisecondsPerDay = TicksPerDay / TicksPerMillisecond;                 //      86,400,000

        public const long SecondsPerMinute = TicksPerMinute / TicksPerSecond;                     //              60
        public const long SecondsPerHour = TicksPerHour / TicksPerSecond;                         //           3,600
        public const long SecondsPerDay = TicksPerDay / TicksPerSecond;                           //          86,400

        public const long MinutesPerHour = TicksPerHour / TicksPerMinute;                         //              60
        public const long MinutesPerDay = TicksPerDay / TicksPerMinute;                           //           1,440

        public const int HoursPerDay = TicksPerDay / TicksPerHour;                               //              24
}
```
## Add JsonElement.ValueSpan

**NeedsWork** | [#runtime/77666](https://github.com/dotnet/runtime/issues/77666#issuecomment-2189742986) | [Video](https://www.youtube.com/watch?v=dD4Xj_sIZQY&t=0h15m8s)


* JsonDocument/JsonElement don't expose their underlying encoding, so we shouldn't add a property that only works assuming UTF-8.
* Exposing the value as a property is also problematic as it's difficult to relate the value safety of the property against the arraypool vs non-arraypool backing of JsonDocument

After a lot of discussion that invented a JsonMarshal type, or adding new overloads of NameEquals/ValueEquals, we ended up deciding to not decide anything right now; we need a better understanding of the driving concerns.

```C#
namespace System.Text.Json
{
    public partial struct JsonElement
    {
        public bool ValueEquals(JsonElement stringElement);
    }

    public partial struct JsonProperty
    {
        public bool NameEquals(JsonProperty other);
    }
}

namespace System.Runtime.InteropServices
{
    // Very raw thoughts here, take with very large grains of salt.
    public partial class JsonMarshal
    {
        public static bool TryGetRawValue(JsonElement element, out ReadOnlySpan<byte> rawValue);

        public static bool TryGetStringValueSpan(JsonElement element, out ReadOnlySpan<byte> utf8ValueSpan);
        public static bool TryGetStringValueSpan(JsonElement element, out ReadOnlySpan<char> valueSpan);

        public static bool TryGetNameSpan(JsonProperty property, out ReadOnlySpan<byte> utf8ValueSpan);
        public static bool TryGetNameSpan(JsonProperty property, out ReadOnlySpan<char> valueSpan);
    }
}
```

## Extend System.Guid with a new creation API for v7

**Approved** | [#runtime/103658](https://github.com/dotnet/runtime/issues/103658#issuecomment-2189843346) | [Video](https://www.youtube.com/watch?v=dD4Xj_sIZQY&t=1h46m3s)


* `NewGuidv7(DateTime)` => `CreateVersion7(DateTimeOffset)`
* `Guid.Max` => `Guid.AllBitsSet`
* We discussed adding a CreateRandom, but the general consensus was that the alias was unnecessary/confusing so long as we didn't have `NewGuid()` paired with `NewGuidV7()`.
* We discussed leaving out the parameterless overload (always requiring the timestamp), but since 99% of callers of V7 create want DateTimeOffset.UtcNow, we should just give that ease of use.

```C#
namespace System;

public partial struct Guid
{
    public static Guid AllBitsSet { get; }

    public int Variant { get; }
    public int Version { get; }

    public static Guid CreateVersion7();
    public static Guid CreateVersion7(DateTimeOffset timestamp);
}
```

