# Microsoft.ML

Status: **Needs more work**

## Notes

* `MLContext`
    - Solves two problems: the chaining for `IHostEnvironment` and the
      discoverability for finding implementations of common abstraction.
    - They are solved by offering extension methods
    - We should align the naming of `XxxCatalog` with `XxxContext`
    - It's a bit odd that the constructor uses nullability to indicate default,
      but sentinel values seem more problem. Overloads don't work very well
      because the types are the same and it will force people to specify value
      of the other. Easiest solution might be to make both nullable. Still not
      idiomatic API design, but at least consistent.
* [Typical example](https://dotnet.microsoft.com/learn/machinelearning-ai/ml-dotnet-get-started-tutorial)
    - Most of the methods aren't verbs; instead of `TextReader()` it should be
      `CreateTextReader()`
    - We should have a pattern for the arguments:
        - Simple set of arguments should just be parameters
        - More complicated arguments should be an option class that is mutable
        - Some types probably want to offer discrete parameters for convenience
          while also being able to accept more complex options. The overloads
          should never mix them as this would create conflicts which value
          "wins".
        - Example:
            ```C#
            var options = new TextFileSource.Options();
            options.Separator = ",",
            options.HasHeader = true,
            options.Column.Add("SepalLength", DataKind.R4, 0);
            options.Column.Add("SepalWidth", DataKind.R4, 1);
            options.Column.Add("PetalLength", DataKind.R4, 2);
            options.Column.Add("PetalWidth", DataKind.R4, 3);
            options.Column.Add("Label", DataKind.Text, 4);

            var reader = mlContext.Data.CreateTextFileSource(options);
            ```
    - Tom check what you were plan on checking :-)