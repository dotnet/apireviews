# Quick Reviews 10/3/2017

## Add QueueUserWorkItem for local threadpool queues

**Approved** | [#corefx/12442](https://github.com/dotnet/corefx/issues/12442) | [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=0h0m0s)

## Add CancellationTokenRegistration.Token property

**Approved** | [#corefx/23828](https://github.com/dotnet/corefx/issues/23828#issuecomment-333935487) | [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=0h36m16s)

Approved as proposed.
## Support casting ReadOnlyMemory<T> to Memory<T>

**Approved** | [#corefx/23879](https://github.com/dotnet/corefx/issues/23879#issuecomment-333939600) | [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=1h26m39s)

OK, it seems we should separate the discussion on the pinnable references. But the readonly casting looks reasonable. We should, however, choose a generic type name so that we can add the pinnable stuff later on. `Buffers` seems like a good candidate:

```C#
namespace System.Runtime.InteropServices
{
    public static class BuffersMarshal
    {
        public static bool TryGetArray<T>(ReadOnlyMemory<T> readOnlyMemory, out ArraySegment<T> arraySegment);
        public static Memory<T> AsMemory<T>(ReadOnlyMemory<T> readOnlyMemory);
    }
}
```
## Add IsGenericTypeParameter and IsGenericMethodParameter to System.Type

**Approved** | [#corefx/23883](https://github.com/dotnet/corefx/issues/23883#issuecomment-333940885) | [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=1h40m56s)

Looks good, but we don't understand why the properties have to be virtual. It seems they are in nature static. At the end you explain that but how would we eliminate the virtual method call?
## S.R.CS.Unsafe and read-only references

**Approved** | [#corefx/23916](https://github.com/dotnet/corefx/issues/23916#issuecomment-333942166) | [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=1h45m26s)

Looks good as proposed.
## Add MemoryHandle.HasPointer

**Approved** | [#corefx/23974](https://github.com/dotnet/corefx/issues/23974#issuecomment-333943725) | [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=1h49m46s)

The member on `MemoryHandle` is called `PinnedPointer`. We should align the name here, so `HasPinnedPointer`. It seems @KrzysztofCwalina is unsure whether the existing property is named correctly. He'll chase this up offline.
