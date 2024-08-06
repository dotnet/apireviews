# API Review 08/01/2024

## WebSocket keep-alive timeout API

**Approved** | [#aspnetcore/57072](https://github.com/dotnet/aspnetcore/issues/57072#issuecomment-2264114693)

- What are the default values?
    - We can, but don't have to, use the same ones as runtime
- We also have KestrelServerLimits.KeepAliveTimeout
    - It doesn't accept 0
    - It defaults to 130 seconds
- What value do we use to express that there is no timeout?
    - MaxValue?
    - 0?
    - Timeout.InfiniteTimeSpan?
- Validate with a test that passing MaxValue doesn't break things (regardless of whether it means no timeout)
- A comment on the runtime PR suggests it was Ping before it was Pong

API Approved!
