# API Review 02/13/2024

## Add managed API surface area for ByRefLike Generic constraints

**Approved** | [#runtime/68002](https://github.com/dotnet/runtime/issues/68002#issuecomment-1942166436) | [Video](https://www.youtube.com/watch?v=9qhbPMCwFuM&t=0h0m0s)

* We decided not to obsolete SpecialConstraintMask, which we can revisit later.
* "AcceptByRefLike" => "AllowByRefLike"
* GenericsAllowByRefLike => ByRefLikeGenerics, to follow the pattern from ByRefFields

```diff
namespace System.Reflection
{
    [Flags]
    public enum GenericParameterAttributes
    {
         SpecialConstraintMask = 0x001C,
+        AllowByRefLike = 0x0020,
    }
}

namespace System.Runtime.CompilerServices
{
    public static partial class RuntimeFeature
    {
+        /// <summary>
+        /// Represents a runtime feature where byref-like types can be used in Generic parameters.
+        /// </summary>
+        public const string ByRefLikeGenerics = nameof(ByRefLikeGenerics);
    }
}
```
## Cannot set ReadMode after creating a NamedPipeClientStream instance

**Approved** | [#runtime/83072](https://github.com/dotnet/runtime/issues/83072#issuecomment-1942189738) | [Video](https://www.youtube.com/watch?v=9qhbPMCwFuM&t=0h32m25s)

Looks good as proposed.  We discussed whether PipeAccessRights needed SupportedOSPlatform("windows"), and decided that "no" was the correct answer.

The nullability annotations should be made correct based on what .NET Framework accepts/rejects.

```C#
namespace System.IO.Pipes;

// existing type
public sealed partial class NamedPipeClientStream : PipeStream
{
    // new constructor
    [SupportedOSPlatform("windows")]
    public NamedPipeClientStream(String serverName, String pipeName, PipeAccessRights desiredAccessRights,
           PipeOptions options, TokenImpersonationLevel impersonationLevel, HandleInheritability inheritability)
}

// existing type moved from System.IO.Pipes.AccessControl assembly.
[System.FlagsAttribute]
public enum PipeAccessRights
{
    ReadData = 1,
    WriteData = 2,
    CreateNewInstance = 4,
    ReadExtendedAttributes = 8,
    WriteExtendedAttributes = 16,
    ReadAttributes = 128,
    WriteAttributes = 256,
    Write = 274,
    Delete = 65536,
    ReadPermissions = 131072,
    Read = 131209,
    ReadWrite = 131483,
    ChangePermissions = 262144,
    TakeOwnership = 524288,
    Synchronize = 1048576,
    FullControl = 2032031,
    AccessSystemSecurity = 16777216,
}
```
## Add first class System.Threading.Lock type

**Approved** | [#runtime/34812](https://github.com/dotnet/runtime/issues/34812#issuecomment-1942213794) | [Video](https://www.youtube.com/watch?v=9qhbPMCwFuM&t=0h49m12s)

Looks good as proposed.

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
        public Scope EnterScope();

        public ref struct Scope
        {
            // Exits the lock. No-op if Dispose was already called.
            // Exceptions: SynchronizationLockException if the current thread does not own the lock
            public void Dispose();
        }
    }
}
```
## Additional Hash Algorithms for X509SubjectKeyIdentifierHashAlgorithm

**Approved** | [#runtime/97158](https://github.com/dotnet/runtime/issues/97158#issuecomment-1942230185) | [Video](https://www.youtube.com/watch?v=9qhbPMCwFuM&t=1h6m45s)

* We dropped the RFC7093 prefix, that part can be explained in docs.
* We went with "Short" instead of "Truncated".  Even though it's a different style of truncation algorithm it's not important to distinguish in the API.
* @terrajobst was bothered by the old values being "(Full)" followed by "Short", so we flipped 3-5 and 6-8.

```diff
public enum X509SubjectKeyIdentifierHashAlgorithm {
    Sha1 = 0, // SHA-1 over the subjectPublicKey
    ShortSha1 = 1, // 0b0100 concat rightmost 60 bits of the SHA-1 over subjectPublicKey
    CapiSha1 = 2, // Some CAPI thing
+   Sha256 = 3, // SHA-256 hash over the full SubjectPublicKeyInfo
+   Sha384 = 4, // SHA-384 hash over the full SubjectPublicKeyInfo
+   Sha512 = 5, // SHA-512 hash over the full SubjectPublicKeyInfo
+   ShortSha256 = 6, //  leftmost 160-bits of the SHA-256 hash over subjectPublicKey
+   ShortSha384 = 7, //  leftmost 160-bits of the SHA-384 hash over subjectPublicKey
+   ShortSha512 = 8, //  leftmost 160-bits of the SHA-512 hash over subjectPublicKey
}
## Safe API for covariant cast of ReadOnlySpan<T>

**Approved** | [#runtime/96952](https://github.com/dotnet/runtime/issues/96952#issuecomment-1942261511) | [Video](https://www.youtube.com/watch?v=9qhbPMCwFuM&t=1h18m51s)

We opted for the static method on `ReadOnlySpan<T>` as that ends up only needing to specify one type argument at the call site, vs two for the extension method.

```C#
namespace System;

public ref struct ReadOnlySpan<T>
{
    public static ReadOnlySpan<T> CastUp<TDerived>(ReadOnlySpan<TDerived> items) where TDerived : class, T;
}
```
## BFloat16

**Approved** | [#runtime/96295](https://github.com/dotnet/runtime/issues/96295#issuecomment-1942329998) | [Video](https://www.youtube.com/watch?v=9qhbPMCwFuM&t=1h30m45s)

Looks good as proposed.  Also with whatever level of generic math (and public visibility thereof) is appropriate.  (`IFloatingPointIeee754<BFloat16>`, most probably)

```C#
namespace System.Numerics
{
    public readonly struct BFloat16
      : IComparable,
        IComparable<BFloat16>,
        IEquatable<BFloat16>
    {
        public static BFloat16 Epsilon { get; }
        public static BFloat16 MinValue { get; }
        public static BFloat16 MaxValue { get; }
                
        // Casting
        public static explicit operator BFloat16(float value);
        public static explicit operator BFloat16(double value);
        public static explicit operator float(BFloat16 value);
        public static explicit operator double(BFloat16 value);

        // Comparison
        public int CompareTo(object value);
        public int CompareTo(BFloat16 value);
        public static bool operator ==(BFloat16 left, BFloat16 right);
        public static bool operator !=(BFloat16 left, BFloat16 right);
        public static bool operator <(BFloat16 left, BFloat16 right);
        public static bool operator >(BFloat16 left, BFloat16 right);
        public static bool operator <=(BFloat16 left, BFloat16 right);
        public static bool operator >=(BFloat16 left, BFloat16 right);

        // Equality
        public bool Equals(BFloat16 obj);
        public override bool Equals(object? obj);
        public override int GetHashCode();
        
        // ToString override
        public override string ToString();
    }
}
```
