# API Review 06/30/2021

## Represent enhanced `#line` directives in syntax API

**Approved** | [#roslyn/54318](https://github.com/dotnet/roslyn/issues/54318#issuecomment-871731075)

## API Review

API Approved

* Remove `Deconstruct` from `LineMapping`. If we want to add `Deconstruct` methods to our simple structs, we can add them in general.
* Before merge, see if `CharacterOffset` on `LineMapping` can be an `int` instead of an `int?`. Are there scenarios where the information loss is actually important?
## Expose a way to identify custom attributes

**Approved** | [#roslyn/53410](https://github.com/dotnet/roslyn/issues/53410#issuecomment-871731737)

## API Review

API Approved.
## Add public API to retrieve metadata token from ISymbol

**Approved** | [#roslyn/53486](https://github.com/dotnet/roslyn/issues/53486#issuecomment-871734638)

## API Review

API Approved

* Adding to `Location`, while somewhat tempting, is concerning from a perf perspective. Metadata symbols can share `Location` arrays for all symbols from the same metadata symbol, this would prevent us from doing that and potentially incur a large number of allocations.
* While we do have precedent for exposing `System.Runtime.Metadata` types from our APIs (`IModuleSymbol.GetMetadata().GetMetadataReader()`), an `int` is a good abstraction here that is aligned with ECMA 335 and can be used by both `System.Reflection` and `System.Reflection.Emit`. We'd also want to hide the handle property on derived types to expose `ParameterHandle`/`PropertyHandle`/etc strongly-typed properties, which can be painful.
