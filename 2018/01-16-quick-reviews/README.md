# Quick Reviews 1/16/2018

## API Propsal: char.GetUnicodeCategory(unicode scalar)

**Approved** | [#corefx/26173](https://github.com/dotnet/corefx/issues/26173#issuecomment-358057543) | [Video](https://www.youtube.com/watch?v=rSVM5RmAhso&t=0h0m0s)

Looks good but we should rename the parameter:

```csharp
 public static class CharUnicodeInfo
 {
      public static UnicodeCategory GetUnicodeCategory(int codePoint);
 }
```
## Helper class for dealing with native shared libraries and function pointers

**Approved** | [#corefx/17135](https://github.com/dotnet/corefx/issues/17135#issuecomment-358069191) | [Video](https://www.youtube.com/watch?v=rSVM5RmAhso&t=0h17m1s)

* `IDisposable`. We're concerned that by making this type `IDisposable` we lead people down the path of disposing these instances over-aggressively (due to static code analysis rules), resulting in undefined behavior. However, we expect most people never having to unload the library. Thus, we believe it's more consistent with other concepts, such as `AssemblyLoadContext`, by calling it `Unload`.

* `throwOnError`. Since failing seems to be the default here, we've changed the APIs to follow the Try-pattern.

* `SearchDirectories`. We've removed `SearchDirectories` as there seems to be no scenario where someone would need to probe that; most probing should be handled by calling `TryOpen.`

```csharp
public sealed class NativeLibrary
{
    public static bool TryOpen(string name, DllImportSearchPath paths, out NativeLibrary result);

    public IntPtr Handle { get; }
    public bool TryGetSymbolAddress(string name, out IntPtr result);
    public bool TryGetDelegate<T>(string name, out T result);
    public void Unload();
}
```

## Add MemoryPool APIs

**Approved** | [#corefx/26357](https://github.com/dotnet/corefx/issues/26357#issuecomment-358079441) | [Video](https://www.youtube.com/watch?v=rSVM5RmAhso&t=0h56m19s)

* **Minimum size**. The sentinel of `int.MinValue` seems over the top, `-1` seesm good enough. `1` would be too harsh, as the semantics we want is "give me any buffer size that is convenient for the pool".

* **Finalizer**. Don't define a finalizer. Otherwise, all derived types (even the fully managed ones) go into the finalizer queue.

* **Default**. Renamed to `Shared` to align with `ArrayPool<T>`.

* **Dispose**. What should `Dispose` do for the `Shared` instance? It seems no-op is the best choice, otherwise everyone has to special case when folks.

```csharp
public abstract class MemoryPool<T> : IDisposable
{
    public static MemoryPool<T> Shared { get; }

    public abstract OwnedMemory<T> Rent(int minBufferSize=-1);
    public abstract int MaxBufferSize { get; }

    protected MemoryPool();
    public void Dispose();
    protected abstract void Dispose(bool disposing);
}
```


## String-like extension methods to ReadOnlySpan<char> Epic

**Approved** | [#corefx/21395](https://github.com/dotnet/corefx/issues/21395#issuecomment-358087851) | [Video](https://www.youtube.com/watch?v=rSVM5RmAhso&t=1h31m24s)

```c#
// Approved, but needs to go somewhere else due to globalization

public static class MemoryExtensions
{
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    
    public static bool Equals(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static int Compare(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static bool Contains(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);

    public static bool StartsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static bool EndsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static bool IsWhiteSpace(this ReadOnlySpan<char> span);
}

// Needs more work

public static class MemoryExtensions
{
    // APIs designed to avoid allocations:

    public static bool Remove(this ReadOnlySpan<char> source, int startIndex, int count, Span<char> destination);
    
    public static bool Replace(this ReadOnlySpan<char> source, ReadOnlySpan<char> oldValue, ReadOnlySpan<char> newValue, Span<char> destination, out int bytesWritten);
    
    // do we need bytesWritten? It should be source.Length on success
    public static bool ToUpper(this ReadOnlySpan<char> source, Span<char> destination);
    public static bool ToUpperInvariant(this ReadOnlySpan<char> source, Span<char> destination);
    public static bool ToLower(this ReadOnlySpan<char> source, Span<char> destination);
    public static bool ToLowerInvariant(this ReadOnlySpan<char> source, Span<char> destination);
}

// We probably don't want these    

public static class MemoryExtensions
{
    public static bool PadLeft(this ReadOnlySpan<char> source, int totalWidth, Span<char> destination); 
    public static bool PadLeft(this ReadOnlySpan<char> source, int totalWidth, char paddingChar, Span<char> destination);
    public static bool PadRight(this ReadOnlySpan<char> source, int totalWidth, Span<char> destination);
    public static bool PadRight(this ReadOnlySpan<char> source, int totalWidth, char paddingChar, Span<char> destination);
}
```
