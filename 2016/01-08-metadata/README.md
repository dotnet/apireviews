# API Review 2016-01-08

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c8o1cl7joerpcalc48ema1l2n1c).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#262-systemreflectionmetadata-should-provide-easier-signature-reading-api) | [#262](https://github.com/dotnet/corefx/issues/262) System.Reflection.Metadata should provide easier signature reading API
* [Notes](#5212-api-review-systemreflectionmetadata-debug-directory-reading-misc) | [#5212](https://github.com/dotnet/corefx/issues/5212) API review: System.Reflection.Metadata debug directory reading + misc

## Notes

### #262 System.Reflection.Metadata should provide easier signature reading API

Status: **Approved with Feedback** |
[Issue](https://github.com/dotnet/corefx/issues/262) |

* The providers could need context, so it might be good to introduce a generic
  parameter that represents the context.
* They should probably be abstract types
* `GetGenericInstance` -> `GetGenericInstantiation`

### #5212 API review: System.Reflection.Metadata debug directory reading + misc

Status: **Approved with Feedback** |
[Issue](https://github.com/dotnet/corefx/issues/5212) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2016-01-08#time=0h0m0s)

* `MethodSemanticsAttribute` should be in `System.Reflection.Metadata`
    - Assuming, we already add enums to `System.Reflection.Metadata`
* Mark assembly as `[CLSCompliant(true)]` and mark individual APIs as
  `[CLSCompliant(false)]`
