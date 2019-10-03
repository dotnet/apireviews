# Quick Reviews 3/19/2019

## Make internal directory separator helpers public

**Approved** | [#corefx/31570](https://github.com/dotnet/corefx/issues/31570) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=0h0m0s)

## ColorConverter in System.ComponentModel.TypeConverters does not handle SystemColors

**Approved** | [#corefx/34252](https://github.com/dotnet/corefx/issues/34252) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=0h31m15s)

## Rename Range to IndexRange

**Rejected** | [#corefx/35145](https://github.com/dotnet/corefx/issues/35145#issuecomment-474491396) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=0h38m18s)

We discussed this today but we feel it's too late to make this change now.
## Simpler way to specify leaveOpen in StreamWriter constructor

**Approved** | [#corefx/8173](https://github.com/dotnet/corefx/issues/8173#issuecomment-474495698) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=0h42m48s)

Just realized that adding a bool would butt heads with `StreamReader(Stream, bool)`. Need to look at this a bit more to see if we can come up with a pattern that will work well across our readers/writers.
## Implement Utf8JsonReader.Skip() with support for incomplete data

**Approved** | [#corefx/33295](https://github.com/dotnet/corefx/issues/33295#issuecomment-474501111) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=0h48m7s)

`TrySkip()` is unfortunate because it's an unbounded buffering read; compared to calling `Read()` which would throw data away as you go. That looks like a perf trap. But it's convenient so let's keep it.

We shouldn't do `Skip()` because callers typically can't know whether it's the final block.

The `TrySkipToDepth(int targetDepth)` API seems fine. Skipping is a weird term, but hey, that's the term everyone else is using so let's keep that.
## Add APIs for some threading metrics

**Approved** | [#corefx/35500](https://github.com/dotnet/corefx/issues/35500#issuecomment-474513603) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=1h0m36s)

Looks good.

The only concern is about local vs. global. Unless there is a specific scenario needing the differentiation. If there isn't we should just expose the total.
## Add SslStream.NegotiatedCipherSuite

**Approved** | [#corefx/34867](https://github.com/dotnet/corefx/issues/34867#issuecomment-474516862) | [Video](https://www.youtube.com/watch?v=vHqgsQ23GRg&t=1h22m39s)

This does *NOT* look good, but it seems we have issues with adjacent numbers that we'll have to to separate by underscores. Given that this means this enum will never look ".NET-ty", and that like five people in the world will care about this API, we might as well expose the IANA names.

Every time I'll be looking at this, I'll throw up in little my mouth, but hey. Approved.

We should make `NegotiatedCipherSuite` nullable in cases we can't determine the TLS cipher. The API might also throw if it wasn't negotiated that.
