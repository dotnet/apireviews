# Microsoft.ML.Core

Status: **Needs more work** | 
[API Ref](Microsoft.ML.Core.md) |
[Dependencies](Microsoft.ML_Dependencies.png) |
[Write-up](https://github.com/TomFinley/machinelearning/blob/a4511133d3c0b5bc993af22f09607945e7fdf063/docs/code/Catalog-Core.md)

## Notes

* `Column` can be a struct
    - Sharing is no longer a concern
* `Schema`
    - Should probably have a struct based enumerator
    - The value tuples don't play nicely with F#
    - We should probably remove them
* `Metadata`
    - Key/Value pairs associated with a single column
    - Uses `Schema` to describe itself, which is powerful, but might make it
      hard for developers to reason about. Can we simplify this? It seems the
      answer is no (concerns around ownership as well as benefits that are
      gained by uniformity with other parts, such as be able to persist the
      schema).
    - *Metadata* is a fairly generic term; the entire schema is metadata. Can we
      come up with a more specific name? Like annotations?
    - The getter seems complicated but it allows them to not keep the metadata
      instances around and they allow for easier chaining.
    - We could have a helper method or property that gets the value in an easier
      way, but it's hard to make the much simpler as they would have to be
      generic or the user would have to handle the type
* `IDataView`
    - `ISchematized` is being removed
    - Should be an abstract class
    - `GetRowCount()`. `lazy = true` means get if you know it, otherwise don't
      bother. `lazy = false` means, the caller *really* would like to know, but
      implementers can still return `null`.
        - Either make it a property `long? Count` or make it
          `bool TryGetCount(out long count)`.
    - `GetRowCursor()` instead of taking `Func<int, bool>` make it `Predicate<Column>`
      which makes it easier to reason about.
    - We should look into whether we should have an async version
    - We want to get rid of the the `IRowCursorConsolidator` base
* `ICounted`
    - It seems it's not taken by anyone
    - It seems it's co-implemented by `IRow` and `ICursor`
    - Can this be collapsed?
* `IRowCursor` is needed, but can we get rid of `ICursor`?
* `UInt128` should be something more specific like `RowId`
* `GetRootCursor()` is an performance optimization because many cursors just
  delegate. This could also be solved by `GetMover()`. If we converted `ICursor`
  to an abstract class, we might be able to hide this in the implementation, or
  at least make it protected.
* `MoveMany()` seems like something that could be removed.
* `ICursor.State` seems to be unused, can this be removed?
* `IRow.IsColumnActive` seems to be mostly used in asserts, is this needed? It
  seems `GetGetter()` could throw or return `null`.
* `IRowCursor` extending `IRow` seems odd. Can these be collapsed? If not, can
  they have an inheritance relationship when we're making them classes?
* `IRowToRowMapper`
    - `Action disposer` should probably be replaced by making the right type
      (`IRow`?) implement `IDisposable`.
    - `Func<int, bool>` is hard to get right. Should this be `Predicate<Column>`
      as well?
* Can we remove `DataKind` entirely? If not, we should probably rename some of the
  members, remove the overlap, and add an entry for `0`, like `Unknown` or `None`.
* `ColumnType`
    - We should remove the many `IsXxx` overloads, especially for the esoteric
      types.
    - It seems this to be heavy context; is it possible to reduce the number
      moving pieces, such as removing the concrete derivatives?