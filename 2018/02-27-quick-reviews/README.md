# Quick Reviews 2/27/2018

## [API Change] Drop `IMemoryList` and replace with abstract `ReadOnlySequenceSegment`

**Approved** | [#corefx/27500](https://github.com/dotnet/corefx/issues/27500) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=0h0m0s)

## Add GetPosition overload to ReadOnlySequence that only takes an int/long

**Approved** | [#corefx/27403](https://github.com/dotnet/corefx/issues/27403#issuecomment-368977923) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=0h16m25s)

We decided to go with @KrzysztofCwalina's proposal:

```c#
var position = sequence.GetPosition(100, sequence.Start);
// and
var position = sequence.GetPosition(100);
```
## [API change] Move Memory.TryGetXxx Extensions to MemoryMarshal.TryGetXxx

**Approved** | [#corefx/27451](https://github.com/dotnet/corefx/issues/27451#issuecomment-368979558) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=0h21m28s)

Argument makes sense, but we don't need the second one as the existing `TryGetArray` covers that.

```csharp
public static class MemoryMarshal
{
    public static bool TryGetString(ReadOnlyMemory<char> readOnlyMemory, out string text, out int start, out int length);
}
````

But we still need to remove these instance methods:

```csharp
public static partial class MemoryExtensions
{
    public static bool TryGetString(this ReadOnlyMemory<char> readOnlyMemory, out string text, out int start, out int length);
}

public static partial struct Memory<T>
{
    public bool TryGetArray(out ArraySegment<T> arraySegment)
}
```
## Change new QueueUserWorkItem method to use `TState`

**Approved** | [#corefx/27464](https://github.com/dotnet/corefx/issues/27464) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=0h28m35s)

## Expose Thread.GetCurrentProcessorId() as a public API

**Approved** | [#corefx/16767](https://github.com/dotnet/corefx/issues/16767#issuecomment-368983869) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=0h29m30s)

Makes but we believe it (1) should be a method and (2) live on `Thread`.

```csharp
namespace System.Threading
{
    public partial class Thread
    {
        public static int GetCurrentProcessorId();
    }
}
```
## SocketsHttpHandler: Consider exposing setting for maximum response drain size

**Approved** | [#corefx/27329](https://github.com/dotnet/corefx/issues/27329) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=0h40m36s)

## New APIs in SyndicationFeedFormatter

**Approved** | [#corefx/24668](https://github.com/dotnet/corefx/issues/24668#issuecomment-369004374) | [Video](https://www.youtube.com/watch?v=1pR7fDL0PBA&t=1h47m22s)

BTW: structs renamed to `*Data`, delegates are `*Callback`, args renamed to `data`.
