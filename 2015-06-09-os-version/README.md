# API Review 2015-06-09

This API review was also recorded and is available on [YouTube](https://www.youtube.com/watch?v=I-WveN1lSXI).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#1017-os-version-information) | [#1017](https://github.com/dotnet/corefx/issues/1017) OS version information

There are also some [follow-ups for the API review board](#follow-ups-for-api-review-board).

## Notes

### #1017 OS version information

Status: **Approved with feedback** |
[Issue](https://github.com/dotnet/corefx/issues/1017) |
[API Reference](issue-1017.md) |
[Video](https://www.youtube.com/watch?v=I-WveN1lSXI)

* In the original design we used a struct as opposed to an enum. The new
  proposal simply uses an enums
    - We've discussed this aspect extensively. Technically, both approaches
      work but we feel the struct better communicates how we think about the
      OS platforms: essentially a strongly typed string
* How do we coordinate adding new platforms?
    - For newer platforms we'd ask the community to first bake their platform
      (current example would be BSD). We don't have to version the API surface
      and expose new field from the get go; consumers can always construct
      the struct with the appropriate value. Once the platform has adoption,
      we can add an 'official' field that has the appropriate value.
* How do you do version checks?
    - The idea is that this API tells you the OS but the developer is expected
      to drop down to the OS API to ask for more details. This is especially
      required on platform like Windows where they added new APIs to avoid
      version checks if the goal is only to ask for the availability of an API.
* Why don't we use the existing APIs
  (e.g. [PlatformID](https://msdn.microsoft.com/en-us/library/3a8hyw88.aspx),
        [OperatingSystem](https://msdn.microsoft.com/en-us/library/system.operatingsystem.aspx))
    - The existing APIs weren't very well designed
    - Have a lot of platforms that are no longer relevant
    - In order to unblock our cross-platform we likely want to evolve this API
      out of band. If we unify with an existing API we'd be slowed to the
      cadence of .NET Framework, which is too slow for this area.

### Follow-ups for API review board

* [Krzysztof](https://github.com/KrzysztofCwalina): We should talk to DNX about their `IRuntimeEnvironment` class
    - Ideally, we can converge that with this design as well