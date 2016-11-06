# API Review 2015-08-11

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cat3257nc9bb9d8snl3l5d1miu0).

## Overview

In this API review we discussed several proposals on how to update `Process`
so that we can remove the API dependency to `SecureString` while preserving
the ability to start processes with a given user name and password.

## Updating `Process` to remove `SecureString`

Status: **Approved** |
[API Diff](System.Diagnostics.Process_v4.1.0.0.md) |
[Video](https://plus.google.com/events/cat3257nc9bb9d8snl3l5d1miu0)

### Contract Versions

In order to preserve the ability to start new process, we need to expose new
APIs that instead of taking `SecureString` take `string`. However, we just
shipped .NET Framework 4.6, so we can't make this API set immediately compatible
with .NET Framework.

So in order to have a version of `Process` that works on both, .NET Core as well
as .NET Framework, we need to expose a contract that doesn't have these new
APIs. This contract (versioned as 4.0.0) will be compatible with both platforms.

In addition, we'll create a new version of `System.Diagnostics.Process.dll`
that will have the new APIs. This contract will be versioned as 4.1.0.0.

### Proposals for the new APIs in 4.1.0.0

1. Only expose `Process.Start` static APIs that take in the credentials options.

    ```C#
    public static Process Start(string userName, string password, string domain);
    public static Process Start(string userName, string password, string domain, bool loadUserProfile); // or just this one.
    ```

    - **Pros**. Minimum API additions, removes the need for a property like
      `PlainTextPassword`  and the `Password` string is not rooted.
    - **Cons**. Some of the `Process` scenarios can't be supported this way. For
      example no way to redirect streams when a process is created with
      credentials.

2. Build on top of (1) and expose static APIs that take in `ProcessStartInfo`
   and credentials

    ```C#
    public static Process Start(ProcessStartInfo startInfo, string userName, string password, string domain);
    public static Process Start(ProcessStartInfo startInfo, string userName, string password, string domain, bool loadUserProfile); // or just this one.
    ```

    - **Pros**. Same as above
    - **Cons**. There will be still some scenarios that won't light up for
      example no way to support `OnExited` and `async` stream reading when a
      process is created with credentials (they are only supported via
      `Process.Start()` instance method).

3. Build on top of (2) and expose variants of `Process.Start()` instance methods
   that take in credentials.

    ```C#
    public static Process Start(ProcessStartInfo startInfo, string userName, string password, string domain);
    public static Process Start(ProcessStartInfo startInfo, string userName, string password, string domain, bool loadUserProfile); // or just this one.
    public bool Start(string userName, string password, string domain);
    public bool Start(string userName, string password, string domain, bool loadUserProfile); // or just this one.
    ```

    - **Pros**. Same as above
    - **Cons**. We add new APIs to an already long list of `Process.Start()`
      options. Desktop design becomes much more complex where credentials can be
      added via `ProcessStartInfo` and new Start APIs.

4. Proposed contract

    ```C#
    namespace System.Diagnostics {
        public class Process : IDisposable {
            public static Process Start(string fileName, string userName, string password, string domain);
            public static Process Start(string fileName, string arguments, string userName, string password, string domain);
        }
        public sealed class ProcessStartInfo {
            public string Domain { get; set; }
            public bool LoadUserProfile { get; set; }
            public string PlainTextPassword { get; set; }
            public string UserName { get; set; }
       }
    }
    ```

    - **Pros**. Story remains more or less the same and the Desktop gets to stay
      cleaner.
    - **Cons**. Adds an alternate property for `Password` and keeps it rooted.

### Conclusion

We all agreed that the rooting argument for the password is artificial. It's a
knee-jerk reaction that doesn't quite make sense given how the system is
implemented. In end, the string will put the password on the heap anyways. The
rooting doesn't really change that. Granted, it *may* prolong the lifetime of
the string but in many cases the lifetime of the `ProcessStartInfo` will likely
be identical.

We feel that creating a process with credentials is an advanced scenario. Hence,
we feel we don't need to expose a static convenience helper that can create
processes with different credentials. This avoids the problem of having two
overloads on desktop that have both, `string` versions as well as `SecureString`
versions.

The only downside is that this may make people's life harder when they port
.NET Framework code to .NET Core. However, none of the apps in our compat lab
are using the existing static methods that take credentials.

This means we only need to add a single API to the .NET Framework:
`ProcessStartInfo.PasswordInClearText`.

This begs the question what behavior we should have if consumers on the .NET
Framework set both, the existing `ProcessStartInfo.Password` as well as the
new `ProcessStartInfo.PasswordInClearText`. We concluded that we throw from
the instance `Process.Start` method if we detect both properties are non-null.
In other words, we'll require callers to either use `Password` or
`PasswordInClearText`.

### Follow-ups

* [PallaviT](http://github.com/pallavit) will make the changes to `Process`