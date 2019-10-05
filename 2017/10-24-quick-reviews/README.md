# Quick Reviews 10/24/2017

## Add Microseconds and Nanoseconds to TimeStamp, DateTime, and DateTimeOffset

**Needs Work** | [#corefx/24555](https://github.com/dotnet/corefx/issues/24555#issuecomment-339062792) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h-39m-32s)

Given the above concerns about overflow and precision, let's discuss a bit more on what would the right return type should be.
## SocketTaskExtensions as instance methods

**Approved** | [#corefx/24442](https://github.com/dotnet/corefx/issues/24442#issuecomment-339063781) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h1m6s)

Given that the methods are already instance methods on the implementation, it implementation wise there is no benefit of doing the move. The driving factor here would be if we wanted to expose for customers as instance methods, so, for example, they can derive from `Socket` and override them. So we'd go with Option 1.
## Add Array.Sort<T>(T[], int, int, Comparison<T>) overload

**Rejected** | [#corefx/24115](https://github.com/dotnet/corefx/issues/24115#issuecomment-339064985) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h5m25s)

Seems like it 😢 
## Make BigInteger BigEndian-friendly

**Approved** | [#corefx/24575](https://github.com/dotnet/corefx/issues/24575#issuecomment-339072735) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h9m31s)

Looks good, minor comments:

* We may want to swap the `isBigEndian` and `isUnsigned`
* It's a bit odd to have one type for signed and unsigned, but adding `UBigInteger` seems like overkill

## Add SpanExtensions.EndsWith

**Approved** | [#corefx/24840](https://github.com/dotnet/corefx/issues/24840#issuecomment-339074798) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h35m28s)

Seems reasonable. We should consider removing the `Span<byte>` as we believe we can specialize the implementation without performance loss.
## Add SpanExtensions.LastIndexOf

**Approved** | [#corefx/24839](https://github.com/dotnet/corefx/issues/24839#issuecomment-339076860) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h42m33s)

Looks good:

* Why do we need the `struct` constraint?
* Can we just collapse the `T` version and the `byte` versions?
* Why is there no `LastIndexOfAny` version?

## UTF8 Parsing and Formatting

**Approved** | [#corefx/24607](https://github.com/dotnet/corefx/issues/24607#issuecomment-339086989) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=0h49m11s)

* `ParsedFormat`  -> `StandardFormat`
* `ParsedFormat` should implement `IEquatable<ParsedFormat>`
* Follow @benaadams's proposal and drop the type names from methods on `Utf8Parser`
* Rename the `format` parameter on `Utf8Parser` to `standardFormat`
## Add Base64 conversion APIs that support UTF-8 for Span<T> 

**Approved** | [#corefx/24568](https://github.com/dotnet/corefx/issues/24568#issuecomment-339096554) | [Video](https://www.youtube.com/watch?v=OZnaGV2omvI&t=1h26m41s)

Looks good as proposed. It seems @GrabYourPitchforks conceded that the enum is useful. We should, however, remove the `dataLength` parameter from `DecodeFromUtf8InPlace` -- it's not needed.
