# API Review 09/01/2026

## Composite ML-KEM CNG identifiers

**Approved** | [#runtime/132680](https://github.com/dotnet/runtime/issues/132680#issuecomment-5497596953) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=0h0m0s)

Looks good as proposed

```csharp
namespace System.Security.Cryptography;

public sealed partial class CngAlgorithm
{
    [Experimental("SYSLIB5006")]
    public static CngAlgorithm CompositeMLKem { get; }
}

public sealed partial class CngAlgorithmGroup
{
    [Experimental("SYSLIB5006")]
    public static CngAlgorithmGroup CompositeMLKem { get; }
}
```
## Array.Create<T> with default value and factory function

**Approved** | [#runtime/121477](https://github.com/dotnet/runtime/issues/121477#issuecomment-5498129393) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=0h4m8s)

* We decided to go with CreateFilled over just Create
* Let's not preemptively add a TState variant
* Let's not preemptively add Func/factory overloads to everything else named Fill.

```csharp
namespace System;

public static partial class Array
{
    public static T[] CreateFilled<T>(int length, T value);
    public static T[] CreateFilled<T>(int length, Func<int, T> factory);
}
```
## Array GetValue SetValue ROS indices

**Approved** | [#runtime/125325](https://github.com/dotnet/runtime/issues/125325#issuecomment-5498207782) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=0h50m27s)

Looks good as proposed.

```csharp
namespace System;

public class Array
{
    public object? GetValue(params ReadOnlySpan<int> indices);
    public void SetValue(object? value, params ReadOnlySpan<int> indices);
}
```
## Implement ISpanParsable on System.Version

**Approved** | [#runtime/125026](https://github.com/dotnet/runtime/issues/125026#issuecomment-5498228018) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=0h57m10s)

Looks good as proposed

```diff
namespace System;

public class Version :
    ICloneable,
    IComparable,
    IComparable<Version?>,
    IEquatable<Version?>,
    ISpanFormattable,
    IUtf8SpanFormattable,
    IUtf8SpanParsable<Version>,
+   ISpanParsable<Version>
{
    public static Version Parse(string input);
+   static Version IParsable.Parse(string s, IFormatProvider? provider);

    public static Version Parse(ReadOnlySpan<char> input);
+   static Version ISpanParsable.Parse(ReadOnlySpan<char> s, IFormatProvider? provider);

    public static Version Parse(ReadOnlySpan<byte> utf8Text);
    static Version IUtf8SpanParsable<Version>.Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);

    public static bool TryParse([NotNullWhen(true)] string? input, [NotNullWhen(true)] out Version? result);
+   static bool IParsable.TryParse([NotNullWhen(true)] string? s, IFormatProvider? provider, [NotNullWhen(true)] out Version? result);

    public static bool TryParse(ReadOnlySpan<char> input, [NotNullWhen(true)] out Version? result);
+   static bool ISpanParsable.TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, [NotNullWhen(true)] out Version? result);

    public static bool TryParse(ReadOnlySpan<byte> utf8Text, [NotNullWhen(true)] out Version? result);
    static bool IUtf8SpanParsable<Version>.TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider, [NotNullWhen(true)] out Version? result);
}
```
## Do not use ReferenceEquals with impossible types

**Approved** | [#runtime/37691](https://github.com/dotnet/runtime/issues/37691#issuecomment-5498322788) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=0h58m56s)

Report when doing ReferenceEquals of two reference types that do not inherit from one another.

* Warn: ReferenceEquals(string, array);
* DontWarn: ReferenceEquals(asymmetricAlgorithm, rsa);

Category: Reliablity
Level: Warning
## [Analyzer]: Extend string IndexOf => Contains analyzers to Span IndexOf/IndexOfAny/IndexOfAnyExcept

**Approved** | [#runtime/87691](https://github.com/dotnet/runtime/issues/87691#issuecomment-5498389129) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=1h6m57s)

Expanding the existing diagnostic seems sound.  Also consider patterns like `span.IndexOf('a') is >= 0` using the keyword `is` unnecessarily.

Category: Usage (same as current)
Severity: Suggestion
## Analyzer: Validate literal arguments to StringSyntaxAttribute parameters/members

**Approved** | [#runtime/64009](https://github.com/dotnet/runtime/issues/64009#issuecomment-5498490274) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=1h12m46s)

Looks good as proposed.

Each different supported syntax should use a different diagnostic code.

Category: Reliability
Severity: Warning
## Warnings for `in`/`readonly ref`

**Approved** | [#runtime/77625](https://github.com/dotnet/runtime/issues/77625#issuecomment-5498590067) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=1h21m16s)

Report on `in T` where T is a reference type, or byte, sbyte, short, ushort, int, uint, long, ulong, float, double, char, bool, nint, nuint.

Reporting on `ref readonly T` is also reasonable, but as a different diagnostic.

Category: Performance
Severity: Info

Fixer 1: Change `in` to `ref readonly` in the method decl.
Fixer 2: Remove `in` (or `ref readonly`) and update callers (as appropriate).
## Boolean format strings

**Approved** | [#runtime/67388](https://github.com/dotnet/runtime/issues/67388#issuecomment-5498894929) | [Video](https://www.youtube.com/watch?v=HnRoyltLc34&t=1h29m4s)


* Since Boolean doesn't respect IFormatProvider, let's leave it out of the public API (those members will be explicitly implemented as they are required by the interfaces)
* Standard formats: G/g and L/l. 
  * Let's leave U/u for a different proposal, since we would want to debate what all types need that new specifier.
* Custom format: Semicolon-separated, use the rules from Int32 for handling escaping and multiple values.

```csharp
namespace System;

public partial readonly struct Boolean : IFormattable, ISpanFormattable, IUtf8SpanFormattable, IParsable<Boolean>, ISpanParsable<Boolean>, IUtf8SpanParsable<Boolean>
{
    string IFormattable.ToString(string? format, IFormatProvider? provider);
    public string ToString(string? format);

    bool IFormattable.TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider provider);
    public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format);

    static Boolean IParsable.Parse(string s, IFormatProvider? provider);
    static bool IParsable.TryParse(string s, IFormatProvider? provider, out Boolean result);
    
    public static Boolean Parse(string s);
    public static bool TryParse(string s, out Boolean result);

    static Boolean IParsable.Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
    static bool IParsable.TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, out Boolean result);
    
    public static Boolean Parse(ReadOnlySpan<char> s);
    public static bool TryParse(ReadOnlySpan<char> s, out Boolean result);

    bool IUtf8SpanFormattable.TryFormat(Span<byte> utf8Destination, out int bytesWritten, ReadOnlySpan<char> format, IFormatProvider provider);
    public bool TryFormat(Span<byte> utf8Destination,, out int bytesWritten, ReadOnlySpan<char> format);

    static Boolean IUtf8SpanParsable.Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);
    static bool IUtf8SpanParsable.TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider, out Boolean result);
    
    public static Boolean Parse(ReadOnlySpan<byte> utf8Text);
    public static bool TryParse(ReadOnlySpan<byte> utf8Text, out Boolean result);
}
```
