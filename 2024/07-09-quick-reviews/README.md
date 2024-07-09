# API Review 07/09/2024

## Migrate `HybridCache` from aspnet to runtime

**NeedsWork** | [#runtime/100290](https://github.com/dotnet/runtime/issues/100290#issuecomment-2218284038) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=0h0m0s)


* Consider moving HybridCache virtual/abstract members to the template method pattern
* Do we have any implementations of HybridCache that we're making public? What layer(s)? How does someone instantiate one (without DI)?
* It was asked if all the (non-generic) ValueTask methods should use Task, and since it feels like the usage does not expect parallel calls from a single caller (followed by e.g. WaitAll), ValueTask is fine.
* CancellationToken parameters should be named `cancellationToken`.  IBufferDistributedCache extends a type that did this wrong, so it should maintain local consistency.

```C#
namespace Microsoft.Extensions.Caching.Distributed
{
    public interface IBufferDistributedCache : IDistributedCache
    {
        bool TryGet(string key, IBufferWriter<byte> destination);
        ValueTask<bool> TryGetAsync(string key, IBufferWriter<byte> destination, CancellationToken token = default);
        void Set(string key, ReadOnlySequence<byte> value, DistributedCacheEntryOptions options);
        ValueTask SetAsync(string key, ReadOnlySequence<byte> value, DistributedCacheEntryOptions options, CancellationToken token = default);
    }
}
namespace Microsoft.Extensions.Caching.Hybrid
{
    public partial interface IHybridCacheSerializer<T>
    {
        T Deserialize(ReadOnlySequence<byte> source);
        void Serialize(T value, IBufferWriter<byte> target);
    }
    public interface IHybridCacheSerializerFactory
    {
        bool TryCreateSerializer<T>([NotNullWhen(true)] out IHybridCacheSerializer<T>? serializer);
    }
    public sealed class HybridCacheEntryOptions
    {
        public TimeSpan? Expiration { get; init; }
        public TimeSpan? LocalCacheExpiration { get; init; }
        public HybridCacheEntryFlags? Flags { get; init; }
    }
    [Flags]
    public enum HybridCacheEntryFlags
    {
        None = 0,
        DisableLocalCacheRead = 1 << 0,
        DisableLocalCacheWrite = 1 << 1,
        DisableLocalCache = DisableLocalCacheRead | DisableLocalCacheWrite,
        DisableDistributedCacheRead = 1 << 2,
        DisableDistributedCacheWrite = 1 << 3,
        DisableDistributedCache = DisableDistributedCacheRead | DisableDistributedCacheWrite,
        DisableUnderlyingData = 1 << 4,
        DisableCompression = 1 << 5,
    }
    public abstract class HybridCache
    {
        [SuppressMessage("ApiDesign", "RS0026:Do not add multiple public overloads with optional parameters", Justification = "Delegate differences make this unambiguous")]
        public abstract ValueTask<T> GetOrCreateAsync<TState, T>(string key, TState state, Func<TState, CancellationToken, ValueTask<T>> factory,
            HybridCacheEntryOptions? options = null, IReadOnlyCollection<string>? tags = null, CancellationToken cancellationToken = default);
        [SuppressMessage("ApiDesign", "RS0026:Do not add multiple public overloads with optional parameters", Justification = "Delegate differences make this unambiguous")]
        public ValueTask<T> GetOrCreateAsync<T>(string key, Func<CancellationToken, ValueTask<T>> factory,
            HybridCacheEntryOptions? options = null, IReadOnlyCollection<string>? tags = null, CancellationToken cancellationToken = default)
            => throw new System.NotImplementedException();

        public abstract ValueTask SetAsync<T>(string key, T value, HybridCacheEntryOptions? options = null, IReadOnlyCollection<string>? tags = null, CancellationToken cancellationToken = default);

        [SuppressMessage("ApiDesign", "RS0026:Do not add multiple public overloads with optional parameters", Justification = "Not ambiguous in context")]
        public abstract ValueTask RemoveAsync(string key, CancellationToken cancellationToken = default);

        [SuppressMessage("ApiDesign", "RS0026:Do not add multiple public overloads with optional parameters", Justification = "Not ambiguous in context")]
        public virtual ValueTask RemoveAsync(IEnumerable<string> keys, CancellationToken cancellationToken = default)
            => throw new System.NotImplementedException();

       [SuppressMessage("ApiDesign", "RS0026:Do not add multiple public overloads with optional parameters", Justification = "Not ambiguous in context")]
        public virtual ValueTask RemoveByTagAsync(IEnumerable<string> tags, CancellationToken cancellationToken = default)
            => throw new System.NotImplementedException();
        public abstract ValueTask RemoveByTagAsync(string tag, CancellationToken cancellationToken = default);
    }
}
```
## Expose Json in SqlDbType

**Approved** | [#runtime/103925](https://github.com/dotnet/runtime/issues/103925#issuecomment-2218294848) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=0h22m3s)

Looks good as proposed

```C#

namespace System.Data
{
    // Specifies the SQL Server data type.
    public enum SqlDbType
    {
        Json = 35,
    }
}
```
## DebuggerDisableUserUnhandledAttribute

**Approved** | [#runtime/103105](https://github.com/dotnet/runtime/issues/103105#issuecomment-2218329653) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=0h28m59s)


* BreakForException => BreakForUserUnhandledException
* It was asked if the parameter to BreakForUserUnhandledException could be removed, and just use `$exception`; but it was felt that the parameter was useful.
* We discussed "DebuggerDisable" or "DisableDebugger", and feel that the prefix "Debugger" gives better alignment to other Debugger-intent attributes in this namespace.
* Spell out "ex" to "exception" in the parameter.

```C#
namespace System.Diagnostics;

[AttributeUsage(AttributeTargets.Method)]
public sealed class DebuggerDisableUserUnhandledExceptionsAttribute : Attribute
{
    public DebuggerDisableUserUnhandledExceptionsAttribute();
}

public partial class Debugger
{
    public static void BreakForUserUnhandledException(Exception exception);
}
```
## Overriding the default behavior in case of unhandled exceptions and fatal errors.

**Approved** | [#runtime/101560](https://github.com/dotnet/runtime/issues/101560#issuecomment-2218393125) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=0h52m25s)

* Instead of creating a new delegate type, use `Func<Exception, bool>`
* We renamed `fatalErrorHandler` to `handler` for consistency.

```C#
namespace System.Runtime.ExceptionServices
{
    public static class ExceptionHandling
    {
        /// <summary>
        /// Sets a handler for unhandled exceptions.
        /// </summary>
        /// <exception cref="ArgumentNullException">If handler is null</exception>
        /// <exception cref="InvalidOperationException">If a handler is already set</exception>
        public static void SetUnhandledExceptionHandler(Func<Exception, bool> handler);

        /// <summary>
        /// .NET runtime is going to call `fatalErrorHandler` set by this method before its own
        /// fatal error handling (creating .NET runtime-specific crash dump, etc.).
        /// </summary>
        /// <exception cref="ArgumentNullException">If fatalErrorHandler is null</exception>
        /// <exception cref="InvalidOperationException">If a handler is already set</exception>
        public static unsafe void SetFatalErrorHandler(delegate* unmanaged<int, void*, int> handler);
    }
}
```
## Expose polymorphic base type in `JsonSchemaExporterContext`

**Approved** | [#runtime/104046](https://github.com/dotnet/runtime/issues/104046#issuecomment-2218399552) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=1h29m49s)

Looks good as proposed.

```C#
namespace System.Text.Json.Schema;

public readonly partial struct JsonSchemaExporterContext
{
    public JsonTypeInfo? BaseTypeInfo { get; }
}
```
## Add JsonElement.ValueSpan

**Approved** | [#runtime/77666](https://github.com/dotnet/runtime/issues/77666#issuecomment-2218417275) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=1h32m38s)

* We changed from a Try to a non-Try, and as a consequence put "Utf8" in the method name.

```C#
namespace System.Runtime.InteropServices;

public partial class JsonMarshal
{
    public static ReadOnlySpan<byte> GetRawUtf8Value(JsonElement element);
}
```
## System.Text.Json: add ability to do semantic comparisons of JSON values à la JToken.DeepEquals()

**Approved** | [#runtime/33388](https://github.com/dotnet/runtime/issues/33388#issuecomment-2218426511) | [Video](https://www.youtube.com/watch?v=4NA1dWe1h_I&t=1h43m56s)

Looks good as proposed.

```C#
namespace System.Text.Json;

public partial struct JsonElement
{
    public static bool DeepEquals(JsonElement element1, JsonElement element2);
}
```
