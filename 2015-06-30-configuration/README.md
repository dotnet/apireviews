# API Review 2015-06-30

This API review was also recorded and is available on [Google+](https://plus.google.com/events/ccje4f1n0t8hjvi1vqgdm9j89jk).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#2199-review-converged-configuration-apis) | [#2199](https://github.com/dotnet/corefx/issues/2199) Review converged configuration APIs

There are also some [follow-ups for the API review board](#follow-ups-for-api-review-board).

## Notes

### #2199 Review converged configuration APIs

Status: **Not reviewed** |
[Issue](https://github.com/dotnet/corefx/issues/2199) |
[API Reference](issue-2199.md) |
[Video](https://plus.google.com/events/ccje4f1n0t8hjvi1vqgdm9j89jk)

* This proposal encompasses two areas:
    1. **Configurations**. Configurations are key-value pairs
        - keys are insensitive: how does this play with sources that case sensitive (e.g. Linux/Unix environment variables)
        - There is a convention for hierarchies
        - Configurations are strings only. Consumers are expected to parse.
    2. **Options**. Allows injecting options in a strongly typed fashion via DI
        - depends on reflection
        - has some dependencies on DI, but could probably be factored out
* In this review, we've been focusing on the configuration abstractions, namely `IConfiguration`
* Primary feedback points:
    1. Do we want the API to be immutable or mutable?
    2. How do consumers know that an `IConfiguration` has been changed?

Notes below. For more details, take a look at the comments in the [API Reference](issue-2199.md).

* Indexer
    - returns `null` if the key can't be found which allows a terse syntax
    - One could chain a *default value source* which would allow to centralize default values in single place and thus free consumers from having to handle the case if no value is present
* What happens if the `:` delimiter is not appropriate for a given configuration source?
    - The source should handle it
    - But it seems we should have conventions
* Should `IConfiguration` be immutable?
    - The implementation is currently mutable but updates are atomic
    - We would need to make a statement about thread safety?
    - If no, should we have a way to hand out read-only snapshots?
* `IConfiguration` supports changes
    - Either by calling `Reload()` or with the indexer
    - How do consumers know it has changed? It needs a change notification concept otherwise caching consumers, such as the options layer, are broken.
* Walking the list of available keys is done by calling `GetChildren`
* `IConfiguration` should be enumerable to easily support expanding it in the debugger or to allow queries via Linq

After a bit of iteration we agreed that this should be the API shape, assuming a mutable design:

```CSharp
public interface IConfiguration : IEnumerable<KeyValuePair<string, string>> {
    string this[string key] { get; set; }
    void Reload();
    bool TryGetValue(string key, out string value);

    string Key { get; }
    string Value { get; set; }
    IConfiguration GetSection(string key);
    IEnumerable<IConfiguration> GetChildren();
}
```

### Follow-ups for API review board

* We'll have a separate discussion on the usage of interfaces vs. abstract classes
