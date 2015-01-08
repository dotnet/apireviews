# API Review 2014-01-07

In this meeting, we reviewed these two PRs:

* [Adds FixedSizeBuilder type #154](https://github.com/dotnet/corefx/pull/154)
    - [API Reference](immutable-builder.md)
* [Add ExtractToImmutable #180](https://github.com/dotnet/corefx/pull/180)
    - [API Reference](immutable-extract.md)

These two PRs are essentially mutually exclusive.

## The Problem

The goal is to avoid the common case that creating an `ImmutableArray<T>` via a
builder causes multiple underlying arrays to be created.

In other words, the following code snippet should only create a single array:

```CSharp
var builder = ImmutableArray.CreateBuilder<int>(10);
for (int i = 0; i < builder.Count; i++)
{
    builder.Add(i);
}

ImmutableArray<int> array = builder.ToImmutable();
```

Today that's not the case. First, the builder doesn't actually store its
elements in a `T[]`. Instead, it uses a struct to avoid the covariance type
checks when writing to the array. This forces `ToImmutable()` to copy the
elements.

Another aspect is that calling `ToImmutable()` doesn't invalidate the builder.
In fact, it leaves all the data in the underlying builder and creates a new
array that is then used to construct the immutable array.

While that pattern can be convenient, it turns out that this has other problems.
In particular, when using a pooling for builders:

1. All builders will eventually get promoted to Gen2. The bigger the data is
   that they hold, the more time the GC needs to scan Gen2.

2. Unless the builders are manually cleared, the pool will keep the contents
   of the builder alive.

Thus, the preferred behavior would be to expose an API that allows

1. To construct an `ImmutableArray<T>` where the underlying array is created
   up-front and no additional buffers are needed.

2. The builders can easily be pooled if necessary without causing the problems
   above.

This naturally yields to a model where the ownership of the underlying array
is essentially transferred to `ImmutableArray<T>`.

## Options

We looked at two approaches:

1. [Adds FixedSizeBuilder type #154](https://github.com/dotnet/corefx/pull/154)
2. [Add ExtractToImmutable #180](https://github.com/dotnet/corefx/pull/180)

The first proposal adds a new builder. The second one just adds a new API on
the existing builder.

We decided to go down with the second approach. Reasons:

1. Using the existing builder with `Add()` methods is more convenient, even
   if the size is already known.
2. Fewer concepts / types / APIs.
3. Easier to migrate code between the two methods

## Open Issues

We need to define a name for this method. We don't like the current proposal
we lean toward `MoveToImmutable()` as it conveys the notion of transferring
ownership of the underlying array to `ImmutableArray<T>`.