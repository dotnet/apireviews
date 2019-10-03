# Quick Reviews 5/28/2019

## SslStream.AuthenticateAsServer/ClientAsync methods should default CancellationToken to default(CancellationToken)

**Approved** | [#corefx/35283](https://github.com/dotnet/corefx/issues/35283#issuecomment-496610570) | [Video](https://www.youtube.com/watch?v=_VAq-ORjkqk&t=0h0m0s)

We should. Looks good.
## Add Environment.LongTickCount

**Approved** | [#corefx/35499](https://github.com/dotnet/corefx/issues/35499#issuecomment-496612786) | [Video](https://www.youtube.com/watch?v=_VAq-ORjkqk&t=0h26m43s)

And we landed on:

```C#
public static class Environment
{
    public static long TickCount64 { get; }
}
```
## Consider moving RuntimeHelper.GetSubArray to Array

**Rejected** | [#corefx/36741](https://github.com/dotnet/corefx/issues/36741#issuecomment-496613684) | [Video](https://www.youtube.com/watch?v=_VAq-ORjkqk&t=0h32m1s)

Since we don't feel super strongly and the compiler would need to change, we're leaving it where it is.
## Remove in and readonly ref from Activity apis

**Approved** | [#corefx/37396](https://github.com/dotnet/corefx/issues/37396#issuecomment-496615275) | [Video](https://www.youtube.com/watch?v=_VAq-ORjkqk&t=0h34m26s)

@tommcdon / @noahfalk / @sywhang are you OK with this? This change looks good to us.
## Comparing Utf8JsonReader to default instance with the == operator

**Rejected** | [#corefx/37534](https://github.com/dotnet/corefx/issues/37534#issuecomment-496618927) | [Video](https://www.youtube.com/watch?v=_VAq-ORjkqk&t=0h38m47s)

We certainly don't want the comparison operators as they feel hard to define. `IsDefault` feels OK & consistent, but we're unsure how much value this adds. Why can't the receive just read? Why is the validation necessary? Why is the `Try`-pattern not sufficient?

For now, we're deciding to not take the API. If you feel differently, please re-open and answer the above questions.
## StreamPipeReader/Writer should allow to leave underlying stream opened

**Approved** | [#corefx/37701](https://github.com/dotnet/corefx/issues/37701#issuecomment-496622595) | [Video](https://www.youtube.com/watch?v=_VAq-ORjkqk&t=0h49m33s)

The API shape as outlined by @anurse's [comment](https://github.com/dotnet/corefx/issues/37701#issuecomment-495461474) looks good.

@tannergooding has filed a separate bug for removing the explicit buffer sizes and instead use sentinels so we can change their values later.
