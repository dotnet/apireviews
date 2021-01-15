# Quick Reviews 01/15/2021

## Obsolete RNGCryptoServiceProvider

**Approved** | [#runtime/40169](https://github.com/dotnet/runtime/issues/40169#issuecomment-761105895) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography
{
    [EditorBrowsable(EditorBrowsableState.Never)] // existing attribute
    [Obsolete(DiagnosticId: nextId)] // new attribute
    public sealed class RNGCryptoServiceProvider : RandomNumberGenerator
    { /* ... */ }
}
```
## Obsolete the Rijndael and RijndaelManaged classes

**Approved** | [#runtime/46930](https://github.com/dotnet/runtime/issues/46930#issuecomment-761107227) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h11m49s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography.Algorithms
{
+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class Rijndael
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class RijndaelManaged
    {
    }
}
```
## Obsolete unnecessary cryptographic derived types

**Approved** | [#runtime/46934](https://github.com/dotnet/runtime/issues/46934#issuecomment-761109630) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h14m18s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography.Algorithms
{
+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class AesCryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class AesManaged
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class DESCryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class MD5CryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class RC2CryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA1Managed
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA1CryptoServiceProvider
    {
    }


+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA256Managed
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA256CryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA384Managed
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA384CryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA512Managed
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class SHA512CryptoServiceProvider
    {
    }

+   [Obsolete(someID)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public partial class TripleDESCryptoServiceProvider
    {
    }
}
```
## Analyzer for methods with PlatformNotSupportedException body but missing platform attributes

**Rejected** | [#runtime/42967](https://github.com/dotnet/runtime/issues/42967#issuecomment-761116392) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h19m1s)

Because we don't currently have the ability to mark things as OS-specific based on parameter input, it can only be reliable for methods that are entirely `throw new PlatformNotSupportedException(...)`.  As such, we don't think there's sufficient value in it at this time.

When it is possible to do something more complicated the analyzer should not warn if the method is declared `[Obsolete]`, in addition to the OS-based attributes.
## Add Thread.UnsafeStart to avoid capturing the ExecutionContext 

**Approved** | [#runtime/46594](https://github.com/dotnet/runtime/issues/46594#issuecomment-761119709) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h32m35s)

Looks good as proposed, but we simplified it to use a default parameter.

```C#
namespace System.Threading
{
     public class Thread 
     {
          void UnsafeStart(object? parameter = null);
     }
}
```
## Introduce CallConvMemberFunction to represent the member-function variants of Cdecl, Stdcall, etc. on Windows

**Approved** | [#runtime/46775](https://github.com/dotnet/runtime/issues/46775#issuecomment-761122659) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h39m21s)

Looks good as proposed.

```C#
namespace System.Runtime.CompilerServices
{
    public class CallConvMemberFunction
    {
    }
}
```
## Expose interop manipulation of SafeHandle

**Approved** | [#runtime/46830](https://github.com/dotnet/runtime/issues/46830#issuecomment-761148142) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=0h45m32s)

We think that a hybrid approach is best:

* Update guidance to say that non-abstract SafeHandle types should have a default ctor (where IsInvalid is true).
  * Update all of our SafeHandle types to follow this guidance.
  * Add an analyzer to suggest this guidance.
* Do not make `SafeHandle.SetHandle` public.
* Add `Marshal.InitHandle` (using "Init" instead of "Set" to suggest that it should only be used on new instances).
* The reflection code to create instances of types lacking a public constructor is easy enough that the CreateSafeHandle method isn't required (`(TSafeHandle)Activator.CreateInstance(typeof(TSafeHandle), nonPublic: true)`)

```C#
namespace System.Runtime.InteropServices
{
    public static class Marshal
    {
        /// <summary>
        /// Initializes the handle of a newly created blah blah blah.
        /// </summary>
        /// <param name="safeHandle"><see cref="SafeHandle"/> instance to update</param>
        /// <param name="handle">Pre-existing handle</param>
        public static void InitHandle(SafeHandle safeHandle, IntPtr handle);
    }
}
```
## Add LoggerMessage.Define overload to disable IsEnabled check

**Approved** | [#runtime/45290](https://github.com/dotnet/runtime/issues/45290#issuecomment-761156324) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=1h20m36s)

Looks good as proposed.

```C#
namespace Microsoft.Extensions.Logging
{
    public static class LoggerMessage
    {
        public static Action<ILogger, T1, Exception> Define<T1>(LogLevel logLevel, EventId eventId, string formatString);
        public static Action<ILogger, T1, T2, Exception> Define<T1, T2>(LogLevel logLevel, EventId eventId, string formatString);
        public static Action<ILogger, T1, T2, T3, Exception> Define<T1, T2, T3>(LogLevel logLevel, EventId eventId, string formatString);
        public static Action<ILogger, T1, T2, T3, T4, Exception> Define<T1, T2, T3, T4>(LogLevel logLevel, EventId eventId, string formatString);
        public static Action<ILogger, T1, T2, T3, T4, T5, Exception> Define<T1, T2, T3, T4, T5>(LogLevel logLevel, EventId eventId, string formatString);
        public static Action<ILogger, T1, T2, T3, T4, T5, T6, Exception> Define<T1, T2, T3, T4, T5, T6>(LogLevel logLevel, EventId eventId, string formatString);
    }
}
```
## System.Numerics.Vectors and Span<T>

**Approved** | [#runtime/25608](https://github.com/dotnet/runtime/issues/25608#issuecomment-761168765) | [Video](https://www.youtube.com/watch?v=qi1HHJyPUEQ&t=1h27m48s)

We think spanifying the constructor and CopyTo methods makes sense, but don't think adding the byte-based variants makes sense at the time.

Semantics of these members should match what `Vector<T>` does.

```C#
namespace System.Numerics
{
    public static partial struct Vector2
    {
        public Vector2(ReadOnlySpan<float> values);

        public readonly void CopyTo(Span<float> destination);
        public readonly bool TryCopyTo(Span<float> destination);
    }

    public static partial struct Vector3
    {
        public Vector3(ReadOnlySpan<float> values);

        public readonly void CopyTo(Span<float> destination);
        public readonly bool TryCopyTo(Span<float> destination);
    }

    public static partial struct Vector4
    {
        public Vector4(ReadOnlySpan<float> values);

        public readonly void CopyTo(Span<float> destination);
        public readonly bool TryCopyTo(Span<float> destination);
    }
}
```
