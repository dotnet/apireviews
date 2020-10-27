# Quick Reviews 10/27/2020

## Add Stream.ValidateBufferArguments helper

**Approved** | [#runtime/43176](https://github.com/dotnet/runtime/issues/43176#issuecomment-717408439) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=0h0m0s)

* There are several derivatives of `Stream` that need to validate arguments
    - Getting the exception and parameter names is tricky and we had several bugs
* We like the pattern (`Validate` prefix, `Arguments` suffix)
    - The names make sense too
    - The choice of not tying it to a specific method makes sense because the buffer one applies to all methods on `Stream` that take a buffer (and the parameter names are thankfully consistent)
* Let's remove the `Stream source` argument from the second (because it's `this`)
    - The caller is responsible for validating `this`, these methods are only about arguments

```C#
namespace System.IO
{
    public abstract class Stream
    {
        protected static void ValidateBufferArguments(byte[] buffer, int offset, int count);
        protected static void ValidateCopyToArguments(Stream destination, int bufferSize);
    }
}
```
## Add HTTP/3 APIs

**Approved** | [#runtime/1293](https://github.com/dotnet/runtime/issues/1293#issuecomment-717411979) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=0h34m38s)

* Looks good as proposed

```C#
namespace System.Net
{
    public class partial HttpVersion
    {
        // Existing
        // public static readonly Version Version10;
        // public static readonly Version Version11;
        // public static readonly Version Version20;
        public static readonly Version Version30;
    }
}
namespace System.Net.Security
{
    public partial struct SslApplicationProtocol
    {
        // Existing
        // public static readonly SslApplicationProtocol Http11;
        // public static readonly SslApplicationProtocol Http2;
        public static readonly SslApplicationProtocol Http3;
    }
}
```

## Implement DisconnectAsync method in SocketTaskExtensions

**Approved** | [#runtime/1608](https://github.com/dotnet/runtime/issues/1608#issuecomment-717417383) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=0h40m48s)

* Looks good as proposed
* We should really consider "removing" SocketTaskExtensions
    - Merge with instance methods
    - Hide type & member
    - If we do this for .NET 6, we should delete `DisconnectAsync` from this type
    - @geoffkizer, could you file an issue for this?
* We should use `ValueTask`

```C#
namespace System.Net.Sockets
{
    public static class SocketTaskExtensions
    {
        // Existing APIs:
        // public IAsyncResult BeginDisconnect (bool reuseSocket, AsyncCallback callback, object state);
        // public void EndDisconnect (IAsyncResult asyncResult);

        public static ValueTask DisconnectAsync(this Socket socket, bool reuseSocket, CancellationToken cancellationToken=default);
    }
}
```

## Add Insert extension method for IConfigurationBuilder

**NeedsWork** | [#runtime/38536](https://github.com/dotnet/runtime/issues/38536#issuecomment-717421092) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=0h50m15s)

@maryamariyan request to table this for now so that they can think through the potential breaking changes first.
## Add method to Microsoft.Extensions.Configuration.IConfiguration that gets a section and then validates it against a model

**NeedsWork** | [#runtime/40695](https://github.com/dotnet/runtime/issues/40695#issuecomment-717423688) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=0h55m7s)

According to the discussion, this seems to warrant more though first.
## Use AsParallel() correctly

**Approved** | [#runtime/33769](https://github.com/dotnet/runtime/issues/33769#issuecomment-717429099) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h0m17s)

This makes sense. The only thing we have to check with Manish is whether they analyzer should report separate IDs due to how fixers are advertised in the IDE.
## Provide default activity name value from [CallerMemberName] in ActivitySource.StartActivity

**Approved** | [#runtime/43483](https://github.com/dotnet/runtime/issues/43483#issuecomment-717443644) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h8m8s)

* Looks good as proposed

```C#
namespace System.Diagnostics
{
    public static partial class ActivitySource
    {
        // Existing API, just mark first parameter optional and add [CallerMemberName]
        public Activity? StartActivity ([CallerMemberName] string name = "",
                                        ActivityKind kind = ActivityKind.Internal);

        public Activity StartActivity (ActivityKind kind,
                                       ActivityContext? parentContext = default,
                                       IEnumerable<KeyValuePair<string,object>>? tags = default,
                                       IEnumerable<ActivityLink>? links = default,
                                       DateTimeOffset startTime = default,
                                       [CallerMemberName] string name = "");
    }
}
```

## Analyzer: warn on use of module initializer

**Approved** | [#runtime/43328](https://github.com/dotnet/runtime/issues/43328#issuecomment-717452492) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h27m6s)

We seem to believe this can even be a warning that is on by default (but it shouldn't apply to generated code). Advanced users (who this feature is for) know how to turn it off.
## Add easy way to create a certificate from just a cert-PEM

**Approved** | [#runtime/43590](https://github.com/dotnet/runtime/issues/43590#issuecomment-717455862) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h34m34s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography.X509Certificates
{
    partial class X509Certificate2
    {
        public static X509Certificate2 CreateFromPem(ReadOnlySpan<char> certPem);
    }
}
```

## Add Span overloads for Socket.SendFile

**Approved** | [#runtime/43846](https://github.com/dotnet/runtime/issues/43846#issuecomment-717460180) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h37m19s)

* Looks good as proposed
* We considered span-ifying the string parameter, but that feels contrived

```C#
namespace System.Net.Sockets
{
    public class Socket
    {
        // Existing API
        // public void SendFile(string fileName, byte[] preBuffer, byte[] postBuffer, TransmitFileOptions flags);

        public void SendFile(string fileName, ReadOnlySpan<byte> preBuffer, ReadOnlySpan<byte> postBuffer, TransmitFileOptions flags);
    }
}
```

## Add API IndexNotOf 

**Rejected** | [#runtime/28795](https://github.com/dotnet/runtime/issues/28795#issuecomment-717466792) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h42m23s)

* These APIs seem fairly specialized; it feels this might open up a large amount of potential new APIs due to combinations
* These seem to live in user code
* So unless there is more evidence that suggests this API is useful

```C#
namespace System
{
    public static partial class MemoryExtensions
    {
        int IndexNotOf(this Span<T> span, T value) where T : IEquatable<T>;
        int IndexNotOfAny(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
        int IndexNotOfAny(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
        int IndexNotOfAny(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;

        int IndexNotOf(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
        int IndexNotOfAny(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
        int IndexNotOfAny(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
        int IndexNotOfAny(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;

        int LastIndexNotOf(this Span<T> span, T value) where T : IEquatable<T>;
        int LastIndexNotOfAny(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
        int LastIndexNotOfAny(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
        int LastIndexNotOfAny(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;

        int LastIndexNotOf(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
        int LastIndexNotOfAny(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
        int LastIndexNotOfAny(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
        int LastIndexNotOfAny(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
    }
}
```

## Introduce Math/F.ReciprocalEstimate and Math/F.ReciprocalSqrtEstimate

**Approved** | [#runtime/39971](https://github.com/dotnet/runtime/issues/39971#issuecomment-717470965) | [Video](https://www.youtube.com/watch?v=viYdlWGUiro&t=1h54m40s)

* Makes sense

```C#
namespace System
{
    public static class Math
    {
        public double ReciprocalEstimate(double d);
        public double SqrtEstimate(double d);
        public double ReciprocalSqrtEstimate(double d);
    }

    public static class MathF
    {
        public float ReciprocalEstimate(float x);
        public float SqrtEstimate(float x);
        public float ReciprocalSqrtEstimate(float x);
    }
}
```
