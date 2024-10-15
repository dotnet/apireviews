# API Review 10/15/2024

## SortedSet<T>.GetAlternateLookup()

**NeedsWork** | [#runtime/108642](https://github.com/dotnet/runtime/issues/108642#issuecomment-2414613977) | [Video](https://www.youtube.com/watch?v=ZDIVbbjl-3M&t=0h0m0s)


* SortedSet is based on a tree implementation using Compare, not Equals, so the existing IAlternateEqualityComparer is not sufficient.
  * This can realistically only go forward with an IAlternateComparer.
    * It's probably on the order of the same amount of work as when we introduced IAlternateEqualityComparer.
    * And should probably look for other places we could use that.

```C#
namespace System.Collections.Generic;

public partial class SortedSet<T>
{
    public readonly struct AlternateLookup<TAlternate>
    {
        public SortedSet<T> Set { get; }
        public bool Add(TAlternate item);
        public bool Contains(TAlternate item);
        public bool Remove(TAlternate item);
        public bool TryGetValue(TAlternate equalValue, [MaybeNullWhen(false)] out T actualValue);
    }

    public AlternateLookup<TAlternate> GetAlternateLookup<TAlternate>();
    public bool TryGetAlternateLookup<TAlternate>(out AlternateLookup<TAlternate> lookup);
}
```

## `JsonNode.WriteTo()` with `Stream`

**Approved** | [#runtime/108643](https://github.com/dotnet/runtime/issues/108643#issuecomment-2414639943) | [Video](https://www.youtube.com/watch?v=ZDIVbbjl-3M&t=0h25m54s)


* While it's hard to discover, this proposal is entirely redundant with JsonSerializer.Serialize{Async}(stream, node, options?)
* We should instead mark the existing WriteTo method as Obsolete, directing potential callers to JsonSerializer.

```C#
namespace System.Text.Json.Nodes;

public abstract class JsonNode
{
    [Obsolete("Something about use JsonSerializer.Serialize{Async} instead.", DiagnosticId = NextUp)]
    public void WriteTo(Utf8JsonWriter stream, JsonSerializerOptions? options = null);
}
```

## Add Span<byte> support to System.Security.Cryptography.ProtectedData

**Approved** | [#runtime/108734](https://github.com/dotnet/runtime/issues/108734#issuecomment-2414658787) | [Video](https://www.youtube.com/watch?v=ZDIVbbjl-3M&t=0h36m35s)


* We flipped `destination` and `scope` to match guidelines
* We discussed whether we need a GetEstimatedSize, and decided no because it's not feasible to implement.

```C#
namespace System.Security.Cryptography;

public partial class ProtectedData {
    public static bool TryProtect(
        ReadOnlySpan<byte> userData,
        Span<byte> destination,
        DataProtectionScope scope,
        out int bytesWritten,
        ReadOnlySpan<byte> optionalEntropy = default);

    public static bool TryUnprotect(
        ReadOnlySpan<byte> encryptedData,
        Span<byte> destination,
        DataProtectionScope scope,
        out int bytesWritten,
        ReadOnlySpan<byte> optionalEntropy = default);

    public static int Protect(
        ReadOnlySpan<byte> userData,
        Span<byte> destination,
        DataProtectionScope scope,
        ReadOnlySpan<byte> optionalEntropy = default);

    public static int Unprotect(
        ReadOnlySpan<byte> encryptedData,
        Span<byte> destination,
        DataProtectionScope scope,
        ReadOnlySpan<byte> optionalEntropy = default);

    public static byte[] Protect(
        ReadOnlySpan<byte> userData,
        DataProtectionScope scope,
        ReadOnlySpan<byte> optionalEntropy = default);

    public static byte[] Unprotect(
        ReadOnlySpan<byte> encryptedData,
        DataProtectionScope scope,
        ReadOnlySpan<byte> optionalEntropy = default);
}
```

## Add unimplemented types for deprecated UI controls

**Approved** | [#winforms/3783](https://github.com/dotnet/winforms/issues/3783#issuecomment-2414723955) | [Video](https://www.youtube.com/watch?v=ZDIVbbjl-3M&t=0h47m6s)

Bringing the types and members back to look like they did in .NET Framework is blanket approved.

The "new" types/members being declared as Obsolete with EditorBrowsable(Never) and Browsable(false) seems like goodness.  While the obsoletion message makes sense to be different for each type/member (as appropriate), they should all use the same diagnostic ID as they were made obsolete in the same wave, and there's no reason to opt in to only parts of them.

```C#
[Obsolete(
    "ContextMenu and its related types has been deprecated. Use ContextMenuStrip instead.",
    error: false,
    DiagnosticId = WFDEV###,
    UrlFormat = "https://aka.ms/winforms-warnings/{0}")]
[EditorBrowsable(EditorBrowsableState.Never)]
[Browsable(false)]
public partial class ContextMenu : Menu
{
    public ContextMenu() : base(items: null) => throw new PlatformNotSupportedException();
}
```
## Expose a new `JsonSerializerDefaults` that is better tailored to Azure applications.

**Approved** | [#runtime/108526](https://github.com/dotnet/runtime/issues/108526#issuecomment-2414771204) | [Video](https://www.youtube.com/watch?v=ZDIVbbjl-3M&t=1h21m38s)


* With no other noun to key off of, "Strict" should be read-compatible with what Default(/General) writes.
* We don't think we want to prematurely version the name.  We'll deal with "V2" when/if it arises.

```C#
namespace System.Text.Json;

public enum JsonSerializerDefaults
{
    General = 0, // The default configuration set
    Web = 1,     // The default configuration set used by aspnetcore
+   Strict = 2 // The proposed configuration set
}

public partial class JsonSerializerOptions
{
    public static JsonSerializerOptions Default { get; }
    public static JsonSerializerOptions Web { get; }
+   public static JsonSerializerOptions Strict { get; }
}
```

