# Quick Reviews 05/13/2021

## System.Runtime.CompilerServices.RuntimeFeature.VirtualStaticsInInterfaces

**Approved** | [#runtime/49905](https://github.com/dotnet/runtime/issues/49905) | [Video](https://www.youtube.com/watch?v=rMcp-58NXZ0&t=0h0m0s)

## LoggerMessageAttribute should have a constructor that takes EventId, LogLevel, and Message

**Approved** | [#runtime/52276](https://github.com/dotnet/runtime/issues/52276#issuecomment-840716389) | [Video](https://www.youtube.com/watch?v=rMcp-58NXZ0&t=0h0m19s)

We think that only one simplified constructor is necessary.  The generator needs to account for the fact that the EventId/LogLevel/String can now be specified both as the ctor and as the property set -- in the same attribute instance.  (`[LoggerMessage(1, ..., ..., EventId = 2)]` is actually event ID 2)

```C#
partial class LoggerMessageAttribute
{
    public LoggerMessageAttribute(int eventId, LogLevel level, string message) { }
}
```
## Add IServiceScopeFactory.CreateAsyncScope

**Approved** | [#runtime/52152](https://github.com/dotnet/runtime/issues/52152) | [Video](https://www.youtube.com/watch?v=rMcp-58NXZ0&t=0h25m16s)

## Async File IO APIs mimicking Win32 OVERLAPPED

**Approved** | [#runtime/24847](https://github.com/dotnet/runtime/issues/24847#issuecomment-840789333) | [Video](https://www.youtube.com/watch?v=rMcp-58NXZ0&t=0h41m15s)

* WriteAtOffset should be void-returning, and throw if it can't complete, like Stream.Write
* Length shouldn't be a property since it has no meaning on pipes (and throws)
* Scatter/Gather should use IReadOnlyList as the outer generic
* We moved the IO operations to a new System.IO.RandomAccess static
* We moved the SafeHandle.Open to File.OpenHandle
* We had a discussion about extension methods, decided no for now.
* We had a discussion about signed vs unsigned and decided signed returns with overloaded inputs.

```C#
namespace System.IO
{
    public static class RandomAccess
    {
        public static long GetLength(SafeFileHandle handle) => throw null;

        public static int ReadAtOffset(SafeFileHandle handle, Span<byte> buffer, long fileOffset);
        public static int ReadAtOffset(SafeFileHandle handle, Span<byte> buffer, ulong fileOffset);
        public static void WriteAtOffset(SafeFileHandle handle, ReadOnlySpan<byte> buffer, long fileOffset);
        public static void WriteAtOffset(SafeFileHandle handle, ReadOnlySpan<byte> buffer, ulong fileOffset);

        public static long ReadScatterAtOffset(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, long fileOffset);
        public static long ReadScatterAtOffset(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, ulong fileOffset);
        public static void WriteGatherAtOffset(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, long fileOffset);
        public static void WriteGatherAtOffset(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, ulong fileOffset);
    
        public static ValueTask<int> ReadAtOffsetAsync(SafeFileHandle handle, Memory<byte> buffer, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask<int> ReadAtOffsetAsync(SafeFileHandle handle, Memory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteAtOffsetAsync(SafeFileHandle handle, ReadOnlyMemory<byte> buffer, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteAtOffsetAsync(SafeFileHandle handle, ReadOnlyMemory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);

        public static ValueTask<long> ReadScatterAtOffsetAsync(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask<long> ReadScatterAtOffsetAsync(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, ulong fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteGatherAtOffsetAsync(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteGatherAtOffsetAsync(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, ulong fileOffset, CancellationToken cancellationToken = default);
    }

    partial class File
    {
        public static SafeFileHandle OpenHandle(string filePath, FileMode mode = FileMode.Open, FileAccess access = FileAccess.Read, FileShare share = FileShare.Read, FileOptions options = FileOptions.None, long preAllocationSize = 0)
    }
}

namespace Microsoft.Win32.SafeHandles
{
    partial class SafeFileHandle
    {
        public bool IsAsync { get; }
    }
}
```
