# Quick Reviews 6/18/2019

## Allow X509Chain to replace the root trust list for a single call

**Approved** | [#corefx/16364](https://github.com/dotnet/corefx/issues/16364#issuecomment-503226830) | [Video](https://www.youtube.com/watch?v=56yfsgHtJ1E&t=0h0m0s)

Looks good as proposed.
## Expose top-level nullability information from reflection

**Needs Work** | [#corefx/38087](https://github.com/dotnet/corefx/issues/38087#issuecomment-503265497) | [Video](https://www.youtube.com/watch?v=56yfsgHtJ1E&t=0h7m8s)

* Given that we don't have all the features in-place, we believe this feature doesn't make the cut for 3.0
* What about the custom attributes, such as `NullIfNotNullAttribute`?
* What about generic type parameters (e.g. for tuples)
* Namespace choice: should this be in the `System.Reflection` namespace or should this go into a nested namespace?
* There is the general question whether MVC or EF should even handle nullable annotations.
* How do we square making runtime decisions with being able to change annotations
* We should consider generalizing the feature to handle `dynamic` and tuple names.

## Support for custom converters and OnXXX callbacks

**Approved** | [#corefx/36639](https://github.com/dotnet/corefx/issues/36639#issuecomment-503265607) | [Video](https://www.youtube.com/watch?v=56yfsgHtJ1E&t=1h50m4s)

Notes from toady:

* `JsonConverterAttribute`: let's make `CreateConverter` public
* `JsonConverter<T>.Read/Write`: We shouldn't catch specific types, instead, we should catch exceptions whose `Source` indicate that it originates from the reader/writer. The reader/writer only marks exception types that it expects the serializer to catch, for example, it would not mark argument exception because those should go unhandled. We should, however, still special case `JsonException` so that implementers of JSON converters don't have to set the source.
* `JsonConverterAttribute` we should add `protected JsonConverterAttribute()` constructor that can be used by derived types.

