# API Review 06/11/2024

## Add synchronous gauge instrument

**Approved** | [#runtime/92625](https://github.com/dotnet/runtime/issues/92625#issuecomment-2161254661) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Diagnostics.Metrics;

public partial class Meter : IDisposable
{
	public static Gauge<T> CreateGauge<T>(string name) where T : struct;
	public static Gauge<T> CreateGauge<T>(string name, string? unit = null, string? description = null, IEnumerable<KeyValuePair<string, object?>>? tags = null) where T : struct;
}

public sealed class Gauge<T> : Instrument<T> where T : struct
{
	public void Record(T value);
	public void Record(T value, KeyValuePair<string, object?> tag);
	public void Record(T value, KeyValuePair<string, object?> tag1, KeyValuePair<string, object?> tag2);
	public void Record(T value, KeyValuePair<string, object?> tag1, KeyValuePair<string, object?> tag2, KeyValuePair<string, object?> tag3);
	public void Record(T value, params ReadOnlySpan<KeyValuePair<string, object?>> tags);
	public void Record(T value, params KeyValuePair<string, object?>[] tags);
	public void Record(T value, in TagList tagList);
}
```
## Add "hints" in Metric API to provide things like histogram bounds

**Approved** | [#runtime/63650](https://github.com/dotnet/runtime/issues/63650#issuecomment-2161272835) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=0h10m53s)

* Looks good as proposed.

```C#
namespace System.Diagnostics.Metrics;

// NEW
public sealed class InstrumentAdvice<T> where T : struct
{
    public IReadOnlyList<T>? HistogramBucketBoundaries { get; init; }
}

public partial class Meter
{
    // NEW: Simple one
    public Histogram<T> CreateHistogram<T> (
        string name) where T : struct;

    // Existing, mark parameters as required and EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    public Histogram<T> CreateHistogram<T> (
        string name,
        string? unit,
        string? description) where T : struct;

    // Also existing, mark EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    public Histogram<T> CreateHistogram<T>(
        string name,
        string? unit,
        string? description,
        IEnumerable<KeyValuePair<string, object?>>? tags) where T : struct;

    // NEW
    public Histogram<T> CreateHistogram<T> (
        string name,
        string? unit = default,
        string? description = default,
        IEnumerable<KeyValuePair<string, object?>>? tags = default,
        InstrumentAdvice<T>? advice = default) where T : struct;
}

public abstract class Instrument<T> : Instrument where T : struct
{
    // NEW: Simple one
    protected Instrument(Meter meter, string name);

    // Existing, mark EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    protected Instrument(Meter meter, string name, string? unit, string? description);

    // Also existing, mark EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    protected Instrument(Meter meter, string name, string? unit, string? description, IEnumerable<KeyValuePair<string, object?>>? tags);

    // NEW
    protected Instrument(
        Meter meter,
        string name,
        string? unit = default,
        string? description = default,
        IEnumerable<KeyValuePair<string, object?>>? tags = default,
        InstrumentAdvice<T>? advice = default);

    // NEW
    public InstrumentAdvice<T>? Advice { get; }
}

public abstract class Instrument
{
    // NEW: Simple one
    protected Instrument(Meter meter, string name);

    // Existing, mark EB never
    [EditorBrowsable(EditorBrowsableState.Never)]
    protected Instrument(Meter meter, string name, string? unit, string? description);

    // Existing, decorate optional parameters with defaults
    protected Instrument(Meter meter, string name, string? unit = default, string? description = default, IEnumerable<KeyValuePair<string, object?>>? tags = default);
}
```
## `JsonObject` should support property order manipulation

**Approved** | [#runtime/102932](https://github.com/dotnet/runtime/issues/102932#issuecomment-2161302838) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=0h21m31s)

* We should make the `int`-based indexer on `JsonNode` work to return the `JsonNode`
    - We don't need to `out` the property from `IndexOf` then
* We should also implement `IList<KeyValuePair<string, JsonNode?>>`

```C#
namespace System.Text.Json.Nodes;

public sealed partial class JsonObject : IDictionary<string, JsonNode?>, IList<KeyValuePair<string, JsonNode?>>
{
    public int IndexOf(string key);
    public void Insert(int index, string key, JsonNode? value);
    public void RemoveAt(int index);
}
```
## Expose the `NullabilityInfoContext.IsSupported` property

**Rejected** | [#runtime/103004](https://github.com/dotnet/runtime/issues/103004#issuecomment-2161335185) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=0h40m24s)

* We think the original sin was to support attribute-level trimming for nullability info as the trimmer doesn't do that for any other attributes.
* We should remove the `AppContext` switch and not expose this property.

```C#
namespace System.Reflection;

public partial class NullabilityInfoContext
{
    public static bool IsSupported { get; } = AppContext.TryGetSwitch("System.Reflection.NullabilityInfoContext.IsSupported", out bool isSupported) ? isSupported : true;
}
```
## Create ReadOnlySpanMarshaller<T, TUnmanaged>.ManagedToUnmanagedOut marshaller

**Approved** | [#runtime/101136](https://github.com/dotnet/runtime/issues/101136#issuecomment-2161346982) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h0m14s)

* Looks good as proposed

```C#
namespace System.Runtime.InteropServices.Marshalling;

[CustomMarshaller(typeof(System.ReadOnlySpan<>), MarshalMode.ManagedToUnmanagedOut, typeof(ReadOnlySpanMarshaller<,>.ManagedToUnmanagedOut))]
public partial class ReadOnlySpanMarshaller<T, TUnmanagedElement> where TUnmanagedElement : unmanaged
{
    public struct ManagedToUnmanagedOut
    {
        public void FromUnmanaged(TUnmanagedElement* unmanaged);
        public ReadOnlySpan<T> ToManaged();
        public ReadOnlySpan<TUnmanagedElement> GetUnmanagedValuesSource(int numElements);
        public Span<T> GetManagedValuesDestination(int numElements);
        public void Free();
    }
}
```
## Tokenizers Library Design

**Approved** | [#machinelearning/7144](https://github.com/dotnet/machinelearning/issues/7144#issuecomment-2161371287) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h7m56s)

* `out int textLength` should be `out int charsConsumed`
* We also need to expose `EncodeResults<T>.CharsConsumed`
* For `Llama` we want a dedicated type (and other types like it)
* We don't want to use `FrozenDictionary` due to their creation cost

```C#
namespace Microsoft.ML.Tokenizers;

public struct EncodeResults<T>
{
    // Existing:
    // public IReadOnlyList<T> Tokens { get; set; }
    // public string? NormalizedText { get; set; }

    public int CharsConsumed { get; set; }
}

public partial class LlamaTokenizer : SentencePieceTokenizer 
{
    public static LlamaTokenizer Create();
}
```
## ReadOnlySet<T>

**Approved** | [#runtime/100113](https://github.com/dotnet/runtime/issues/100113#issuecomment-2161379927) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h23m20s)

* Looks good as proposed

```C#
namespace System.Collections.ObjectModel;

[DebuggerDisplay("Count = {Count}")]
public class ReadOnlySet<T> : IReadOnlySet<T>, ISet<T>, ICollection
{
    public ReadOnlySet(ISet<T> set);
    public static ReadOnlySet<T> Empty { get; }
    protected ISet<T> Set { get; }
    public int Count { get; }
    public IEnumerator<T> GetEnumerator();
    public bool Contains(T item);
    public bool IsProperSubsetOf(IEnumerable<T> other);
    public bool IsProperSupersetOf(IEnumerable<T> other);
    public bool IsSubsetOf(IEnumerable<T> other);
    public bool IsSupersetOf(IEnumerable<T> other);
    public bool Overlaps(IEnumerable<T> other);
    public bool SetEquals(IEnumerable<T> other);
}
```
## Add File.WriteAllBytes taking ReadOnlySpan<byte>

**Approved** | [#runtime/99823](https://github.com/dotnet/runtime/issues/99823#issuecomment-2161384390) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h28m33s)

* Looks good as proposed

```C#
public static class File
{
    public static void WriteAllBytes(string path, ReadOnlySpan<byte> bytes);
    public static Task WriteAllBytesAsync(string path, ReadOnlyMemory<byte> bytes, CancellationToken cancellationToken = default);

    public static void WriteAllText(string path, ReadOnlySpan<char> contents);
    public static void WriteAllText(string path, ReadOnlySpan<char> contents, Encoding encoding);
    public static Task WriteAllTextAsync(string path, ReadOnlyMemory<byte> contents, CancellationToken cancellationToken = default);
    public static Task WriteAllTextAsync(string path, ReadOnlyMemory<byte> contents, Encoding encoding, CancellationToken cancellationToken = default);

    public static void AppendAllBytes(string path, ReadOnlySpan<byte> bytes);
    public static Task AppendAllBytesAsync(string path, ReadOnlyMemory<byte> bytes, CancellationToken cancellationToken = default);

    public static void AppendAllText(string path, ReadOnlySpan<char> contents);
    public static void AppendAllText(string path, ReadOnlySpan<char> contents, Encoding encoding);
    public static Task AppendAllTextAsync(string path, ReadOnlyMemory<char> contents, CancellationToken cancellationToken = default);
    public static Task AppendAllTextAsync(string path, ReadOnlyMemory<char> contents, Encoding encoding, CancellationToken cancellationToken = default);
}
```
## TensorPrimitives.HammingDistance

**Approved** | [#runtime/102980](https://github.com/dotnet/runtime/issues/102980#issuecomment-2161390236) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h31m31s)

* Looks good as proposed

```C#
namespace System.Numerics.Tensors;

public static class TensorPrimitives
{
    // Existing
    // public static void PopCount<T>(ReadOnlySpan<T> x, Span<T> destination);
    
    // New
    public static long PopCount<T>(ReadOnlySpan<T> x) where T : IBinaryInteger<T>;
    public static int HammingDistance<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y);
    public static long HammingBitDistance<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y) where T : IBinaryInteger<T>;
}
```
## Clone for IncrementalHash and KMAC

**Approved** | [#runtime/103170](https://github.com/dotnet/runtime/issues/103170#issuecomment-2161391899) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h35m8s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography;

public partial class IncrementalHash
{
    public IncrementalHash Clone();
}

public partial class Kmac128
{
    public Kmac128 Clone();
}

public partial class KmacXof128
{
    public KmacXof128 Clone();
}

public partial class Kmac256
{
    public Kmac256 Clone();
}

public partial class KmacXof256
{
    public KmacXof256 Clone();
}
```
## Regex.EnumerateSplits

**Approved** | [#runtime/100369](https://github.com/dotnet/runtime/issues/100369#issuecomment-2161405366) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h36m12s)

* Looks good as proposed

```C#
namespace System.Collections.Generic;

public class Regex
{
    public static ValueSplitEnumerator EnumerateSplits(ReadOnlySpan<char> input, [StringSyntax(StringSyntaxAttribute.Regex)] string pattern);
    public static ValueSplitEnumerator EnumerateSplits(ReadOnlySpan<char> input, [StringSyntax(StringSyntaxAttribute.Regex, nameof(options))] string pattern, RegexOptions options);
    public static ValueSplitEnumerator EnumerateSplits(ReadOnlySpan<char> input, [StringSyntax(StringSyntaxAttribute.Regex, nameof(options))] string pattern, RegexOptions options, TimeSpan matchTimeout);

    public ValueSplitEnumerator EnumerateSplits(ReadOnlySpan<char> input);
    public ValueSplitEnumerator EnumerateSplits(ReadOnlySpan<char> input, int count);
    public ValueSplitEnumerator EnumerateSplits(ReadOnlySpan<char> input, int count, int startat);
    
    public ref struct ValueSplitEnumerator
    {
        public readonly ValueSplitEnumerator GetEnumerator();
        public bool MoveNext();
        public readonly Range Current { get; }
    }
}
```
## Add a generic OrderedDictionary class

**Approved** | [#runtime/24826](https://github.com/dotnet/runtime/issues/24826#issuecomment-2161430034) | [Video](https://www.youtube.com/watch?v=I1c7VjiZK88&t=1h44m1s)

* Looks good as proposed
* We might want to add an overload of `GetAt()`/`SetAt` that takes `Index`
* Should probably go into `System.Collections.dll`

```C#
namespace System.Collections.Generic;

public class OrderedDictionary<TKey, TValue> :
    IDictionary<TKey, TValue>, IReadOnlyDictionary<TKey, TValue>, IDictionary,
    IList<KeyValuePair<TKey, TValue>>, IReadOnlyList<KeyValuePair<TKey, TValue>>, IList
    where TKey : not null
{
    public OrderedDictionary();
    public OrderedDictionary(int capacity);
    public OrderedDictionary(IEqualityComparer<TKey>? comparer);
    public OrderedDictionary(int capacity, IEqualityComparer<TKey>? comparer);
    public OrderedDictionary(IDictionary<TKey, TValue> dictionary);
    public OrderedDictionary(IDictionary<TKey, TValue> dictionary, IEqualityComparer<TKey>? comparer);
    public OrderedDictionary(IEnumerable<KeyValuePair<TKey, TValue>> collection);
    public OrderedDictionary(IEnumerable<KeyValuePair<TKey, TValue>> collection, IEqualityComparer<TKey>? comparer);

    public IEqualityComparer<TKey> Comparer { get; }
    public OrderedDictionary<TKey, TValue>.KeyCollection Keys { get; }
    public OrderedDictionary<TKey, TValue>.ValueCollection Values { get; }
    public int Count { get; }
    public TValue this[TKey key] { get; set; }

    public void Add(TKey key, TValue value);
    public void Clear();
    public bool ContainsKey(TKey key);
    public bool ContainsValue(TValue value);
    public KeyValuePair<TKey, TValue> GetAt(int index);
    public OrderedDictionary<TKey, TValue>.Enumerator GetEnumerator();
    public int IndexOf(TKey key);
    public void Insert(int index, TKey key, TValue value);
    public bool Remove(TKey key);
    public bool Remove(TKey key, [MaybeNullWhen(false)] out TValue value);
    public void RemoveAt(int index);
    public void SetAt(int index, TValue value);
    public void SetAt(int index, TKey key, TValue value);
    public void TrimExcess();
    public bool TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value);

    public struct Enumerator : IEnumerator<KeyValuePair<TKey, TValue>>
    {
        public KeyValuePair<TKey, TValue> Current { get; }
        public void Dispose();
        public bool MoveNext();
    }

    public sealed class KeyCollection : IList<TKey>, IReadOnlyList<TKey>, IList
    {
        public int Count { get; }
        public bool Contains(TKey key);
        public void CopyTo(TKey[] array, int arrayIndex);
        public OrderedDictionary<TKey, TValue>.KeyCollection.Enumerator GetEnumerator();

        public struct Enumerator : IEnumerator<TKey>
        {
            public TKey Current { get; }
            public bool MoveNext();
            public void Dispose();
        }
    }

    public sealed class ValueCollection : IList<TValue>, IReadOnlyList<TValue>, IList
    {
        public int Count { get; }
        public void CopyTo(TValue[] array, int arrayIndex);
        public OrderedDictionary<TKey, TValue>.ValueCollection.Enumerator GetEnumerator();

        public struct Enumerator : IEnumerator<TValue>
        {
            public TValue Current { get; }
            public bool MoveNext();
            public void Dispose();
        }
    }
}
```
