# Quick Reviews 06/01/2021

## General purpose non-cryptographic hashing API for .NET

**Approved** | [#runtime/24328](https://github.com/dotnet/runtime/issues/24328#issuecomment-852297606) | [Video](https://www.youtube.com/watch?v=8K42B4aH34s&t=0h0m0s)

* Since the ctors for XxHash* are being overloaded for usability, also overload the `byte[] => byte[]` statics for usability.  The other versions are complicated enough that the default is OK.

```C#
namespace System.IO.Hashing
{
    public abstract class NonCryptographicHashAlgorithm
    {
        public int HashLengthInBytes { get; }

        protected NonCryptographicHashAlgorithm(int hashLengthInBytes);

        public abstract void Append(ReadOnlySpan<byte> source);
        public abstract void Reset();
        protected abstract void GetCurrentHashCore(Span<byte> destination);

        public void Append(byte[] source);
        public void Append(Stream stream);
        public Task AppendAsync(Stream stream, CancellationToken cancellationToken = default);
        public byte[] GetCurrentHash();
        public bool TryGetCurrentHash(Span<byte> destination, out int bytesWritten);
        public int GetCurrentHash(Span<byte> destination);
        public byte[] GetHashAndReset();
        public bool TryGetHashAndReset(Span<byte> destination, out int bytesWritten);
        public int GetHashAndReset(Span<byte> destination);

        protected virtual void GetHashAndResetCore(Span<byte> destination);

        [EditorBrowsable(EditorBrowsableState.Never)]
        [Obsolete("Use GetCurrentHash() to retrieve the computed hash code.", true)]
        public int GetHashCode();
    }
    public sealed class XxHash32 : NonCryptographicHashAlgorithm
    {
        public XxHash32();
        public XxHash32(int seed);
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(byte[] source, int seed = 0);
        public static byte[] Hash(ReadOnlySpan<byte> source, int seed = 0);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, int seed = 0);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination, int seed = 0);
    }
    public sealed class XxHash64 : NonCryptographicHashAlgorithm
    {
        public XxHash64();
        public XxHash64(long seed);
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(byte[] source, long seed);
        public static byte[] Hash(ReadOnlySpan<byte> source, long seed = 0);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, long seed = 0);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination, long seed = 0);
    }
    public sealed class Crc32 : NonCryptographicHashAlgorithm
    {
        public Crc32();
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(ReadOnlySpan<byte> source);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination);
    }
    public sealed class Crc64 : NonCryptographicHashAlgorithm
    {
        public Crc64();
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(ReadOnlySpan<byte> source);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination);
    }
}
```
## Resource limit APIs

**NeedsWork** | [#runtime/52079](https://github.com/dotnet/runtime/issues/52079#issuecomment-852384542) | [Video](https://www.youtube.com/watch?v=8K42B4aH34s&t=0h7m37s)

API Review notes:

* The current design says that Dispose must be called only once.  That is against our general IDisposable guidelines, because sometimes the state of things gets tied together.  (The first Dispose should do the release semantics, subsequent calls should no-op).
  * There was discussion that it should look like IValueTaskSource, with the struct just having an incremented ID that represents which request it is.  (e.g. `public long LeaseId { get; }`)
  * Another alternative is just making ResourceLease be a class, especially if "state" is allocated for each call to Acquire.
* The `public object State` on the lease doesn't feel like it's set up to be successful in solving the scenarios that it is projected for.
  * One suggested approach is an `IReadOnlyDictionary<SomeKey,object>`, where `SomeKey` is a strongly typed string.
* "EstimatedCount" isn't really clear (available?, total? currently consumed?)
  * Something like EstimatedAvailability would better say what it is a count of.
* EstimatedCount also probably shouldn't be a property, so that it can better convey it's a point-in-time (and thus shouldn't be used in a for loop condition).
  * Together, this becomes something like `GetEstimatedAvailability()`
* "ResourceLimiter" feels a bit over-reaching.  "RateLimiter" seems more appropriate to the described situations. (Though "RateLease" does feel odd)
* The namespace has both concern with "Threading" and "ResourceLimits", the "ResourceLimits" being the same over-reaching concern of ResourceLimiter.
* Next time @bartonjs will probably suggest that Acquire and WaitAsync use the template method pattern.
  * public ResourceLease Acquire(int requestedCount) { if (requestedCount < 0) { throw new ArgumentOutOfRangeException(nameof(requestedCount) } return AcquireCore(requestedCount); }
  * protected abstract ResourceLease AcquireCore(int requestedCount);
  * Same for WaitAsync

