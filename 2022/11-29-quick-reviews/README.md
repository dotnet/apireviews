# API Review 11/29/2022

## Deconstruct for DateTime,  DateTimeOffset, DateOnly, and TimeOnly

**Approved** | [#runtime/24601](https://github.com/dotnet/runtime/issues/24601#issuecomment-1331167655) | [Video](https://www.youtube.com/watch?v=i44coDOxgUY&t=0h0m0s)

* We should consider having "simple" deconstructors that return the logical components
    - `DateTime` -> `(DateOnly, TimeOnly)`
    - `DateTimeOffset` -> `(DateOnly, TimeOnly, TimeSpan)`
* Conversely, we should consider adding them as constructors
    - `DateTime(DateOnly, TimeOnly)`
    - `DateTimeOffset(DateOnly, TimeOnly, TimeSpan)`
* We should give milliseconds without microseconds

```C#
namespace System;

public readonly partial struct DateTime
{
    public DateTime(DateOnly date, TimeOnly time);
    public DateTime(DateOnly date, TimeOnly time, DateTimeKind kind);
    public void Deconstruct(out DateOnly date, out TimeOnly time);
    public void Deconstruct(out int year, out int month, out int day);
    //public void Deconstruct(out int year, out int month, out int day, out int hour, out int minute, out int second);
    //public void Deconstruct(out int year, out int month, out int day, out int hour, out int minute, out int second, out int millisecond, out int microsecond);
}

public readonly partial struct DateTimeOffset
{
    public DateTimeOffset(DateOnly date, TimeOnly time, TimeSpan offset);
    public void Deconstruct(out DateOnly date, out TimeOnly time, out TimeSpan offset);
    // public void Deconstruct(out int year, out int month, out int day, out TimeSpan offset);
    // public void Deconstruct(out int year, out int month, out int day, out int hour, out int minute, out int second, out TimeSpan offset);
    // public void Deconstruct(out int year, out int month, out int day, out int hour, out int minute, out int second, out int millisecond, out int microsecond, out TimeSpan offset);
}

public readonly partial struct DateOnly
{
    public void Deconstruct(out int year, out int month, out int day);
}

public readonly partial struct TimeOnly
{
    public void Deconstruct(out int hour, out int minute);
    public void Deconstruct(out int hour, out int minute, out int second);
    public void Deconstruct(out int hour, out int minute, out int second, out int millisecond);
    public void Deconstruct(out int hour, out int minute, out int second, out int millisecond, out int microsecond);
}
```
## IndexOfAnyValues<T>.Contains(T)

**Approved** | [#runtime/78722](https://github.com/dotnet/runtime/issues/78722#issuecomment-1331176246) | [Video](https://www.youtube.com/watch?v=i44coDOxgUY&t=1h5m3s)

* Looks good but we'd prefer if the public method weren't virtual, i.e. the virtuality would be hidden.

```C#
namespace System.Buffers;

public class IndexOfAnyValues<T> where T : IEquatable<T>?
{
    public bool Contains(T value);
}
```
## Alternative span/memory/string splitting API

**Approved** | [#runtime/76186](https://github.com/dotnet/runtime/issues/76186#issuecomment-1331219782) | [Video](https://www.youtube.com/watch?v=i44coDOxgUY&t=1h12m42s)

* Let's drop the `AsRanges()` suffix

```C#
namespace System;

public static class MemoryExtensions
{
    public static int Split(this ReadOnlySpan<char> source, Span<Range> destination, char separator, StringSplitOptions options = StringSplitOptions.None);
    public static int Split(this ReadOnlySpan<char> source, Span<Range> destination, ReadOnlySpan<char> separator, StringSplitOptions options = StringSplitOptions.None);
    public static int SplitAny(this ReadOnlySpan<char> source, Span<Range> destination, ReadOnlySpan<char> separators, StringSplitOptions options = StringSplitOptions.None);
    public static int SplitAny(this ReadOnlySpan<char> source, Span<Range> destination, ReadOnlySpan<string> separators, StringSplitOptions options = StringSplitOptions.None);
}
```
## Add PipeOptions.FirstPipeInstance enum value

**Approved** | [#runtime/76984](https://github.com/dotnet/runtime/issues/76984#issuecomment-1331227478) | [Video](https://www.youtube.com/watch?v=i44coDOxgUY&t=1h50m22s)

* The proposal looks reasonable, but given the angle is security, we can't just no-op this in the non-Windows implementation. Is there a reasonable cross-platform implementation or do we need to consider this a Windows-only feature?
    - If it's Windows-only we should mark the enum member as `[SupportedOSPlatform("windows")]` and throw `PlatformNotSupportedException` from the corresponding methods using it (i.e. the constructors)

```C#
namespace System.IO.Pipes
{
    [Flags]
    public enum PipeOptions
    {
        FirstPipeInstance = unchecked((int)0x00080000)
    }
}
```
