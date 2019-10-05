# Quick Reviews 11/28/2017

## Add overload to Path.GetFullPath() to specify base path

**Approved** | [#corefx/25535](https://github.com/dotnet/corefx/issues/25535) | [Video](https://www.youtube.com/watch?v=9F_NsviGT0Y&t=0h0m0s)

## Need Span overloads for Path APIs

**Approved** | [#corefx/25539](https://github.com/dotnet/corefx/issues/25539) | [Video](https://www.youtube.com/watch?v=9F_NsviGT0Y&t=0h9m18s)

## Need Span based path join API

**Approved** | [#corefx/25536](https://github.com/dotnet/corefx/issues/25536#issuecomment-347624492) | [Video](https://www.youtube.com/watch?v=9F_NsviGT0Y&t=0h13m12s)

It seems weird to me to introduce a new API without deprecating the old API. However, deprecating the old API would be fairly invasive (high usage).

1. Could we fix the behavior of `Combine`? We could quirk the behavior. @JeremyKuhne will take a look.
2. It seems less ideal but more acceptable to me to introduce an overload of `Combine` that takes `Span` with the new behavior. People would opt-into the new behavior by converting to `Span`. Reason being we have a unified set of verbs.
## Add PipeOptions.CurrentUserOnly option

**Approved** | [#corefx/25427](https://github.com/dotnet/corefx/issues/25427) | [Video](https://www.youtube.com/watch?v=9F_NsviGT0Y&t=0h44m2s)

## Add Scalar Intel hardware intrinsic functions

**Approved** | [#corefx/23519](https://github.com/dotnet/corefx/issues/23519) | [Video](https://www.youtube.com/watch?v=9F_NsviGT0Y&t=0h49m16s)

