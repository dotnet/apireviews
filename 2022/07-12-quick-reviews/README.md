# API Review 07/12/2022

## [QUIC] QuicConnection

**Approved** | [#runtime/68902](https://github.com/dotnet/runtime/issues/68902#issuecomment-1182102876) | [Video](https://www.youtube.com/watch?v=vhy7LqB2MC4&t=0h0m0s)

* Maybe we should replace `required` with regular constructors or just forgo all required checking and move it to `QuicConnection`

```C#
namespace System.Net.Quic;

public sealed class QuicConnection : IAsyncDisposable
{
    public static bool IsSupported { get; }
    public IPEndPoint RemoteEndPoint { get; }
    public IPEndPoint LocalEndPoint { get; }
    public X509Certificate2? RemoteCertificate { get; }
    public SslApplicationProtocol NegotiatedApplicationProtocol { get; }
    public static ValueTask<QuicConnection> ConnectAsync(QuicConnectionOptions options, CancellationToken cancellationToken = default);
    public ValueTask<QuicStream> OpenOutboundStreamAsync(QuicStreamType type, CancellationToken cancellationToken = default);
    public ValueTask<QuicStream> AcceptInboundStreamAsync(CancellationToken cancellationToken = default);
    public ValueTask CloseAsync(long errorCode, CancellationToken cancellationToken = default);
    public void DisposeAsync();
}
public abstract class QuicConnectionOptions
{
    public int MaxInboundBidirectionalStreams { get; set; }
    public int MaxInboundUnidirectionalStreams { get; set; }
    public TimeSpan IdleTimeout { get; set; } = TimeSpan.Zero;
    public long DefaultStreamErrorCode { get; set; }
    public long DefaultCloseErrorCode { get; set; }
}
public sealed class QuicClientConnectionOptions : QuicConnectionOptions
{
    public QuicClientConnectionOptions();
    public SslClientAuthenticationOptions ClientAuthenticationOptions { get; set; }
    public EndPoint RemoteEndPoint { get; set; }
    public IPEndPoint? LocalEndPoint { get; set; }
}
public sealed class QuicServerConnectionOptions : QuicConnectionOptions
{
    public QuicServerConnectionOptions();
    public SslServerAuthenticationOptions ServerAuthenticationOptions { get; set; }
}
```
## [QUIC] QuicStream

**Approved** | [#runtime/69675](https://github.com/dotnet/runtime/issues/69675#issuecomment-1182155415) | [Video](https://www.youtube.com/watch?v=vhy7LqB2MC4&t=0h38m24s)

* Looks good as proposed, given that it's in preview.

```C#
namespace System.Net.Quic;

public sealed class QuicStream : Stream
{
    public long Id { get; }
    public QuicStreamType Type { get; }
    public Task ReadsClosed { get; }
    public Task WritesClosed { get; }
    public void Abort(QuicAbortDirection abortDirection, long errorCode);
    public void CompleteWrites();
    public ValueTask WriteAsync(ReadOnlyMemory<byte> buffer, bool completeWrites, CancellationToken cancellationToken = default);
}

[Flags]
public enum QuicAbortDirection
{
   Read = 1,
   Write = 2,
   Both = Read | Write
}

public enum QuicStreamType
{
    Unidirectional,
    Bidirectional
}
```
## RateLimitPartition rename Create methods

**Approved** | [#runtime/71352](https://github.com/dotnet/runtime/issues/71352#issuecomment-1182192677) | [Video](https://www.youtube.com/watch?v=vhy7LqB2MC4&t=1h0m36s)

* Let' use `Get`

```diff
namespace System.Threading.RateLimiting;

public static class RateLimitPartition
{
-    public static RateLimitPartition<TKey> Create<TKey>(
+    public static RateLimitPartition<TKey> Get<TKey>(
        TKey partitionKey,
        Func<TKey, RateLimiter> factory)
    {
    }

-    public static RateLimitPartition<TKey> CreateConcurrencyLimiter<TKey>(
+    public static RateLimitPartition<TKey> GetConcurrencyLimiter<TKey>(
        TKey partitionKey,
        Func<TKey, ConcurrencyLimiterOptions> factory)
    {
    }

-    public static RateLimitPartition<TKey> CreateNoLimiter<TKey>(TKey partitionKey)
+    public static RateLimitPartition<TKey> GetNoLimiter<TKey>(TKey partitionKey)
    {
    }

-    public static RateLimitPartition<TKey> CreateTokenBucketLimiter<TKey>(
+    public static RateLimitPartition<TKey> GetTokenBucketLimiter<TKey>(
        TKey partitionKey,
        Func<TKey, TokenBucketRateLimiterOptions> factory)
    {
    }

-    public static RateLimitPartition<TKey> CreateSlidingWindowLimiter<TKey>(
+    public static RateLimitPartition<TKey> GetSlidingWindowLimiter<TKey>(
        TKey partitionKey,
        Func<TKey, SlidingWindowRateLimiterOptions> factory)
    {
    }

-    public static RateLimitPartition<TKey> CreateFixedWindowLimiter<TKey>(
+    public static RateLimitPartition<TKey> GetFixedWindowLimiter<TKey>(
        TKey partitionKey,
        Func<TKey, FixedWindowRateLimiterOptions> factory)
    {
    }
```
## Rename RateLimiter.WaitAsync

**Approved** | [#runtime/71775](https://github.com/dotnet/runtime/issues/71775#issuecomment-1182219052) | [Video](https://www.youtube.com/watch?v=vhy7LqB2MC4&t=1h16m15s)

* We don't like the proposed name `AcquireAsync` as it would imply that the existing sync `Acquire` method is the non-async counterpart, which some tooling also depends on (e.g. code fixer in async context).
    - We decided on `WaitAndAcquireAsync`

```diff
namespace System.Threading.RateLimiting;

public abstract class RateLimiter : IAsyncDisposable, IDisposable
{
-    public ValueTask<RateLimitLease> WaitAsync(int permitCount = 1, CancellationToken cancellationToken = default);
+    public ValueTask<RateLimitLease> WaitAndAcquireAsync(int permitCount = 1, CancellationToken cancellationToken = default);

-    protected abstract ValueTask<RateLimitLease> WaitAsyncCore(int permitCount, CancellationToken cancellationToken);
+    protected abstract ValueTask<RateLimitLease> WaitAndAcquireAsyncCore(int permitCount, CancellationToken cancellationToken);
}

public abstract class PartitionedRateLimiter<TResource> : IAsyncDisposable, IDisposable
{
-    public ValueTask<RateLimitLease> WaitAsync(TResource resourceID, int permitCount = 1, CancellationToken cancellationToken = default);
+    public ValueTask<RateLimitLease> WaitAndAcquireAsync(TResource resourceID, int permitCount = 1, CancellationToken cancellationToken = default);

-    protected abstract ValueTask<RateLimitLease> WaitAsyncCore(TResource resourceID, int permitCount, CancellationToken cancellationToken);
+    protected abstract ValueTask<RateLimitLease> WaitAndAcquireAsyncCore(TResource resourceID, int permitCount, CancellationToken cancellationToken);
}
```
## IBinaryInteger<TSelf> read methods

**Approved** | [#runtime/71902](https://github.com/dotnet/runtime/issues/71902#issuecomment-1182306312) | [Video](https://www.youtube.com/watch?v=vhy7LqB2MC4&t=1h27m17s)

* Looks good as proposed.
    - Note: The top six will all be DIM'ed

```diff
namespace System.Numerics;

public interface IBinaryInteger<TSelf>
    : IBinaryNumber<TSelf>,
      IShiftOperators<TSelf, int, TSelf>
    where TSelf : IBinaryInteger<TSelf>
{ 
+    public static virtual TSelf ReadBigEndian(byte[] source, bool isUnsigned);
+    public static virtual TSelf ReadBigEndian(byte[] source, int startIndex, bool isUnsigned);
+    public static virtual TSelf ReadBigEndian(ReadOnlySpan<byte> source, bool isUnsigned);

+    public static virtual TSelf ReadLittleEndian(byte[] source, bool isUnsigned);
+    public static virtual TSelf ReadLittleEndian(byte[] source, int startIndex, bool isUnsigned);
+    public static virtual TSelf ReadLittleEndian(ReadOnlySpan<byte> source, bool isUnsigned);

+    public static abstract bool TryReadBigEndian(ReadOnlySpan<byte> source, bool isUnsigned, out TSelf value);
+    public static abstract bool TryReadLittleEndian(ReadOnlySpan<byte> source, bool isUnsigned, out TSelf value);
}
```
