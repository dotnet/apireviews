# API Review 2015-07-08

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cvu59jk75ff9cv6sfb4h6cte7mo).

## Overview

In this API review we reviewed the following PRs and proposals:

* ILDASM. API to produce ILDASM like text from a given assembly
* System.Drawing.Primitives. Proposal for exposing a consistent set of APIs
  for points and sizes that is independent from a given UI stack.

## Notes

Status: **No ready yet** |
[API Reference](ILDasm.md) |
[Video](https://youtu.be/HuRc6CpiOVg)

### ILDASM

* Object model API on top of `System.Reflection.Metadata` to produce IL
* You can get a `string` at any level in the tree
  - So you can textualize pieces or the entire assembly
* Ideally, this library can also work off in-memory representations
  - For example Roslyn could use this in tests
  - It seems it's fine if this requires the entire assembly (due to other pieces
    of information, such as tokens)
* The larger question is: is this library using the MD reader as an
  implementation
  - Both are possible needs to be closed on
* Should ILDasm expose modules?
* We need to think about the names -- we're likely creating conflicts with
  - Should we prefix the generic types with `IL`
* Can we support PDB information such as sequence points?
  - Would be useful to get an interspersed output of IL and actual source (C#)
* We decouple the concept of walking and dumping
  - Probably the `Dump` methods should be on a single type that acts as a
    visitor
* We should probably have a layer of ILDasm that doesn't allocate
* We should rename `Dump` to `WriteTo`
* The `Dump` methods should probably take a `TextWriter` to avoid many string
  allocations
* For the options we should probably expose some sort of settings object to be
  passed around
* `IDasmAssembly` lazily creates the contents -- there are life time issues
  as neither ILDasm nor ILDasmAssembly are disposable
  - It may be easier to let the caller pass in the MDReader or the PEReader
* `ILDasmType` and `ILDasmTypeDefinition` need to be worked on -- it seems may
  want to unify them
* Do we want expose the raw tables as well? The ILDASM tool today does support
  that

### System.Drawing.Primitives

Status: **Approved** |
[API Reference](Drawing.md) |
[Video](https://youtu.be/HuRc6CpiOVg?t=3477)

* Many folks are using drawing APIs outside of drawing, such as banking
* We [have a GitHub issue](https://github.com/dotnet/corefx/issues/1563) that
  discussed the shape, we concluded that they look similar to `System.Drawing`
* We didn't include `Color` as it doesn't seem to be sufficient -- there will
  be another GitHub issue to track that work