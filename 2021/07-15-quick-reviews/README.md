# API Review 07/15/2021

## Adding a rough draft of the "minimum viable product" for the .NET Libraries APIs to support generic math

**NeedsWork** | [#designs/205](https://github.com/dotnet/designs/pull/205#issuecomment-880930751) | [Video](https://www.youtube.com/watch?v=B5gKYCMoh3c&t=0h0m0s)

Notes from today's review:

* We'd like to keep the split of `IParseable<TSelf>` and `ISpanParseable<TSelf>` because spans are more advanced and we don't want to force span processing on everyone
    - However, if there is a limitation on number of interfaces, then this is a good candidate to collapse
    - There is also an argument one could make that span is a core concept and we generally want type providers to support it moving forward and collapsing would make that mandate more explicit. However, this might have implications on languages that don't support spans.
* We should extract the `Create` methods to an `IConvertible<TSelf>`, akin to `IParseable<TSelf>`
    - This would make conversion of types without boxing available outside of generic math and not be limited to numbers
    - We should probably use the `Convert` verb, rather than `Create`
* For converting, we have three behaviors when the value doesn't fit:
    1. Truncating/wrapping
    2. Saturating
    3. Throwing
* It seems we can't agree on the default, so it seems we should have three differently named methods:
    - `CreateTruncated`
    - `CreateClamped`
    - `CreateChecked`
* We talked about which members we'd implement explicitly vs implicitly
    - We're generally OK with implementing most implicitly, which would increase the API surface of the primitive types, but that seems fine
    - However, we should implement members explicitly that are nonsensical or aren't needed outside of a generic context, such as most of the `Create` methods

## Expose nint/nuint overloads for BitOperations

**Approved** | [#runtime/54390](https://github.com/dotnet/runtime/issues/54390#issuecomment-880933389) | [Video](https://www.youtube.com/watch?v=B5gKYCMoh3c&t=1h39m36s)

Looks good as proposed:

```C#
namespace System.Numerics
{
    public static partial class BitOperations
    {
        public static bool IsPow2(nint value);
        public static bool IsPow2(nuint value);

        public static nuint RoundUpToPowerOf2(nuint value);

        public static int LeadingZeroCount(nuint value);

        public static int Log2(nuint value);

        public static int PopCount(nuint value);

        public static int TrailingZeroCount(nint value);
        public static int TrailingZeroCount(nuint value);

        public static nuint RotateLeft(nuint value, int offset);
        public static nuint RotateRight(nuint value, int offset);
    }
}
```
