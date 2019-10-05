# Quick Reviews 2/20/2018

## Consider adding MemoryMarshal.GetOwner(ROM)

**Approved** | [#corefx/27237](https://github.com/dotnet/corefx/issues/27237#issuecomment-367072826) | [Video](https://www.youtube.com/watch?v=9BC3Q1kgWAA&t=0h0m0s)

We believe this API would slightly better and would be more consistent:

```C#
namespace System.Runtime.InteropServices
{
    public static class MemoryMarshal
    {
        public static bool TryGetOwnedMemory<T, TOwner>(ReadOnlyMemory<T> readOnlyMemory, out TOwner ownedMemory)
           where TOwner: OwnedMemory<T>;
        public static bool TryGetOwnedMemory<T, TOwner>(ReadOnlyMemory<T> readOnlyMemory, out TOwner ownedMemory, out int index, out int length)
           where TOwner: OwnedMemory<T>;
    }
}
```
## Add DecompressionMethods.Brotli?

**Approved** | [#corefx/26995](https://github.com/dotnet/corefx/issues/26995) | [Video](https://www.youtube.com/watch?v=9BC3Q1kgWAA&t=0h25m14s)

## Add file enumeration extensibility points

**Approved** | [#corefx/25873](https://github.com/dotnet/corefx/issues/25873#issuecomment-367082482) | [Video](https://www.youtube.com/watch?v=9BC3Q1kgWAA&t=0h38m11s)

We should probably rename `MatchType.Dos` to `MatchType.Win32`. Otherwise, looks good as proposed.
