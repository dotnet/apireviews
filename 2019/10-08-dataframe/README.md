# Data Frame Notes

**Needs more work** |
[API](Microsoft.Data.DataFrame.md) |
[Video](https://youtu.be/FAaw4uaYvgY?t=114)

## General notes

* .NET version of Pandas
* Two primary scenarios
    - Spark.NET
    - Interactive exploration (Jupyter Notebook)
* `DataFrame` is to `IDataView` as
  `List<T>` is to `IEnumerable<T>`
* Would it be possible to align the `DataFrame` API more with `IDataView` so
  that they feel like orthogonal features of the same technology, rather than
  two competing technologies.
* Equality operators seem problematic because it messes with people's intuition
    - This applies to `<`, `<=`, `>`, `>=`
    - Should we talk to C# about more operators?
    - This sadly means that certain compact notations aren't expressible

## BaseColumn

* Shouldn't use the `Base` prefix. What about `DataFrameColumn`?

## DataFrame

* `ColumnCount` and `Columns.Count` seem odd? Presumably the are both `O(1)`?
* You should probably not return `IList<T>` for columns
    - `ReadOnlyCollection<BaseColumn>` or your own custom type seems more
      appropriate
* The indexers seems inconsistent
    - The `IEnumerable<int>` should be of `long`
    - The `BaseColumn` has surprising behavior (selects rows)
    - The `row`, `column` indexer seems at odds with the one takes
      `IEnumerable<>`
    - Maybe we need better parameter names?
    - One issue seems to be related to range-based indexing. `Range` doesn't
      work because it's only up to `int.MaxValue` rows (rows are long here)

## Next steps

* We should close on the package name and namespace name
* There are some concerns that this API shape is very specific to Apache Arrow.
  If that's the case we should not put it in a general purpose namespace
    - Examples: stores data in colums, rather than rows, `RecordBatch`