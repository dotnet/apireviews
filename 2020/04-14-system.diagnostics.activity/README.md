# System.Diagnostics.Activity

**Approved with feedback** |
[Spec](https://github.com/dotnet/designs/pull/98) |
[API Ref](APIRef.md) |
[Video](https://youtu.be/a3xPdYi6jYU)

## Notes

* `ActivityKind`
  * Looks like `ActivityKind.Internal` is supposed to be the default which
    [Python defines][python_activity] as `0`. We should do the same.
  * Should we put these API in a namespace of `System.Diagnostics`? This
    namespace already contains many technologies.
* `ActivityContext`
  * We should name the parameters of `operator==` and `operator!=` as `left` and
    `right`
  * We should name the parameter of the strongly typed `Equals` as `value`
* `ActivityEvent`
  * Can we make this a `struct`?
* `ActivityLink`
  * We should name the parameters of `operator==` and `operator!=` as `left` and
    `right`
  * We should name the parameter of the strongly typed `Equals` as `value`
  * Should `IDictionary<string, string>` be a `IReadOnlyDictionary<string, string>`?
  * Should the `attributes` parameter be optional and defaulted to `null`?
* `ActivitySource`
  * ~~`StartActivity()` seems to take a lot of arguments that seem quite heavy;
    even though we're returning `null` when we don't have any listeners, we're
    still potentially paying some significant overhead.~~ It was clarified that
    they can be added after the activity got returned. This allows the caller to
    check for `null` and only do expensive data collection then.
  * `StartActivity` takes an `IEnumerable<KeyValuePair<string, string>>`. Should
    this be a `Dictionary<string, string>`? The discussion seems to indicate
    that we need to preserve the order; if so, then `Dictionary<string, string>`
    would be the wrong choice. However, then we should consider having a
    dedicated type like `ActivityTagCollection` that would do that, otherwise
    users will likely construct a dictionary.
  * The callback passed to `AddListener` takes a lot of arguments. For regular
    methods this isn't a big problem because if we need more arguments in the
    future, we can just add an overload, but this doesn't work well for
    callbacks. We should collapse the arguments into an `EventArgs` like type.
    If allocations are a concern, we can use a struct. We should also consider
    extracting the callback as a named delegate type instead of using the
    generic func to avoid nested generics. This would also allow us to pass the
    the event args struct by reference.
* `Activity`
  * It should implement the `Dispose` pattern, i.e. add `protected virtual void
    Dispose(bool disposing)` and `Dispose()` should call `Dispose(true)`.
  * `SetCustomProperty` and `GetCustomProperty` have no way to differentiate
    between a property is set to `null` vs. a property wasn't set at all. Is
    this OK?
  * Why is `Source` required and should it be nullable? It's not nullable
    because there is global singleton that is the default.

[python_activity]: https://github.com/open-telemetry/opentelemetry-python/blob/c8b336d338289a3fed761053e917398f3bf6fdd7/opentelemetry-api/src/opentelemetry/trace/__init__.py#L144