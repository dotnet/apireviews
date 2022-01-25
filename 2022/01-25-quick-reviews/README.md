# API Review 01/25/2022

## Add `ref` field feature support to `RuntimeFeature` class

**Approved** | [#runtime/64165](https://github.com/dotnet/runtime/issues/64165#issuecomment-1021471976) | [Video](https://www.youtube.com/watch?v=2rlbyoSZF3k&t=0h0m0s)

* Looks good as proposed
* We should make sure that other runtimes, such as Unity, that features listed in the `RuntimeFeature` are required
    - Practically, if another runtime pulls in our library code they will likely find out, given that we usually use those features

```C#
namespace System.Runtime.CompilerServices
{
    public static partial class RuntimeFeature
    {
        public const string ByRefFields = nameof(ByRefFields);
    }
}
```
## Regex.Count

**Approved** | [#runtime/61425](https://github.com/dotnet/runtime/issues/61425#issuecomment-1021481038) | [Video](https://www.youtube.com/watch?v=2rlbyoSZF3k&t=0h10m39s)

* Looks good as proposed
* We should consider adding an analyzer the looks for code that does `Regex.Count(...) > 0` and suggests `Regex.IsMatch(...)`

```C#
namespace System.Text.RegularExpressions
{
    public partial class Regex
    {
        public int Count(string input);
        public static int Count(string input, string pattern);
        public static int Count(string input, string pattern, RegexOptions options);
        public static int Count(string input, string pattern, RegexOptions options, TimeSpan matchTimeout);

        // And once https://github.com/dotnet/runtime/issues/59629 is approved and we support spans
        public int Count(ReadOnlySpan<char> input);
        public static int Count(ReadOnlySpan<char> input, string pattern);
        public static int Count(ReadOnlySpan<char> input, string pattern, RegexOptions options);
        public static int Count(ReadOnlySpan<char> input, string pattern, RegexOptions options, TimeSpan matchTimeout);
    }
}
```
## RequiredMemberAttribute and NoRequiredMembersAttribute

**Approved** | [#runtime/64248](https://github.com/dotnet/runtime/issues/64248#issuecomment-1021504089) | [Video](https://www.youtube.com/watch?v=2rlbyoSZF3k&t=0h21m29s)

* We'd prefer the name `SatisfiesRequiredMembersAttribute`
    - This gives us the extensibility by adding constructors/properties to allow specifying which members will be initialized/not initialized later, if the language wants to go there.
* If we want serializers to throw an exception unless all members are specified, we should consider exposing a reflection API

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Class |
                    AttributeTargets.Struct |
                    AttributeTargets.Field |
                    AttributeTargets.Property,
                    AllowMultiple=false,
                    Inherited=false)]
    public sealed class RequiredMemberAttribute : Attribute
    {
        public RequiredMemberAttribute();
    }
}
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.Constructor,
                    AllowMultiple=false,
                    Inherited=false)]
    public sealed class SetsRequiredMembersAttribute : Attribute
    {
        public SetsRequiredMembersAttribute();
    }
}
```
## Support Metrics UpDownCounter instrument

**Approved** | [#runtime/63648](https://github.com/dotnet/runtime/issues/63648#issuecomment-1021529630) | [Video](https://www.youtube.com/watch?v=2rlbyoSZF3k&t=0h50m35s)

* We considered using generic math to constrain the `T`, to, for example, `INumber` but the list of types are taken from the OpenTelemetry spec
* > `UpDownCounter` and `ObservableUpDownCounter` support only the following generic parameter types: `Byte`, `Int16`, `Int32`, `Int64`, `Single`, `Double`, and `Decimal`.
    - Should we consider supporting unsigned types?
    - Should we drop `byte`?
    - We should strive for consistency with what we shipped before for `Counter` (if we support `byte` there, we should do it here too)
    - We can consider adding support for unsigned types later too

```C#
namespace System.Diagnostics.Metrics
{
    public sealed class UpDownCounter<T> : Instrument<T> where T : struct
    {
        public void Add(T delta);
        public void Add(T delta, KeyValuePair<string, object?> tag);
        public void Add(T delta, KeyValuePair<string, object?> tag1, KeyValuePair<string, object?> tag2);
        public void Add(T delta, KeyValuePair<string, object?> tag1, KeyValuePair<string, object?> tag2, KeyValuePair<string, object?> tag3);
        public void Add(T delta, ReadOnlySpan<KeyValuePair<string, object?>> tags);
        public void Add(T delta, params KeyValuePair<string, object?>[] tags);
        public void Add(T delta, in TagList tagList);
    }
    public sealed class ObservableUpDownCounter<T> : ObservableInstrument<T> where T : struct
    {
        protected override IEnumerable<Measurement<T>> Observe();
    }
    public partial class Meter : IDisposable
    {
        public UpDownCounter<T> CreateUpDownCounter<T>(string name, string? unit = null, string? description = null) where T : struct ;
        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<T> observeValue,
                            string? unit = null,
                            string? description = null) where T : struct;
        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<Measurement<T>> observeValue,
                            string? unit = null,
                            string? description = null) where T : struct;
        public ObservableUpDownCounter<T> CreateObservableUpDownCounter<T>(
                            string name,
                            Func<IEnumerable<Measurement<T>>> observeValues,
                            string? unit = null,
                            string? description = null) where T : struct;
    }
}
```

## Add Path.Exists -- to replace inefficient FileExists || DirectoryExists

**Approved** | [#runtime/21678](https://github.com/dotnet/runtime/issues/21678#issuecomment-1021543111) | [Video](https://www.youtube.com/watch?v=2rlbyoSZF3k&t=1h21m32s)

* We considered returning whether it was a file or directory but
    - It seems rarely useful
    - It's more expensive in case of symlinks

```C#
namespace System.IO
{
    public static partial class Path
    {
        // Implementation is logically equivalent to File.Exists(path) || Directory.Exists(path)
        public static bool Exists([NotNullWhenAttribute(true)] string? path);
    }
}
```

## Analyzer for `System.Runtime.CompilerServices.DisableRuntimeMarshallingAttribute`

**Approved** | [#runtime/63714](https://github.com/dotnet/runtime/issues/63714#issuecomment-1021551711) | [Video](https://www.youtube.com/watch?v=2rlbyoSZF3k&t=1h37m56s)

**Category**: Interoperability

**Severity:**

* The first half should be `Warning`
    * > `SetLastError` is set to `true` on a P/Invoke
    * > `[LCIDConversionAttribute]` is used on a P/Invoke
    * > A type that is not a C#-unmanaged type is in a P/Invoke signature or the signature of a delegate with the `[UnmanagedFunctionPointer]` attribute.
    * > A C#-unmanaged type that has any fields with `[StructLayout(LayoutKind.Auto)]` that is in a P/Invoke signature or the signature of a delegate with the `[UnmanagedFunctionPointer]` attribute.
* The second half should be `IDE Suggestion`:
    * > When `Marshal.SizeOf` is used on an unmanaged type. (We could also include a code-fix here to recommend switching to `Unsafe.SizeOf` or the `sizeof` keyword)
    * > When `Marshal.OffsetOf` is used.
    * > When `Marshal.StructureToPtr` or `Marshal.PtrToStructure` is used. (We can include code-fixes here to use "unsafe" code or other unsafe APIs).

