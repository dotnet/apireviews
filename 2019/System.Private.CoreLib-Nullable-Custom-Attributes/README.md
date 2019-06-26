# Nullability Custom Attributes for System.Private.CoreLib

Status: **Approved with Feedback**

This is the set of [custom attributes] that allows tweaking the nullability
analysis of the compiler.

[custom attributes]: https://github.com/dotnet/corefx/issues/37826

## General rules

* `TryParse`-style APIs for the `out` parameters:
    - `[MayBeNullWhen(false)]` when `T` is unconstrained
    - `[NotNullWhen(true)]` when `T` is constrained to be a reference type

## System.Runtime

[Diff](System.Runtime.md) |
[Video](https://youtu.be/t1MQePMqRmQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=1756)

* Why does IDictionary's indexer have `[DisallowNull]`?
* Should things like `ICollection<T>.Contains(T item)` be marked as allowing
  null values, even when instantiated with a non-null `T`? This removes the need
  from the user having to null check, and, the behavior of the method is seem
  well-defined (return false). However, we believe there are implementations
  that will reject nulls here.

## System.Runtime.Extensions

[Diff](System.Runtime.Extensions.md) |
[Video](https://youtu.be/t1MQePMqRmQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=4154)

* `Convert.ChangeType` user could return `null` for non-null inputs, but they
  would violate the contract of `IConvertible`.
* All `Convert.ChangeType` overloads should also be marked with
  `NotNullIfNotNull`.

## System.Runtime.InteropServices

[Diff](System.Runtime.InteropServices.md) |
[Video](https://youtu.be/t1MQePMqRmQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=4653)

* `Marshal.CreateWrapperOfType<T, TWrapper>` should probable attributed with
  either `MayBeNull` or even `NotNullIfNotNull`.
* `Marshal.GetObjectsForNativeVariants<T>` we can't express that it returns `T?[]`

## System.Runtime.InteropServices.WindowsRuntime

[Diff](System.Runtime.InteropServices.WindowsRuntime.md) |
[Video](https://youtu.be/t1MQePMqRmQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=5548)

* `EventRegistrationTokenTable<T>.InvocationList` should be declared `T?`

## System.Threading

[Diff](System.Threading.md) |
[Video](https://youtu.be/t1MQePMqRmQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=6246)

* `Interlocked.Exchange` return attribute should refer to `location1`

## System.Threading.Thread

[Diff](System.Threading.Thread.md) |
[Video](https://youtu.be/t1MQePMqRmQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=6854)

* Looks good.

## System.Collections.Concurrent

[Diff](System.Collections.Concurrent.md) |
[Video](https://youtu.be/dt-MEB8ujgk?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=556)

* Looks good.

## System.Collections

[Diff](System.Collections.md) |
[Video](https://youtu.be/dt-MEB8ujgk?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=826)

* It's a bit odd that `Comparer<T>` explicitly allows `null`. Folks could easily
  use `Comparer<string>` (same for `EqualityComparer<string>`) but it's unlikely
  that people extending it would be generic or specialized over null vs.
  non-null.
* `Dictionary.this[object key]` is marked with [DisallowNullAttribute] which
  seems wrong because we don't accept a null `key`, but we do accept a null
  `value`.

## System.Diagnostics.Contracts

[Diff](System.Diagnostics.Contracts.md) |
[Video](https://youtu.be/dt-MEB8ujgk?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=1635)

* Looks good.

## System.Diagnostics.Debug

[Diff](System.Diagnostics.Debug.md) |
[Video](https://youtu.be/dt-MEB8ujgk?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=1682)

* Looks good.

## System.Memory

[Diff](System.Memory.md) |
[Video](https://youtu.be/dt-MEB8ujgk?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=1952)

* Looks good.
