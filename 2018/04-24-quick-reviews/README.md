# Quick Reviews 4/24/2018

## SignedCms extensibility to support external private keys

**Approved** | [#corefx/26406](https://github.com/dotnet/corefx/issues/26406#issuecomment-384009048) | [Video](https://www.youtube.com/watch?v=E9X-kNjBtG8&t=0h-3m-55s)

Looks good, any reason we wouldn't expose a matching property? @bartonjs suggested to use the name `privateKey` instead of just `key`, which seems reasonable to me too.
## Add FileSystem.Watcher.FilterList Property

**Approved** | [#corefx/29273](https://github.com/dotnet/corefx/issues/29273#issuecomment-384014580) | [Video](https://www.youtube.com/watch?v=E9X-kNjBtG8&t=0h5m10s)

Looks a good. A few points:

* We should just use a plural form instead of the `List` suffix, e.g. `Filters`.
* The property shouldn't be settable as this avoids problems when the watcher needs to use a derived type in the implementation.
* The existing API `Filter` will return the first item in `Filters`. If set, it will clear `Filters` and add the one item.
* Adding a constructor that takes path and filters would be a source breaking change if people call it with a null value for the second argument (because we have an existing one that takes string). However, we could add a longer version that would also accept, say, the matcher type. But that would be a separate API review.
## Public setter for System.Diagnostics.Activity.Current

**Approved** | [#corefx/29207](https://github.com/dotnet/corefx/issues/29207) | [Video](https://www.youtube.com/watch?v=E9X-kNjBtG8&t=0h22m43s)

## Consider Span<char> overloads on Regex classes

**Needs Work** | [#corefx/24145](https://github.com/dotnet/corefx/issues/24145) | [Video](https://www.youtube.com/watch?v=E9X-kNjBtG8&t=0h32m59s)

## New Zip LINQ Tuple Overload API

**Approved** | [#corefx/16011](https://github.com/dotnet/corefx/issues/16011#issuecomment-384022447) | [Video](https://www.youtube.com/watch?v=E9X-kNjBtG8&t=0h34m7s)

❤️ 
