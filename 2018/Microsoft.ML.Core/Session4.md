# Microsoft.ML

Status: **Needs more work**

## Notes

* Remove `SchemaException`
* `ITranformer`
    - Could we model schema mapping solve by passing an empty `IDataView`?
* `IRowToRowMapper`
    - Is analogous to `IDataView`, not `ITransformer`
    - Used for transforming a row in-place; it's much cheaper than a general
      `ITransformer` as that goes has to allocate cursors which access the
      columns via reflection.
    - It seems even on the ML/our team there is some confusion around this type
      and how to implement it/how it works.
    - Do we need to expose this interface? Should we try to simplify this to
      that we can have the same perf gains without having to increase the
      concept count?
* `SchemaShape`
    - Could this be combined with `Schema`?
    - Could this be moved to a more specialized namespace?
    - It's propagated by estimators that can't look at data
    - In contrast, `Schema` represents the information that was derived by
      looking at data, thus has more information.
* `IDataReader<in TSource>`
    - `GetOutputSchema()` should be a property, probably just named `Schema`.
* `IHostEnvironment`
    - Remove `Process`
    - Remove `IsCancelled`, look at `CancellationToken`
    - Remove `CreateOutputFile`, `CreateTempFile`, `OpenInputFile`
    - `ComponentCatalog` is a homegrown version of MEF. Tom suggested to replace
      it with MEF v1 but a dependency on MEF in the public API looks
      problematic; the community isn't keen on MEF and ASP.NET Core designed the
      system for DI but deliberately didn't use MEF. You might want to look at
      MEF v2 where you can write rules that allow importing/exporting types
      based on rules (like all type derived from a certain base).
* `IChannel`
    - Should probably be replaced with a logger. You might want to look at the
      BCL logging libraries or `Microsoft.Extensions.Logging`.
* `IPipe<TMessage>`
    - Consider moving to `System.Threading.Channels.ChannelWriter<T>`
* `IProgressChannel`, `IProgressChannelProvider`
    - To be removed

## Next steps

1. Finish `MLContext`
2. Typical examples of implementations of common abstractions:
    * "typical" transformer (select columns, normalizer, PCA, SDCA, text loader)
    * "typical" trainer
    * This batch should get close to understand most of the basic samples