# API Review 2015-06-15

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cof15fdk84m4u1icqahsitsblj4).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#2056-review-webspecific-encoding-apis) | [#2056](https://github.com/dotnet/corefx/issues/2056) Review web-specific encoding APIs

## Notes

### #2056 Review web-specific encoding APIs

Status: **Not ready yet** |
[Issue](https://github.com/dotnet/corefx/issues/2056) |
[API Reference](issue-2056.md) |
[Samples](issue-2056-samples.md) |
[Video](https://plus.google.com/events/cof15fdk84m4u1icqahsitsblj4)

* ASP.NET has interface based encoders in order to be DI friendly
    - We made those abstract classes so that we can evolve them
* Encoders support encoding to a string to a `TextWriter`
    - The current design uses an encoder with unsafe methods
    - The higher-level methods are extension methods
    - [KrzysztofCwalina]: Why are those extensions? Should probably be methods on the abstract `Encoder` type
* All encoders will encode angle brackets
    - However, they will use the encoding that is legal for their format
    - That's part of ASP.NET's defense in-depth
* Even though there are filters, the encoders might still encode characters even the filter says no
    - This is usually done if the character is always illegal, for example angle brackets in HTML
    - The way to think about it is that the encoders have a built-in filter and they intersect that with the filter being passed in
* Some of the methods on the encoder types should probably be protected, but today they can't be due to the fact that those are called by extension methods
* What about the existing `Encoder` types?
    - The `System.Text.Encoder` is stateful, but the web encoders require not to be stateful, otherwise those can no longer be used concurrently
    - There are also concerns with the shape of the input and outputs (the existing encoder is from text to bytes, the web ones are from text to text)
* `CodePointFilter` is conceptually a whitelist. The type name and usage doesn't necessarily convey those semantics.
* The `UnicodeRanges` type is new and should probably go into a different DLL
* More notes are in the [API Reference](issue-2056.md)

[KrzysztofCwalina]: https://github.com/KrzysztofCwalina