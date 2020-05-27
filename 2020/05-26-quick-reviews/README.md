# Quick Reviews 5/26/2020

## RequireMethodImplToRemainInEffectAttribute

**Needs Work** | [#runtime/35315](https://github.com/dotnet/runtime/issues/35315#issuecomment-634163634) | [Video](https://www.youtube.com/watch?v=I7whBbUrMcc&t=0h0m0s)

A few questions:

* We're not entirely sure what this attribute does:
	- The issue talks about covariant returns but all samples use `void`
	- None of the methdos use `virtual` functions
	- What are the behavior differences of with and without the attribute? If this can't be expressed in C#, then ILDASM code would help too
* `RequireMethodImplToRemainInEffectAttribute` this name seems a bit abstract. Can make this a bit clearer, like `PreserveBaseOverridesAttribute`?
* Does it have to be a new attribute or could be a new flag on `MethodImplAttributes`?
* How does this attribute play with default interface methods?
* How does this play with versioning where overrides are added to the middle of the hierarchy later?
## Add activity Ids and Context to all logs 

**Approved** | [#runtime/34305](https://github.com/dotnet/runtime/issues/34305#issuecomment-634181323) | [Video](https://www.youtube.com/watch?v=I7whBbUrMcc&t=0h21m10s)

* We should remove `ActivityTrackingOptions.Default` because we can't change its value ever. Instead, we should just set the value in the `LoggerFactoryOptions` contructor
* We should make sure that default parameters in constructors are part of the spec that 3rd party DI containers have to support

```C#
namespace Microsoft.Extensions.Logging
{
    [Flags]
    public enum ActivityTrackingOptions
    {
        None        = 0x0000,
        SpanId      = 0x0001,
        TraceId     = 0x0002,
        ParentId    = 0x0004,
        TraceState  = 0x0008,
        TraceFlags  = 0x0010
    }
    public class LoggerFactoryOptions
    {
        public LoggerFactoryOptions();
        public ActivityTrackingOptions ActivityTrackingOptions  { get; set; }
    }
    public partial class LoggerFactory
    {
        public LoggerFactory(IEnumerable<ILoggerProvider> providers,
                             IOptionsMonitor<LoggerFilterOptions> filterOption,
                             IOptions<LoggerFactoryOptions> options = null);
    }
    public static partial class LoggingBuilderExtensions
    {
        public static ILoggingBuilder Configure(this ILoggingBuilder builder,
                                                Action<LoggerFactoryOptions> action);
    }
}
```
## Add a GetEncodings method to System.Text.EncodingProvider to support enumerating available character encodings

**Approved** | [#runtime/25819](https://github.com/dotnet/runtime/issues/25819#issuecomment-634199380) | [Video](https://www.youtube.com/watch?v=I7whBbUrMcc&t=0h54m58s)

* `EncodingProvider.GetEncodings()` should be `IEnumerable<EncodingInfo>`
* `EncodingProvider.GetEncodings()` should be virtual and return an `Enumerable.Empty<EncodingInfo>()` (instead of throwing). While this isn't correct it means that one provider that isn't updated doesn't spoil the enumeration for everyone else.
* `Encoding.GetEncodings()` should return all registered encodings across all providers and de-dupe them if necessary.
* `EncodingInfo.GetEncoding()` calls the static `Encoding.GetEncoding()` method, which means getting encoding infos and using `EncodingInfo.GetEncoding()` might create an encoding that isn't tied to that encoding provider.
    - `EncodingInfo` should take an `EncodingProvider` that we'll use in `EncodingInfo.GetEncoding()` and call `EncodingProvider.GetEncoding(codePage)`.

```C#
namespace System.Text
{
    public partial class EncodingProvider
    {
        public virtual IEnumerable<EncodingInfo> GetEncodings();
    }
    
    public partial class EncodingInfo
    {
        public EncodingInfo(EncodingProvider provider, int codePage, string name, string displayName);
    }
}
```


## StringSplitOptions.TrimEntries

**Approved** | [#runtime/31038](https://github.com/dotnet/runtime/issues/31038#issuecomment-634204949) | [Video](https://www.youtube.com/watch?v=I7whBbUrMcc&t=1h26m58s)

```C#
namespace System
{
    [Flags]
    public enum StringSplitOptions
    {
        // Existing:
        // None = 0,
        //RemoveEmptyEntries = 1,

        // New:
        TrimEntries = 2
    }
}
```
## Obsolete RuntimeHelpers.OffsetToStringData

**Approved** | [#runtime/31406](https://github.com/dotnet/runtime/issues/31406#issuecomment-634212546) | [Video](https://www.youtube.com/watch?v=I7whBbUrMcc&t=1h37m59s)

* Looks good.

```C#
namespace System.Runtime.CompilerServices
{
    public sealed class RuntimeHelpers
    {
        [Obsolete("Use string.GetPinnableReference() instead.")]
        public static int OffsetToStringData { get; }
    }
}
```
## expose System.Text.Encoding.Latin1 getter publicly

**Approved** | [#runtime/31549](https://github.com/dotnet/runtime/issues/31549#issuecomment-634217668) | [Video](https://www.youtube.com/watch?v=I7whBbUrMcc&t=1h52m23s)

* Looks good.
* We won't be exposing the `Latin1Encoding` type, because it's not necessary

```C#
namespace System.Text
{
    public partial class Encoding
    {
        public static Encoding Latin1 => Encoding.GetEncoding("iso-8859-1");
    }
}
```
