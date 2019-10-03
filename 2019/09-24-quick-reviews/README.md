# Quick Reviews 9/24/2019

## Add a Generic version of GetValues to Enum (probably GetName/GetNames)

**Approved** | [#corefx/27453](https://github.com/dotnet/corefx/issues/27453#issuecomment-534690642) | [Video](https://www.youtube.com/watch?v=8M5rFkINsus&t=0h0m0s)

I stand corrected, we believe we'd prefer this API shape:

```C#
namespace System
{
    public partial abstract class Enum
    {
        public static TEnum[] GetValues<TEnum>() where TEnum : struct, Enum;
        public static string GetName<TEnum>(TEnum value) where TEnum : struct, Enum;
        public static string[] GetNames<TEnum>() where TEnum : struct, Enum;     
        public static bool IsDefined<TEnum>(TEnum value) where TEnum : struct, Enum;       
        public static bool HasFlag<TEnum>(TEnum value, TEnum test) where TEnum : struct, Enum;
    }
}
```
## Add Path.RemoveRelativeSegments Api

**Approved** | [#corefx/30701](https://github.com/dotnet/corefx/issues/30701#issuecomment-534695882) | [Video](https://www.youtube.com/watch?v=8M5rFkINsus&t=0h24m12s)

Alright, here is the shape we'd like to see:

```C#
namespace System.IO
{
    public class Path
    {
        public static string RemoveRedundantSegments(string path);
        public static string RemoveRedundantSegments(ReadOnlySpan<char> path);
        public static bool TryRemoveRedundantSegments(ReadOnlySpan<char> path, Span<char> destination, out int charsWritten);
    }
}
```
## Add List<T> AsSpan to CollectionsMarshal

**Approved** | [#corefx/31597](https://github.com/dotnet/corefx/issues/31597#issuecomment-534703891) | [Video](https://www.youtube.com/watch?v=8M5rFkINsus&t=0h37m23s)

Approved shape:

```C#
namespace System.Runtime.InteropServices
{
    public partial static class CollectionsMarshal
    {
        public static Span<T> AsSpan(List<T> list);
        public static ReadOnlySpan<T> AsReadOnlySpan(List<T> list);

        // We don't want this one:
        // public Memory<T> AsMemory(List<T> list);
    }
}
```
