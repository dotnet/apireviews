# API Review 2015-10-27

This API review was also recorded and is available on [Google+](https://plus.google.com/events/ckjlqlts64s47aj8a9sbebkf2pg).

## Overview

In this API review we reviewed additions to the Roslyn interactive APIs,
AKA scripting APIs.

## Notes

### C# Scripting APIs

Status: **Approved with Feedback** |
[APIs](APIs.md) |
[Video](https://plus.google.com/events/ckjlqlts64s47aj8a9sbebkf2pg)

* `CSharpScript.Build()` should be renamed `CSharpScript.Compile()`
* Chaining potentially creates many objects, flattening would be nice
    - However, it seems that there is no major impact because large script
      files will be a single `CSharpScript` object
    - It only affects interactive hosting, such as REPL
* `ScriptRunner<>` should be `Func`/`Action`
    - Maybe it's worse because every value is returned as `Task` and it takes
      object
* `CancellationToken` isn't passed to the script, the host is responsible for
  passing it to the script via its own global type
* More feedback is in the [API Ref](APIs.md)

## Follow-ups

* We should document how to version APIs with optional parameters

