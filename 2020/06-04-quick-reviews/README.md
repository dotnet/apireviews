# Quick Reviews 6/4/2020

## Add overloads to string trimming

**Approved** | [#runtime/14386](https://github.com/dotnet/runtime/issues/14386#issuecomment-639002448) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=0h0m0s)

We made minor adjustments:

```C#
namespace System
{
    public partial class String
    {
        public string RemoveStart(string value, StringComparison comparisonType);
        public string RemoveEnd(string value, StringComparison comparisonType);
    }

    public static partial class MemoryExtensions
    {
        public static ReadOnlySpan<char> RemoveStart(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
        public static ReadOnlySpan<char> RemoveEnd(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
    }
}
namespace System.Globalization
{
     public class CompareInfo
     {
        public Range RemovePrefix(string source, string prefix, CompareOptions options = CompareOptions.None);
        public Range RemovePrefix(ReadOnlySpan<char> source, ReadOnlySpan<char> prefix, CompareOptions options = CompareOptions.None);

        public Range RemoveSuffix(string source, string suffix, CompareOptions options = CompareOptions.None);
        public Range RemoveSuffix(ReadOnlySpan<char> source, ReadOnlySpan<char> suffix, CompareOptions options = CompareOptions.None);
     }
}
```
## Non value returning TaskCompletionSource

**Approved** | [#runtime/17393](https://github.com/dotnet/runtime/issues/17393#issuecomment-639004797) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=0h37m42s)

Looks good as proposed.

```C#
namespace System.Threading.Tasks
{
    public class TaskCompletionSource
    {
        public TaskCompletionSource();
        public TaskCompletionSource(object? state);
        public TaskCompletionSource(TaskCreationOptions creationOptions);
        public TaskCompletionSource(object? state, TaskCreationOptions creationOptions);
        public Task Task { get; }
        public void SetCanceled();
        public void SetException(IEnumerable<Exception> exceptions);
        public void SetException(Exception exception);
        public void SetResult();
        public bool TrySetCanceled();
        public bool TrySetCanceled(CancellationToken cancellationToken);
        public bool TrySetException(IEnumerable<Exception> exceptions);
        public bool TrySetException(Exception exception);
        public bool TrySetResult();
    }
}
```
## Add methods to convert between hexadecimal strings and bytes

**Approved** | [#runtime/17837](https://github.com/dotnet/runtime/issues/17837#issuecomment-639018813) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=0h42m8s)

* We decided to go with a simpler route.

```C#
namespace System
{
    public static class Convert
    {
        public static byte[] FromHexString(string s);
        public static byte[] FromHexString(ReadOnlySpan<char> chars);

        public static string ToHexString(byte[] inArray);
        public static string ToHexString(byte[] inArray, int offset, int length);
        public static string ToHexString(ReadOnlySpan<byte> bytes);
    }
}
```
## TCP Fast Open implementation?

**Approved** | [#runtime/1476](https://github.com/dotnet/runtime/issues/1476#issuecomment-639025242) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=1h9m0s)

* We need to decide wether we throw or whether we do a softwar fallback, but naming looks fine:

```C#
namespace System.Net.Sockets
{
    public enum SocketOptionName
    {
        FastOpen = 15
    }
}
```
## Add Task.WhenAny(Task,Task) overload to avoid Task[] allocation

**Approved** | [#runtime/23021](https://github.com/dotnet/runtime/issues/23021#issuecomment-639034041) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=1h20m41s)

Looks good as proposed

```C#
public class Task
{
    // existing
    //public static Task<Task> WhenAny(params Task[] tasks);
    //public static Task<Task> WhenAny(IEnumerable<Task> tasks);
    //public static Task<Task<TResult>> WhenAny<TResult>(params Task<TResult>[] tasks);
    //public static Task<Task<TResult>> WhenAny<TResult>(IEnumerable<Task<TResult>> tasks);

    // new
    public static Task<Task> WhenAny(Task task1, Task task2);
    public static Task<Task<TResult>> WhenAny<TResult>(Task<TResult> task1, Task<TResult> task2);
}
```
## Add System.Numerics.Half 16 bit floating point number conforming to IEEE 754:2008 binary16

**Approved** | [#runtime/936](https://github.com/dotnet/runtime/issues/936#issuecomment-639050110) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=1h30m9s)

* Type looks good as proposed
* But we decided to put it in `System` to align with the other primitves

```C#
namespace System
{
    public readonly struct Half : IComparable, IFormattable, IComparable<Half>, IEquatable<Half>, IConvertible, ISpanFormattable
    {
        public static readonly Half MinValue;
        public static readonly Half Epsilon;
        public static readonly Half MaxValue;
        public static readonly Half PositiveInfinity;
        public static readonly Half NegativeInfinity;
        public static readonly Half NaN;
        public static bool IsInfinity(Half h);
        public static bool IsFinite(Half value);
        public static bool IsNaN(Half h);
        public static bool IsNegativeInfinity(Half h);
        public static bool IsPositiveInfinity(Half h);
        public static bool IsNormal(Half h);
        public static bool IsSubnormal(Half h);
        public static bool IsNegative(Half h);
        public static Half Parse(string s);
        public static Half Parse(string s, NumberStyles style);
        public static Half Parse(string s, NumberStyles style, IFormatProvider provider);
        public static Half Parse(string s, IFormatProvider provider);
        public static Half Parse(ReadOnlySpan<char> s);
        public static Half Parse(ReadOnlySpan<char> s, NumberStyles style);
        public static Half Parse(ReadOnlySpan<char> s, IFormatProvider provider);
        public static Half Parse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider);
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider provider);
        public static bool TryParse(string s, out Half result);
        public static bool TryParse(string s, NumberStyles style, IFormatProvider provider, out Half result);
        public static bool TryParse(ReadOnlySpan<char> s, out Half result);
        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Half result);
        public int CompareTo(object value);
        public int CompareTo(Half value);
        public bool Equals(Half obj);
        public override bool Equals(object obj);
        public override int GetHashCode();
        public TypeCode GetTypeCode();
        public string ToString(IFormatProvider provider);
        public string ToString(string format);
        public string ToString(string format, IFormatProvider provider);
        public override string ToString();
        public static explicit operator Half(float value);
        public static explicit operator Half(double value);
        public static explicit operator float(Half value);
        public static explicit operator double(Half value);
        public static bool operator ==(Half left, Half right);
        public static bool operator !=(Half left, Half right);
        public static bool operator <(Half left, Half right);
        public static bool operator >(Half left, Half right);
        public static bool operator <=(Half left, Half right);
        public static bool operator >=(Half left, Half right);
    }
}
```
## Add "Create" method to EqualityComparer<> class

**Needs Work** | [#runtime/24460](https://github.com/dotnet/runtime/issues/24460#issuecomment-639054509) | [Video](https://www.youtube.com/watch?v=H3obznO9_uo&t=1h47m29s)

Can someone update the sample to make a compelling case for the currently proposed API? The sample code was for a different proposal.

If `Distinct` is the only use case, then maybe we should fix `Distinct` itself.
