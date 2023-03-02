# API Review 03/02/2023

## IPAddress should implement ISpanFormattable/Parsable<IPAddress>

**Approved** | [#runtime/82842](https://github.com/dotnet/runtime/issues/82842#issuecomment-1452422426) | [Video](https://www.youtube.com/watch?v=q5V2PiDqBRg&t=0h0m0s)

Looks good as proposed

```diff
namespace System.Net

public class IPAddress
+    : ISpanFormattable, ISpanParsable<IPAddress>
{
    // all of the interface members implemented explicitly
}
```
## MemoryExtensions.IndexOfAny{Except}WhiteSpace

**Approved** | [#runtime/77959](https://github.com/dotnet/runtime/issues/77959#issuecomment-1452432035) | [Video](https://www.youtube.com/watch?v=q5V2PiDqBRg&t=0h7m16s)

Looks good as proposed.

When looking for consistency, we saw that the `ReadOnlySpan<char>` methods on MemoryExtensions are not entirely consistent with whether there's also a `Span<char>` overload.  In the event that the `Span<char>` members are desired, they're approved by pattern-matching.

Notably, MemoryExtensions.IsWhiteSpace does not have a `Span<char>` overload.

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static int IndexOfAnyWhiteSpace(this ReadOnlySpan<char> span);
    public static int IndexOfAnyExceptWhiteSpace(this ReadOnlySpan<char> span);
    public static int LastIndexOfAnyWhiteSpace(this ReadOnlySpan<char> span);
    public static int LastIndexOfAnyExceptWhiteSpace(this ReadOnlySpan<char> span);
}
```


## MemoryMappedFile.CreateFromFile(SafeFileHandle fileHandle)

**Approved** | [#runtime/79187](https://github.com/dotnet/runtime/issues/79187#issuecomment-1452441875) | [Video](https://www.youtube.com/watch?v=q5V2PiDqBRg&t=0h16m24s)

Looks good as proposed, a simple overload of the ability to create from a FileStream.

Please ensure there is a clear ownership documented for the `fileHandle` parameter in both the leaveOpen and !leaveOpen cases.

```C#
using Microsoft.Win32.SafeHandles;

namespace System.IO.MemoryMappedFiles
{
    public class MemoryMappedFile : IDisposable
    {
        public static MemoryMappedFile CreateFromFile(SafeFileHandle fileHandle, string? mapName, long capacity, MemoryMappedFileAccess access, HandleInheritability inheritability, bool leaveOpen);
    }
}
```
## Add JsonSerializerOptions.TryGetTypeInfo

**Approved** | [#runtime/81269](https://github.com/dotnet/runtime/issues/81269#issuecomment-1452460559) | [Video](https://www.youtube.com/watch?v=q5V2PiDqBRg&t=0h26m2s)

Based on our expectation of there being a low number of callers, and the method having only been added in the most recent version, we've recommended changing the existing method to allow a null return.

```diff
 public partial class JsonSerializerOptions
{
-        public JsonTypeInfo GetTypeInfo(Type type);
+        public JsonTypeInfo? GetTypeInfo(Type type);
} 
```
## Add Random.GetItem(IReadOnlyList<T> choices)

**Rejected** | [#runtime/80651](https://github.com/dotnet/runtime/issues/80651#issuecomment-1452475498) | [Video](https://www.youtube.com/watch?v=q5V2PiDqBRg&t=0h43m51s)

Given that the proposed method is just `choices[random.Next(choices.Length)]`, plus the additional overhead of checking for `choices.Length == 0`, we aren't sure that the method has positive value.

If there were a strong/compelling use case, we might feel differently.

```C#
namespace System.Collections.Generic;

public partial class Random
{
    public T GetItem<T>(ReadOnlySpan<T> choices);
    public T GetItem<T>(IReadOnlyList<T> choices);
}
```
