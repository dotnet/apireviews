# API Review 2015-11-20

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c1lurs510abbeeeqln1bm7nshj8).

## Overview

In this API review we took a look at the proposal for adding an array based
buffer pool.

## Notes

### Portable PDB

Status: **Approved with Feedback** |
[Proposal](https://github.com/dotnet/corefx/issues/4547) |
[Video](https://plus.google.com/events/c1lurs510abbeeeqln1bm7nshj8)

* The contract is that this API doesn't automatically pin the arrays -- user
  code would be responsible. The rationale is that many scenarios don't require
  pinning and we want to avoid unnecessary overhead.
* The `struct` constraint will go away. Ideally, we could say `where T: primitive`
  which would mean only primitive types (such as `int` or `byte`) and value
  types composed of only primitives, but CLR/C# don't offer that. Plus, we want
  to make sure ASP.NET can take advantage of this pool and they are today using
  arrays of reference types.
* We should bring back a static property that provides a shared pool. In fact,
  we should make that the only pool and make the .ctor private. The rationale
  is that we'd use an adaptive algorithm to decide how many buffers to maintain
  and what sizes to choose. We don't want a situation where ever component is
  creating their own pool because they don't have a convenient way to share
  without introducing inter-dependencies (e.g. JSON.NET and ASP.NET).
* The pool operations are thread-safe
* We should have a "no pooling mode" for debugging purposes. The idea is that
  in those cases renting will always allocate a new array and returning is a
  no-op. This would allow to quickly pin point whether issues in a larger system
  are related to pooling or not.
* Double returns can result in double rents (i.e. it poisons the free list). We
  need to think of a low-overhead way to avoid this.
* We shouldn't clear the buffers on rent -- it's just busy work with dubious
  value.
* Returning a `T[]` that holds onto huge object graphs is problematic, hence we
  should consider always clearing on return if `T` isn't a primitive type
* We should not make `ReturnBuffer` take a `ref`. The idea of `ref` was to
  force it to be nulled-out but all the bugs where a pool is returned and yet
  still being used are unlikely to be addressed by this API.
* We should rename `ManagedBufferPool<T>` --> `ArrayPool<T>`
* All methods should drop the `Buffer` suffix
* We aren't happy with `Rent` and `Return` but it seems fine for now