# API Review 01/24/2023

## [LSG] LoggerMessage - Add diagnostic when unexpected order provided in message template

**Rejected** | [#runtime/52225](https://github.com/dotnet/runtime/issues/52225#issuecomment-1402395360) | [Video](https://www.youtube.com/watch?v=3rhPF0C_Z7o&t=0h0m0s)

We concluded that since the source generator already does the right thing, we don't need to add the diagnostic.  While the old model was positional, there is no confusion because the old model had no named parameter. We believe this is good enough to avoid confusion. We don't need to force users of the source generator to be positional.
## [LSG] LoggerMessage - Add diagnostic - Can't have malformed format strings

**Approved** | [#runtime/52226](https://github.com/dotnet/runtime/issues/52226#issuecomment-1402410936) | [Video](https://www.youtube.com/watch?v=3rhPF0C_Z7o&t=0h21m35s)

Looks good as proposed (severity, category, ID). We should also change the generator to not emit any code rather than emitting code that won't compile with an unintelligent error.
## [LSG] LoggerMessage - Add diagnostic - Can't have the same template with different casing

**NeedsWork** | [#runtime/52228](https://github.com/dotnet/runtime/issues/52228#issuecomment-1402438184) | [Video](https://www.youtube.com/watch?v=3rhPF0C_Z7o&t=0h30m22s)

It seems we generally want to allow message parameters and parameters in C# to be matched case-insensitively such that folks can use Pascal-casing in the message for example. However, when one does it in the case of parameters that have the same name but differ in case the result is ill-defined today.

We can either do a diagnostic and disallow this (proposal) or we can change the source generator to first match case-sensitively and if that doesn't produce a match fall back to case-insensitive matching. We're leaning towards the latter.
## Abstract System.Reflection.Emit public types

**Approved** | [#runtime/78542](https://github.com/dotnet/runtime/issues/78542#issuecomment-1402461199) | [Video](https://www.youtube.com/watch?v=3rhPF0C_Z7o&t=0h52m32s)

Looks good as proposed except for the `GetMetdataToken()`.

We should have distinct method groups to avoid the (conceptual) ambiguity around what the string-based one does:

* `GetTypeMetadataToken(Type type)`
* `GetFieldMetadataToken(FieldInfo field)`
* `GetMethodMetadataToken(MethodInfo method)`
* `GetMethodMetadataToken(ConstructorInfo constructor)`
* `GetSignatureMetadataToken(SignatureHelper signature)`
* `GetStringMetadataToken(string stringConstant)`
## FrozenDictionary/Set.ToFrozen* overloads that take optimization level

**Approved** | [#runtime/81088](https://github.com/dotnet/runtime/issues/81088#issuecomment-1402499138) | [Video](https://www.youtube.com/watch?v=3rhPF0C_Z7o&t=1h9m57s)

* We don't like the percentage-based approach because it seems we'd have to document where the cut-offs are.
    - We do, however, believe the on-off semantics for optimizations make sense
    - For now, let's go with `bool`. If a third option shows before we ship, we'll switch to a enum. Otherwise we do overloads or enums later.

```diff
public static partial class FrozenDictionary
{
    public static FrozenDictionary<TKey, TValue> ToFrozenDictionary<TKey, TValue>(this IEnumerable<KeyValuePair<TKey, TValue>> source, IEqualityComparer<TKey>? comparer = null) where TKey : notnull;
+   public static FrozenDictionary<TKey, TValue> ToFrozenDictionary<TKey, TValue>(this IEnumerable<KeyValuePair<TKey, TValue>> source, IEqualityComparer<TKey>? comparer, bool optimizeForReading) where TKey : notnull;
    public static FrozenDictionary<TKey, TSource> ToFrozenDictionary<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null) where TKey : notnull;
    public static FrozenDictionary<TKey, TElement> ToFrozenDictionary<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey>? comparer = null) where TKey : notnull;
}

public static partial class FrozenSet
{
    public static FrozenSet<T> ToFrozenSet<T>(this IEnumerable<T> source, IEqualityComparer<T>? comparer = null);
+   public static FrozenSet<T> ToFrozenSet<T>(this IEnumerable<T> source, IEqualityComparer<T>? comparer, bool optimizeForReading);
}
```
## Proposal: Task.WhenAllSuccessfulOrAnyFailed

**NeedsWork** | [#runtime/21617](https://github.com/dotnet/runtime/issues/21617#issuecomment-1402531274) | [Video](https://www.youtube.com/watch?v=3rhPF0C_Z7o&t=1h40m25s)

@stephentoub wants to think about the naming and shape a bit more.

```C#
public partial class Task
{
    public static Task WhenAllSuccessfulOrAnyFailed(params Task[] tasks);
    public static Task WhenAllSuccessfulOrAnyFailed(IEnumerable<Task> tasks);
}
```
