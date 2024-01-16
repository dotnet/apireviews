# API Review 01/16/2024

## Get InvocationList of MulticastDelegate without any allocation

**Approved** | [#runtime/41849](https://github.com/dotnet/runtime/issues/41849#issuecomment-1894294984) | [Video](https://www.youtube.com/watch?v=StEWcIIegU4&t=0h0m0s)

* We discussed whether the new enumerator needs to declare IEnumerable and/or IEnumerator, and we feel that's not necessary given that GetInvocationList() is already a Linq.Enumerable-friendly version of the data.
* EnumerateInvocationList being static was questioned, and it was answered that it was to make the enumerated values be typed as the input type, rather than going back to just System.Delegate.
* `IsSingle` got changed to the first alternative proposal of `HasSingleTarget`
* `GetInvocationListLength` was removed, pending a more concrete need in the future.

```C#
public class Delegate
{
    // Existing API
    // public virtual System.Delegate[] GetInvocationList();

    // Returns whether the delegate has a single target. It is a property to highlight that it is guaranteed 
    // to be a very cheap operation that is important for set of scenarios addressed by this proposal.
    public bool HasSingleTarget { get; }

    // Returns invocation list enumerator
    public static InvocationListEnumerator<TDelegate> EnumerateInvocationList<TDelegate>(TDelegate d) where TDelegate : Delegate;

    // The shape of this enumerator matches existing StringBuilder.ChunkEnumerator and Activity.Enumerator
    // Neither of these existing enumerators implement IEnumerable and IEnumerator. This can be changed. 
    // Q: How are we deciding whether enumerators like this should implement IEnumerable and IEnumerator?
    // Note that implementing these interfaces makes each instantiation of the type more expensive, so it is a tradeoff.
    public struct InvocationListEnumerator<TDelegate> where TDelegate : Delegate
    {
        public TDelegate Current { get; }
        public bool MoveNext();

        // EditorBrowsable to match existing StringBuilder.ChunkEnumerator and Activity.Enumerator
        [EditorBrowsable(EditorBrowsableState.Never)] // Only here to make foreach work
        public InvocationListEnumerator<TDelegate> GetEnumerator() => this;
    }
}
```
## Consider adding TaskCompletionSource.SetFromTask(Task)

**Approved** | [#runtime/47998](https://github.com/dotnet/runtime/issues/47998#issuecomment-1894306184) | [Video](https://www.youtube.com/watch?v=StEWcIIegU4&t=0h23m23s)

* The `task` parameters were renamed to `completedTask` to help indicate that the parameter must always be in a `task.IsCompleted == true` state.

```C#
namespace System.Collections.Threading.Tasks
{
    public partial class TaskCompletionSource
    {
        public void SetFromTask(Task completedTask);
        public bool TrySetFromTask(Task completedTask);
    }

    public partial class TaskCompletionSource<T>
    {
        public void SetFromTask(Task<T> completedTask);
        public bool TrySetFromTask(Task<T> completedTask);
    }
}
## Add string keyed dictionary ReadOnlySpan<char> lookup support

**NeedsWork** | [#runtime/27229](https://github.com/dotnet/runtime/issues/27229#issuecomment-1894431654) | [Video](https://www.youtube.com/watch?v=StEWcIIegU4&t=0h31m53s)

After a very long discussion, the group seemed to be favoring the "proxy type" approach.  Perhaps the best suggested name for the type is `SpanLookup`, though that didn't necessarily reach consensus.

So the proposal should be converted to a format like

```C#
partial class Dictionary<TKey, TValue>
{
    public SpanLookup<TElement> GetSpanLookup<TElement>();

    public readonly struct SpanLookup<TElement>
    {
        public Dictionary<TKey, TValue> Dictionary { get; }

        public bool ContainsKey(ReadOnlySpan<TElement> key);
        ...
    }
}

...
```

For completeness, we will probably empower all of

* Dictionary`2
* HashSet`1
* ConcurrentDictionary`2
* FrozenDictionary`2
* FrozenSet`1


And we haven't landed on the name for `IMappedSpanEqualityComparer`.
