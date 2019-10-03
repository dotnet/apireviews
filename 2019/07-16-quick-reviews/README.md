# Quick Reviews 7/16/2019

## Add Encode(ReadOnlySpan<char>) method to TextEncoder for performance

**Approved** | [#corefx/39523](https://github.com/dotnet/corefx/issues/39523) | [Video](https://www.youtube.com/watch?v=JdaQ2RP2tqM&t=0h-33m-56s)

## Deprecate PipeWriter.OnReaderCompleted

**Approved** | [#corefx/38362](https://github.com/dotnet/corefx/issues/38362) | [Video](https://www.youtube.com/watch?v=JdaQ2RP2tqM&t=0h10m49s)

## (System.Numerics) Cross Product for Vector2 and Vector4

**Approved** | [#corefx/35434](https://github.com/dotnet/corefx/issues/35434#issuecomment-511933825) | [Video](https://www.youtube.com/watch?v=JdaQ2RP2tqM&t=0h32m15s)

Generally this seems like a reasonable API. The only sticking point is that it seems undefined and only DirectX exposes this operation for `Vector2` and `Vector4`. This may be odd, but doesn't seem to be a blocker to us.
## Add StringBuilder(Char)

**Rejected** | [#corefx/17745](https://github.com/dotnet/corefx/issues/17745#issuecomment-511942559) | [Video](https://www.youtube.com/watch?v=JdaQ2RP2tqM&t=1h28m39s)

So after discussion we think it's not worth it. It's been 20 years and this isn't a widespread enough problem.
