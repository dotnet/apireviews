# Quick Reviews 4/10/2018

## Remove SequenceMarshal.TryGetMemoryManager for ReadOnlySequence

**Approved** | [#corefx/28959](https://github.com/dotnet/corefx/issues/28959) | [Video](https://www.youtube.com/watch?v=FwqoZUlanYg&t=0h-9m-58s)

## Make CreateFromPinnedArray visible and move it to MemoryMarshal

**Approved** | [#corefx/28954](https://github.com/dotnet/corefx/issues/28954) | [Video](https://www.youtube.com/watch?v=FwqoZUlanYg&t=0h5m0s)

## Remove .Length from MemoryManager<T>

**Approved** | [#corefx/28751](https://github.com/dotnet/corefx/issues/28751) | [Video](https://www.youtube.com/watch?v=FwqoZUlanYg&t=0h13m35s)

## Add GetPinnableReference back to Span and ReadOnlySpan

**Approved** | [#corefx/28969](https://github.com/dotnet/corefx/issues/28969#issuecomment-380190393) | [Video](https://www.youtube.com/watch?v=FwqoZUlanYg&t=0h22m45s)

* The API looks fine. It's a bit weird to have these APIs on `Span<T>` and `ReadOnlySpan<T>`, it would be more consistent to have these APIs on `MemoryMarshal`. However, one could also argue that these API are are safe, as they return `null` for empty spans, so callers will get a null reference rather than being able to get into corrupted states.

* It's true that we mostly don't have extension methods there, but I don't see a reason why we wouldn't declare them as extension methods. However, the real issue is that moving them to `MemoryMarshal` is that without the using, the feature wouldn't work. Yes, its not different from Linq, but it seems desirable to have these APIs just work.

Hence, we believe these should be instance methods on `Span<T>` and `ReadOnlySpan<T>`. We'd still mark the APIs as `[EditorBrowsable(Never)]` as this API is plumbing only.
