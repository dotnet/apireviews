# Quick Reviews 10/31/2017

## Add TransformationStatus enum from corefxlab

**Rejected** | [#corefx/22412](https://github.com/dotnet/corefx/issues/22412#issuecomment-340833769) | [Video](https://www.youtube.com/watch?v=bHwCPVNQLwo&t=0h0m0s)

This is now tracked as part of other work item.
## Productizing APIs for {ReadOnly}Memory<T> and friends

**Approved** | [#corefx/23862](https://github.com/dotnet/corefx/issues/23862#issuecomment-340845398) | [Video](https://www.youtube.com/watch?v=bHwCPVNQLwo&t=0h5m11s)

We don't want to add the bulk methods. We believe it could become a performance trap and it invites having to keep `Span<T>` and `Memory<T>` in sync.

```C#
// instance methods
public struct Memory<T> {
  public void CopyTo(Memory<T> destination);
  public bool TryCopyTo(Memory<T> destination);
}
public struct ReadOnlyMemory<T> {
  public void CopyTo(Memory<T> destination);
  public bool TryCopyTo(Memory<T> destination);
}
// extension methods
// Merge with SpanExtensions
public static class MemoryExtensions {
  public static void CopyTo<T>(this T[] array, Memory<T> destination);
}
```

We should probably rename `SpanExtensions` to `MemoryExtensions`.
## Add Overlaps/Within extension methods for ReadOnlySpan<T>

**Approved** | [#corefx/24103](https://github.com/dotnet/corefx/issues/24103#issuecomment-340856211) | [Video](https://www.youtube.com/watch?v=bHwCPVNQLwo&t=0h41m10s)

We seem to believe `Overlaps` seems like a good addition (having both overloads), but it looks like `Within` is redundant/not any faster. 

```C#
public static class SpanExtensions
{
    public static bool Overlaps<T>(this ReadOnlySpan<T> first, ReadOnlySpan<T> second);
    public static bool Overlaps<T>(this ReadOnlySpan<T> first, ReadOnlySpan<T> second,  out int elementOffset);
}
```
## Memory and ReadOnlyMemory validation errors not matching

**Approved** | [#corefx/24296](https://github.com/dotnet/corefx/issues/24296#issuecomment-340868030) | [Video](https://www.youtube.com/watch?v=bHwCPVNQLwo&t=1h15m51s)

We went back and forth, but it seems we're in agreement that these can be useful. We shouldn't expose static types `Span` and `Memory` in `InteropServices`. Instead, we should add them to the new [`MemoryMarshal` type](https://github.com/dotnet/corefx/issues/23879#issuecomment-340861229).
