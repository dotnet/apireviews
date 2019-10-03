# Quick Reviews 7/2/2019

## Add WriteTo convenience APIs on JsonDocument and JsonProperty

**Approved** | [#corefx/39037](https://github.com/dotnet/corefx/issues/39037#issuecomment-507769350) | [Video](https://www.youtube.com/watch?v=SdNdvSN_OHY&t=0h0m0s)

We liked option Part A) + Part B) 2, i.e. a single `WriteTo(writer)` on all three elements.

> Can we instead add Write methods to `Utf8JsonWriter`?

We don't want this for layering reasons, i.e. the DOM knows about the writer, but the writer shouldn't know about the DOM.
## PipeOptions, StreamPipeReaderOptions, and StreamPipeWriterOptions should not hardcode default sizes.

**Approved** | [#corefx/37984](https://github.com/dotnet/corefx/issues/37984#issuecomment-507774253) | [Video](https://www.youtube.com/watch?v=SdNdvSN_OHY&t=0h9m37s)

That makes sense. We should make the same change across

* `PipeOptions` constructor
* `StreamPipeReaderOptions` constructor
* `StreamPipeWriterOptions` constructor

After construction, the properties should reflect the actual default. In other words, we should only change the literal of the parameter itself to `-1`.

@bartonjs brought up the question if we should expose something like `Buffer.DefaultBufferSize` that is `-1`. We believe that's overkill. Also, many of the BCL construct that accept buffer sizes currently don't accept negative values. We could change that, but this would be a sizable work item and we're in the wrong part in the cycle to do that.


## Deprecate PipeWriter.OnReaderCompleted

**Approved** | [#corefx/38362](https://github.com/dotnet/corefx/issues/38362#issuecomment-507770664) | [Video](https://www.youtube.com/watch?v=SdNdvSN_OHY&t=0h22m51s)

@davidfowl the method is abstract. Does that mean we'll make it virtual and a no-nop? Also, what does deprecation mean to you, marking in Obsolete? What's the message gonna say?
## Change how the Arm intrinsics are exposed

**Approved** | [#corefx/37199](https://github.com/dotnet/corefx/issues/37199#issuecomment-507775790) | [Video](https://www.youtube.com/watch?v=SdNdvSN_OHY&t=0h23m40s)

This was reviewed and approved over e-mail. I've started the work on changing the surface area to match this here: https://github.com/dotnet/coreclr/pull/25508, but it won't get merged until after dotnet/master begins targeting .NET 5.
