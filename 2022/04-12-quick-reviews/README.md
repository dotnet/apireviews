# API Review 04/12/2022

## System.Runtime.CompilerServices.CompilerFeatureRequiredAttribute

**Approved** | [#runtime/66167](https://github.com/dotnet/runtime/issues/66167#issuecomment-1097002415) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=0h0m0s)

* The `Language` property is meant to express optionality, we should just make it a bool that is `false` by default

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.All, AllowMultiple = true, Inherited = false)]
    public sealed class CompilerFeatureRequiredAttribute : Attribute
    {
        public CompilerFeatureRequiredAttribute(string featureName)
        {
            FeatureName = featureName;
        }

        public string FeatureName { get; }

        public bool IsOptional { get; init; }

        public const string RefStructs = nameof(RefStructs);

        public const string RequiredMembers = nameof(RequiredMembers);
    }
}
```
## Api handle Activity.Current value changes

**Approved** | [#runtime/67276](https://github.com/dotnet/runtime/issues/67276#issuecomment-1097020685) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=0h29m39s)

* Makes sense, but we should rename the struct and make the properties `init`

```C#
namespace System.Diagnostics
{
    public readonly struct ActivityChangedEventArgs
    {
        public Activity? Previous { get; init; }
        public Activity? Current { get; init; }
    }

    public partial class Activity : IDisposable
    {
        public static event EventHandler<ActivityChangedEventArgs>? CurrentChanged;
    }
}
```
## Add API IndexNotOf 

**Approved** | [#runtime/28795](https://github.com/dotnet/runtime/issues/28795#issuecomment-1097040761) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=0h48m3s)

* We'd prefer a less-Yoda like name such as `IndexOfAnyExcept` which would also allow us to fold the single case into the multiple case.

```C#
namespace System;

public static partial class MemoryExtensions
{
    int IndexOfAnyExcept(this Span<T> span, T value) where T : IEquatable<T>;
    int IndexOfAnyExcept(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
    int IndexOfAnyExcept(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    int IndexOfAnyExcept(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;

    int IndexOfAnyExcept(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
    int IndexOfAnyExcept(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
    int IndexOfAnyExcept(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    int IndexOfAnyExcept(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;

    int LastIndexOfAnyExcept(this Span<T> span, T value) where T : IEquatable<T>;
    int LastIndexOfAnyExcept(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
    int LastIndexOfAnyExcept(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    int LastIndexOfAnyExcept(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;

    int LastIndexOfAnyExcept(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
    int LastIndexOfAnyExcept(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
    int LastIndexOfAnyExcept(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    int LastIndexOfAnyExcept(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
}
```
## Add ConditionalWeakTable.TryAdd API

**Approved** | [#runtime/29368](https://github.com/dotnet/runtime/issues/29368#issuecomment-1097047393) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=1h11m29s)

* Looks good as proposed

```C#
namespace System.Runtime.CompilerServices;

public sealed class ConditionalWeakTable<TKey, TValue>
{
    public bool TryAdd(TKey key, TValue value);
}
```
## Add JsonTypeInfo overloads to System.Memory.Data

**Approved** | [#runtime/54979](https://github.com/dotnet/runtime/issues/54979#issuecomment-1097054646) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=1h18m52s)

* Looks good as proposed

```C#
namespace System;

public partial class BinaryData
{
    // Existing APIs
    // public BinaryData(object? jsonSerializable, JsonSerializerOptions? options = null, Type? type = null);
    // public static BinaryData FromObjectAsJson<T>(T jsonSerializable, JsonSerializerOptions? options = null);
    // public T? ToObjectFromJson<T>(JsonSerializerOptions? options = null);

    // New APIs
    public BinaryData(object? jsonSerializable, JsonSerializerContext context, Type? type = null);
    public static BinaryData FromObjectAsJson<T>(T jsonSerializable, JsonTypeInfo<T> jsonTypeInfo);
    public T? ToObjectFromJson<T>(JsonTypeInfo<T> jsonTypeInfo);
}
```
## MetadataReader.GetAssemblyName(string assemblyFile)

**Approved** | [#runtime/63960](https://github.com/dotnet/runtime/issues/63960#issuecomment-1097069934) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=1h26m24s)

* Looks good as proposed
    - Assuming @buyaa-n is also fine with this shape.

```C#
namespace System.Reflection.Metadata;

public sealed partial class MetadataReader
{
    public static AssemblyName GetAssemblyName(string assemblyFile);
}
```

## MemoryExtensions.CommonPrefixLength<T>

**Approved** | [#runtime/64271](https://github.com/dotnet/runtime/issues/64271#issuecomment-1097103048) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=1h37m19s)

* Looks good as proposed
    - Need overloads so that it shows up on instances of `Span<T>`
    - We should constrain the first one to `IEquatable<T>`
    - We might need to add an overload that is specific to `char` and takes `StringComparison`

```C#
namespace System;

public static class MemoryExtensions
{
    public static int CommonPrefixLength<T>(this Span<T> span, ReadOnlySpan<T> other) where T: IEquatable<T>;
    public static int CommonPrefixLength<T>(this Span<T> span, ReadOnlySpan<T> other, IEqualityComparer<T>? comparer = null);

    public static int CommonPrefixLength<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other) where T: IEquatable<T>;
    public static int CommonPrefixLength<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other, IEqualityComparer<T>? comparer = null);
}
```

## Expose xarch serialize instruction.

**Approved** | [#runtime/66467](https://github.com/dotnet/runtime/issues/66467#issuecomment-1097108305) | [Video](https://www.youtube.com/watch?v=gArwWQDPGnU&t=1h54m37s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

[Intrinsic]
[CLSCompliant(false)]
public abstract class X86Serialize : X86Base
{
    public static new bool IsSupported { get; }

    [Intrinsic]
    public new abstract class X64 : X86Base.X64
    {
        public static new bool IsSupported { get; }
    }

    public static void Serialize();
}
```

