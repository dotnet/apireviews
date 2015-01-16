# API Review 2014-01-14

This API review was also recorded and is available on [Channel 9](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14).

## Overview

In this API review we reviewed the following PRs and proposals:

### Pull requests

* [#316](https://github.com/dotnet/corefx/pull/316): Implement `IList<T>`, `IReadOnlyList<T>`, and `IList` on Regex Collections
    - [API Reference](issue-316.md)
    - [Notes](#316-implement-ilistt-ireadonlylistt-and-ilist-on-regex-collections)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=0m2s)
* [#110](https://github.com/dotnet/corefx/pull/110): Add async document/element loading for XLinq
    - [API Reference](issue-110.md)
    - [Notes](#110-add-async-documentelement-loading-for-xlinq)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=30m21s)

### Proposals

* [#400](https://github.com/dotnet/corefx/issues/400): Add `Cast<T>` and `CastFrom<TDerived>` to `ImmutableArray<T>`
    - [Notes](#400-add-castt-and-castfromtderived-to-immutablearrayt)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=48m38s)
* [#394](https://github.com/dotnet/corefx/issues/394): Add `GetOrAdd` and `AddOrUpdate` overloads to `ConcurrentDictionary<TKey, TValue>` that take a `TArg` factoryArgument
    - [Notes](#394-add-getoradd-and-addorupdate-overloads-to-concurrentdictionarytkey-tvalue-that-take-a-targ-factoryargument)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=57m36s)
* [#382](https://github.com/dotnet/corefx/issues/382): Add ctor to `HashSet<T>` that allows the initial capacity to be specified
    - [Notes](#382-add-constructor-to-hashsett-that-allows-the-initial-capacity-to-be-specified)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h11m46s)
* [#304](https://github.com/dotnet/corefx/issues/304): `Regex` should provide a validation method
    - [Notes](#304-regex-should-provide-a-validation-method)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h12m55s)
* [#311](https://github.com/dotnet/corefx/issues/311): Bring back `Console.CancelKeyPress`
    - [Notes](#311-bring-back-consolecancelkeypress)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h32m33s)
* [#306](https://github.com/dotnet/corefx/issues/306): Make Process.Start have a option to change handle inheritance
    - [Notes](#306-make-processstart-have-a-option-to-change-handle-inheritance)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h33m12s)
* [#318](https://github.com/dotnet/corefx/issues/318): Proposal for `ImmutableInterlocked.ApplyChange` API
    - [Notes](#318-proposal-for-immutableinterlockedapplychange-api)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h46m50s)
* [#373](https://github.com/dotnet/corefx/issues/373): `BitVector32` should implement `IEquatable<T>`
    - [Notes](#373-bitvector32-should-implement-iequatablet)
    - [Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h49m50s)

## Notes

### #316: Implement `IList<T>`, `IReadOnlyList<T>`, and `IList` on Regex Collections

Status: **Approved with feedback** <br>
[API Reference](issue-316.md) <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=0m2s)

* We had a general discussion around why we should implement the mutable collection interfaces as well as the read-only interfaces
    - We concluded that, for the most part, the answer is yes, because that's the current design point of collections
    - However, in most cases that's a given because the collections are simply extending `ReadOnlyCollection<T>`
    - For very domain-specific collection types, such as the struct-based collections in the metadata reader, it seems more trouble than being worth
    - We should instead consider adding a adapter based design where the consumer can call a method that converts one to the other
    - Until we have a plan in-place we believe we should leave the PR as proposed, i.e. implement both, the mutable as well as the read-only interfaces
* There is a potential serialization impact because they are marked as serializable.
    - We need to investigate whether implementing `IList` would be a breaking change for the .NET Framework serializers
* The implemented strongly typed, non-mutating methods should be implemented implicitly, i.e. publicly, not explicitly
* We should make sure that all mutating operations throw the same exception as `ReadOnlyCollection<T>`

### #110: Add async document/element loading for XLinq

Status: **Not ready yet** <br>
[API Reference](issue-110.md) <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=30m21s)

* We had a general discussion about whether it's OK to use optional parameters
    - In the past, we've avoided optional parameters because of issues around API versioning
    - It seems the Roslyn team came up with a way to version APIs that have optional parameters over time
    - We may need tooling to enforce that so that we don't accidentally introduce source breaking changes
    - However, that story isn't in-place yet
    - So we concluded that we leave the proposed set of overloads as-is, i.e. we don't introduce overloads that make `CancellationToken` optional. We'll revisit this once we have the closed on the usage of optional parameters.
* The PR as proposed is incomplete; we also need to add `SaveAsync` methods for both `XDocument` and `XElement`
* We would like to obsolete the `Load(string uri)` so we should not expose the `LoadAsync(String uri)`.
* `Parse` is compute bound and not IO bound so we don't want async overloads for it.

### #400: Add `Cast<T>` and `CastFrom<TDerived>` to `ImmutableArray<T>`

Status: **Accepting PRs** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=48m38s)

* We like the proposed APIs
* However, we just need to work out the naming of those APIs so that they convey what they are doing.
    - `Cast` name conflicts with `Enumerable.Cast` so we need to avoid that name.
    - `CastFrom` is weird as casting is always conceptually casting from something.

### #394: Add `GetOrAdd` and `AddOrUpdate` overloads to `ConcurrentDictionary<TKey, TValue>` that take a `TArg` factoryArgument

Status: **Not ready yet** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=57m36s)

* People can add these on top of the public APIs with similar performance.
* We've a general concern with APIs that introduce an open-ended, never finished set of utility methods
    - It seems that both `GetOrAdd` and `AddOrUpdate` fall into this category, so we're hesitant to just add them
    - The question is: where do we draw the line between adding lots of corner case overload APIs vs just providing sample code for these.
    - If there are no benefits, like performance or very common use cases, for having them as instance methods then we should avoid adding them to the core API set to avoid the bloat on our core types.
* **[@stephentoub](https://github.com/stephentoub)** will investigate if there are performance wins in our current implementations.

### #382: Add constructor to `HashSet<T>` that allows the initial capacity to be specified

Status: **Accepting PRs** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h11m46s)

* Seems like a no-brainer, let's do it!

### #304: `Regex` should provide a validation method

Status: **Rejected** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h12m55s)

* We should split this proposal into two:
    - P1: We currently throw an `ArgumentException` for parse errors and we agree we should try to at least add more information like the character index. We should consider adding a `RegexParseException` that inherits from `ArgumentException`.
    - P2: We could add a `TryParse` that avoids throwing exceptions if the parsing fails.
* We also don't want expose an enumeration for the error code as this makes adding to it somewhat harder
* It also seems overkill to support multiple errors. It seems when a consumer wants to implement full-blown regular expression experience that, that it requires to re-implement the entire regex parsing anyways
* We definitely don't want to expose a full AST
    - For one, it's a lot of work to change our existing regex engine
    - Secondly, it would very likely trade design-time experience against runtime performance and the designer scenario is fairly uncommon.

### #311: Bring back `Console.CancelKeyPress`

Status: **Accepted** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h32m33s)

* There is nothing wrong with the API
* Only reason it's missing is because we haven't ported it yet
* We'll bring in the existing implementation (hence we don't mark it as `Accepting PRs`)

### #306: Make `Process.Start` have a option to change handle inheritance

Status: **Not ready yet** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h33m12s)

* There is no good way to disable inhering handles on Linux
* We should use `System.IO.HandleInheritablility` enum if we expose this property.
* How does this interact with how this places with redirects and `UseShellExecute`?
    - `Process` already has a bunch of settings that don't work with Win32's ShellExecute or cross-plat;
    - We need to make sure that we can throw meaningful exceptions without making it impossible to do something simple that works everywhere

### #318: Proposal for `ImmutableInterlocked.ApplyChange` API

Status: **Accepting PRs** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h46m50s)

* Looks good, bug we don't like the name. We'll settle on the name when the PR is being reviewed.

### #373: `BitVector32` should implement `IEquatable<T>`

Status: **Not ready yet** <br>
[Video](http://channel9.msdn.com/Series/NET-Framework/NET-Core-API-Review-2015-01-14#time=1h49m50s)

* `BitVector32` is in `System.Collections.Specialized` which we consider a legacy component
    - We primarily added it make it easier for people to migrate existing code
    - We don't want to introduce new APIs as we don't want to encourage new usage
* The question is: is `BitVector32` a legacy API?
    - If it's not, then we should type-forward the type somewhere else, e.g. `System.Collections`
    - If it is, then we should reject the proposal and all future API additions
* API review board will need to follow-up

### Follow-ups for API review board

* **[@KrzysztofCwalina](https://github.com/KrzysztofCwalina)** wants to add some general purpose tests for collection interfaces to ensure they implement them correctly.
* Work on a design/proposal for allowing default parameters in public APIs.
* Work on a design/proposal for allowing CancelationToken for sync APIs.
* Work on a design/proposal for when we throw exceptions or not.
* `BitVector32` related questions
    - What are the differences between `BitArray` and `BitVector32`? Is one the replacement for the other?
    - What's the usage on these types?
