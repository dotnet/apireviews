# API Review 2016-02-09

This API review was also recorded and is available on [Google Hangouts](https://plus.google.com/events/c4l3me8hagne0cgeipqh8959gk4).

## API for GetColumnSchema

Status: **Approved with feedback** |
[Issue](https://github.com/dotnet/corefx/issues/5915) |
[Video](https://plus.google.com/events/c4l3me8hagne0cgeipqh8959gk4)

After some discussion we decided to simplify this design to keep it simpler for V2. Originally, we said we'll add a virtual method on `DbDataReader` that in V2 we'll replace the current proposal that derivatives of `DbDataReader` need to implement the interface `IDbColumnSchemaGenerator`, which is what we plan on doing now in order avoid having add an API to the .NET Framework (V1)

We concluded that there is no way to make this work well in V2, specifically around cases where a V1 consumers tries to deal with a V2 provider or a V2 consumer uses a V1 provider.

More notes:

* `DbColumn`
    - Setters should be `protected`
    - Should the indexer throw for non existing keys or return `null`? We should do one of the following:
        1. Agree that `null` is a good enough value for non-existing keys.
        2. Follow `Dictionary<K,V>` and throw for non-existing keys and also offer `TryGetValue` to avoid exceptions.
    - The indexer exposes properties defined on the derived type, which allows consumers to light up for specific providers without having a static dependency on the derived type
* No virtual for V2
    - Provider will continue to provide schema by implementing the interface