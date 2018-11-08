# Microsoft.ML

Status: **Needs more work**

## Notes

* Tom is working on internalizing large chunks of `Microsoft.ML.Core`
* We didn't do much today as we didn't have quorum. But we did agree on a set of
  next steps:
    1. `Microsoft.ML.Core` (wrapping up)
        - `IEstimator`
        - `ITransformer`
        - `IDataReader`
        - `IDataReaderEstimator`
        - `SchemaShape`
        - `MLContext`
        - `IHostEnvironment`
        - `IHost`
    2. Typical examples of implementations of common abstractions:
        - "typical" transformer (select columns, normalizer, pca, sdca, text loader)
        - "typical" trainer
        - This batch should get close to under to understand most of the basic samples
