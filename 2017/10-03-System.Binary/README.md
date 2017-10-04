# System.Binary

Status: **Needs more work**
| [Issue](https://github.com/dotnet/corefx/issues/24144 )
| [API Ref](System.Binary.md)
| [Video](https://www.youtube.com/watch?v=m4BUM3nJZRw&t=404s)

## Notes

* Extension methods
    - If we want to expose them as extension methods we need to have both
      overloads for `ReadOnlySpan<T>` and `Span<T>`
    - Let's do non-extension methods for now; we can make them extension methods
      later.
        + But let's leave the type name to be `BinaryPrimitives` (as opposed to
          `BufferExtensions`)
        + But we should still put them in a separate namespace so that making
          them extension methods later, developers can decide whether they get
          these extensions by importing/not importing the namespace
* Endianness
    - Is there any point in having both little & big endian? Default seems to be
      little endian
        + Yes, we need to support parsing from the networking, which is
          generally big endian. We could remove `LittleEndian` by making it the
          default, given that all machines are basically little endian, but that
          seems weird if we ever have to support a big endian machine. Seems
          it's better to be consistent and explicit.
    - Can we collapse the endianness methods into overloads with making it
      argument?
        + No, because we'd probably lose some performance
* `Reverse`
    - The proposed (generic) API we have for reversing bytes doesn't work
      because endianness has to be done at the granularity of smaller types
      (int, long) etc. Hence, we removed it.
    - `Reverse(byte)` seems weird. Was only added to allow code to be converted
      with forcing folks to add/remove `Reverse()` calls.
    - Should we rename `Reverse` to `ReverseEndianness`?
* `Read<T>`/`Write<T>`
    - Can we remove the specialized reader/writer methods in favor of the
      generic `Read<T>`/`Write<T>` method?
        + No, because the generic method cannot handle endianness.
    - `Read<T>`/`Write<T>` should be renamed so that the name indicates it uses
      machine endianness
    - Should `Read<T>` return a `ref T`?
        + No, because that wouldn't guarantee any alignment. Returning a `T`
          guarantees that it's aligned because we always align the stack
    - Should `Write<T>` take a `readonly ref T`?
        + Probably yes
    - Should we rename `ReadXxx` and `WriteXxx` to `GetXxx`/`SetXxx`? The name
      seems to imply an updated cursor.
        + No, because that name would feel weird when called as a static.
        + Let's leave them as `ReadXxx` and `WriteXxx`

## Namespace

The family of `Span<T>` features is across these namespaces:

* `System`
    - `Span<T>`
    - `Memory<T>`
* `System.Buffers`
    - `OwnedMemory<T>`
    - Lifetime managers
* `System.Buffers.Text`
    - Formatting & parsing
* `System.Buffers.Binary`
    - The type discussed here
    - `BinaryPrimitives`

Thus, using `System.Buffers.Binary` feels very consistent.

## Assemblies

The family of `Span<T>` exists across these assemblies:

1. `System.Memory`
    - OOB `Span<T>`
    - Later type forwarded to the core library where span is in-box
2. `System.Buffers`
    - Already contains the existing V1 `ArrayPool`

(1) has to be in-box
(2) doesn't have to be, but might due to `ArrayPool`. In fact, on .NET Core
`ArrayPool` lives in `System.Private.Corelib`

For now, we've decided to put `BinaryPrimitives` into `System.Buffers`.