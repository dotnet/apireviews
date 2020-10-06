# Quick Reviews 10/06/2020

## Max Array Length Property/Method

**Approved** | [#runtime/31366](https://github.com/dotnet/runtime/issues/31366#issuecomment-704422839) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=0h0m0s)

* Looks good as proposed

```C#
namespace System
{
    public class Array
    {
        public static int GetMaxLength<T>();
    }
}
```

## Add overloads for CancellationToken.[Unsafe]Register that pass the CancellationToken to the registered function

**Approved** | [#runtime/40475](https://github.com/dotnet/runtime/issues/40475#issuecomment-704427405) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=0h13m51s)

* Looks good as proposed
* We omitted the `bool useSynchronizationContext` as we don't want to encourage its use. If needed, we can add it later. The behavior of these new methods is as if it was `false`.

```C#
namespace System.Threading
{
    public struct CancellationToken
    {
        public CancellationTokenRegistration Register(Action<object?, CancellationToken> callback, object? state);
        public CancellationTokenRegistration UnsafeRegister(Action<object?, CancellationToken> callback, object? state);
    }
}
```

## RandomNumberGenerator.GetBytes(int)

**Approved** | [#runtime/42769](https://github.com/dotnet/runtime/issues/42769#issuecomment-704433885) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=0h22m21s)

* Looks good as proposed
* We decided we don't want an overload for `GetNonZeroBytes()` because we don't like that API to begin with

```C#
namespace System.Security.Cryptography
{
    public partial class RandomNumberGenerator
    {
        public static byte[] GetBytes(int byteCount);
    }
}
```

## Add MicrosoftPlatformCryptoProvider to CngProvider

**Approved** | [#runtime/42906](https://github.com/dotnet/runtime/issues/42906#issuecomment-704436463) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=0h34m18s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography
{
    public partial class CngProvider
    {
        public static CngProvider MicrosoftPlatformCryptoProvider { get; }
    }
}
```

## Analyzer suggestion: flag calls to RandomNumberGenerator.GetNonZeroBytes

**NeedsWork** | [#runtime/42763](https://github.com/dotnet/runtime/issues/42763#issuecomment-704445834) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=0h39m2s)

* A custom analyzer would effectively do the same as obsoleting it.
* We should just obsolete it, asssuming we can add turn it off by default by putting the diagnostic ID in the SDK `.editorconfig` root file. We should tie to the security rules
* @GrabYourPitchforks, please follow up to figure out why people call this API. Some usage seems high and it's hard to imagine people are generating secrets.

```C#
namespace System.Security.Cryptography
{
    public abstract class RandomNumberGenerator
    {
        public abstract void GetNonZeroBytes(byte[] data);
        public abstract void GetNonZeroBytes(Span<byte> data);
    }
}
```

## Add ability to update Activity.Baggage

**Approved** | [#runtime/42706](https://github.com/dotnet/runtime/issues/42706#issuecomment-704451527) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=0h56m25s)

* Looks good as proposed

```C#
namespace System.Diagnostics
{
    public partial class Activity
    {
        public Activity SetBaggage(string key, string? value)
    }
}
```

## Port OpenExisting and TryOpenExisting methods for EventWaitHandle/Mutex/Semaphore from .NET Framework

**Approved** | [#runtime/42710](https://github.com/dotnet/runtime/issues/42710#issuecomment-704454746) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=1h6m4s)

* Looks good as proposed, follows the pattern

```C#
namespace System.Threading
{
    public static partial class EventWaitHandleAcl
    {
        public static EventWaitHandle OpenExisting(string name, EventWaitHandleRights rights);
        public static bool TryOpenExisting(string name, EventWaitHandleRights rights, out EventWaitHandle result);
    }
    public static partial class MutexAcl
    {
        public static Mutex OpenExisting(string name, MutexRights rights);
        public static bool TryOpenExisting(string name, MutexRights rights, out Mutex result);
    }
    public static partial class SemaphoreAcl
    {
        public static Semaphore OpenExisting(string name, SemaphoreRights rights);
        public static bool TryOpenExisting(string name, SemaphoreRights rights, out Semaphore result);
    }
}
```

## Add ParseValue methods to JsonElement

**Approved** | [#runtime/42755](https://github.com/dotnet/runtime/issues/42755#issuecomment-704460950) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=1h11m48s)

* Looks good as proposed

```C#
namespace System.Text.Json
{
    public partial struct JsonElement
    {
        public static JsonElement ParseValue(ref Utf8JsonReader reader);
        public static bool TryParseValue(ref Utf8JsonReader reader, [NotNullWhen(true)] out JsonElement? element);
    }
}
```

## Enable AsVector<T> in Vector<T>

**Approved** | [#runtime/508](https://github.com/dotnet/runtime/issues/508#issuecomment-704464181) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=1h21m20s)

* Looks good as proposed, unless @CarolEidt has concerns

```C#
namespace System.Numerics
{
    public partial class Vector
    {
        public static Vector<TTo> As<TFrom, TTo>(Vector<TFrom> vector)
            where TFrom : struct
            where TTo : struct;
    }
}
```

## Span<char> from null-terminated char*

**Approved** | [#runtime/40202](https://github.com/dotnet/runtime/issues/40202#issuecomment-704487238) | [Video](https://www.youtube.com/watch?v=5nMDURGNsQ0&t=1h26m8s)

* The API should return `Span<T>`, we can add a `ReadOnly` version later
* We should throw `InvalidOperationException` when the size would exceed `int.MaxValue`.

```C#
namespace System.Runtime.InteropServices
{
    public static class MemoryMarshal
    {
        public static unsafe Span<char> CreateSpanFromNullTerminated(char* value);
        public static unsafe Span<byte> CreateSpanFromNullTerminated(byte* value);
    }
}
```

