# API Review 08/08/2023

## Meter Configuration and Pipeline

**Approved** | [#runtime/85684](https://github.com/dotnet/runtime/issues/85684) | [Video](https://www.youtube.com/watch?v=58aXHiinJOs&t=0h0m0s)

## Proposal: Add IdnMapping Span-based APIs

**Approved** | [#runtime/32411](https://github.com/dotnet/runtime/issues/32411#issuecomment-1670060439) | [Video](https://www.youtube.com/watch?v=58aXHiinJOs&t=0h45m53s)

* Looks good as proposed

```C#
namespace System.Globalization;

public sealed class IdnMapping
{
    // Existing API
    // public string GetAscii(string unicode);
    // public string GetAscii(string unicode, int index);
    // public string GetAscii(string unicode, int index, int count);
    //
    // public string GetUnicode(string ascii);
    // public string GetUnicode(string ascii, int index);
    // public string GetUnicode(string ascii, int index, int count);

    // Proposed API
    public string GetAscii(ReadOnlySpan<char> unicode);
    public bool TryGetAscii(ReadOnlySpan<char> unicode, Span<char> destination, out int charsWritten);

    public string GetUnicode(ReadOnlySpan<char> ascii);
    public bool TryGetUnicode(ReadOnlySpan<char> ascii, Span<char> destination, out int charsWritten);
}
```
