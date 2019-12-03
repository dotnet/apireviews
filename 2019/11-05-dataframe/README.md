# Data Frame Notes

**Needs more work** |
[API](Microsoft.Data.DataFrame.md) |
[Video](https://www.youtube.com/watch?v=s1R5GRTVb48) |
[Demo](Microsoft.Data.DataFrame-Demo.md)

## Demo notes

* `StringDataFrameColumn`
    - `ApplyElementWise` looks odd, especially 

## General notes

* `DataFrame`
    - Factor out the `RecordBatch` dependency
    - `LoadCsv` should have an overload that is short as people have trouble with optional values
    - `LoadCsv` should also take an encoding
    - We shouldn't define operator overloads that always throw (e.g. `+(DataFrame df, bool value)`)

* Namespace & package name
    - `Microsoft.Data.Analysis`
        - Core stuff
        - `DataFrame`
        - Other stuff
    - `Microsoft.Data.Analysis.Arrow`
        - Arrow dependency
