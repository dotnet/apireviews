# API Review 09/12/2023

## architecture definitions for RISC-V ISA

**Approved** | [#runtime/73437](https://github.com/dotnet/runtime/issues/73437#issuecomment-1716122572)

We changed the casing from `Riscv` to `RiscV` since the V component is a different pronounced word.

```diff
namespace System.Runtime.InteropServices;

public enum Architecture
{
    ...
+   RiscV64,
}

namespace System.Reflection.PortableExecutable;

public enum Machine : ushort
{
    ...
+   RiscV32 = 0x5032,
+   RiscV64 = 0x5064,
+   RiscV128 = 0x5128,
}
```
## Add first class System.Threading.Lock type

**Approved** | [#runtime/34812](https://github.com/dotnet/runtime/issues/34812#issuecomment-1716143748)

The TryEnterScope methods create for an odd pattern of a) a Try method not returning a boolean answer, and b) require either using a nested type in a common pattern or we need to move the type out to be a sibling.  So we think the `if (TryEnter()) try { .. } finally { Exit(); }}` pattern is good enough.

```C#
namespace System.Threading
{
    // This is a lock that can be entered by one thread at a time. A thread may enter the lock recursively multiple
    // times, in which case the thread should also exit the lock the same number of times to fully exit the lock.
    public sealed class Lock
    {
        // Exceptions:
        //   - LockRecursionException if the lock was entered recursively by the current thread the maximum number of times
        public void Enter();

        // Returns true if the lock was acquired by the current thread, false otherwise.
        // Exceptions:
        //   - LockRecursionException if the lock was entered recursively by the current thread the maximum number of times.
        //     In this corner case an exception would be better than returning false, as the calling thread cannot make
        //     further progress with entering the lock before exiting it.
        public bool TryEnter();

        // Returns true if the lock was acquired by the current thread, false otherwise.
        // Exceptions:
        //   - ArgumentOutOfRangeException if the timeout value converted to an integer milliseconds value is negative
        //     and not equal to -1, or greater than Int32.MaxValue
        //   - LockRecursionException if the lock was entered recursively by the current thread the maximum number of times
        //     In this corner case an exception would be better than returning false, as the calling thread cannot make
        //     further progress with entering the lock before exiting it.
        public bool TryEnter(TimeSpan timeout);

        // Returns true if the lock was acquired, false otherwise.
        // Exceptions:
        //   - ArgumentOutOfRangeException if the timeout value is negative and not equal to -1
        //   - LockRecursionException if the lock was entered recursively by the current thread the maximum number of times
        //     In this corner case an exception would be better than returning false, as the calling thread cannot make
        //     further progress with entering the lock before exiting it.
        public bool TryEnter(int millisecondsTimeout);

        // Exceptions: SynchronizationLockException if the current thread does not own the lock
        public void Exit();

        // Returns true if the current thread holds the lock, false otherwise.
        //
        // This is analogous to Monitor.IsEntered(), but more similar to SpinLock.IsHeldByCurrentThread. There could
        // conceivably be a Lock.IsHeld as well similarly to SpinLock.IsHeld, or Lock.IsHeldByAnyThread to be more
        // precise, which would return true if the lock is held by any thread.
        public bool IsHeldByCurrentThread { get; }

        // Enters the lock and returns a holder. Enables integration with the "lock" keyword.
        // Exceptions:
        //   - LockRecursionException if the lock was entered recursively by the current thread the maximum number of times
        public Scope EnterLockScope();

        public ref struct Scope
        {
            // Exits the lock. No-op if Dispose was already called.
            // Exceptions: SynchronizationLockException if the current thread does not own the lock
            public void Dispose();
        }
    }
}
```
## Hash and HMAC one-shots for HashAlgorithName

**Approved** | [#runtime/91407](https://github.com/dotnet/runtime/issues/91407#issuecomment-1716154520)

Looks good as proposed.

```C#
namespace System.Security.Cryptography;

// Existing class
public static partial class CryptographicOperations {
        public static byte[] HmacData(HashAlgorithmName hashAlgorithm, byte[] key, byte[] source);
        public static byte[] HmacData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);
        public static int HmacData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);
        public static bool TryHmacData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

        public static byte[] HmacData(HashAlgorithmName hashAlgorithm, byte[] key, Stream source);
        public static byte[] HmacData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, Stream source);
        public static int HmacData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, Stream source, Span<byte> destination);
        public static ValueTask<byte[]> HmacDataAsync(HashAlgorithmName hashAlgorithm, byte[] key, Stream source, CancellationToken cancellationToken = default);
        public static ValueTask<int> HmacDataAsync(HashAlgorithmName hashAlgorithm, ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HmacDataAsync(HashAlgorithmName hashAlgorithm, ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);

        public static byte[] HashData(HashAlgorithmName hashAlgorithm, byte[] source);
        public static byte[] HashData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> source);
        public static int HashData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> source, Span<byte> destination);
        public static bool TryHashData(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

        public static byte[] HashData(HashAlgorithmName hashAlgorithm, Stream source);
        public static int HashData(HashAlgorithmName hashAlgorithm, Stream source, Span<byte> destination);
        public static ValueTask<int> HashDataAsync(HashAlgorithmName hashAlgorithm, Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
        public static ValueTask<byte[]> HashDataAsync(HashAlgorithmName hashAlgorithm, Stream source, CancellationToken cancellationToken = default);
}
```
## Linq CountBy method

**Approved** | [#runtime/77716](https://github.com/dotnet/runtime/issues/77716#issuecomment-1716216479)

After a long discussion we ended up back at the previously-approved shape, with a delayed evaluation.

```C#
namespace System.Linq;

public static partial class Enumerable
{
    public static IEnumerable<KeyValuePair<TKey, int>> CountBy<TSource, TKey>(
        this IEnumerable<TSource> source,
        Func<TSource, TKey> keySelector,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
}

public static partial class Queryable
{
    public static IQueryable<KeyValuePair<TKey, int>> CountBy<TSource, TKey>(
        this IQueryable<TSource> source,
        Expression<Func<TSource, TKey>> keySelector,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
}
```
## Add Int128 and UInt128 `BitConverter.GetBytes` support

**Approved** | [#runtime/80337](https://github.com/dotnet/runtime/issues/80337#issuecomment-1716219637)

Looks good as proposed.

```C#
namespace System;

public static partial class BitConverter
{
    public static byte[] GetBytes(Int128 value);
    public static Int128 ToInt128(ReadOnlySpan<byte> value);
    public static Int128 ToInt128(byte[] value, int startIndex);
    public static bool TryWriteBytes(Span<byte> destination, Int128 value);

    public static byte[] GetBytes(UInt128 value);
    public static UInt128 ToUInt128(ReadOnlySpan<byte> value);
    public static UInt128 ToUInt128(byte[] value, int startIndex);
    public static bool TryWriteBytes(Span<byte> destination, UInt128 value);
}
```
## Consider adding an `AggregateBy` method to LINQ

**Approved** | [#runtime/91533](https://github.com/dotnet/runtime/issues/91533#issuecomment-1716241537)

* We changed `<TSource, TAccumulate, TKey>` to `<TSource, TKey, TAccumulate>`
* We moved `keySelector` to be the second parameter.
* We removed a `TKey` from the `func` on the second overload.  If it's actually useful we can add it to both shapes in the future.

```C#
namespace System.Linq;

public static partial class Enumerable
{
    public static IEnumerable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(
        this IEnumerable<TSource> source,
        Func<TSource, TKey> keySelector,
        TAccumulate seed,
        Func<TAccumulate, TSource, TAccumulate> func,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;

    public static IEnumerable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(
        this IEnumerable<TSource> source,
        Func<TSource, TKey> keySelector, 
        Func<TKey, TAccumulate> seed,
        Func<TAccumulate, TSource, TAccumulate> func,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
}

public static partial class Queryable
{
    public static IQueryable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(
        this IQueryable<TSource> source,
        Expression<Func<TSource, TKey>> keySelector,
        TAccumulate seed,
        Expression<Func<TAccumulate, TSource, TAccumulate>> func,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;

    public static IQueryable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(
        this IQueryable<TSource> source,
        Expression<Func<TSource, TKey>> keySelector, 
        Expression<Func<TKey, TAccumulate>> seed,
        Expression<Func<TAccumulate, TSource, TAccumulate>> func,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
}
```

