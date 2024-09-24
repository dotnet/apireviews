# API Review 09/24/2024

## Add Message property to ExperimentalAttribute

**Approved** | [#runtime/107963](https://github.com/dotnet/runtime/issues/107963#issuecomment-2371903372) | [Video](https://www.youtube.com/watch?v=XAEbP9V7M5Q&t=0h0m0s)

* We decided against the constructor as optional arguments are usually set via properties
* The shape of `[Obsolete]` is already subtly different, the constructor might be confusing
* We probably wouldn't use it most of the time, but we shouldn't block it for easy cases / third parties
* This requires work in the C#/VB compilers to support message and output it if present
    - https://github.com/dotnet/roslyn/issues/75221

```C#
namespace System;

public partial class ExperimentalAttribute
{
    public string? Message { get; set; }
}
```
## Add System.Text.Json.Nodes.JsonArray.Remove(All|Range)

**Approved** | [#runtime/107962](https://github.com/dotnet/runtime/issues/107962#issuecomment-2371922756) | [Video](https://www.youtube.com/watch?v=XAEbP9V7M5Q&t=0h21m28s)

* We should be using `Func<T, bool>` as that's the standard now the "natural" type for a lambda
* We can consider DIMs for `IList<T>` later

```C#
namespace System.Text.Json.Nodes;

public class JsonArray
{
    public int RemoveAll(Func<JsonNode, bool> match);
    public void RemoveRange(int index, int count);
}
```
## Add an `OrderedDictionary.TryAdd` overload that returns the `int`-based index of an entry if the key already exists

**Approved** | [#runtime/107947](https://github.com/dotnet/runtime/issues/107947#issuecomment-2371941944) | [Video](https://www.youtube.com/watch?v=XAEbP9V7M5Q&t=0h31m56s)

* We should also have an overload for `TryGetValue`

```C#
namespace System.Collections.Generic;

public partial class OrderedDictionary<TKey, TValue>
{
    // Existing
    // public bool TryAdd(TKey key, TValue value);
    // public bool TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value);
    public bool TryAdd(TKey key, TValue value, out int index);
    public bool TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value, out int index);
}
```
