# Quick Reviews 1/22/2019

## Add IAsyncEnumerable<T> support to System.Threading.Channels

**Approved** | [#corefx/32742](https://github.com/dotnet/corefx/issues/32742#issuecomment-456511279) | [Video](https://www.youtube.com/watch?v=jjgr5xruXj4&t=0h0m0s)

We concluded that exposing it as a method is better as it's "destructive", i.e. it drains the items. We settled on `ReadAllAsync()`.
## IAsyncDisposable.ConfigureAwait(bool continueOnCapturedContext)

**Approved** | [#corefx/33336](https://github.com/dotnet/corefx/issues/33336) | [Video](https://www.youtube.com/watch?v=jjgr5xruXj4&t=0h23m17s)

## PlaceholderText API addition to TextBox control

**Approved** | [#winforms/134](https://github.com/dotnet/winforms/issues/134#issuecomment-456519247) | [Video](https://www.youtube.com/watch?v=jjgr5xruXj4&t=0h29m43s)

:shipit: #238
## Allow recovering writable/heapable buffers from "lower level" types.

**Rejected** | [#corefx/33717](https://github.com/dotnet/corefx/issues/33717#issuecomment-456520584) | [Video](https://www.youtube.com/watch?v=jjgr5xruXj4&t=0h45m26s)

* `Overlaps<T>()` with memories. We concluded we don't want these overloads. We generally want to poeple to go from memory to span; we also believe they are fast enough. If we ever need them for perf reasons, we can revisit this.

* `SliceTo`. We're unsure whether this API will fix the usability issues of the `Overlap<T>()` methods. For now, we don't think we want this API. However, we might want add a different API that will address these issues and makes working with spans when you need to compare bounds of other values (memories, other span, strings etc) based on how to two other spans relate to each other.
## Add Rune creation API from UTF-16 surrogate pair

**Approved** | [#corefx/34683](https://github.com/dotnet/corefx/issues/34683#issuecomment-456524835) | [Video](https://www.youtube.com/watch?v=jjgr5xruXj4&t=0h52m11s)

Can we name this just `TryCreate`, given the overloads? It's clear from your signature that you're creating a rune from a surrogate pair.

We should also add the corresponding constructor.
