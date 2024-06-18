# API Review 06/18/2024

## New overloads for named wait handles that enable creating/opening user-specific synchronization primitives

**Approved** | [#runtime/102682](https://github.com/dotnet/runtime/issues/102682#issuecomment-2176700064) | [Video](https://www.youtube.com/watch?v=FzL-Knw-Ngo&t=0h0m0s)


* We changed the flags enum to a struct
* The default value for the struct will be current-user in current-scope.  The property names that were chosen mean they have to default to `true`.
* We've marked all the existing constructors / OpenExisting methods as `[Obsolete]`.  Right now SYSLIB0057 is the next available diagnostic ID, but use whatever is next in `Obsoletions.cs` when creating the PR.
* It's felt that current-user+current-scope is the best default going forward, but that we shouldn't change the behavior for the existing members for .NET 9.  We may revise this for .NET 10, since there will have been an intervening release with the obsoletion.

```C#
namespace System.Threading
{
    public struct NamedWaitHandleOptions
    {
        private bool _allUsers;
        private bool _allSessions;

        // Note that for default(NamedWaitHandleOptions), both of these properties will return `true`.

        public bool CurrentUserOnly { get => !_allUsers; set => _allUsers = !value; }
        public bool CurrentSessionOnly { get => !_allSessions; set => _allSessions = !value; }
    }

    public sealed partial class Mutex : WaitHandle
    {
        //
        // Proposed
        //

        public Mutex(string? name, NamedWaitHandleOptions options) { }
        public Mutex(bool initiallyOwned, string? name, NamedWaitHandleOptions options) { }
        public Mutex(bool initiallyOwned, string? name, NamedWaitHandleOptions options, out bool createdNew) { }
        public static Mutex OpenExisting(string name, NamedWaitHandleOptions options) { }
        public static bool TryOpenExisting(string name, NamedWaitHandleOptions options, [NotNullWhen(true)] out Mutex? result) { }

        //
        // Existing
        //
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public Mutex(bool initiallyOwned, string? name) { }
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public Mutex(bool initiallyOwned, string? name, out bool createdNew) { }
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public static Mutex OpenExisting(string name) { }
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public static bool TryOpenExisting(string name, [NotNullWhen(true)] out Mutex? result) { }
    }

    public sealed partial class Semaphore : WaitHandle
    {
        //
        // Proposed
        //

        public Semaphore(int initialCount, int maximumCount, string? name, NamedWaitHandleOptions options) { }
        public Semaphore(int initialCount, int maximumCount, string? name, NamedWaitHandleOptions options, out bool createdNew) { }
        [SupportedOSPlatform("windows")]
        public static Semaphore OpenExisting(string name, NamedWaitHandleOptions options) { }
        [SupportedOSPlatform("windows")]
        public static bool TryOpenExisting(string name, NamedWaitHandleOptions options, [NotNullWhen(true)] out Semaphore? result) { }

        //
        // Existing
        //

        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public Semaphore(int initialCount, int maximumCount, string? name) { }
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public Semaphore(int initialCount, int maximumCount, string? name, out bool createdNew) { }
        [SupportedOSPlatform("windows")]
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public static Semaphore OpenExisting(string name) { }
        [SupportedOSPlatform("windows")]
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public static bool TryOpenExisting(string name, [NotNullWhen(true)] out Semaphore? result) { }
    }

    public partial class EventWaitHandle : WaitHandle
    {
        //
        // Proposed
        //

        public EventWaitHandle(bool initialState, EventResetMode mode, string? name, NamedWaitHandleOptions options) { }
        public EventWaitHandle(bool initialState, EventResetMode mode, string? name, NamedWaitHandleOptions options, out bool createdNew) { }
        [SupportedOSPlatform("windows")]
        public static EventWaitHandle OpenExisting(string name, NamedWaitHandleOptions options) { }
        [SupportedOSPlatform("windows")]
        public static bool TryOpenExisting(string name, NamedWaitHandleOptions options, [NotNullWhen(true)] out EventWaitHandle? result) { }

        //
        // Existing
        //

        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public EventWaitHandle(bool initialState, EventResetMode mode, string? name) { }
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public EventWaitHandle(bool initialState, EventResetMode mode, string? name, out bool createdNew) { }
        [SupportedOSPlatform("windows")]
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public static EventWaitHandle OpenExisting(string name) { }
        [SupportedOSPlatform("windows")]
        [Obsolete("some message", DiagnosticId = "SYSLIB0057")]
        public static bool TryOpenExisting(string name, [NotNullWhen(true)] out EventWaitHandle? result) { }
    }
}
```

## Volatile barrier APIs

**Approved** | [#runtime/98837](https://github.com/dotnet/runtime/issues/98837#issuecomment-2176777100) | [Video](https://www.youtube.com/watch?v=FzL-Knw-Ngo&t=1h12m2s)

Looks good as proposed.

There was a very long discussion about memory models, what the barrier semantics are, and whether we want to do something more generalized in this release.  In the end, we accepted the original proposal. 

```C#
namespace System.Threading;

public static class Volatile
{
    public static void ReadBarrier();
    public static void WriteBarrier();
}
```

