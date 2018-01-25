# System.IO.Enumerations

Status: **Needs more work** | 
[API](System.IO.FileSystem.md) |
[Video](https://www.youtube.com/watch?v=osuw4V-H8hA)

## Notes

* Most notes are in the [API reference](System.IO.FileSystem.md).
* High-level:
    - Does this assembly need to be made available to .NET Framework OOB?
    - We should move the enumeration feature to a nested namespace
      `System.IO.Enumeration`.
    - We should rename `FindData<T>` to `FileSystemEntry<T>`
    - We should try to remove the user supplied state from `FindData<T>` and
      make it instead part of the callback signature.
    - Calling the `FindEnumerable<TResult, TState>` is quite busy due to the
      fact that everything is a delegate. Consider making them virtual methods
      and let users derive from `FindEnumerable<TResult, TState>`.
    - Collapse the static types holding the matchers into a single type.
  
## Requirements from CPS/MSBuild

* Required for CPS and benefit for MSBuild: `BeginDirectoryEnumeration` /
  `EndDirectoryEnumeration` callbacks.
* Benefit for MSBuild: enumeration overload accepting a `RecurseDirectory`
  callback that also returns a new state object to use for that subtree (either
  this or we'll have to use a global dictionary in the global state object to
  keep the state per subtree).