# API Review 03/26/2024

## Add string keyed dictionary ReadOnlySpan<char> lookup support

**Approved** | [#runtime/27229](https://github.com/dotnet/runtime/issues/27229#issuecomment-2021234425) | [Video](https://www.youtube.com/watch?v=9VN0gNmtP5w&t=0h0m0s)


* Mapped => Alternate
  * On the interface, Map => Create.
* The syntax of a generic method on `Dictionary<TKey, TValue>` looks nicer than an extension method, because of a lack of generic inference.  But if we need it to be an extension method because of the size/cost of the method on Dictionary itself, then the extension method is better than nothing.
  * The nested types may also be a problem, TBD.
* We discussed making IAlternateEqualityComparer : IEqualityComparer, but if `TAlternate` == `T` then Equals(TAlternate, T) conflicts with Equals(T, T) and makes the universe implode.

```c#

namespace System.Collections.Generic
{
    // implemented by all string-related comparers, e.g. the comparer instances returned from StringComparer.Ordinal, EqualityComparer<string>.Default, etc.
    public interface IAlternateEqualityComparer<in TAlternate, T> where TAlternate : allow ref struct
    {
        bool Equals(TAlternate alternate, T other);
        int GetHashCode(TAlternate span);
        T Create(TAlternate alternate);
    }

    public static partial class CollectionExtensions
    {
        public static Dictionary<TKey, TValue>.AlternateLookup<TAlternateKey> GetAlternateLookup<TKey, TValue, TAlternateKey>(
            this Dictionary<TKey, TValue> dictionary)
            where TKey : notnull where TAlternateKey : allow ref struct;
        public static bool TryGetAlternateLookup<TKey, TValue, TAlternateKey>(
            this Dictionary<TKey, TValue> dictionary,
            out Dictionary<TKey, TValue>.AlternateLookup<TAlternateKey> lookup)
            where TKey : notnull where TAlternateKey : allow ref struct;

        public static HashSet<T>.AlternateLookup<TAlternate> GetAlternateLookup<TAlternate>(
            this HashSet<T> set)
            where TAlternate : allow ref struct;
        public static bool TryGetAlternateLookup(
            this HashSet<T> set, out HashSet<T>.AlternateLookup<TAlternate> lookup)
            where TAlternate : allow ref struct;
    }

    public class Dictionary<TKey, TValue>
    {
        public readonly struct AlternateLookup<TAlternateKey> where TAlternateKey : allow ref struct
        {
            public bool ContainsKey(TAlternateKey key);
            public bool TryAdd(TAlternateKey  key, TValue value);
            public bool TryGetValue(TAlternateKey key, [MaybeNullWhen(false)] out TValue value);
            public bool TryGetValue<TKey, TValue, TAlternateKey>(TAlternateKey  key, [NotNullWhen(true)] out TKey? actualKey, [MaybeNullWhen(false)] out TValue value);
            public bool Remove(TAlternateKey key);
            public bool Remove(TAlternateKey  key, [MaybeNullWhen(false)] out TValue value));
        }
    }

    public class HashSet<T>
    {
        public readonly struct AlternateLookup<TAlternate>
        {
            public bool Add(TAlternate item);
            public bool Contains(TAlternate item);
            public bool Remove(TAlternate item);
            public bool TryGetValue(TAlternate equalValue, [MaybeNullWhen(false)] out T actualValue);
        }
    }
}

namespace System.Runtime.InteropServices
{
    public static class CollectionsMarshal
    {
        public static ref TValue? GetValueRefOrAddDefault<TKey, TValue, TAlternateKey>(
            Dictionary<TKey, TValue>.AlternateLookup<TAlternateKey> dictionary,
            TAlternateKey key, out bool exists)
            where TKey : notnull where TAlternateKey : allow ref struct;
        public static ref TValue GetValueRefOrNullRef<TKey, TValue, TAlternateKey>(
            Dictionary<TKey, TValue>.AlternateLookup<TAlternateKey> dictionary,
            TAlternateKey key)
            where TKey : notnull where TAlternateKey : allow ref struct;
    }
}

namespace System.Collections.Concurrent
{
    public class ConcurrentDictionary<TKey, TValue>
    {
        public ConcurrentDictionary<TKey, TValue>.AlternateLookup<TAlternateKey> GetAlternateLookup<TAlternateKey>() 
            where TAlternateKey : allow ref struct;
        public bool TryGetAlternateLookup<TAlternateKey>(
            out ConcurrentDictionary<TKey, TValue>.AlternateLookup<TAlternateKey> lookup)
            where TAlternateKey : allow ref struct;

        public readonly struct AlternateLookup<TAlternateKey>
        {
            public bool ContainsKey<TKey, TValue, TAlternateKey>(TAlternateKey key);
            public bool TryAdd<TKey, TValue, TAlternateKey>(TAlternateKey key, TValue value);
            public bool TryGetValue<TKey, TValue, TAlternateKey>(TAlternateKey key, [MaybeNullWhen(false)] out TValue value);
            public bool TryGetValue<TKey, TValue, TAlternateKey>(TAlternateKey key, [NotNullWhen(true)] out TKey? actualKey, [MaybeNullWhen(false)] out TValue value);
            public bool TryRemove<TKey, TValue, TAlternateKey>(TAlternateKey key, [MaybeNullWhen(false)] out TValue value);
            public bool TryRemove<TKey, TValue, TAlternateKey>(TAlternateKey key, [NotNullWhen(true) out TKey? actualKey, [MaybeNullWhen(false)] out TValue value);
        }
    }
}

namespace System.Collections.Immutable
{
    public class FrozenDictionary<TKey, TValue>
    {
        public FrozenDictionary<TKey, TValue>.AlternateLookup<TAlternateKey> GetAlternateLookup<TAlternateKey>()
            where TAlternateKey : allow ref struct;
        public bool TryGetAlternateLookup<TAlternateKey>(
            out FrozenDictionary<TKey, TValue>.AlternateLookup<TAlternateKey> lookup)
            where TAlternateKey : allow ref struct;

        public readonly struct AlternateLookup<TAlternateKey>
        {
            public bool ContainsKey<TKey, TValue, TAlternateKey>(TAlternateKey key);
            public bool TryGetValue<TKey, TValue, TAlternateKey>(TAlternateKey key, [MaybeNullWhen(false)] out TValue value);
            public bool TryGetValue<TKey, TValue, TAlternateKey>(TAlternateKey key, [NotNullWhen(true)] out TKey? actualKey, [MaybeNullWhen(false)] out TValue value);
        }
    }

    public class FrozenSet<T>
    {
        public FrozenSet<T>.AlternateLookup<TAlternate> GetAlternateLookup<TAlternate>()
            where TAlternateKey : allow ref struct;
        public bool TryGetAlternateLookup<TAlternateKey>(
            out FrozenSet<T>.AlternateLookup<TAlternate> lookup)
            where TAlternateKey : allow ref struct;

        public readonly struct AlternateLookup<TAlternate>
        {
            public bool Contains(TAlternate item);
            public bool TryGetValue(TAlternate equalValue, [MaybeNullWhen(false)] out T actualValue);
        }
    }
}
```
## Making "Process asynchronous tasks as they complete" easy by using IAsyncEnumerable

**Approved** | [#runtime/61959](https://github.com/dotnet/runtime/issues/61959#issuecomment-2021271566) | [Video](https://www.youtube.com/watch?v=9VN0gNmtP5w&t=0h42m29s)

Looks good as proposed

```c#
namespace System.Threading.Tasks;

public class Task
{
    public static IAsyncEnumerable<Task> WhenEach(params Task[] tasks);
    public static IAsyncEnumerable<Task> WhenEach(params ReadOnlySpan<Task> tasks);
    public static IAsyncEnumerable<Task> WhenEach(IEnumerable<Task> tasks);

    public static IAsyncEnumerable<Task<TResult>> WhenEach(params Task<TResult>[] tasks);
    public static IAsyncEnumerable<Task<TResult>> WhenEach(params ReadOnlySpan<Task<TResult>> tasks);
    public static IAsyncEnumerable<Task<TResult>> WhenEach(IEnumerable<Task<TResult>> tasks);
}
```
