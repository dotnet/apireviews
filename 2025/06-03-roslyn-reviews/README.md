# API Review 06/03/2025

## New APIs for "User Defined Compound Assignment Operators" feature

**Approved** | [#roslyn/78468](https://github.com/dotnet/roslyn/issues/78468#issuecomment-2936314258)

### API Review

* Update from the feature around conflicts.

**Conclusion**: Approved
## Add CommandLineResource API

**Approved** | [#roslyn/78678](https://github.com/dotnet/roslyn/issues/78678#issuecomment-2936323757)

### API Review

* Part of the issue is that `ResourceDescription` has callbacks that can read from a stream, which can be expensive. At the very least, creating one creates a `Func` callback for reading.
    * This is why we want a new property, not adding something on the existing one. Similar to other instances of adding a new property that is preferred for most scenarios over an existing property for perf reasons.
* The change refactored the command line API to not construct `ResourceDescription`s until actually accessed, only creating the new `CommandLineResource` by default
* We want to make `ToDescription` internal, not public.

**Conclusion**: Approved with `ToDescription` being internal instead
## Extensions: public API for extension syntax in CREF

**Approved** | [#roslyn/78738](https://github.com/dotnet/roslyn/issues/78738#issuecomment-2936391836)

### API Review

* Based on the current proposed design, still needs to be looked at by LDM.
* We're fine with it as currently proposed, but any changes from LDM will need to be ratified. Can likely be done over email.

**Conclusion**: Tentatively approved
