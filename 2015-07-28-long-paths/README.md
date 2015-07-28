# API Review 2015-07-28

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cu7i66er1ien3s3975o8a5hs8i8).

## Overview

In this API review we discussed how we want address long path limitations due
to `MAX_PATH`.

## `MAX_PATH` Limitations

Status: **Design in progress** |
[Video](https://plus.google.com/events/cu7i66er1ien3s3975o8a5hs8i8)

### Enabling Long Paths

* .NET Core needs to handle > MAX_PATH (260) paths for Unix
  - Enable CoreFX (Unix mostly done)
  - Enable CoreCLR (Assembly & resource loading, not done)
* Also want to support > MAX_PATH on Windows
  - Sharing early thinking, will recircle with deeper dives
  - Many details need uncovered / answered

### Implementation Order

* Unblock long paths on Unix
  - CoreFX then CoreCLR
  - CoreFX mostly done
* Allow extended format long paths on Windows
  - CoreFX then CoreCLR
  - CoreFX initial syntax work done, path length still limited to 260
* Allow long paths without extended format on Windows
  - CoreFX then CoreCLR

### Path Restrictions

* Total path lengths are a restriction of the OS, not the file systems
* Path segment (file/directory names) restrictions vary, usually 255 (UDF is 254!)
* Unix based systems vary (1024/4096?)
* Windows APIs and tools compatibility restrictions
  - Constrained to 260 characters for compatibility
  - Certain characters and character sequences also constrained for compatibility
    + Trailing spaces, period
    + Single & double period
    + Device names (`CON`, `LPT1`, etc.)

### Windows Extended Path Syntax

* Paths start with special character sequence
  - `\\?\` for local paths
  - `\\?\UNC\` for UNCs (`\\Server\Share`)
* Allows ~32K length paths
  - exact path length has to fit when converted to internal NT path
* Additional characters (trailing spaces, period)
* Allows legacy device names
* Does not normalize (`.`, `..``)

## Windows Support: Goals

* Allow long paths without extended syntax to "just work“
  - Facilitates xplat coding with files
  - Allows legacy normalization for better legacy code compat
* Allow extended syntax `\\?\` on Windows
  - Allows writing code that works for any path (`\\?\C:\Foo.`)
* Facilitate writing path code that works

### Known Issues

* Curent working directory
  - Can’t set past 260, even with extended
  - New Path APIs may help mitigate
* Iteration/querying paths that need extended syntax
  - Characters that are traditionally normalized away
  - Ignore/filter/return as extended?
* Back porting to the Desktop CLR

### Long Paths Just Work

* Normalize > 260
  - Legacy resolution (`..` and `.` are resolved)
  - Legacy character support (no trailing spaces, etc.)
  - Extended syntax not given back
* Intercept before P/Invoke
  - Append extended if needed
  - Trim extended on return if added

### Allow Extended Syntax (`\\?\`)

* Don't attempt to normalize
* Very basic correctness checks (invalid chars, `.`, `..``)
* Only throw PathTooLong if > 32,767 (don't try and guess internal translation)
* CoreFx work has begun, key scenarios unblocked (>260 not done yet)
* CoreCLR work about to start

### API Changes (Path.GetFullPath)

* "Normal" paths allow >260 with legacy normalization and validation
  - Trailing whitespace is trimmed
  - Separators collapsed and normalized
  - Periods normalized/rejected
  - Short file paths expanded
* `\\?\` paths are validated, but not resolved
* New overload (string rootPath, string relativePath)
  - Always assumes `..` & `.` segments are relative indicators in relativePath
  - Allows working around limitations in CWD
  - Keeps extended characters (trailing space) if rootPath is extended
  - `\\?\` paths only have relative segments evaluated from relativePath

### New APIs

* `Path.PathContains(string firstPath, string secondPath)`
  - Is the second path contained in the first?
  - Both must be non-relative?
  - Considers extended syntax, casing
* `Path.PathsAreEquivalent(string firstPath, string secondPath)`
  - Do both paths represent the same file?
  - Both must be non-relative?
  - Considers extended syntax, casing
* `PathComparer : IEqualityComparer`

**Notes:**

* What about comparing paths?
    - Case sensitivity is hard because Unix systems have a single path space
      across devices which means that parts of the path are sensitive and parts
      are insensitive
* How does Java handle these issues?
    - Linux & Mac both a have C API 'real path' that returns a canonical pat

### Follow-ups

* @JeremyKuhne will follow-up with a more concrete design proposal