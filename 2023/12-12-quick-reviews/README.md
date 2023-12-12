# API Review 12/12/2023

## Allow specifying indent size and whitespace character when writing JSON with Utf8JsonWriter

**Approved** | [#runtime/63882](https://github.com/dotnet/runtime/issues/63882#issuecomment-1852620212)

- Looks good as proposed
- The default is going to be a single space and indent size of 2
- We should probably limit the inputs to something reasonable (e.g. character can be tab, space but no line breaks and intent size should be something like < 128).

```C#
namespace System.Text.Json
{
    public sealed class JsonSerializerOptions
    {
        public char IndentCharacter { get; set; }
        public int IndentSize { get; set; }
    }

    public struct JsonWriterOptions
    {
        public char IndentCharacter { get; set; }
        public int IndentSize { get; set; }
    }
}

namespace System.Text.Json.Serialization
{
    public partial class JsonSourceGenerationOptionsAttribute
    {
        public char IndentCharacter { get; set; }
        public int IndentSize { get; set; }
    }
}
```
## Linq method for getting value and index

**Approved** | [#runtime/95563](https://github.com/dotnet/runtime/issues/95563#issuecomment-1852656539)

* We decided against `WithIndex` because it sounds like a wither which this one isn't. F# calls it `Indexed`; following nomenclature in the rest of Linq we chose a verb version (akin to the recently added `Order`)
* We decided to rename `Value` to `Item` because `Value` implies a singleton vs `Item` which is used as a collection indexer
* We decided to return `Index` first, `Value` second because it feels more natural despite the fact that the existing `Select()` method provides value first and index second.
    ```C#
    IEnumerable<string> lines = GetLines();

    foreach (var (i, line) in lines.Index())
    {
        // ...
    }
    ```
* We should match the existing `Select`'s method behavior when enumerating a source that returns more than `int.MaxValue` items

```C#
namespace System.Linq;

public static partial class Enumerable
{
    public static IEnumerable<(int Index, T Item)> Index<T>(this IEnumerable<T> source);
}

public static partial class Queryable
{
    public static IQueryable<(int Index, T Item)> Index<T>(this IQueryable<T> source);
}
```

## Support Multiselect property in FolderBrowserDialog class

**Approved** | [#winforms/8298](https://github.com/dotnet/winforms/issues/8298#issuecomment-1852676818)

* Looks good as proposed
* We should match the behavior of `FileDialog` when `Multiselect` is `false`
* We should probably also match behavior of the property getter of `FileDialog` and return a new array each time

```C#
namespace System.Windows.Forms;

// Existing:
//
// public abstract class FileDialog : CommonDialog
// {
//     public string[] FileNames { get; }
// }
//
// public sealed class OpenFileDialog : FileDialog
// {
//     public bool Multiselect { get; set; }
// }

public sealed class FolderBrowserDialog : CommonDialog
{
    public bool Multiselect { get; set; }
    public string[] SelectedPaths { get; }
}
```


## Add ability to stream large binary content to Utf8JsonWriter

**Approved** | [#runtime/67337](https://github.com/dotnet/runtime/issues/67337#issuecomment-1852685210)

* Looks good as proposed
* The methods have to track the state necessary to know
    - when to put an open quote
    - how to parse code points across span boundaries
* This allows writing large strings, but this doesn't support reading them yet. Do we need to add this as well?
    - The `Utf8JsonReader` will fetch more buffers until the next token is contained in a single contiguous buffer, which wouldn't be efficient for large strings
    - We can solve this by letting the reader early with a new token type (such as `StringSegment`) but this would break consumers that aren't handling this token type, but we could make this opt-in on the reader
    - Ideally that opt in isn't state on the reader but is a function of what API is called (e.g. `TryGetStringSegment` / `GetStringSegment()`) because it allows multiple parties to opt-in individually (e.g. some converter doesn't but continues to work, a new token type would make everyone throw).

```C#
namespace System.Text.Json;

public partial class Utf8JsonWriter
{
    // Existing
    // public void WriteStringValue(ReadOnlySpan<char> value);
    // public void WriteStringValue(ReadOnlySpan<byte> value);
    // public void WriteBase64String(ReadOnlySpan<byte> value);

    public void WriteStringValueSegment(ReadOnlySpan<char> value, bool isFinalSegment);
    public void WriteStringValueSegment(ReadOnlySpan<byte> value, bool isFinalSegment); 
    public void WriteBase64StringSegment(ReadOnlySpan<byte> value, bool isFinalSegment);
}
```
