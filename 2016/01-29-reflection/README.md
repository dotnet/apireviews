# API Review 2016-01-29

This API review was also recorded and is available on [Google Hangouts](https://plus.google.com/events/cim35s6be58n3qnv0s4ebdaevs0)

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#5381-add-missing-members-for-systemreflection-namespace) | [#5381](https://github.com/dotnet/corefx/issues/5381) Add missing members for System.Reflection namespace

## Notes

### #5381 Add missing members for System.Reflection namespace

Status: **Not reviewed** |
[Issue](https://github.com/dotnet/corefx/issues/5381) |
[API Reference](System.Reflection.md) |
[Video](https://plus.google.com/events/cim35s6be58n3qnv0s4ebdaevs0)

* Don't expose the notion of `ReflectionOnlyLoad`
* `GetExecutingAssembly()`
* Don't expose any `RuntimeXxxHandle` APIs
* Don't expose `Resolve` methods that resolve tokens to `*Info` classes
* * Don't expose `ReflectedType`
* Don't expose `GetCurrentMethod`
* `GetInterfaceMap()` isn't implemented by .NET Native and would be costly
    - Use is unclear; can be replaced by calling through the interface
    - Used by DCS and JSON

For more details, take a look at the [API level notes](NetFx_NetCore.md).