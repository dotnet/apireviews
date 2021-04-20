# Quick Reviews 04/20/2021

## C# 10 interpolated strings support, part 2

**Approved** | [#runtime/50635](https://github.com/dotnet/runtime/issues/50635#issuecomment-823508701) | [Video](https://www.youtube.com/watch?v=RQfgeW0cOFk&t=0h0m0s)

* Builders that are nested types should be [EditorBrowsable(Never)]
* Change System.Runtime.CompilerServices.InterpolatedSpanBuilder to System.MemoryExtensions+InterpolatedTryWriteBuilder
* Interpolated string target methods on StringBuilder should be Append, not AppendFormat, since no separate format string is supplied.
* Also do it for StringBuilder.AppendLine
* The scratchBuffer String.Format should an overload of String.Create
  * Therefore the IFormatProvider and builder one should also be String.Create
* We're not fully happy with the name scratchBuffer, but we're not sure what a good answer is.  The adjective temporary received a minor happy response.  temporaryBuffer?


```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Parameter, AllowMultiple = false, Inherited = false)]
    public sealed class InterpolatedBuilderArgumentAttribute : Attribute
    {
        public InterpolatedBuilderArgumentAttribute(string argument);
        public InterpolatedBuilderArgumentAttribute(params string[] arguments);

        public string[] Arguments { get; }
    }

    public ref struct InterpolatedStringBuilder // from https://github.com/dotnet/runtime/issues/50601
    {
        public static InterpolatedStringBuilder Create(int literalLength, int formattedCount, IFormatProvider? provider); // additional factory
        public static InterpolatedStringBuilder Create(int literalLength, int formattedCount, IFormatProvider? provider, Span<char> scratchBuffer); // additional factory
        …
    }
}

namespace System
{
    public static class MemoryExtensions
    {
        public static bool TryWrite(this Span<char> span, [InterpolatedBuilderArgument("span")] ref InterpolatedSpanBuilder builder, out int charsWritten);
        public static bool TryWrite(this Span<char> span, IFormatProvider? provider, [InterpolatedBuilderArgument("span", "provider")] ref InterpolatedSpanBuilder builder, out int charsWritten);
        …

        [EditorBrowsable(Never)]
        public ref struct InterpolatedTryWriteBuilder
        {
            public static InterpolatedTryWriteBuilder Create(int literalLength, int formattedCount, Span<char> destination, out bool success);
            public static InterpolatedTryWriteBuilder Create(int literalLength, int formattedCount, Span<char> destination, IFormatProvider? provider, out bool success);
    
            // Same members as on InterpolatedStringBuilder
            public bool AppendLiteral(string s);
            public bool AppendFormatted<T>(T value);
            public bool AppendFormatted<T>(T value, string? format);
            public bool AppendFormatted<T>(T value, int alignment);
            public bool AppendFormatted<T>(T value, int alignment, string? format);
            public bool AppendFormatted(ReadOnlySpan<char> value);
            public bool AppendFormatted(ReadOnlySpan<char> value, int alignment = 0, string? format = null);
            public bool AppendFormatted(string? value);
            public bool AppendFormatted(string? value, int alignment = 0, string? format = null);
            public bool AppendFormatted(object? value, int alignment = 0, string? format = null);
        }
    }

    public partial sealed class String
    {
        public static string Create(IFormatProvider? provider, Span<char> scratchBuffer, [InterpolatedBuilderArgument("provider", "scatchBuffer")] ref InterpolatedStringBuilder builder);
        public static string Create(IFormatProvider? provider, [InterpolatedBuilderArgument("provider")] ref InterpolatedStringBuilder builder);
    }
}

namespace System.Text
{
    public sealed class StringBuilder
    {
        public StringBuilder Append([InterpolatedBuilderArgument("this")] ref InterpolatedAppendFormatBuilder builder);
        public StringBuilder Append(IFormatProvider? provider, [InterpolatedBuilderArgument("this", "provider")] ref InterpolatedAppendFormatBuilder builder);
        public StringBuilder AppendLine([InterpolatedBuilderArgument("this")] ref InterpolatedAppendFormatBuilder builder);
        public StringBuilder AppendLine(IFormatProvider? provider, [InterpolatedBuilderArgument("this", "provider")] ref InterpolatedAppendFormatBuilder builder);

        [EditorBrowsable(Never)]
        public struct InterpolatedAppendFormatBuilder
        {
            public static InterpolatedAppendFormatBuilder Create(int literalLength, int formattedCount, StringBuilder stringBuilder);
            public static InterpolatedAppendFormatBuilder Create(int literalLength, int formattedCount, StringBuilder stringBuilder, IFormatProvider? provider);

            // Same members as on InterpolatedStringBuilder
            public bool AppendLiteral(string s);
            public bool AppendFormatted<T>(T value);
            public bool AppendFormatted<T>(T value, string? format);
            public bool AppendFormatted<T>(T value, int alignment);
            public bool AppendFormatted<T>(T value, int alignment, string? format);
            public bool AppendFormatted(ReadOnlySpan<char> value);
            public bool AppendFormatted(ReadOnlySpan<char> value, int alignment = 0, string? format = null);
            public bool AppendFormatted(string? value);
            public bool AppendFormatted(string? value, int alignment = 0, string? format = null);
            public bool AppendFormatted(object? value, int alignment = 0, string? format = null);
        }

        …
    }
}
```
## SslStream delayed client certificate negotiation

**Approved** | [#runtime/49346](https://github.com/dotnet/runtime/issues/49346#issuecomment-823526100) | [Video](https://www.youtube.com/watch?v=RQfgeW0cOFk&t=1h25m38s)

* We changed "Get" to "Negotiate" to make it clear that complicated protocol stuff is happening.
* We removed the value from the return, the caller needs to just check the RemoteCertificate property again.
* Because we don't really want it to be called, we don't see the need for the synchronous version.
* It's virtual because all of the authenticate methods are virtual.
* Sounds like it'll need some SupportedOS attributes.

```C#
namespace System.Net.Security
{
    public partial class SslStream
    {
        public virtual Task NegotiateClientCertificateAsync(CancellationToken cancellationToken = default) => throw null;
    }
}
```
