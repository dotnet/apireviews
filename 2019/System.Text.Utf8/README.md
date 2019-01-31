# Utf8 and Rune APIs

Status: **Approved with feedback** |
[Issue](https://github.com/dotnet/corefx/issues/34761)

## Notes

* Breaking down the APIs into multiple was done because reviewers didn't give
  actionable feedback before. We should fix the review process rather than doing
  micro-reviews because it's harder to see the larger picture.
* We should change the API as follows:

```C#
public static OperationStatus ToBytes(ReadOnlySpan<char> source, Span<byte> destination, out int numCharsRead, out int numBytesWritten, bool replaceInvalidSequences = true, bool isFinalBlock = true);
public static OperationStatus ToChars(ReadOnlySpan<byte> source, Span<char> destination, out int numBytesRead, out int numCharsWritten, bool replaceInvalidSequences = true, bool isFinalBlock = true);
```

