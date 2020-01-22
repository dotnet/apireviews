# Quick Reviews 1/14/2020

## Port `MemoryMappedFileSecurity` and add extensions for `MemoryMappedFile` #941

**Needs work** | [#runtime/941](https://github.com/dotnet/runtime/issues/941#issuecomment-574306474) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

* We need to know a little more about the scenario. In particular, is the only
  reason the `FileStream` is accepted in the ctor is so that there's something
  to dispose at the end of the operation? Can we instead dispose the
  `FileStream` and the memmapped handle at the same time through some clever ref
  counting mechanism?
* See if there's perhaps a way to enable this scenario without exposing this
  particular ctor.
 
## Add TimeSpan.Infinite

**Rejected** | [#corefx/40693](https://github.com/dotnet/corefx/issues/40693#issuecomment-574308350) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

This sentinel value would only be special-cased by specific APIs, and in some
contexts -1 is a perfectly valid value for `TimeSpan` parameters. This really
should be a doc issue demonstrating correct use of the existing `Timeout` API
rather than a public API being added to `TimeSpan`.

## [System.Runtime.Intrinsics] API suggestion: Introduce an intrinsic all bits set field to Vector intrinsic types #40504

**Approved** | [#corefx/40504](https://github.com/dotnet/corefx/issues/40504#issuecomment-574310177) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

* `AllBitsSet` seems like a decent name given the scenario.
* Also rename the internal member `Vector<T>.AllOnes` at the same time so that
  if it does become public in the future we have consistent naming already.
 
## ExecutionContext.Run<TContext> overloads

**Approved** | [#corefx/41131](https://github.com/dotnet/corefx/issues/41131#issuecomment-574312085) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

Please match parameter names for the new overloads to parameter names for the existing overloads.

## StringSplitOptions.TrimEntries

**Approved** | [#corefx/41487](https://github.com/dotnet/corefx/issues/41487#issuecomment-574317661) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

See if there's any usage of `Split` followed by `TrimStart` and `TrimEnd` as
opposed to just `Trim`. That'll inform us on whether these enum values being
added are sufficient for common use cases.
 
## Unsafe.NullRef, IsNullRef

**Needs work** | [#corefx/41783](https://github.com/dotnet/corefx/issues/41783#issuecomment-574325067) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

This really should be `in` from a purity perspective, but should sit down with
language folks to figure out exactly what pitfalls might exist and whether the
language itself could be evolved to address any concerns. I'll punt this back to
myself.
 
## One-shot AES CBC and ECB

**Approved** | [#corefx/42372](https://github.com/dotnet/corefx/issues/42372#issuecomment-574334307) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

* CBC padding mode should default to PKCS7, ECB padding modes should be
  non-default (require explicit value at call site). For all methods, move
  non-_out_ parameters before _out_ parameters.
* @bartonjs - please document whether the newly introduced virtual members would
  ever mutate the underlying instance (changing block mode or padding mode) or
  throw.
 
## Ctor of sorted collection classes should support IReadOnlyDictionary or IEnumerable

**Needs work** | [#corefx/38395](https://github.com/dotnet/corefx/issues/38395#issuecomment-574338733) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

We can't add the desired ctor because it would conflict with the
`.ctor(IDictionary<TKey, TValue>, ...)` ctor and would be a compile-time
breaking change. We might be able to get away with using one of the existing
ctors and instead adding a method `AddRange(IEnumerable<KeyValuePair<TKey,
TValue>>)`, so the call site would look like:

```C#
IReadOnlyDictionary<TKey, TValue> myDict = GetDictionary();
var sortedDictionary = new SortedDictionary<TKey, TValue>(comparer);
sortedDictionary.AddRange(myDict);
```

Perhaps a ctor that takes a capacity would also benefit here, that way the
instance is already sized appropriately for the data that will be added to it.

Would this proposed API be sufficient for your scenarios?

## ObjectDisposedException missing constructor with all: objectName, message, innerException

**Needs work** | [#runtime/1210](https://github.com/dotnet/runtime/issues/1210#issuecomment-574342594) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)

* Per @scalablecory's comments, what's the scenario for this? Why not match the
  existing constructors in your own subclassed type?
 
## Mark Value and HasValue of System.Nullable<T> as readonly

**Approved** | [#runtime/836](https://github.com/dotnet/runtime/issues/836#issuecomment-574346202) | [Video](https://www.youtube.com/watch?v=dJLmN6u98Z4&t=0h0m0s)
 
* It looks like the Roslyn compiler already special-cases this, so perf is
  already in a good way. But we should probably do this anyway for non-Roslyn
  compiler scenarios, and so that the API surface matches the expectation.
* Approved for these APIs specifically:
    - `get_Value`
    - `get_HasValue`
    - `GetValueOrDefault` (both overloads)