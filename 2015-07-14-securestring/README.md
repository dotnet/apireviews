# API Review 2015-07-14

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cms40br51f6vt7eg73grju99690).

## Overview

In this API review we discussed what we do with `SecureString` moving forward.

## Problems with `SecureString`

Status: **Under investigation** |
[Video](https://plus.google.com/events/cms40br51f6vt7eg73grju99690)

* The purpose of `SecureString` is to avoid having secrets stored in the process
  memory as plain text.
* However, even on Windows `SecureString` doesn't exist as an OS concept
    - It just makes the window getting the plain text shorter; it doesn't fully
      prevent it as .NET still has to convert the string to a plain text
      representation
    - The benefit is that the plain text representation doesn't hang around
      as an instance of `System.String` -- the lifetime of the native buffer is
      shorter.
* On Windows the contents of the internal char array is encrypted. We've
  trouble with supporting the encryption in all environments, either due to
  missing APIs or key management issues.
* Does `SecureString` provide value if we wouldn't encrypt?
    - For example, we could make the GC understand the type in order to keep
      the window small, by, for example, zeroing the memory more aggressively.
    - Benefit seems limited though
    - Name may provide a false sense of security
* In general, there is a move away from using passwords
    - Ideally, we'd have an abstraction to store a reference to a means for an
      authenticated user
    - APIs would then take the abstraction as opposed to individual pieces of
      of information, such as user name / password.

### Conclusion

* We probably shouldn't expose `SecureString` in .NET Core
* The only incoming API dependency is `Process`. The primary consumer of the
  APIs involving `SecureString` is PowerShell
    - We *may* be able remove that entirely
* Networking also uses `SecureString` but only in its implementation

### Follow-ups

1. @weshaggard will talk to PowerShell to see whether we can remove the `SecureString`
    - Need a contract without user/password which will be a proper subset of the .NET Framework 4.6
    - Need another contract that adds new APIs for user/password which will not be supported by .NET Framework 4.6 but a later version
2. If we can't remove the APIs:
    - We could could give PowerShell the code to make an implementation dependency
    - We should remove encryption from `SecureString` across all platforms in .NET Core
    - We should obsolete `SecureString`
3. If we need to keep `SecureString` we could try to make it useful *without*
   encryption, for example, by adding a way to zero out the objects when the
   process crashes

## T-Shirts!

You may have noticed the shirt @terrajobst was wearing:

![](shirt.jpg)

Kudos to @kangaroo! Based [on this Twitter discussion][shirt-tweet] he sent us
a few of these shirts. They shall serve us as a reminder to act responsibly
when reviewing designs :-)

[shirt-tweet]: https://twitter.com/terrajobst/status/616094502579650560