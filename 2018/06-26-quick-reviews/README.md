# Quick Reviews 6/26/2018

## Add SpanExtensions.LastIndexOf StringComparison overload

**Approved** | [#corefx/30649](https://github.com/dotnet/corefx/issues/30649) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=0h-7m-39s)

## API for interpretting Span<byte> as struct without copying

**Approved** | [#corefx/30613](https://github.com/dotnet/corefx/issues/30613#issuecomment-400400504) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=0h4m9s)

This API and scenarios make sense but we shouldn't use `Struct` in the name. It seems to make more sense to align this with `Unsafe.AsRef()`, thus:


```csharp
static class MemoryMarshal
{
    public static ref readonly T AsRef<T>(ReadOnlySpan<byte> span) where T : struct;
    public static ref T AsRef(Span<byte> span) where T : struct;
}
```
## Add Interlocked ops w/ explicit memoryOrder

**Needs Work** | [#corefx/29544](https://github.com/dotnet/corefx/issues/29544#issuecomment-400405014) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=0h27m29s)

Are we sure the enum members are sensibly named? The names do not make a lot of sense to us, but that might just domain knowledge. Also, @GrabYourPitchforks just noticed that `Release` is being deprecated. Before approving I'd like to get confirmation on the names.
## Path Span APIs that write into a specified buffer

**Approved** | [#corefx/27418](https://github.com/dotnet/corefx/issues/27418#issuecomment-400409299) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=0h41m43s)

Let's remove ` TryGetTempPath` but otherwise it looks good.
## Need a method similar to S.R.CS.RuntimeHelpers.InitializeArray, but for spans

**Approved** | [#corefx/26948](https://github.com/dotnet/corefx/issues/26948#issuecomment-400409742) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=0h55m11s)

Alright, this is it:

```csharp
class RuntimeHelpers
{
	ReadOnlySpan<T> CreateSpan<T>(RuntimeFieldHandle fldHandle);	
}
```
## "ItemRef" - Ref element accessor for types that already have an indexer.

**Rejected** | [#corefx/25189](https://github.com/dotnet/corefx/issues/25189#issuecomment-400418417) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=1h4m29s)

This issue was about the naming convention (concept suffixing with `Ref`); it wasn't a particular set of members. It seems there are issues with the particular APIs being added (specifically `List<T>` and `Dictionary<K,V>`.)

Please file API review requests for the specific members so we can review those (or alternatively update the issue description to have their signatures and reopen this one).
## BoundedConcurrentQueue<T>

**Needs Work** | [#corefx/24365](https://github.com/dotnet/corefx/issues/24365) | [Video](https://www.youtube.com/watch?v=PKQLZXybOUA&t=1h24m46s)

