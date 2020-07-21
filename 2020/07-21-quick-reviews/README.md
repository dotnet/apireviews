# Quick Reviews 07/21/2020

## Expose transaction savepoints in ADO.NET

**Approved** | [#runtime/33397](https://github.com/dotnet/runtime/issues/33397#issuecomment-662004505) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=0h0m0s)

* Looks good, but we'd prefer `SupportsSavepoints`
* We considered exposing a new type `DbSavepoint` but we felt it's overkill for a niche feature like this
* The `System.SqlClient` and `Microsoft.SqlClient` APIs should be updated to override these new base methods (instead of just hiding them) as well as `SupportsSavepoints`.

```C#
namespace System.Data.Common
{
    public class DbTransaction
    {
        public virtual Task SaveAsync(string savepointName, CancellationToken cancellationToken = default);
        public virtual Task RollbackAsync(string savepointName, CancellationToken cancellationToken = default);
        public virtual Task ReleaseAsync(string savepointName, CancellationToken cancellationToken = default);

        public virtual void Save(string savepointName);
        public virtual void Rollback(string savepointName);
        public virtual void Release(string savepointName);

        public virtual bool SupportsSavepoints { get; }
    }
}
```

## Add new overloads for AddSystemdConsole and AddJsonConsole not needing configure argument

**Approved** | [#runtime/39631](https://github.com/dotnet/runtime/issues/39631#issuecomment-662009556) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=0h34m24s)

* Makes sense
* We should also add `AddSimpleConsole` without `configure`, just for consistency

```C#

namespace Microsoft.Extensions.Logging
{
    public static class ConsoleLoggerExtensions
    {
        // Existing APIs:
        // public static ILoggingBuilder AddConsole(this ILoggingBuilder builder);
        // public static ILoggingBuilder AddConsole(this ILoggingBuilder builder, System.Action<ConsoleLoggerOptions> configure);
        // public static ILoggingBuilder AddJsonConsole(this ILoggingBuilder builder, System.Action<JsonConsoleFormatterOptions> configure);
        // public static ILoggingBuilder AddSimpleConsole(this ILoggingBuilder builder, System.Action<SimpleConsoleFormatterOptions> configure);
        // public static ILoggingBuilder AddSystemdConsole(this ILoggingBuilder builder, System.Action<ConsoleFormatterOptions> configure);
        public static ILoggingBuilder AddJsonConsole(this ILoggingBuilder builder);
        public static ILoggingBuilder AddSimpleConsole(this ILoggingBuilder builder);
        public static ILoggingBuilder AddSystemdConsole(this ILoggingBuilder builder);
    }
}
```

## Add additional Windows-specific APIs

**Approved** | [#designs/142](https://github.com/dotnet/designs/pull/142#issuecomment-662020828) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=0h43m11s)

Needs some tuning and review from area owners, but otherwise looks fine.
## ConsoleKeyInfo does not implement IEquatable

**Approved** | [#runtime/2127](https://github.com/dotnet/runtime/issues/2127#issuecomment-662022231) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=1h4m6s)

* Makes sense

```C#
namespace System
{
    public partial readonly struct ConsoleKeyInfo : IEquatable<ConsoleKeyInfo>
    {
    }
}
```

## Implement IEquatable<T> for AsyncFlowControl

**Approved** | [#runtime/31996](https://github.com/dotnet/runtime/issues/31996#issuecomment-662023094) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=1h7m27s)

* Makes sense

```C#
namespace System.Threading
{
    public partial struct AsyncFlowControl : IEquatable<AsyncFlowControl>
    {
    }
}
```

## System.Drawing.Color is possibly missing 8 colors

**Approved** | [#runtime/38244](https://github.com/dotnet/runtime/issues/38244#issuecomment-662030348) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=1h9m4s)

* For better or worse, framework spelling is en-us.
* If this would be parsing, supporting both `grey` and `gray` seems reasonable, but duplicating API surface to offer AE and BE feels wrong, so we don't feel we should be exposing those.

```C#
namespace System.Drawing
{
    public enum KnownColor
    {
        RebeccaPurple
    }
    public readonly struct Color : IEquatable<Color>
    {
        public static Color RebeccaPurple;
    }
}
```

## Add BindConfiguration extension method for OptionsBuilder

**Approved** | [#runtime/36209](https://github.com/dotnet/runtime/issues/36209#issuecomment-662034563) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=1h22m20s)

* Looks reasonable
* `configPath` looks like a file system path; we should name it `configSectionPath`, unless we use `configPath` elsewhere already.

```C#
namespace Microsoft.Extensions.DependencyInjection
{
    public static class OptionsBuilderConfigurationExtensions
    {
        public static OptionsBuilder<TOptions> BindConfiguration<TOptions>(
            this OptionsBuilder<TOptions> optionsBuilder,
            string configSectionPath,
            Action<BinderOptions> configureBinder = null)
            where TOptions : class => null;
    }
}
```

## Extend ProcessStartInfo to allow setting 

**Approved** | [#runtime/28114](https://github.com/dotnet/runtime/issues/28114#issuecomment-662039350) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=1h29m45s)

* The existing API already deviated from the Win32 naming convention (`Load`)
* We should give it a better name, to make it clear what it does
* We should mark the API as Windows-specific. We should check which API version this was introduced in, but it's probably Win7 or earlier anyways.

```C#
namespace System.Diagnostics.Process
{
    public class ProcessStartInfo
    {
        public bool LoadUserProfile { get; set; }
        public bool UseCredentialsForNetworkingOnly { get; set; }
    }
}
```

## Add EnumMember API

**Approved** | [#runtime/28198](https://github.com/dotnet/runtime/issues/28198#issuecomment-662047560) | [Video](https://www.youtube.com/watch?v=bGFsZSISlZ0&t=1h39m50s)

We should add this API:

```C#
namespace System
{
    public partial class Enum
    {
        public static T[] GetValues<T>();
    }
}
```

So this code:

```C#
var values = (MyEnum[])Enum.GetValues(typeof(MyEnum));
var names = Enum.GetNames(typeof(MyEnum));
for (int i = 0; i < values.Length; ++i)
{
    MyEnum value = values[i];
    string name = names[i];
}
```

becomes

```C#
var values = Enum.GetValues<MyEnum>();
foreach (var value in values)
{
    var name = value.ToString();
}
```

With respect to custom attributes, you can already do this:

```C#
FieldInfo enumField = ...;
var description = enumField.GetCustomAttributes<DescriptionAttribute>()
                           .SingleOrDefault()?.Description ?? "";
```

