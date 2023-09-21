# API Review 09/21/2023

## InitialCapacity for CborWriter

**Approved** | [#runtime/91978](https://github.com/dotnet/runtime/issues/91978#issuecomment-1730093190) | [Video](https://www.youtube.com/watch?v=gtnntYhxRE4&t=0h0m0s)

* We changed from an initial required parameter to a trailing defaulted parameter (and consequently will update the existing constructor to no longer have defaults, and now be never-browsable

```C#
namespace System.Formats.Cbor;

public partial class CborWriter {
    [EditorBrowsable(Never)]
    public CborWriter(
       CborConformanceMode conformanceMode,
       bool convertIndefiniteLengthEncodings,
       bool allowMultipleRootLevelValues);

    public CborWriter(
       CborConformanceMode conformanceMode = CborConformanceMode.Strict,
       bool convertIndefiniteLengthEncodings = false,
       bool allowMultipleRootLevelValues = false,
       int initialCapacity = -1);
}
```
## VectorNNN.Create with broadcasting

**Approved** | [#runtime/92299](https://github.com/dotnet/runtime/issues/92299#issuecomment-1730102482) | [Video](https://www.youtube.com/watch?v=gtnntYhxRE4&t=0h21m49s)

* We went ahead and squared the circle, adding the Vector64 overloads.

```C#
namespace System.Runtime.Intrinsics
{
    public static partial class Vector128
    {
        public static Vector128<T> Create<T>(Vector64<T> value) where T : struct;
    }

    public static partial class Vector256
    {
        public static Vector256<T> Create<T>(Vector64<T> value) where T : struct;
        public static Vector256<T> Create<T>(Vector128<T> value) where T : struct;
    }

    public static partial class Vector512
    {
        public static Vector512<T> Create<T>(Vector64<T> value) where T : struct;
        public static Vector512<T> Create<T>(Vector128<T> value) where T : struct;
        public static Vector512<T> Create<T>(Vector256<T> value) where T : struct;
    }
}
```
## Add `IReadOnlyList<TElement>` to System.Linq.Grouping<TKey, TElement>

**NeedsWork** | [#runtime/92165](https://github.com/dotnet/runtime/issues/92165#issuecomment-1730125727) | [Video](https://www.youtube.com/watch?v=gtnntYhxRE4&t=0h28m24s)

This issue prompted two competing options: explore trying to use a DIM to make `IList<T> : IReadOnlyList<T>`, or writing a tool (analyzer?) to identify all IList-implementing types that are missing IReadOnlyList and fixing them in bulk.

So it's in principle approved, but not quite yet pending followups.

@terrajobst 
## Add lowercase support for Convert.ToHexString

**Approved** | [#runtime/60393](https://github.com/dotnet/runtime/issues/60393#issuecomment-1730147073) | [Video](https://www.youtube.com/watch?v=gtnntYhxRE4&t=0h45m9s)

We discussed it, and rather than adding an enum (or bool), we feel that the parallel method group with the "Lower" suffix is better, even if slightly unbalanced.

```C#
namespace System
{
    public static class Convert
    {
        public static string ToHexStringLower(byte[] inArray);
        public static string ToHexStringLower(byte[] inArray, int offset, int length);
        public static string ToHexStringLower(ReadOnlySpan<byte> bytes);
        public static bool TryToHexStringLower(ReadOnlySpan<byte> source, Span<char> destination, out int charsWritten);
    }
}
```
