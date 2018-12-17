# IAsyncEnumerable

Status: **Needs more discussions** | 
[Video](https://www.youtube.com/watch?v=XgC0oV29OqM)

* Issues
    - [#32640: IAsyncEnumerable & IAsyncDisposable](https://github.com/dotnet/corefx/issues/32640)
    - [#32664: ManualResetValueTaskSource](https://github.com/dotnet/corefx/issues/32664)
    - [#32684: IAsyncEnumerable.ConfigureAwait ](https://github.com/dotnet/corefx/issues/32684)
    - [#32665: Implement IAsyncDisposable in the BCL](https://github.com/dotnet/corefx/issues/32665)

* We believe we need an `IAsyncDisposable` in order to model releasing resources
  in an asynchronous fashion, which is often needed, for example to close file
  handles. The compiler will use this in order to model `try/finally` in
  iterators.

## Notes

* We're using `ValueTask` and `ValueTask<T>` to improve performance in cases
  where the operation completes asynchronously.
* `ConfigureAwait` is supported with some machinery in `S.R.CS`. The support is
  the same
    - We'll add an extension method to S.T.Task.TaskExtension
* It should be OK for a type to only implement `IAsyncDisposable` if the type
  can only be used in an async context.

## Next actions

* We need to talk more about cancellation
    - Right now, it's ambient state that the implementer has to store on the
      the instance that implements `IAsyncEnumerable<T>`, which doesn't seem
      right as the state belongs to the enumerator, not the enumerable. Which
      means single use enumerable is fine, multi-use isn't.
    - The problem is there no easy way to pass it
