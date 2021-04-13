# Quick Reviews 04/13/2021

## C# 10 interpolated strings support, part 1

**Approved** | [#runtime/50601](https://github.com/dotnet/runtime/issues/50601#issuecomment-818990566) | [Video](https://www.youtube.com/watch?v=_lSOebMGeo8&t=0h0m0s)

API Review notes:

* Why do we need the InterpolationHole version for string? (To specialize out of T/object and to avoid future "universal formatting")
* InterpolatedStringBuilder is a ref struct.  Is that important?
  * What's the behavior in async?
  * Answers were "yes", and "we'll make it work".
* Do/can we do anything special for T : IFormattable or ISpanFormattable?
  * Seems like when we want it (value types) it's already handled by the JIT
* TryFormatBaseString => AppendLiteral
* TryFormatInterpolationHole => AppendFormatted
* "Should we add support for void* or T*?" We're happy with "no".
* Do we need InterpolatedStringBuilder to be IDisposable? We're happy with "no".
* We decided ToString() shouldn't be destructive, so added ToStringAndClear() (which the compiler will call).
* InterpolatedStringBuilder.Create, rename parameters to literalLength and formattedCount (to match the method renames)

```C#
namespace System
{
    public interface ISpanFormattable : IFormattable // currently internal
    {
        bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
    }
}

namespace System.Runtime.CompilerServices
{
    public ref struct InterpolatedStringBuilder
    {
        // Create the builder and extract its string / clean up
        public static InterpolatedStringBuilder Create(int literalLength, int formattedCount);
        public string ToStringAndClear();

        // To handle the base string portions of the interpolated string
        public void AppendLiteral(string value);

        // To handle most types, full set of overloads to minimize IL bloat at call sites
        public void AppendFormatted<T>(T value);
        public void AppendFormatted<T>(T value, string? format);
        public void AppendFormatted<T>(T value, int alignment);
        public void AppendFormatted<T>(T value, int alignment, string? format);

        // To allow for ROS to be in holes
        public void AppendFormatted(ReadOnlySpan<char> value);
        public void AppendFormatted(ReadOnlySpan<char> value, int alignment = 0, string? format = null);

        // To handle strings, because they're very common and we can optimize for them specially
        public void AppendFormatted(string? value);
        public void AppendFormatted(string? value, int alignment = 0, string? format = null);

        // Fallback for everything that can't be target typed
        public void AppendFormatted(object? value, int alignment = 0, string? format = null);
    }
}
```
