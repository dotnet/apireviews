# API Review 06/17/2021

## Support polymorphic serialization through new option

**NeedsWork** | [#runtime/29937](https://github.com/dotnet/runtime/issues/29937#issuecomment-863459930) | [Video](https://www.youtube.com/watch?v=VUefFIL86go&t=0h0m0s)

API Review Feedback:

* We're concerned that the model as proposed is still too open for accidental data exposure, and so we recommend a "closed hierarchy" approach, similar to DataContractSerializer's known-types attribute.
  *  The "late-bound" doodle for this was `public Dictionary<Type, IEnumerable<Type>> PolymorphicSerialization { get; }`
* Given that the same closed hierarchy would apply to deserialization, we feel that the two should be reviewed together, and that a simplified serialization-only can be designed later... but not first.
## Debug.Assert overloads with interpolated string handler

**Approved** | [#runtime/53211](https://github.com/dotnet/runtime/issues/53211#issuecomment-863481970) | [Video](https://www.youtube.com/watch?v=VUefFIL86go&t=1h16m1s)


* We acknowledge that there's a mild risk of breaking programs (e.g. `Trace.Assert(condition, $"Condition failed on iteration {x++}");`), but think it's generally good.
* Approved as-is, plus all the "we could"s

```C#
namespace System.Diagnostics
{
    public static class Debug
    {
        [Conditional("DEBUG")]
        public static void Assert(
            [DoesNotReturnIf(false)] bool condition,
            [InterpolatedStringHandlerArgument("condition")] AssertInterpolatedStringHandler message);
 
        [Conditional("DEBUG")]
        public static void Assert(
            [DoesNotReturnIf(false)] bool condition,
            [InterpolatedStringHandlerArgument("condition")] AssertInterpolatedStringHandler message,
            [InterpolatedStringHandlerArgument("condition")] AssertInterpolatedStringHandler detailedMessage);

        [InterpolatedStringHandlerAttribute]
        [EditorBrowsable(EditorBrowsableState.Never)]
        public struct AssertInterpolatedStringHandler
        {
            public AssertInterpolatedStringHandler(int literalLength, int formattedCount, bool condition, out bool assert);
            public void AppendLiteral(string value);
            public void AppendFormatted<T>(T value);
            public void AppendFormatted<T>(T value, string? format);
            public void AppendFormatted<T>(T value, int alignment);
            public void AppendFormatted<T>(T value, int alignment, string? format);
            public void AppendFormatted(System.ReadOnlySpan<char> value);
            public void AppendFormatted(System.ReadOnlySpan<char> value, int alignment = 0, string? format = null);
            public void AppendFormatted(object? value, int alignment = 0, string? format = null);
            public void AppendFormatted(string? value);
            public void AppendFormatted(string? value, int alignment = 0, string? format = null);
        }

        [Conditional("DEBUG")]
        public static void WriteIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message);

        [Conditional("DEBUG")]
        public static void WriteIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message,
            string category);

        [Conditional("DEBUG")]
        public static void WriteLineIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message);

        [Conditional("DEBUG")]
        public static void WriteLineIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message,
            string category);

        [InterpolatedStringHandlerAttribute]
        [EditorBrowsable(EditorBrowsableState.Never)]
        public struct WriteIfInterpolatedStringHandler
        {
            public WriteIfInterpolatedStringHandler(int literalLength, int formattedCount, bool condition, out bool assert);
            public void AppendLiteral(string value);
            public void AppendFormatted<T>(T value);
            public void AppendFormatted<T>(T value, string? format);
            public void AppendFormatted<T>(T value, int alignment);
            public void AppendFormatted<T>(T value, int alignment, string? format);
            public void AppendFormatted(System.ReadOnlySpan<char> value);
            public void AppendFormatted(System.ReadOnlySpan<char> value, int alignment = 0, string? format = null);
            public void AppendFormatted(object? value, int alignment = 0, string? format = null);
            public void AppendFormatted(string? value);
            public void AppendFormatted(string? value, int alignment = 0, string? format = null);
        }
    }

    public static class Trace
    {
        [Conditional("TRACE")]
        public static void Assert(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] AssertInterpolatedStringHandler message);
 
        [Conditional("TRACE")]
        public static void Assert(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] AssertInterpolatedStringHandler message, [InterpolatedStringHandlerArgument("condition")] AssertInterpolatedStringHandler detailedMessage);

        [InterpolatedStringHandlerAttribute]
        [EditorBrowsable(EditorBrowsableState.Never)]
        public struct AssertInterpolatedStringHandler
        {
            public AssertInterpolatedStringHandler(int literalLength, int formattedCount, bool condition, out bool assert);
            public void AppendLiteral(string value);
            public void AppendFormatted<T>(T value);
            public void AppendFormatted<T>(T value, string? format);
            public void AppendFormatted<T>(T value, int alignment);
            public void AppendFormatted<T>(T value, int alignment, string? format);
            public void AppendFormatted(System.ReadOnlySpan<char> value);
            public void AppendFormatted(System.ReadOnlySpan<char> value, int alignment = 0, string? format = null);
            public void AppendFormatted(object? value, int alignment = 0, string? format = null);
            public void AppendFormatted(string? value);
            public void AppendFormatted(string? value, int alignment = 0, string? format = null);
        }

        [Conditional("TRACE")]
        public static void WriteIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message);

        [Conditional("TRACE")]
        public static void WriteIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message,
            string category);

        [Conditional("TRACE")]
        public static void WriteLineIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message);

        [Conditional("TRACE")]
        public static void WriteLineIf(
            bool condition,
            [InterpolatedStringHandlerArgument("condition")] WriteIfInterpolatedStringHandler message,
            string category);

        [InterpolatedStringHandlerAttribute]
        [EditorBrowsable(EditorBrowsableState.Never)]
        public struct WriteIfInterpolatedStringHandler
        {
            public WriteIfInterpolatedStringHandler(int literalLength, int formattedCount, bool condition, out bool assert);
            public void AppendLiteral(string value);
            public void AppendFormatted<T>(T value);
            public void AppendFormatted<T>(T value, string? format);
            public void AppendFormatted<T>(T value, int alignment);
            public void AppendFormatted<T>(T value, int alignment, string? format);
            public void AppendFormatted(System.ReadOnlySpan<char> value);
            public void AppendFormatted(System.ReadOnlySpan<char> value, int alignment = 0, string? format = null);
            public void AppendFormatted(object? value, int alignment = 0, string? format = null);
            public void AppendFormatted(string? value);
            public void AppendFormatted(string? value, int alignment = 0, string? format = null);
        }
    }
}
```
