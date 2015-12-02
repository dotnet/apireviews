# API Review 2015-11-24

This API review was also recorded and is available on [Google+](https://plus.google.com/events/ck32jai0jurth6849hkldto1o98).

## Overview

In this API review we took a look at the new networking APIs we're adding to
.NET Core.

## Notes

### Networking

Status: **Approved with Feedback** |
[API Rev (All)](All ASPNET5 contracts.md) |
[API Rev (Desktop vs NetCore)](Desktop_vs_ASPNET5 contracts.md) |
[Video](https://plus.google.com/events/ck32jai0jurth6849hkldto1o98)

* The new APIs are fully OOB on desktop
* Should `SocketTaskExtensions` ship OOB on desktop or should we add to the
  desktop implementations?
    - For now the answer is: we can implement it on top
* The `Func<>` for the server callback is a bit messy but it's preferred over
  a named delegate because:
    - There is an existing named delegate that takes a cert 1 that we can't
      reuse, so we'd have to come with a new name
    - There are dependency issues for the arguments; there is no good contract
      in which we could put the named delegate
* `SocketReceiveFromResult` and `SocketReceiveMessageFromResult` both use
  public fields -- shouldn't those be properties?
