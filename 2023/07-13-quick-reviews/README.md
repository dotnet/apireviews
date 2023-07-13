# API Review 07/13/2023

## BCL additions to support C# 12's "Collection Literals" feature.

**Approved** | [#runtime/87569](https://github.com/dotnet/runtime/issues/87569#issuecomment-1634635140) | [Video](https://www.youtube.com/watch?v=dgd2OmOCJPc&t=0h0m0s)

* We should mark it as `AllowMultiple = false` for now
* The proposal allows interfaces. Should we add attributes for the immutable interfaces? Are we OK with making a decision which concrete types are the canonical implementations for the attributed interface?

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Class |
                    AttributeTargets.Interface |
                    AttributeTargets.Struct,
                    Inherited = false,
                    AllowMultiple = false)]
    public sealed class CollectionBuilderAttribute : Attribute
    {
        public CollectionBuilderAttribute(Type builderType, string methodName);
        public Type BuilderType { get; }
        public string MethodName { get; }
    }
}

namespace System.Collections.Immutable
{
    [CollectionBuilder(typeof(ImmutableArray), "Create")]
    public readonly partial struct ImmutableArray<T> { }

    [CollectionBuilder(typeof(ImmutableHashSet), "Create")]
    public sealed partial class ImmutableHashSet<T> { }

    [CollectionBuilder(typeof(ImmutableList), "Create")]
    public sealed partial class ImmutableList<T> { }

    [CollectionBuilder(typeof(ImmutableQueue), "Create")]
    public sealed partial class ImmutableQueue<T> { }

    [CollectionBuilder(typeof(ImmutableSortedSet), "Create")]
    public sealed partial class ImmutableSortedSet<T> { }

    [CollectionBuilder(typeof(ImmutableStack), "Create")]
    public sealed partial class ImmutableStack<T> { }
}
```
## Improve feature parity between JsonSerializerOptions and JsonSourceGenerationOptionsAttribute

**Approved** | [#runtime/57321](https://github.com/dotnet/runtime/issues/57321#issuecomment-1634657071) | [Video](https://www.youtube.com/watch?v=dgd2OmOCJPc&t=0h28m29s)

* We considered exposing the constructor parameter as a property but we don't believe anyone should need that

```diff
namespace System.Text.Json.Serialization;

public partial class JsonSourceGenerationOptionsAttribute
{
    public JsonSourceGenerationOptionsAttribute();
+   public JsonSourceGenerationOptionsAttribute(JsonSerializerDefaults defaults);

+   public bool AllowTrailingCommas { get; set; }
+   public Type[]? Converters { get; set; }
+   public int DefaultBufferSize { get; set; }
    public JsonIgnoreCondition DefaultIgnoreCondition { get; set; }
+   public JsonKnownNamingPolicy DictionaryKeyPolicy { get; set; }
    public bool IgnoreReadOnlyFields { get; set; }
    public bool IgnoreReadOnlyProperties { get; set; }
    public bool IncludeFields { get; set; }
+   public int MaxDepth { get; set; }
+   public JsonNumberHandling NumberHandling { get; set; }
+   public JsonObjectCreationHandling PreferredObjectCreationHandling { get; set; }
+   public bool PropertyNameCaseInsensitive { get; set; }
    public JsonKnownNamingPolicy PropertyNamingPolicy { get; set; }
+   public JsonCommentHandling ReadCommentHandling { get; set; }
+   public JsonUnknownTypeHandling UnknownTypeHandling { get; set; }
+   public JsonUnmappedMemberHandling UnmappedMemberHandling { get; set; }
    public bool WriteIndented { get; set; }
}
```
## JsonSourceGenerationOptionsAttribute should provide an option for enabling string enum serialization globally

**Approved** | [#runtime/87195](https://github.com/dotnet/runtime/issues/87195#issuecomment-1634682787) | [Video](https://www.youtube.com/watch?v=dgd2OmOCJPc&t=0h46m42s)

* Looks good as proposed
* We decided to add a converter that lets people serialize enums as numbers as a way to offset the global switch

```C#
namespace System.Text.Json.Serialization;

public partial class JsonSourceGenerationOptionsAttribute
{
     public bool UseStringEnumConverter { get; set; } = false;
}

public partial class JsonNumberEnumConverter<TEnum> : JsonConverterFactory
    where TEnum : struct, Enum
{
    public JsonNumberEnumConverter();

    public sealed override bool CanConvert(System.Type typeToConvert);
    public sealed override JsonConverter CreateConverter(Type typeToConvert, JsonSerializerOptions options);
}
```
## Configure SocketsHttpHandler for HttpClientFactory with IConfiguration

**Approved** | [#runtime/84075](https://github.com/dotnet/runtime/issues/84075#issuecomment-1634723825) | [Video](https://www.youtube.com/watch?v=dgd2OmOCJPc&t=1h7m4s)

* We shouldn't take `IConfigurationSection` but `IConfiguration` (which `IConfigurationSection` extends)

```C#
namespace Microsoft.Extensions.DependencyInjection;

public static partial class HttpClientBuilderExtensions
{
#if NET5_0_OR_GREATER
    [UnsupportedOSPlatform("browser")]
    public static IHttpClientBuilder UseSocketsHttpHandler(
        this IHttpClientBuilder builder,
        Action<SocketsHttpHandler, IServiceProvider>? configureHandler = null);

    [UnsupportedOSPlatform("browser")]
    public static IHttpClientBuilder UseSocketsHttpHandler(
        this IHttpClientBuilder builder,
        Action<ISocketsHttpHandlerBuilder> configureBuilder);
#endif
}

#if NET5_0_OR_GREATER

public interface ISocketsHttpHandlerBuilder
{
    string Name { get; }
    IServiceCollection Services { get; }
}

public static class SocketsHttpHandlerBuilderExtensions
{
    [UnsupportedOSPlatform("browser")]
    public static ISocketsHttpHandlerBuilder Configure(
        this ISocketsHttpHandlerBuilder builder,
        Action<SocketsHttpHandler, IServiceProvider> configure);

    [UnsupportedOSPlatform("browser")]
    public static ISocketsHttpHandlerBuilder Configure(
        this ISocketsHttpHandlerBuilder builder,
        IConfiguration configuration);
}

#endif
```
## add overloads with SocketAddress to Socket's SendTo and ReceiveFrom

**Approved** | [#runtime/87397](https://github.com/dotnet/runtime/issues/87397#issuecomment-1634730211) | [Video](https://www.youtube.com/watch?v=dgd2OmOCJPc&t=1h40m39s)

* We don't like `ReceiveFrom` because it mutates the socket address

```C#
namespace System.Net.Sockets;

public partial class Socket
{
    // public int ReceiveFrom(Span<byte> buffer, SocketFlags socketFlags, SocketAddress socketAddress);
    // public ValueTask<int> ReceiveFromAsync(Memory<byte> buffer, SocketFlags socketFlags, SocketAddress socketAddress, CancellationToken cancellationToken = default);

    public int SendTo(ReadOnlySpan<byte> buffer, SocketFlags socketFlags, SocketAddress socketAddress);
    public ValueTask<int> SendToAsync(ReadOnlyMemory<byte> buffer, SocketFlags socketFlags, SocketAddress socketAddress, CancellationToken cancellationToken = default);
}
```
