# Adapter for System.IO.Pipelines

Status: **Needs more work** | 
[API Ref](https://github.com/dotnet/corefx/issues/35765) |
[Video](https://www.youtube.com/watch?v=aaFcBs6cFqg)

## Notes

* The current proposal uses interfaces; which are very hard to version, even
  with Default Interface Methods (DIM) this might be hard due to the fact there
  is a hierarchy.
* The original proposal used abstract classes but external libraries weren't
  able to easily retrofit their existing types over ours; interfaces would
  easier to inject.
* Most libraries have their tensors baked by native memory. Given that tensors
  inherently have different shapes, most of them have a class hierarchy.
  However, some of the libraries use structs to keep the overhead on the managed
  side as low as possible. For that case, interface might be a better fit.
* `ITensor<T>.Dimensions` assumes that the lengths are conveniently in memory in
  a consecutive way. To make it more general, maybe we should make it a method,
  like array's `int GetLength(int dimension)`.
* The indexers on `ITensor<T>` will be relatively slow. Should we even offer
  them or should we force people to drop down to the underlying memory (i.e.
  handle `IDenseTensor<T>` or `ISparseTensor<T>`)?
* There are a lot of concerns with making these abstractions interfaces as it's
  basically impossible to evolve them. While the concept might be fixed, the
  mechanics aren't (for example, we're adding a different way to index using
  `Index` and `Range`). We should do more validation that these types have the
  right shape and that we really can't model them using classes.
* It would be good to have written guidance like:
    1. If you're ML.NET, how should you consume tensors
    2. If you're TensorFlow Sharp, how should you expose your tensors
* This would allow us to judge the API surface and see how valuable / suitable
  the shape of the exchange type really is.
