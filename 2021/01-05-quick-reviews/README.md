# Quick Reviews 01/05/2021

## More arities for tuple returning zip extension method

**Approved** | [#runtime/43702](https://github.com/dotnet/runtime/issues/43702#issuecomment-754807609) | [Video](https://www.youtube.com/watch?v=TPCjoYyp6e0&t=0h0m0s)

* Let's support only three for now as four seems fairly rare.
* We decided to not add an overload for the one taking the result selector.

```C#
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<(TFirst First, TSecond Second, TThird Third)> Zip<TFirst, TSecond, TThird>(this IEnumerable<TFirst> first, IEnumerable<TSecond> second, IEnumerable<TThird> third);
        public static IEnumerable<TResult> Zip<TFirst, TSecond, TThird, TResult>(this IEnumerable<TFirst> first, IEnumerable<TSecond> second, IEnumerable<TThird> third, Func<TFirst, TSecond, TThird, TResult> resultSelector);
    }

    public static class Queryable
    {
        public static IQueryable<(TFirst First, TSecond Second, TThird Third)> Zip<TFirst, TSecond, TThird>(this IQueryable<TFirst> source1, IEnumerable<TSecond> source2, IEnumerable<TThird> source3);
        public static IQueryable<TResult> Zip<TFirst, TSecond, TThird, TResult>(this IQueryable<TFirst> first, IEnumerable<TSecond> second, IEnumerable<TThird> third, Expression<Func<TFirst, TSecond, TThird, TResult>> resultSelector);
    }
}
```

## Add SingleFileUnsupportedAttribute

**Approved** | [#runtime/45705](https://github.com/dotnet/runtime/issues/45705#issuecomment-754838507) | [Video](https://www.youtube.com/watch?v=TPCjoYyp6e0&t=0h14m45s)

* The attribute seems reasonable
    - We shouldn't take a message because they are hard to change
    - We should take a diagnostic id
* Generalizing this to a capability-based model seems too complex
* We should consider whether we want a capability API (globally for single file or per assembly like `HasLocation`)
    - Do we need all the cases?
    - Is framework loaded from disk or memory?
    - Is user code loaded from disk or memory?
    - Is application extracted?
* Is the attribute named correctly?
    - Unsupported seems to imply it throws; here it just means "the behavior might not be what you want" while some actually throw

```C#
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.All, Inherited = false, AllowMultiple = false)]
    public sealed class SingleFileUnsupportedAttribute : Attribute
    {
        public SingleFileUnsupportedAttribute();
        public string DiagnosticId { get; set; }
        public string UrlFormat { get; set; }
    }
}
```

## Add compression support in WebSocket

**Approved** | [#runtime/31088](https://github.com/dotnet/runtime/issues/31088#issuecomment-754854011) | [Video](https://www.youtube.com/watch?v=TPCjoYyp6e0&t=1h9m43s)

* Let's try to put the deflate settings in a single spot. This also solves the problem when we have to support a different compression algorithm with separate settings.

```C#
namespace System.Net.WebSockets
{
    public sealed class WebSocketDeflateOptions
    {
        public int ClientMaxWindowBits { get; set; } = 15;
        public bool ClientContextTakeover { get; set; } = true;
        public int ServerMaxWindowBits { get; set; } = 15;
        public bool ServerContextTakeover { get; set; } = true;
    }

    public sealed class WebSocketCreationOptions
    {
        public bool IsServer { get; set; }
        public string? SubProtocol { get; set; }
        public TimeSpan KeepAliveInterval { get; set; }
        public WebSocketDeflateOptions? DeflateOptions { get; set; } = null;
    }

    public partial class ClientWebSocketOptions
    {
        public WebSocketDeflateOptions? DeflateOptions { get; set; } = null;
    }

    public partial class WebSocket
    {
        public static WebSocket CreateFromStream(Stream stream, WebSocketCreationOptions options);
    }    
}
```

## Should we add support to ignore cycles on serialization?

**Approved** | [#runtime/40099](https://github.com/dotnet/runtime/issues/40099#issuecomment-754882930) | [Video](https://www.youtube.com/watch?v=TPCjoYyp6e0&t=1h39m19s)

* The concern would be ensure that the output is deterministic regardless how we're traversing the object graph. Is this guaranteed? If that's the case, this feature seems reasonable.
* When an a list contains a cycle, we should write out `null` value, instead of just omitting the value (as that's consistent with the ignore-null-behavior and doesn't change indices).

```C#
namespace System.Text.Json.Serialization
{
    public partial class ReferenceHandler
    {   
        // Existing:
        // public static ReferenceHandler Preserve { get; }
        public static ReferenceHandler IgnoreCycle { get; }
    }
}
```

## Extend ComWrappers API to better support aggregation

**Approved** | [#runtime/44446](https://github.com/dotnet/runtime/issues/44446#issuecomment-754884582) | [Video](https://www.youtube.com/watch?v=TPCjoYyp6e0&t=2h35m15s)

* Looks good, but we should change the order to preserve positional order between overloads.

```C#
namespace System.Runtime.InteropServices
{
    [Flags]
    public partial enum CreateObjectFlags
    {
        // None = 0,
        // TrackerObject = 1,
        // UniqueInstance = 2,
        Aggregation = 4,
        Unwrap = 8,
    }
}
namespace System.Runtime.InteropServices
{
    public abstract class ComWrappers
    {
        // Exists
        // public object GetOrRegisterObjectForComInstance(IntPtr externalComObject, CreateObjectFlags flags, object wrapper);
        public object GetOrRegisterObjectForComInstance(IntPtr externalComObject, CreateObjectFlags flags, object wrapper, IntPtr inner);
    }
}
```

