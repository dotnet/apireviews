# API Review 01/20/2026

## Allow HttpClientLoggerHandler in different places of the handler pipeline

**Approved** | [#extensions/5924](https://github.com/dotnet/extensions/issues/5924#issuecomment-3774338268) | [Video](https://www.youtube.com/watch?v=KpqIje2wzZM&t=0h0m0s)


* We recommend defaulting the new parameter to true to show the behavior of the older overloads.

```c#
namespace Microsoft.Extensions.DependencyInjection;

public static partial class HttpClientLoggingHttpClientBuilderExtensions
{
     public static IHttpClientBuilder AddExtendedHttpClientLogging(this IHttpClientBuilder builder, bool wrapHandlersPipeline = true);
     public static IHttpClientBuilder AddExtendedHttpClientLogging(this IHttpClientBuilder builder, IConfigurationSection section, bool wrapHandlersPipeline = true);
     public static IHttpClientBuilder AddExtendedHttpClientLogging(this IHttpClientBuilder builder, Action<LoggingOptions> configure, bool wrapHandlersPipeline = true);
}
```
## Add TarEntryFormat parameter to TarFile.CreateFromDirectory

**Approved** | [#runtime/121819](https://github.com/dotnet/runtime/issues/121819#issuecomment-3774359196) | [Video](https://www.youtube.com/watch?v=KpqIje2wzZM&t=0h14m26s)

Looks good as proposed.

```c#
namespace System.Formats.Tar;

public static partial class TarFile
{
     public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, TarEntryFormat format);
     public static void CreateFromDirectory(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, TarEntryFormat format);
     public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, TarEntryFormat format, CancellationToken cancellationToken = default);
     public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, TarEntryFormat format, CancellationToken cancellationToken = default);
}
```
## `ICollectionDebugView<T>`

**Approved** | [#runtime/121543](https://github.com/dotnet/runtime/issues/121543#issuecomment-3774492984) | [Video](https://www.youtube.com/watch?v=KpqIje2wzZM&t=0h20m20s)


* It would be very unusual to have a class name like "ICollection...", so we think "CollectionDebugView" is better.
    * But since it could work for IEnumerable, we then renamed it to EnumerableDebugView and made it take IEnumerable
* CompilerServices doesn't seem like a good fit, since it would make sense for users to put them on custom collection types.  Next to DebuggerTypeProxy in System.Diagnostics
* More understanding is needed about the required generic arity/arities here, compared with the collection types (will it work with non-generic collections? with `Dictionary<TKey,TValue>`?)

```c#
namespace System.Diagnostics;

public sealed class EnumerableDebugView<T>
{
    public EnumerableDebugView(IEnumerable<T> source);

    [DebuggerBrowsable(DebuggerBrowsableState.RootHidden)]
    public T[] Items { get; }
}
```
## Console: low-level APIs for getting STD IN/OUT/ERR handles

**Approved** | [#runtime/122802](https://github.com/dotnet/runtime/issues/122802#issuecomment-3774538055) | [Video](https://www.youtube.com/watch?v=KpqIje2wzZM&t=0h53m8s)

Looks good as proposed.

```C#
namespace System;

public static partial class Console
{
   public static SafeFileHandle OpenStandardInputHandle();
   public static SafeFileHandle OpenStandardOutputHandle();
   public static SafeFileHandle OpenStandardErrorHandle();
}
```
## File: low-level APIs for opening null device

**Approved** | [#runtime/122803](https://github.com/dotnet/runtime/issues/122803#issuecomment-3774608460) | [Video](https://www.youtube.com/watch?v=KpqIje2wzZM&t=1h4m53s)

* Since File is already in the type name, and the return type name, we can simplify the method name to just `OpenNullHandle()`

```c#
namespace System.IO;

public static partial class File
{
    public static SafeFileHandle OpenNullHandle();
}
```
