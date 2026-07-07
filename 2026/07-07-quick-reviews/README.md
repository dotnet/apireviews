# API Review 07/07/2026

## low level TLS machine

**Approved** | [#runtime/128871](https://github.com/dotnet/runtime/issues/128871#issuecomment-4907218722) | [Video](https://www.youtube.com/watch?v=IUPqHxMrkRk&t=0h0m0s)


* Changed TlsContext.Create to CreateClient and CreateServer
* Changed TlsSession to use constructors (default and SocketHandle)
* TlsContext no longer takes nullable server options
* Removed TlsContext.IsServer
* TlsOperationStatus.NeedsServerOptions => NeedsTlsContext
* TlsSession.SetServerContext => SetContext
* Split TlsSession into a socket-attached and a manual mode
* Should HasPendingOutput move to TlsBufferSession?
* Removed the span-returning GetClientHelloBytes, replaced it with a copying pattern.
* Renamed WantCredentials to CertificateRequested ("Requested" to account for the state where a server will accept null)
* Made SetClientCertificateContext nullable to account for the client saying no.
* Renamed WantRead/WantWrite to NeedMoreData and DestinationTooSmall (copying from OperationStatus)

```csharp
namespace System.Net.Security;

[Experimental("SYSLIB5007", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
public enum TlsOperationStatus
{
    Complete = 0,
    DestinationTooSmall = 1,
    NeedMoreData = 2,
    Closed = 3,
    CertificateRequested = 4,
    NeedsCertificateValidation = 5,
    NeedsTlsContext = 6,
}

[Experimental("SYSLIB5007", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
public sealed class TlsContext : IDisposable
{
    public static TlsContext CreateServer(SslServerAuthenticationOptions options);
    public static TlsContext CreateClient(SslClientAuthenticationOptions options);
    public void Dispose();
}

[Experimental("SYSLIB5007", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
public sealed class TlsBufferSession : TlsSession
{
    public TlsBufferSession();

    public TlsOperationStatus Handshake(
        ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);

    public TlsOperationStatus Write(
        ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);

    public TlsOperationStatus Read(
        ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);

    public TlsOperationStatus Shutdown(Span<byte> ciphertext, out int bytesWritten);
    public TlsOperationStatus DrainPendingOutput(Span<byte> ciphertext, out int bytesWritten);
    public TlsOperationStatus RequestClientCertificate(Span<byte> ciphertext, out int bytesWritten);
}

[Experimental("SYSLIB5007", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
public sealed class TlsSocketSession : TlsSession
{
    public TlsSocketSession(SafeSocketHandle socket);

    public SafeSocketHandle Socket { get; }

    public TlsOperationStatus Handshake();
    public TlsOperationStatus Read(Span<byte> buffer, out int bytesRead);
    public TlsOperationStatus Write(ReadOnlySpan<byte> buffer, out int bytesWritten);
    public TlsOperationStatus Shutdown();
    public TlsOperationStatus RequestClientCertificate();
}

[Experimental("SYSLIB5007", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
public abstract class TlsSession : IDisposable
{
    private protected TlsSession();

    public void SetContext(TlsContext context);

    public bool IsHandshakeComplete { get; }
    public bool HasPendingOutput { get; }
    public string? TargetHostName { get; set; } 
    
    public SslClientHelloInfo? ClientHelloInfo { get; }

    public int GetClientHelloLength();
    public bool TryGetClientHelloBytes(Span<byte> destination, out int bytesWritten);

    public SslProtocols NegotiatedProtocol { get; }
    [CLSCompliant(false)]
    public TlsCipherSuite NegotiatedCipherSuite { get; }
    public SslApplicationProtocol NegotiatedApplicationProtocol { get; }
    public X509Certificate2? LocalCertificate { get; }

    public SslPolicyErrors AcceptWithDefaultValidation();
    public X509Certificate2? GetRemoteCertificate();
    public X509Certificate2Collection? GetRemoteCertificates();
    public void SetRemoteCertificateValidationResult(SslPolicyErrors errors);
    public void SetClientCertificateContext(SslStreamCertificateContext? context);
    public IReadOnlyList<string>? GetAcceptableIssuers();
    public ChannelBinding? GetChannelBinding(ChannelBindingKind kind);

    public void Dispose();
}
```

## HybridCache: Setting expiration based on value in cache entry factory

**Approved** | [#runtime/121530](https://github.com/dotnet/runtime/issues/121530#issuecomment-4907351718) | [Video](https://www.youtube.com/watch?v=IUPqHxMrkRk&t=1h31m17s)

* Added the internal HybridCacheEntryContext ctor that was missing (to indicate non-instantiable)

```csharp
namespace Microsoft.Extensions.Caching.Hybrid;

public sealed class HybridCacheEntryContext
{
    internal HybridCacheEntryContext();

    public TimeSpan? Expiration { get; set; }
    public TimeSpan? LocalCacheExpiration { get; set; }
    public HybridCacheEntryFlags? Flags { get; set; }
    public long? LocalSize { get; set; }
    public int Revision { get; }
}

public partial abstract class HybridCache
{
    public virtual ValueTask<T> GetOrCreateAsync<TState, T>(
        string key, TState state,
        Func<TState, HybridCacheEntryContext, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    public ValueTask<T> GetOrCreateAsync<T>(
        string key,
        Func<HybridCacheEntryContext, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

#if NET
    public virtual ValueTask<T> GetOrCreateAsync<TState, T>(
        ReadOnlySpan<char> key, TState state,
        Func<TState, HybridCacheEntryContext, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    public ValueTask<T> GetOrCreateAsync<T>(
        ReadOnlySpan<char> key,
        Func<HybridCacheEntryContext, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    public ValueTask<T> GetOrCreateAsync<TState, T>(
        ref DefaultInterpolatedStringHandler key, TState state,
        Func<TState, HybridCacheEntryContext, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);

    public ValueTask<T> GetOrCreateAsync<T>(
        ref DefaultInterpolatedStringHandler key,
        Func<HybridCacheEntryContext, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);
#endif
}
```

## Mark MemoryMarshal APIs as caller-unsafe

**Approved** | [#runtime/127098](https://github.com/dotnet/runtime/issues/127098#issuecomment-4907526979) | [Video](https://www.youtube.com/watch?v=IUPqHxMrkRk&t=1h47m35s)

Looks good as proposed.

```diff
public static partial class MemoryMarshal
{
+   unsafe
    public static ReadOnlySpan<byte> AsBytes<T>(ReadOnlySpan<T> span) where T : struct { throw null; }

+   unsafe
    public static Span<byte> AsBytes<T>(Span<T> span) where T : struct { throw null; }

+   unsafe
    public static Memory<T> AsMemory<T>(ReadOnlyMemory<T> memory) { throw null; }

+   unsafe
    public static ref readonly T AsRef<T>(ReadOnlySpan<byte> span) where T : struct { throw null; }

+   unsafe
    public static ref T AsRef<T>(Span<byte> span) where T : struct { throw null; }

+   unsafe
    public static ReadOnlySpan<TTo> Cast<TFrom, TTo>(ReadOnlySpan<TFrom> span) where TFrom : struct where TTo : struct { throw null; }

+   unsafe
    public static Span<TTo> Cast<TFrom, TTo>(Span<TFrom> span) where TFrom : struct where TTo : struct { throw null; }

+   unsafe
    public static Memory<T> CreateFromPinnedArray<T>(T[]? array, int start, int length) { throw null; }

+   unsafe
    public static ReadOnlySpan<T> CreateReadOnlySpan<T>(scoped ref readonly T reference, int length) { throw null; }

+   unsafe
    public static Span<T> CreateSpan<T>(scoped ref T reference, int length) { throw null; }

+   unsafe
    public static ref byte GetArrayDataReference(Array array) { throw null; }

+   unsafe
    public static ref T GetArrayDataReference<T>(T[] array) { throw null; }

+   unsafe
    public static ref T GetReference<T>(ReadOnlySpan<T> span) { throw null; }

+   unsafe
    public static ref T GetReference<T>(Span<T> span) { throw null; }

+   unsafe
    public static T Read<T>(ReadOnlySpan<byte> source) where T : struct { throw null; }

+   unsafe
    public static bool TryGetArray<T>(ReadOnlyMemory<T> memory, out ArraySegment<T> segment) { throw null; }

+   unsafe
    public static bool TryGetMemoryManager<T, TManager>(ReadOnlyMemory<T> memory, [NotNullWhen(true)] out TManager? manager) where TManager : MemoryManager<T> { throw null; }

+   unsafe
    public static bool TryGetMemoryManager<T, TManager>(ReadOnlyMemory<T> memory, [NotNullWhen(true)] out TManager? manager, out int start, out int length) where TManager : MemoryManager<T> { throw null; }

+   unsafe
    public static bool TryRead<T>(ReadOnlySpan<byte> source, out T value) where T : struct { throw null; }

+   unsafe
    public static bool TryWrite<T>(Span<byte> destination, in T value) where T : struct { throw null; }

+   unsafe
    public static void Write<T>(Span<byte> destination, in T value) where T : struct { }
}
```
