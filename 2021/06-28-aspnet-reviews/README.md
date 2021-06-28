# API Review 06/28/2021

## QueryStringEnumerable API

**Approved** | [#aspnetcore/33910](https://github.com/dotnet/aspnetcore/pull/33910#issuecomment-869901305)

* We think `DecodeName` / `DecodeValue` are useful APIs to have. With `DecodeName` we can optimize for the common cases where the name does not need decoding and return the encoded value as is. For `DecodeValue` it's less useful in the common case, but could help in the event if the value need to be parsed (e.g. `int.TryParse`).

* If we do need to share this publicly in the future, we can consider creating a M.Extensions.HttpHelperUtility package.
## Support custom delimiter KeyPerFile configuration source

**Approved** | [#aspnetcore/33693](https://github.com/dotnet/aspnetcore/pull/33693#issuecomment-869905167)

* API triage: Rename `Delimiter` -> `SectionDelimiter`. API approved.
