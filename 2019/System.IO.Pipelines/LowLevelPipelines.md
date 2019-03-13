# Making pipelines a low-level IO primitive

## Problem

We're currently not using pipelines like we do `Stream`, i.e. there is no
exchange type that APIs will accept. Instead, the idea is that those will accept
`Stream` and that there is an adapter from `IPipeReader` and `IPipeWriter` to
`Stream`.

The downside is that there is some overhead that is intrinsic to the adapter
between `Stream` and pipelines because streams don't manage buffers -- you need
to pass a buffer in. That causes copies (i.e. CPU time) and some amount of
allocations (although those can likely be amortized by e.g. using a pool).

Right now, there is a desire to make the JSON reader/writer APIs directly take
a `IPipeReader` and `IPipeWriter` to avoid this overhead. However, this means that
the JSON APIs have to depend on pipelines, thus begging the question if pipelines
should be considered a suitable low-level IO primitive.

## Option 1 - Declare it good

We can just declare pipelines a low-level IO primitive. It doesn't depend on
anything but corlib so that would probably work for most scenarios. However, it
wouldn't work for APIs that are inside of corlib. While we could in principle
move pipelines into corlib we really don't want to because it's about 200 KB
worth of IL.

## Option 2 - Provide a new exchange type

Introduce an abstraction that pipelines could be implement. This abstraction
could live next to `Stream` in corlib and serve as the exchange type for all
APIs that would like to special case pipeline-like APIs for performance reasons.
The downside is that we potentially fork the ecosystem and it might take a long
time for it before enough parties explicitly add support for it.

## Option 3 - Make Stream aware of pipelines

We could extend `Stream` with some virtual(s) that allow consumers to benefit
from streams that are baked by something that already has a buffer without
forcing consumers to call different APIs.

## Next steps

* Fowler/Pavel: propose API shape for `AsyncBufferedReader<T>` and
  `AsyncBufferedWriter<T>`
* Clarify how to handle sync. We might want to add synchronous APIs to the new
  `AsyncBufferedReader<T>` and `AsyncBufferedWriter<T>` types.