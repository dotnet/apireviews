# API Review 2015-10-30

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cquf537h1tijs1aq9gf548f8eqg).

## Overview

In this API review we reviewed additions to the Roslyn interactive APIs,
AKA scripting APIs.

## Notes

### Environment.GetCommandLineArgs()

* We don't expose the `CommandLine` property
    - Can't implement meaningfully x-plat, shell expanded wildcards etc.
* The native CLR host can control behavior already, thus we don't expose a
  managed way to control the args
* We'll expose `Environment.GetCommandLineArgs()` as-proposed

### RuntimeInformation should expose APIs to detect OS architecture and OS skus

* https://github.com/dotnet/corefx/issues/4137

* **Namespace**: `System.Runtime.InteropServices`
* **Contract**: `System.Runtime.InteropServices.RuntimeInformation.dll`
* We should expose both, `GetOSArchitecture()` as well as
  `GetProcessArchitecture()`
* We should expose the display name methods to `RuntimeInformation`

Here is how do the look like:

**Window** where type in {Server, Client} and version is from the Win32 version helper APIs:

```
string.Format("Windows {0} {1} Or Greater", type, version);
```

**Linux** Output of `uname -sr` which returns the following on Ubuntu 14.04:

```
Linux 3.16.0-50-generic
```

**OSX**. Output of `uname -sr` which returns the following on OSX 10.10.3:

```
Darwin 14.3.0
```
