# API Review 08/06/2024

## TypeName.MakeSimpleTypeName(AssemblyNameInfo) to avoid type name reparsing

**Approved** | [#runtime/106022](https://github.com/dotnet/runtime/issues/106022#issuecomment-2271766049) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Reflection.Metadata;

public partial class TypeName
{
    public TypeName MakeSimpleTypeName(AssemblyNameInfo? assemblyInfo);
}
```
## Add `JsonElement.GetPropertyCount()`

**Approved** | [#runtime/104692](https://github.com/dotnet/runtime/issues/104692#issuecomment-2271776173) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=0h6m45s)

* Looks good as proposed

```C#
namespace System.Text.Json;

public partial struct JsonElement
{
    public int GetPropertyCount();
}
```
## Implement IEnumerator<T> on ref struct enumerators

**Approved** | [#runtime/105276](https://github.com/dotnet/runtime/issues/105276#issuecomment-2271791573) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=0h11m36s)

* Looks good as proposed

```C#
namespace System
{
    public partial static class MemoryExtensions
    {
        public partial ref struct SpanSplitEnumerator<T> : IEnumerator<Range>
        {
        }
    }
    public readonly ref struct ReadOnlySpan<T>
    {
        public partial ref struct Enumerator : IEnumerator<T>
        {
        }
    }
    public readonly ref struct Span<T>
    {
        public partial ref struct Enumerator : IEnumerator<T>
        {
        }
    }
}
namespace System.Text
{
    public partial ref struct SpanLineEnumerator : IEnumerator<ReadOnlySpan<char>>
    {
    }
    public partial ref struct SpanRuneEnumerator : IEnumerator<Rune>
    {
    }
}
namespace System.Numerics.Tensors
{
    public readonly ref struct ReadOnlyTensorSpan<T>
    {
        public partial ref struct Enumerator : IEnumerator<T>
        {
        }
    }
    public readonly ref struct TensorSpan<T>
    {
        public partial ref struct Enumerator : IEnumerator<T>
        {
        }
    }
}
namespace System.Text.RegularExpressions
{
    public partial ref struct ValueMatchEnumerator : IEnumerator<ValueMatch>
    {
    }
    public partial ref struct ValueSplitEnumerator : IEnumerator<Range>
    {
    }
}
```
## MemoryMappedViewAccessor: Dispose flushes on temporary files

**NeedsWork** | [#runtime/94383](https://github.com/dotnet/runtime/issues/94383#issuecomment-2271817141) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=0h20m39s)

* Instead of exposing the new parameter, could we handle this automatically?
* If we expose it, should `size` and `access` be defaulted?
    - If so, we should hide the existing second and third overload
    - We should keep the first for simplicity

```C#
namespace System.IO.MemoryMappedFiles;

public partial class MemoryMappedFile
{
    // Existing overloads:
    // public MemoryMappedViewAccessor CreateViewAccessor()
    // public MemoryMappedViewAccessor CreateViewAccessor(long offset, long size)
    // public MemoryMappedViewAccessor CreateViewAccessor(long offset, long size, MemoryMappedFileAccess access);
    public MemoryMappedViewAccessor CreateViewAccessor(long offset, long size, MemoryMappedFileAccess access, bool flushOnDispose = true);
}
```
## Generic Enumerable.Range method

**Approved** | [#runtime/97689](https://github.com/dotnet/runtime/issues/97689#issuecomment-2271862567) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=0h34m38s)

* Might result in a source breaking change for cases where the first parameter isn't typed as `int` but implicitly convert to `int`, such as `char` and `short`. With this API, the return value changes from `IEnumerable<int>` to `IEnumerable<TypeOfFirstParameter>`
    - We believe it's an acceptable source breaking change
* `Enumerable.Range` with a step is also interesting but has nuance that would need further discussion
    - As proposed it's not as expressive as `Range(T start, int count, T step)` would be

```C#
public static class Enumerable
{
    public static IEnumerable<T> Range<T>(T start, int count) where T : IBinaryInteger<T>
}
```
## Add an `ISet<T>.AsReadOnly()` extension method

**Approved** | [#runtime/29387](https://github.com/dotnet/runtime/issues/29387#issuecomment-2271868090) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=1h0m39s)

* Looks good as proposed

```C#
namespace System.Collections.Generic;

public partial class CollectionExtensions
{
    // Existing:
    // public ReadOnlyCollection<T> AsReadOnly(this IList<T> list);
    // public ReadOnlyDictionary<T> AsReadOnly(this IDictionary<T> dictionary);
    public ReadOnlySet<T> AsReadOnly(this ISet<T> set);
}
```
## BinaryReader.ReadExactly

**Approved** | [#runtime/101614](https://github.com/dotnet/runtime/issues/101614#issuecomment-2271879172) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=1h3m59s)

* Looks good as proposed

```C#
namespace System.IO;

public partial class BinaryReader
{
    public virtual ReadExactly(Span<byte> buffer);
}
```
## Unify whether throw helpers are [StackTraceHidden] or not

**Approved** | [#runtime/90539](https://github.com/dotnet/runtime/issues/90539#issuecomment-2271915089) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=1h10m14s)

* The primary value of making it `[StackTraceHidden]` is that the debugger breaks at the call side for unhandled exception, rather than in the body of the throw helper
    - However, this is also the behavior of "Just My Code", which most people have turned on
    - Let's be consistent by not marking any throw helpers
    - We should probably remove it from internal throw helpers too

```C#
namespace System;

public partial class ObjectDisposedException : InvalidOperationException
{
    // Remove: [StackTraceHidden]
    public static void ThrowIf([DoesNotReturnIf(true)] bool condition, object! instance);

    // Remove: [StackTraceHidden]
    public static void ThrowIf([DoesNotReturnIf(true)] bool condition, Type! type);
}
```
## `ActivatorUtilities.CreateFactory<T>` with params span

**NeedsWork** | [#runtime/101889](https://github.com/dotnet/runtime/issues/101889#issuecomment-2271924770) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=1h30m26s)

* It seems to odd to have an API that avoids the array allocation, given how expensive the call is
* The alternative design makes more sense to use, at least the returned delegate is strongly typed to the arguments
    - We'd probably prefer `Func<...>` over creating 15 new delegate types

```C#
namespace Microsoft.Extensions.DependencyInjection;

public delegate T ObjectFactoryWithParams<out T>(IServiceProvider serviceProvider, params ReadOnlySpan<object?> arguments);

public static class ActivatorUtilities
{
    public static ObjectFactoryWithParams<T> CreateFactory2<T>(params ReadOnlySpan<Type> argumentTypes);
}
```
## Mark the Form.OnClosed and OnClosing methods with the ObsoleteAttribute

**Approved** | [#winforms/11727](https://github.com/dotnet/winforms/issues/11727) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=1h36m18s)

## DateOnly support in IsoWeek

**Approved** | [#runtime/101597](https://github.com/dotnet/runtime/issues/101597#issuecomment-2271935631) | [Video](https://www.youtube.com/watch?v=oxcEHoh23Rw&t=1h40m48s)

* Looks good as proposed

```C#
namespace System.Globalization;

public partial static class ISOWeek
{
    // Existing:
    // public static int GetWeekOfYear(DateTime date);
    // public static int GetYear(DateTime date);
    // public static DateTime ToDateTime(int year, int week, DayOfWeek dayOfWeek);

    public static int GetWeekOfYear(DateOnly date);
    public static int GetYear(DateOnly date);
    public static DateOnly ToDateOnly(int year, int week, DayOfWeek dayOfWeek);
}
```
