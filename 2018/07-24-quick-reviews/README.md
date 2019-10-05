# Quick Reviews 7/24/2018

## BoundedConcurrentQueue<T>

**Needs Work** | [#corefx/24365](https://github.com/dotnet/corefx/issues/24365#issuecomment-407484728) | [Video](https://www.youtube.com/watch?v=vkOaqpdiF5U&t=0h0m0s)

Personally, I'm fine with the first two bullet points but I don't understand the last. Could you elaborate? Reading the issue description it sounds like peeking could result in marking all data as "preserve" which would block dequeues from removing data and thus could result in a state where enqueues would fail and the collection is stuck. Is copying not an option?

There are also some concerns about the second bullet point although I find this acceptable as callers have to opt-in.
## Allow easier access to binary representation of blittable types

**Rejected** | [#corefx/26313](https://github.com/dotnet/corefx/issues/26313#issuecomment-407486861) | [Video](https://www.youtube.com/watch?v=vkOaqpdiF5U&t=0h15m40s)

> Isn't this equivalent to turning a `ref T` into a `Span<T>` followed by `Cast<T, byte>`? I thought turning a `ref T` into a `Span<T>` is not possible, even with fast span.

Right. Thus we don't believe we need a dedicated API for this. Yet.
## SocketsHttpHandler: Add MaxHttpVersion property

**Needs Work** | [#corefx/30527](https://github.com/dotnet/corefx/issues/30527#issuecomment-407499225) | [Video](https://www.youtube.com/watch?v=vkOaqpdiF5U&t=0h21m48s)

We've talked with @davidsh about the proposed shape and it looks good.

If we find ourselves wanting to add this property to `HttpMessageHandler` we can just physically move the property from `SocketsHttpHandler` as that's binary/source compatible. We don't need the property to be virtual.

From a usage standpoint, it seems more logical to put the property on the `HttpClient` as that's what vast majority of users is going to do. @davidsh's argument is that API is part of the underlying pipe which is what the handler represents. There is another problem with that design in that consumers of `HttpClient` can still construct an `HttpRequestMessage` which wouldn't/can't honor the value from `HttpClient`.

@bartonjs is proposing to expose `HttpClient.DefaultHttpVersion` (instance property) that would control what the default value for `HttpRequestMessage` objects will be that are constructed by `HttpClient`.

Any objections to this?
