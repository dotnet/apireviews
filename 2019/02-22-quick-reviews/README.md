# Quick Reviews 2/22/2019

## Complex Rune enumeration over spans of UTF-16 and UTF-8 text

**Approved** | [#corefx/34826](https://github.com/dotnet/corefx/issues/34826#issuecomment-466507394) | [Video](https://www.youtube.com/watch?v=0ReXmy2bQvQ&t=0h0m0s)

* `RunPosition`
    - We considered swapping `rune` and `startIndex` in the deconstruct method
      to match Go, but decided against it because we also expose `length` and
      the order `startIndex`, `rune`, `length` would be very weird.
    - We should have a constructor that sets all four fields
* `Rune`
    - We should move the enumerate methods to `RunePosition`, which allows
      making the names shorter

        ```C#
        public readonly struct RunPosition
        {
            public static Utf16Enumerator EnumerateUtf16(ReadOnlySpan<char> span);
            public static Utf8Enumerator EnumerateUtf8(ReadOnlySpan<byte> span);

            public ref struct Utf8Enumerator;
            public ref struct Utf16Enumerator;
        }
        ```

