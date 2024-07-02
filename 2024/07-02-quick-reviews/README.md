# API Review 07/02/2024

## Let Utf8JsonReader process input with one complete JSON document per line

**Approved** | [#runtime/33030](https://github.com/dotnet/runtime/issues/33030#issuecomment-2204030729) | [Video](https://www.youtube.com/watch?v=vBTqcKN8Eg0&t=0h0m0s)


* AllowTrailingContent became AllowMultipleValues
* The property was removed from JsonSerializarOptions (and the sourcegen equivalent) because it creates slight confusion and isn't strictly necessary
* The JsonDeserializeAsyncEnumerableMode seemed like it needs to be more complicated than just an enum.  In lieu of designing an options type during the meeting, we accepted an overload with a boolean that opted into this new behavior.
  * We settled on naming this boolean `topLevelValues`

```C#
namespace System.Text.Json
{
    public partial struct JsonReaderOptions
    {
        public bool AllowMultipleValues { get; set; }
    }

    public partial static class JsonSerializer
    {
        public static IAsyncEnumerable<T> DeserializeAsyncEnumerable<T>(Stream utf8Json, bool topLevelValues, JsonSerializerOptions options = null, CancellationToken cancellationToken = default);
        public static IAsyncEnumerable<T> DeserializeAsyncEnumerable<T>(Stream utf8Json, JsonTypeInfo<T> jsonTypeInfo, bool topLevelValues, CancellationToken cancellationToken = default);
    }
}
```
## Add 'split' support for ReadOnlySpan<char> similar to string

**Approved** | [#runtime/934](https://github.com/dotnet/runtime/issues/934#issuecomment-2204117523) | [Video](https://www.youtube.com/watch?v=vBTqcKN8Eg0&t=1h28m5s)


* We decided Split is better than EnumerateSplits
* We're leaving SplitAny(ROS-char, ROS-string) out for now, and only adding the generic ones.
* We added a SearchValues overload to SplitAny, though it might or might not get implemented this release
* These may need an IEquatable constraint... if they do, add it.

```C#
public static class MemoryExtensions
{
    public static SpanSplitEnumerator<T> Split<T>(this ReadOnlySpan<T> source, T separator);
    public static SpanSplitEnumerator<T> Split<T>(this ReadOnlySpan<T> source, ReadOnlySpan<T> separator);
    public static SpanSplitEnumerator<T> SplitAny<T>(this ReadOnlySpan<T> source, params ReadOnlySpan<T> separators);
    public static SpanSplitEnumerator<T> SplitAny<T>(this ReadOnlySpan<T> source, SearchValues<T> separators);
    
    public ref struct SpanSplitEnumerator<T>
    {
        public SpanSplitEnumerator<T> GetEnumerator();
        public bool MoveNext();
        public Range Current { get; }
    }
}
```
