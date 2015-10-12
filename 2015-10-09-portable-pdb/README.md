# API Review 2015-10-09

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cc1vin923d7nuddasqkd487b8fk).

## Overview

In this API review we reviewed additions to the `System.Reflection.Metadata`
APIs to deal with an amendment to ECMA-335 Partition II metadata for embedding
[debug information](https://github.com/dotnet/roslyn/blob/master/docs/specs/PortablePdb-Metadata.md).

## Notes

### Portable PDB

Status: **Approved with Feedback** |
[APIs](APIs.md) |
[Spec](https://github.com/dotnet/roslyn/blob/master/docs/specs/PortablePdb-Metadata.md) |
[Video](https://plus.google.com/events/cc1vin923d7nuddasqkd487b8fk)

* Will existing readers trip over due to new tables?
    - We believe the answer is no, but **@TMat** should clarify
* Should the functionality live in a separate assembly?
    - We concluded that it shouldn't be split -- it shares a bunch of infrastructure
* Some very generic names, especially Document
    - We should qualify them, e.g. `SourceDocument`
* ~~Should `DebugMetadataHeader` be collapsed and expose `EntryPoint` as a method
  on `MetadataReader`, e.g. `GetDebugEntryPoint()` or something?~~
    - Now has an extra `Id` property, so it's fine as-is.